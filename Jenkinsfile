pipeline {
    agent any

    environment {
        DOCKER_USER = "asura0009"
        IMAGE_BACKEND = "asura0009/backend"
        IMAGE_FRONTEND = "asura0009/frontend"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git 'https://github.com/devashih/Mean.git'
            }
        }

        stage('Build Backend Image') {
            steps {
                sh 'docker build -t $IMAGE_BACKEND ./backend'
            }
        }

        stage('Build Frontend Image') {
            steps {
                sh 'docker build -t $IMAGE_FRONTEND ./frontend'
            }
        }

        stage('Login to DockerHub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'asura0009',
                    passwordVariable: 'asura009$'
                )]) {
                    sh 'echo $PASSWORD | docker login -u $USERNAME --password-stdin'
                }
            }
        }

        stage('Push Images') {
            steps {
                sh 'docker push $IMAGE_BACKEND'
                sh 'docker push $IMAGE_FRONTEND'
            }
        }

        stage('Deploy to VM') {
            steps {
                sh '''
                ssh ubuntu@your-ec2-ip "
                    docker pull $IMAGE_BACKEND &&
                    docker pull $IMAGE_FRONTEND &&
                    docker-compose down &&
                    docker-compose up -d
                "
                '''
            }
        }
    }
}
