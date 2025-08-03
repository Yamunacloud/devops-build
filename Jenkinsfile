pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('6be745f1-1b13-43d9-a7a0-02d2ec7f96ed')
        IMAGE_BASE = "yamunacloud/prod"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Determine Docker Tag') {
            steps {
                script {
                    // You can use env.BRANCH_NAME in multibranch pipelines
                    def branch = env.BRANCH_NAME ?: sh(script: 'git rev-parse --abbrev-ref HEAD', returnStdout: true).trim()
                    echo "Building for branch: ${branch}"
                    
                    if (branch == 'master') {
                        env.IMAGE_TAG = 'prod'
                    } else if (branch == 'dev') {
                        env.IMAGE_TAG = 'dev'
                    } else {
                        env.IMAGE_TAG = branch.replaceAll('/', '-')
                    }

                    env.FULL_IMAGE_NAME = "${IMAGE_BASE}:${IMAGE_TAG}"
                    echo "Docker image will be tagged as ${env.FULL_IMAGE_NAME}"
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $FULL_IMAGE_NAME .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    sh "echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin"
                    sh "docker push $FULL_IMAGE_NAME"
                }
            }
        }
    }
}
