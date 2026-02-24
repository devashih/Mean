pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-cred')
        DOCKERHUB_REPO = "asura0009"
        IMAGE_TAG = "latest"
    }

    stages {

        stage('Build Backend') {
            steps {
                sh 'docker build -t $DOCKERHUB_REPO/mean-backend:$IMAGE_TAG ./backend'
            }
        }

        stage('Build Frontend') {
            steps {
                sh 'docker build -t $DOCKERHUB_REPO/mean-frontend:$IMAGE_TAG ./frontend'
            }
        }

        stage('Push Images') {
            steps {
                sh '''
                echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin
                docker push $DOCKERHUB_REPO/mean-backend:$IMAGE_TAG
                docker push $DOCKERHUB_REPO/mean-frontend:$IMAGE_TAG
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                cd /home/ubuntu/mean-app
                docker-compose pull
                docker-compose down
                docker-compose up -d
                '''
            }
        }
    }
}
