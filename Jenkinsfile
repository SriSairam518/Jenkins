pipeline {
    agent any

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
                docker build -t my-k8s-app:${BUILD_NUMBER}  .
                docker tag my-k8s-app:${BUILD_NUMBER} valakatisrisairam/my-k8s-app:${BUILD_NUMBER}"
                """
            }
        }

       stage('Push Docker Image){
       	steps{
       		sh """
       			withDockerRegistry([credentialsId : 'dockerhub-creds', url : '']){
       				sh "docker push ${Docker_IMAGE}"
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
