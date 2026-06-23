pipeline {
    agent any

    environment {
        AWS_REGION = "ap-south-1"
        ACCOUNT_ID = "245112717518"
        ECR_REPO = "my-app"

        IMAGE_TAG = "${BUILD_NUMBER}"
        IMAGE_URI = "${ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPO}:${IMAGE_TAG}"
    }

    stages {

        stage('Build') {
            steps {
                sh '''
                docker build -t my-app .
                '''
            }
        }

        stage('Login ECR') {
            steps {
                sh '''
                aws ecr get-login-password --region $AWS_REGION | \
                docker login --username AWS --password-stdin \
                $ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com
                '''
            }
        }

        stage('Tag') {
            steps {
                sh '''
                docker tag my-app:latest $IMAGE_URI
                '''
            }
        }

        stage('Push') {
            steps {
                sh '''
                docker push $IMAGE_URI
                '''
            }
        }
    }

    post {
        success {
            echo 'Image pushed successfully'
        }

        failure {
            echo 'Pipeline failed'
        }
    }
}