pipeline {
agent any

environment {
    IMAGE_NAME = "travel-website-automation"
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
            docker build -t $IMAGE_NAME .
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
            $IMAGE_NAME
            '''
        }
    }

    stage('Verify Deployment') {
        steps {
            sh '''
            docker ps
            '''
        }
    }
}

post {

    success {
        echo 'Travel Website Deployment Successful'
    }

    failure {
        echo 'Travel Website Deployment Failed'
    }

    always {
        sh 'docker ps -a'
    }
}


}
