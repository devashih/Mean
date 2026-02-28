pipeline {
    agent any

    environment {
        BACKEND_IMAGE = "asura0009/backend"
        FRONTEND_IMAGE = "asura0009/frontend"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Backend') {
            steps {
                sh 'docker build -t $BACKEND_IMAGE ./backend'
            }
        }

        stage('Build Frontend') {
            steps {
                sh 'docker build -t $FRONTEND_IMAGE ./frontend'
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub',
                    usernameVariable: 'asura0009',
                    passwordVariable: 'asura009$'
                )]) {
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                }
            }
        }

        stage('Push Images') {
            steps {
                sh 'docker push $BACKEND_IMAGE'
                sh 'docker push $FRONTEND_IMAGE'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                docker pull asura0009/backend
                docker pull asura0009/frontend
                docker-compose down
                docker-compose up -d
                '''
            }
        }
    }
}
