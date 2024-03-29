pipeline {
    agent any

    environment {
        REGION = 'eu-west-2'
        EKS_API = 'https://2EEB7F59B852652671CAF2216159BF28.gr7.eu-west-2.eks.amazonaws.com'
        EKS_CLUSTER_NAME = 'user19-eks'
        EKS_JENKINS_CREDENTIAL_ID = 'kube-cred'
        ECR_PATH = '879772956301.dkr.ecr.eu-west-2.amazonaws.com'
        ECR_IMAGE = 'user19-products'
        AWS_CREDENTIAL_ID = 'afae444d-250b-4788-a6aa-72a361045aa9'
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
                    image = docker.build("${ECR_PATH}/${ECR_IMAGE}")                  
                } 
            }  
        }
        stage('Push to ECR') {
            steps {
                script {
                    docker.withRegistry("https://${ECR_PATH}", "ecr:${REGION}:${AWS_CREDENTIAL_ID}") {
                        image.push("v${env.BUILD_NUMBER}")
                    }
                }
            }
        }
        stage('CleanUp Images') {
            steps {
                sh """
                docker rmi ${ECR_PATH}/${ECR_IMAGE}:v$BUILD_NUMBER
                docker rmi ${ECR_PATH}/${ECR_IMAGE}:latest
                """
            }
        }
        stage('Deploy to k8s') {
            steps {
                script {
                        withKubeConfig([credentialsId: "${EKS_JENKINS_CREDENTIAL_ID}",
                                        serverUrl: "${EKS_API}",
                                        clusterName: "${EKS_CLUSTER_NAME}"]) {
                            sh "sed 's/latest/v${env.BUILD_ID}/g' kubernetes/deploy.yaml > output.yaml"
                            sh "cat output.yaml"
                            sh "kubectl apply -f output.yaml"
                            sh "kubectl apply -f kubernetes/service.yaml"
                            sh "rm output.yaml"
                        }
                }
            }
        } 
    }
}
