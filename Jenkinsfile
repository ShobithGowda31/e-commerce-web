pipeline {

    agent any

    tools {
        maven 'Maven3'
    }

    environment {
        IMAGE_NAME = "ecommerce-app"
        CONTAINER_NAME = "ecommerce-container"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                url: 'https://github.com/<your-github-username>/e-commerce-web.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                docker rm -f $CONTAINER_NAME || true
                docker run -d \
                --name $CONTAINER_NAME \
                -p 8085:8080 \
                $IMAGE_NAME
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                sh 'sleep 15'
                sh 'curl http://localhost:8085'
            }
        }

    }

    post {

        success {
            echo 'Deployment Successful'
        }

        failure {
            echo 'Deployment Failed'
        }

    }

}
