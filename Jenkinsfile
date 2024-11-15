pipeline {
    agent any

    environment {
        REGISTRY = 'user19.azurecr.io'
        IMAGE_NAME = 'product'
        AKS_CLUSTER = 'user19-aks'
        RESOURCE_GROUP = 'user19-rsrcgrp'
        AKS_NAMESPACE = 'default'
        AZURE_CREDENTIALS_ID = 'Azure-Cred'
        TENANT_ID = '29d166ad-94ec-45cb-9f65-561c038e1c7a' // Service Principal 등록 후 생성된 ID
        GIT_USER_NAME = 'kyusooK'
        GIT_USER_EMAIL = 'rbtn110@uengine.org'
        GITHUB_CREDENTIALS_ID = 'Github-Cred'
    }
 
    stages {
        stage('Clone Repository') {
            steps {
                checkout scm
            }
        }
        
        stage('Maven Build') {
            steps {
                withMaven(maven: 'Maven') {
                    sh 'mvn package -DskipTests'
                }
            }
        }
        
        stage('Docker Build') {
            steps {
                script {
                    image = docker.build("${REGISTRY}/${IMAGE_NAME}:v${env.BUILD_NUMBER}")
                }
            }
        }
        
        stage('Azure Login') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: env.AZURE_CREDENTIALS_ID, usernameVariable: 'AZURE_CLIENT_ID', passwordVariable: 'AZURE_CLIENT_SECRET')]) {
                        sh 'az login --service-principal -u $AZURE_CLIENT_ID -p $AZURE_CLIENT_SECRET --tenant ${TENANT_ID}'
                    }
                }
            }
        }
        
        stage('Push to ACR') {
            steps {
                script {
                    sh "az acr login --name ${REGISTRY.split('\\.')[0]}"
                    sh "docker push ${REGISTRY}/${IMAGE_NAME}:v${env.BUILD_NUMBER}"
                }
            }
        }
        
        stage('CleanUp Images') {
            steps {
                sh """
                docker rmi ${REGISTRY}/${IMAGE_NAME}:v$BUILD_NUMBER
                """
            }
        }

        stage('Update and Push to GitHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: GITHUB_CREDENTIALS_ID, usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
                    sh """
                        sed -i 's/latest/v${env.BUILD_ID}/g' kubernetes/deploy.yaml
                        git config --global user.name "kyusooK"
                        git config --global user.email "rbtn110@uengine.org"
                        
                        # credential.helper를 통해 안전하게 인증 정보 설정
                        git config --global credential.helper '!f() { echo username=\\$GIT_USERNAME; echo password=\\$GIT_PASSWORD; }; f'
                        
                        git add kubernetes/deploy.yaml
                        git commit -m "Update deployment image tag to v${env.BUILD_ID}"
                        git push origin HEAD:main
                        
                        # 인증 정보 제거
                        git config --global --unset credential.helper
                    """
                }
            }
        }
        // stage('Deploy to AKS') {
        //     steps {
        //         script {
        //             sh "az aks get-credentials --resource-group ${RESOURCE_GROUP} --name ${AKS_CLUSTER}"
        //             sh """
        //             sed 's/latest/v${env.BUILD_ID}/g' kubernetes/deploy.yaml > output.yaml
        //             cat output.yaml
        //             kubectl apply -f output.yaml
        //             kubectl apply -f kubernetes/service.yaml
        //             rm output.yaml
        //             """
        //         }
        //     }
        // }
    }
}
