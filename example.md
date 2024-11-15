pipeline {
    agent any

    environment {
        REGISTRY = 'user07.azurecr.io'
        IMAGE_NAME = 'product'
        // ... 기존 환경 변수들 ...
        GIT_USER_NAME = 'Jenkins'
        GIT_USER_EMAIL = 'jenkins@example.com'
    }
    
    // ... existing code ...
        stage('Update and Push to GitHub') {
            steps {
                script {
                    sh """
                    sed -i 's/latest/v${env.BUILD_ID}/g' kubernetes/deploy.yaml
                    git config --global user.email "${GIT_USER_EMAIL}"
                    git config --global user.name "${GIT_USER_NAME}"
                    git add kubernetes/deploy.yaml
                    git commit -m "Update deployment image tag to v${env.BUILD_ID}"
                    git push origin HEAD:main
                    """
                }
            }
        }
    // ... rest of the pipeline ...
