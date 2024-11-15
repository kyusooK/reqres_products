pipeline {
    agent any

    environment {  
        REGISTRY = 'user07.azurecr.io'
        IMAGE_NAME = 'product'
        AZURE_CREDENTIALS_ID = 'Azure-Cred'
        TENANT_ID = 'f46af6a3-e73f-4ab2-a1f7-f33919eda5ac' // Service Principal 등록 후 생성된 ID
        GITHUB_CREDENTIALS_ID = 'Github-Cred'
        GITHUB_REPO = 'https://github.com/your-repo/your-repo-name.git' // 적절히 수정
        GITHUB_BRANCH = 'main' // 업로드할 브랜치
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
        
        stage('Update deploy.yaml') {
            steps {
                script {
                    sh """
                    sed 's/latest/v${env.BUILD_ID}/g' kubernetes/deploy.yaml > updated_deploy.yaml
                    mv updated_deploy.yaml kubernetes/deploy.yaml
                    cat kubernetes/deploy.yaml
                    """
                }
            }
        }
        
        stage('Commit and Push to GitHub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: GITHUB_CREDENTIALS_ID, usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
                        sh """
                        git config --global user.email "your-email@example.com"
                        git config --global user.name "Jenkins CI"
                        git clone https://${GIT_USERNAME}:${GIT_PASSWORD}@${GITHUB_REPO} repo
                        cp kubernetes/deploy.yaml repo/kubernetes/deploy.yaml
                        cd repo
                        git add kubernetes/deploy.yaml
                        git commit -m "Update deploy.yaml with build ${env.BUILD_NUMBER}"
                        git push origin ${GITHUB_BRANCH}
                        """
                    }
                }
            }
        }
    }
}
