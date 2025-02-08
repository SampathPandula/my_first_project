pipeline {
    agent any
    environment {
        ECR_REPO = "713881815267.dkr.ecr.ap-east-1.amazonaws.com/demo-app"
    }
    stages {
        stage('Checkout Code') {
            steps {
                git credentialsId: 'GitHub-Token', url: 'https://github.com/SampathPandula/my_first_project.git', branch: 'main'
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
                sh 'kubectl apply -f deployment.yaml'
            }
        }
    }
}
