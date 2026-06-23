pipeline {
    agent any

    environment {
        AWS_REGION = "ap-south-1"
        ECR_REPO = "jenkins-demo"

        ACCOUNT_ID = "245112717518"

        IMAGE_TAG = "${BUILD_NUMBER}"

        IMAGE_URI =
        "${ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPO}:${IMAGE_TAG}"
    }

    stages {

        stage('Clone') {
            steps {
                git branch: 'main',
                url: 'https://github.com/manjunath-js/jenkins-demo.git'
            }
        }

        stage('Build') {
            steps {
                sh '''
                docker build -t $ECR_REPO .
                '''
            }
        }

        stage('Login ECR') {
            steps {
                sh '''
                aws ecr get-login-password \
                --region $AWS_REGION |
                docker login \
                --username AWS \
                --password-stdin \
                $ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com
                '''
            }
        }

        stage('Tag') {
            steps {
                sh '''
                docker tag \
                $ECR_REPO \
                $IMAGE_URI
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
}