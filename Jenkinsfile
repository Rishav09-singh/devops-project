pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "rishavsingh09/devops-app"
    }

    stages {

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Login to DockerHub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS'
                )]) {
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                }
            }
        }

        stage('Push Image') {
            steps {
                sh 'docker push $DOCKER_IMAGE'
            }
        }

      stage('Deploy on App Server') {
    steps {
        sh '''
        ssh -o StrictHostKeyChecking=no ubuntu@54.252.167.15 << EOF
        docker pull rishavsingh09/devops-app
        docker stop devops-container || true
        docker rm devops-container || true
        docker run -d -p 80:3000 --name devops-container rishavsingh09/devops-app
        EOF
        '''
    }
}
    }
}
