pipeline {
    agent any

    environment {
        IMAGE_NAME = "vkdamodar8389/devops-capstone"
        IMAGE_TAG  = "v1"
        EC2_IP     = "98.91.17.173"
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
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

        stage('Deploy to EC2') {
            steps {
                sshagent(credentials: ['ec2-ssh']) {
                    bat """
                    ssh -o StrictHostKeyChecking=no ubuntu@%EC2_IP% ^
                    docker pull %IMAGE_NAME%:%IMAGE_TAG% && ^
                    docker stop capstone || exit 0 && ^
                    docker rm capstone || exit 0 && ^
                    docker run -d --name capstone -p 80:80 %IMAGE_NAME%:%IMAGE_TAG%
                    """
                }
            }
        }
    }
}

