pipeline {
    agent any

    environment {
        IMAGE_NAME = "newbieshubham/shubham-resume"
        IMAGE_TAG = "latest"
        DOCKER_CREDENTIALS_ID = "dockerhub-creds"
        GITHUB_CREDENTIALS_ID = "Github"  // Corrected GitHub PAT credential ID from your Jenkins
    }

    stages {
        stage('Clone GitHub Repo') {
            steps {
                withCredentials([string(credentialsId: "${GITHUB}", variable: 'GITHUB_PAT')]) {
                    sh """
                        git clone https://ShubhamP1028:${GITHUB_PAT}@github.com/ShubhamP1028/Portfoliio.git
                    """
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t shubham-resume ."
            }
        }

        stage('Tag Docker Image') {
            steps {
                sh "docker tag shubham-resume ${IMAGE_NAME}:${IMAGE_TAG}"
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: "${dockerhub-creds}", usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh "echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin"
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                sh "docker push ${IMAGE_NAME}:${IMAGE_TAG}"
            }
        }
    }

    post {
        success {
            echo '✅ Docker image successfully pushed to Docker Hub!'
        }
        failure {
            echo '❌ Build failed. Please check the logs.'
        }
    }
}
