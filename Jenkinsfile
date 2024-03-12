z// // pipeline {
// //     agent any

// //     environment {
// //         AWS_ACCESS_KEY_ID = credentials('jenkins-aws-access-key-id')
// //         AWS_SECRET_ACCESS_KEY = credentials('jenkins-secret-key-id')
// //         AWS_DEFAULT_REGION = 'ap-south-1'
// //         ECR_REPOSITORY = '722011624210.dkr.ecr.ap-south-1.amazonaws.com/my-node-app'
// //         IMAGE_TAG = 'latest'
// //     }

// //     stages {
// //         stage('Checkout') {
// //             steps {
// //                 checkout scm
// //             }
// //         }

// //         stage('Build Docker image') {
// //             steps {
// //                 sh 'docker build -t $ECR_REPOSITORY:$IMAGE_TAG .'
// //             }
// //         }

// //         stage('Tag Docker image') {
// //             steps {
// //                 sh 'docker tag $ECR_REPOSITORY:$IMAGE_TAG $ECR_REPOSITORY:$IMAGE_TAG'
// //             }
// //         }

// //         stage('Push to ECR') {
// //             steps {
// //                 sh 'aws ecr get-login-password --region $AWS_DEFAULT_REGION | docker login --username AWS --password-stdin $ECR_REPOSITORY'
// //                 sh 'docker push $ECR_REPOSITORY:$IMAGE_TAG'
// //             }
// //         }
// //     }
// // }


pipeline {
    agent any

    environment {
        AWS_ACCESS_KEY_ID = credentials('jenkins-aws-access-key-id')
        AWS_SECRET_ACCESS_KEY = credentials('jenkins-secret-key-id')
        AWS_DEFAULT_REGION = 'ap-south-1'
        ECR_REPOSITORY = '722011624210.dkr.ecr.ap-south-1.amazonaws.com/my-node-app'
        IMAGE_TAG = 'latest'
        KUBECONFIG = credentials('kubeconfig') // Assuming you have stored your kubeconfig file in Jenkins credentials
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        // stage('Create EKS Cluster') {
        //     steps {
        //         sh 'terraform init'
        //         sh 'terraform destroy -auto-approve'
        //         // Assuming you have a main.tf file in your repository that describes your EKS cluster
        //     }
        // }


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

        // stage('Connect to EKS Cluster') {
        //     steps {
        //         withCredentials([file(credentialsId: 'kubeconfig', variable: 'KUBECONFIG')]) {
        //             sh 'kubectl config use-context my-cluster'  // Set the correct context name
        //         }
        //     }
        // }

        stage('Deploy to EKS') {
            steps {
                sh 'kubectl apply -f deployment.yaml' // Assuming you have a deployment.yaml file in your repository
            }
        }
    }
}


