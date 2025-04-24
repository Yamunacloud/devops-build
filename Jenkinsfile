pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('6be745f1-1b13-43d9-a7a0-02d2ec7f96ed') // Add in Jenkins credentials
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
                sh 'sudo docker build -t $IMAGE_NAME:prod .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    sh "echo $DOCKERHUB_CREDENTIALS_PSW | sudo docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin"
                    sh "sudo docker push $IMAGE_NAME:prod"
                }
            }
        }
    }
}
