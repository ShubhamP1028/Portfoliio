pipeline {
    agent any

    environment {
        DOCKER_CREDENTIALS = credentials('dockerhub') // Replace 'dockerhub' with your actual Jenkins credential ID for Docker Hub
        IMAGE_NAME = 'shubhamp1028/portfolio'         // Replace with your Docker Hub repo name
    }

    stages {
        stage('Clone GitHub Repo') {
            steps {
                echo '✅ Cloning the repository...'
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                echo '🐳 Building Docker image...'
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Login to Docker Hub') {
            steps {
                echo '🔐 Logging into Docker Hub...'
                sh "echo $DOCKER_CREDENTIALS_PSW | docker login -u $DOCKER_CREDENTIALS_USR --password-stdin"
            }
        }

        stage('Push Docker Image') {
            steps {
                echo '📤 Pushing Docker image to Docker Hub...'
                sh 'docker push $IMAGE_NAME'
            }
        }
    }

    post {
        success {
            echo '✅ Build and Push successful!'
        }
        failure {
            echo '❌ Build failed. Please check the logs.'
        }
    }
}
