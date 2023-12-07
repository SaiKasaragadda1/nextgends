pipeline {
    agent {
        label 'DevOps-node'
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/ashwini-eks']], extensions: [], userRemoteConfigs: [[credentialsId: 'githu-cred', url: 'https://github.com/Research-Associate-Internship/rac3-nextgendes.git']])
            }
        }
        stage('Apply Kubernetes files'){
            steps {
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding',credentialsId: 'aws_credentials']]) {
                        sh 'aws eks --region us-east-1 update-kubeconfig --name NextGenDS-rac3-cluster'
                }
            }
        }
        stage('Kubectl deploy') {
            steps {
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding',credentialsId: 'aws_credentials']]) {
                    dir("${WORKSPACE}/k8s-specifications") {
                        sh 'kubectl apply -f db-deployment.yaml'
                        sh 'kubectl apply -f redis-deployment.yaml'
                        sh 'kubectl apply -f vote-deployment.yaml'
                        sh 'kubectl apply -f result-deployment.yaml'
                        sh 'kubectl apply -f redis-service.yaml'
                        sh 'kubectl apply -f db-service.yaml'
                        sh 'kubectl apply -f worker-deployment.yaml'
                        sh 'kubectl apply -f vote-service.yaml'
                        sh 'kubectl apply -f result-service.yaml'
                        sh 'kubectl apply -f ingress.yaml'
                        
                    }
                }
            }
        }
        
    }
}
