pipeline {
    agent any

    environment {
        REGISTRY = 'user07.azurecr.io'
        IMAGE_NAME = 'product'
        AKS_CLUSTER = 'user07-aks'
        RESOURCE_GROUP = 'user07-rsrcgrp'
        AKS_NAMESPACE = 'default'
        AZURE_CREDENTIALS_ID = 'Azure-Cred'
        TENANT_ID = 'f46af6a3-e73f-4ab2-a1f7-f33919eda5ac' // Service Principal 등록 후 생성된 ID
        GIT_USER_NAME = 'kyusooK'
        GITHUB_TOKEN = credentials('GITHUB_TOKEN_ID') 
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
                script {
                    sh """
                    sed -i 's/latest/v${env.BUILD_ID}/g' kubernetes/deploy.yaml
                    git config --global user.name "${GIT_USER_NAME}"
                    git remote set-url origin https://${GITHUB_TOKEN}@github.com/username/repository.git
                    git add kubernetes/deploy.yaml
                    git commit -m "Update deployment image tag to v${env.BUILD_ID}"
                    git push origin HEAD:main
                    """
                }
            }
        }
        stage('Deploy to AKS') {
            steps {
                script {
                    sh "az aks get-credentials --resource-group ${RESOURCE_GROUP} --name ${AKS_CLUSTER}"
                    sh """
                    sed 's/latest/v${env.BUILD_ID}/g' kubernetes/deploy.yaml > output.yaml
                    cat output.yaml
                    kubectl apply -f output.yaml
                    kubectl apply -f kubernetes/service.yaml
                    rm output.yaml
                    """
                }
            }
        }
    }
}
