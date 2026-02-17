pipeline {
    agent any

    environment {
        IMAGE_NAME = "vkdamodar8389/devops-capstone"
        IMAGE_TAG  = "v1"
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {

                bat 'docker build -t devops-capstone:v1 .'

                bat "docker build -t %IMAGE_NAME%:%IMAGE_TAG% ."
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    bat "docker login -u %DOCKER_USER% -p %DOCKER_PASS%"
                }
            }
        }

        stage('Push Image to Docker Hub') {
            steps {
                bat "docker push %IMAGE_NAME%:%IMAGE_TAG%"
 
            }
        }
    }
}

