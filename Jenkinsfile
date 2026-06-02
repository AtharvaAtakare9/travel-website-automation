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
                sh '''
                docker build -t $IMAGE_NAME:${BUILD_NUMBER} .
                '''
            }
        }

        stage('DockerHub Login') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-creds',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )
                ]) {
                    sh '''
                    echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                    '''
                }
            }
        }

        stage('Push Image') {
            steps {
                sh '''
                docker push $IMAGE_NAME:${BUILD_NUMBER}

                docker tag $IMAGE_NAME:${BUILD_NUMBER} $IMAGE_NAME:latest

                docker push $IMAGE_NAME:latest
                '''
            }
        }

        stage('Remove Existing Container') {
            steps {
                sh '''
                docker rm -f $CONTAINER_NAME || true
                '''
            }
        }

        stage('Deploy Application') {
            steps {
                sh '''
                docker run -d \
                --name $CONTAINER_NAME \
                -p $APP_PORT:80 \
                $IMAGE_NAME:${BUILD_NUMBER}
                '''
            }
        }
    }

    post {
        success {
            echo 'DockerHub Push Successful'
        }

        failure {
            echo 'Pipeline Failed'
        }
    }
}