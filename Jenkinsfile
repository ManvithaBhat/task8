// Jenkinsfile (Docker Hub)
pipeline {
    agent any
    environment {
        DOCKER_IMAGE = "manvithabhat/myapp"
    }
    stages {
        stage('Checkout') {
            steps { checkout scm }
        }
        stage('Build') {
            steps {
                sh "docker build -t ${DOCKER_IMAGE}:${env.BUILD_NUMBER} ."
            }
        }
        stage('Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                      echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                      docker push ${DOCKER_IMAGE}:${BUILD_NUMBER}
                    '''
                }
            }
        }
    }
    post {
        always {
            sh "docker rmi ${DOCKER_IMAGE}:${env.BUILD_NUMBER} || true"
        }
    }
}
