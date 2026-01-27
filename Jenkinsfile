pipeline {
    agent any

    triggers {
        githubPush()
    }

    stages {
        stage ('git checkout') {
            steps {
               git branch: 'main', url: 'https://github.com/iamsubbu3/maven-web-application.git'
            }
        }
        
        stage ('deploy to eks') {
            steps {
                script {
                    dir('k8s-manifests') {
                        withKubeConfig(caCertificate: '', clusterName: '', contextName: '', credentialsId: 'kubeconfig-file', namespace: '', restrictKubeConfigAccess: false, serverUrl: '') {
                            sh 'kubectl apply -f mongodb-deployment.yml'
                            sh 'kubectl apply -f mongodb-service.yml'
                            sh 'kubectl apply -f userprofile-deployment.yml'
                            sh 'kubectl apply -f usernode-js-service.yml'
                        }
                    }
                }
            }
        }
    }
}