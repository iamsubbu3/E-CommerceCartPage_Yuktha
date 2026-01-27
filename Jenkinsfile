pipeline {
    agent any

    triggers {
        githubPush()
    }

    stages {
        stage ('git checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/iamsubbu3/E-CommerceCartPage_Yuktha.git'
            }
        }
        
        stage('deploy to eks') {
            environment {
                // This automatically exports these as shell environment variables
                AWS_ACCESS_KEY_ID     = credentials('aws-access-key')
                AWS_SECRET_ACCESS_KEY = credentials('aws-secret-key')
                AWS_DEFAULT_REGION    = 'us-east-1' 
            }
            steps {
                script {
                    dir('k8s-manifests') {
                        // 1. Refresh the kubeconfig
                        sh "aws eks update-kubeconfig --region ${AWS_DEFAULT_REGION} --name subbu-cluster"
                        
                        // 2. Apply manifests
                        sh "kubectl apply -f mongodb-deployment.yaml"
                        sh "kubectl apply -f mongodb-service.yaml"
                        sh "kubectl apply -f app-deployment.yaml"
                        sh "kubectl apply -f app-service.yaml"
                    }
                }
            }
        }
    }
}