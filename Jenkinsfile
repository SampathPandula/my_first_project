pipeline {
    agent any
    environment {
        ECR_REPO = "<AWS_ACCOUNT_ID>.dkr.ecr.ap-east-1.amazonaws.com/demo-app"
    }
    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/your-repo.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $ECR_REPO:latest .'
            }
        }
        stage('Push Image to ECR') {
            steps {
                sh 'aws ecr get-login-password --region ap-east-1 | docker login --username AWS --password-stdin $ECR_REPO'
                sh 'docker push $ECR_REPO:latest'
            }
        }
        stage('Deploy to EKS') {
            steps {
                sh 'kubectl apply -f k8s/deployment.yaml'
            }
        }
    }
}
