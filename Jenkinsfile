pipeline {
    agent any
    environment {
        IMAGE_NAME      = "myapp"
        DOCKER_HUB_USER = "urk23cs1221"
    }
    stages {
        stage('Clone') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/URK23CS1221/end_lab.git'
            }
        }
        stage('Build Image') {
            steps {
                sh 'docker build -t $DOCKER_HUB_USER/$IMAGE_NAME .'
            }
        }
        stage('Push to Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                      docker push $DOCKER_HUB_USER/$IMAGE_NAME:latest
                    '''
                }
            }
        }
        stage('Deploy') {
            steps {
                sh 'docker run -d -p 5000:5000 $DOCKER_HUB_USER/$IMAGE_NAME'
            }
        }
    }
}