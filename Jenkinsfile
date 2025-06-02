pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = 'raulcoast/meuappflutter:latest'  // Nome da imagem no Docker Hub
        DOCKER_CREDENTIALS = '38d9aa3d-0a5f-4a7d-ac18-204e126c6d2d'  // ID das credenciais do Docker no Jenkins
    }

    stages {
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([string(credentialsId: DOCKER_CREDENTIALS, variable: 'DOCKER_PASSWORD')]) {
                    sh 'echo "$DOCKER_PASSWORD" | docker login -u raulcoast --password-stdin'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                sh 'docker push $DOCKER_IMAGE'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying application...'
                sh 'docker run -d -p 8080:8080 $DOCKER_IMAGE'
            }
        }
    }
}