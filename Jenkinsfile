pipeline {
    agent any

    environment {
        IMAGE_NAME = "atharvaatakare9/travel-website-automation"
        CONTAINER_NAME = "travel-website-app"
        APP_PORT = "8081"
    }

    stages {

        stage('Checkout Source Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh """
                docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} .
                docker tag ${IMAGE_NAME}:${BUILD_NUMBER} ${IMAGE_NAME}:latest
                """
            }
        }

        stage('DockerHub Login') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-creds',
                        usernameVariable: 'DOCKER_USERNAME',
                        passwordVariable: 'DOCKER_PASSWORD'
                    )
                ]) {
                    sh '''
                    echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin
                    '''
                }
            }
        }

        stage('Push Image to DockerHub') {
            steps {
                sh """
                docker push ${IMAGE_NAME}:${BUILD_NUMBER}
                docker push ${IMAGE_NAME}:latest
                """
            }
        }

        stage('Remove Existing Container') {
            steps {
                sh '''
                docker rm -f travel-website-app || true
                '''
            }
        }

        stage('Deploy Application') {
            steps {
                sh """
                docker run -d \
                --name ${CONTAINER_NAME} \
                -p ${APP_PORT}:80 \
                ${IMAGE_NAME}:${BUILD_NUMBER}
                """
            }
        }

        stage('Verify Deployment') {
            steps {
                sh '''
                sleep 10
                docker ps
                curl -I http://localhost:8081
                '''
            }
        }
    }

    post {
        success {
            echo '====================================='
            echo 'Travel Website Deployment Successful'
            echo '====================================='
        }

        failure {
            echo '====================================='
            echo 'Travel Website Deployment Failed'
            echo '====================================='
            sh 'docker ps -a || true'
        }
    }
}