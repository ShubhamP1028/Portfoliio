pipeline {
    agent any

    environment {
        IMAGE_NAME = "newbieshubham/shubham-resume"
        IMAGE_TAG = "latest"
        DOCKER_CREDENTIALS_ID = "dockerhub-creds"
        GITHUB_CREDENTIALS_ID = "github-pat"  // Your Jenkins GitHub PAT credential ID
    }

    stages {
        stage('Clone GitHub Repo') {
            steps {
                withCredentials([string(credentialsId: "${GITHUB_CREDENTIALS_ID}", variable: 'GITHUB_PAT')]) {
                    sh """
                        git config --global credential.helper store
                        git clone https://$GITHUB_USER:$GITHUB_PAT@github.com/ShubhamP1028/Portfoliio.git
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
                withCredentials([usernamePassword(credentialsId: "${DOCKER_CREDENTIALS_ID}", usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
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
            echo 'Docker image successfully pushed to Docker Hub!'
        }
        failure {
            echo 'Build failed. Please check the logs.'
        }
    }
}
