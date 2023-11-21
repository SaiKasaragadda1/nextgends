pipeline {
    agent {
        label 'DevOps-node'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'githu-cred', url: 'https://github.com/Research-Associate-Internship/rac3-nextgendes.git']])
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
                        sh 'kubectl apply -f *.yaml'
                    }
                }
            }
        }
        
    }
}