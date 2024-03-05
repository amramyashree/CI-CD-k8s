pipeline {
    agent any

    environment {
        AWS_ACCESS_KEY_ID = credentials('jenkins-aws-access-key-id')
        AWS_SECRET_ACCESS_KEY = credentials('jenkins-secret-key-id')
        AWS_DEFAULT_REGION = 'ap-south-1'
        ECR_REPOSITORY = '722011624210.dkr.ecr.ap-south-1.amazonaws.com/my-node-app'
        IMAGE_TAG = 'latest'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker image') {
            steps {
                sh 'docker build -t $ECR_REPOSITORY:$IMAGE_TAG .'
            }
        }

        stage('Tag Docker image') {
            steps {
                sh 'docker tag $ECR_REPOSITORY:$IMAGE_TAG $ECR_REPOSITORY:$IMAGE_TAG'
            }
        }

        stage('Push to ECR') {
            steps {
                sh 'aws ecr get-login-password --region $AWS_DEFAULT_REGION | docker login --username AWS --password-stdin $ECR_REPOSITORY'
                sh 'docker push $ECR_REPOSITORY:$IMAGE_TAG'
            }
        }
    }
}
