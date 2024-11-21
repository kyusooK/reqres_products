pipeline {
    agent any

    environment {
        REGISTRY = 'user19.azurecr.io'
        SERVICES = 'order' // fix your microservices
        AKS_CLUSTER = 'user19-aks'
        RESOURCE_GROUP = 'user19-rsrcgrp'
        AKS_NAMESPACE = 'default'
        AZURE_CREDENTIALS_ID = 'Azure-Cred'
        TENANT_ID = '29d166ad-94ec-45cb-9f65-561c038e1c7a'
        GIT_USER_NAME = 'kyusooK'
        GIT_USER_EMAIL = '{Your-GitHub-Email}'
        GITHUB_CREDENTIALS_ID = 'Github-Cred'
        GITHUB_REPO = 'github.com/${GIT_USER_NAME}/12stmall-demo.git'
        GITHUB_BRANCH = 'main' // 업로드할 브랜치
    }
 
    stages {
        stage('Check Modified Files') {
            steps {
                script {
                      // Git 변경된 파일 목록 가져오기
                    def changedFiles = sh(returnStdout: true, script: 'git diff --name-only HEAD~1 HEAD').trim().split('\n')

                // 'src/' 폴더 변경 여부 확인
                    def targetFolder = 'order/src/*'
                    def isModified = changedFiles.any { it.startsWith(targetFolder) }
    
                    if (isModified) {
                        echo "Changes detected in ${targetFolder}. Proceeding with the pipeline."
                    } else {
                        echo "No changes in ${targetFolder}. Stopping pipeline execution."
                        currentBuild.result = 'NOT_BUILT'
                        return
                    }
                }
            }
        }
      
        stage('Clone Repository') {
            when {
                expression {
                    currentBuild.result != 'NOT_BUILT'
                }
            }
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
        
        stage('Deploy to AKS') {
            when {
                expression {
                    currentBuild.result != 'NOT_BUILT'
                }
            }
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
