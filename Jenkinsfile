pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "valakatisrisairam/my-k8s-app"
        IMAGE_TAG = "${BUILD_NUMBER}"
    }

    stages {

        stage('Checkout from GitHub') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/SriSairam518/Jenkins.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh """
                docker build -t $DOCKER_IMAGE:$IMAGE_TAG .
                docker tag $DOCKER_IMAGE:$IMAGE_TAG $DOCKER_IMAGE:latest
                """
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    withDockerRegistry([credentialsId: 'dockerhub-creds', url: '']) {
                        sh """
                        docker push $DOCKER_IMAGE:$IMAGE_TAG
                        docker push $DOCKER_IMAGE:latest
                        """
                    }
                }
            }
        }
    }

    post {
        success {
            echo "✅ Image pushed successfully!"
        }
        failure {
            echo "❌ Pipeline failed. Check logs."
        }
    }
}
