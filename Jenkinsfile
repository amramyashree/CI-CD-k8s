pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t my-node-app .'
            }
        }
        stage('Push to ECR') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'ecr-credentials', usernameVariable: 'AKIA2QGZ7CMJCH3OX6OT', passwordVariable: 'YFSTZfZHaFzwWxRIZmVi+/IjnseTOMBmHwyHCKQ6')]) {
                    sh 'aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 722011624210.dkr.ecr.ap-south-1.amazonaws.com'
                    sh 'docker tag my-node-app:latest 722011624210.dkr.ecr.ap-south-1.amazonaws.com/my-node-app:latest'
                    sh 'docker push 722011624210.dkr.ecr.ap-south-1.amazonaws.com/my-node-app:latest'
                }
            }
        }
        // stage('Deploy to EKS') {
        //     steps {
        //         sh 'kubectl apply -f deployment.yaml'
        //     }
        // }
    }
}
