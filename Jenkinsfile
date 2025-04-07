pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('docker-hub-creds') // Jenkins stored credentials
        IMAGE_NAME = 'sai127001/nginx-app'  // Update with your Docker Hub repo
        IMAGE_TAG = 'latest'
    }

    stages {
        stage('Checkout Source Code') {
            steps {
                git branch: 'master', url: 'https://github.com/SAI127001/Mar-19_Task.git' 
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    sh "echo ${DOCKERHUB_CREDENTIALS_PSW} | docker login -u ${DOCKERHUB_CREDENTIALS_USR} --password-stdin"
                    sh "docker push ${IMAGE_NAME}:${IMAGE_TAG}"
                }
            }
        }

        stage('Run Nginx Container in Background') {
            steps {
                script {
                    sh "docker run -d --name nginx-server -p 80:80 ${IMAGE_NAME}:${IMAGE_TAG}"
                }
            }
        }
    }

    post {
        success {
            echo "Nginx container is running on port 80!"
        }
        failure {
            echo "Pipeline failed!"
        }
    }
}
