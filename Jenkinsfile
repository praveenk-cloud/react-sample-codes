
pipeline {
    agent any

    environment {
        IMAGE_NAME = 'react-jenkins-demo'
        CONTAINER_NAME = 'react-jenkins-demo'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t myreact .'
            }
        }

        stage('Deploy Container') {
            steps {
                sh 'docker rm -f react-container || true'
                sh 'docker run -d -p 5173:5173 --name react-container myreact'
            }
        }
    }

    post {

        success {
            emailext(
                to: 'praveenkumar.sankar08@gmail.com',
                subject: "SUCCESS: React-Job #${env.BUILD_NUMBER}",
                body: "React app deployed successfully. Build URL: ${env.BUILD_URL}"
            )
        }

        failure {
            emailext(
                to: 'praveenkumar.sankar08@gmail.com',
                subject: "FAILED: React-Job #${env.BUILD_NUMBER}",
                body: "Pipeline failed. Check console output: ${env.BUILD_URL}"
            )
        }
    }
}
