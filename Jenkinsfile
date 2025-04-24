pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('3d9acecb-ef2e-4f9b-97cf-b6ee4583c685') // Add in Jenkins credentials
        IMAGE_NAME = "yamunacloud/prod"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:prod .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    sh "echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin"
                    sh "docker push $IMAGE_NAME:prod"
                }
            }
        }
    }
}
