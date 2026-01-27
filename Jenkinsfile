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
        
        stage ('deploy to eks') {
            steps {
                script {
                    dir('k8s-manifests') {
                        withKubeConfig(caCertificate: '', clusterName: 'subbu-cluster', contextName: '', credentialsId: 'kubeconfig-file', namespace: 'subbu-1-ns', restrictKubeConfigAccess: false, serverUrl: '') {
                            sh 'kubectl apply -f mongodb-deployment.yaml'
                            sh 'kubectl apply -f mongodb-service.yaml'
                            sh 'kubectl apply -f app-deployment.yaml'
                            sh 'kubectl apply -f app-service.yaml'
                        }
                    }
                }
            }
        }
    }
}