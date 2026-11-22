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
                 sh 'docker build -t react-sample-image .'
            } 
        }
        stage('Deploy Container') { 
            steps {
                sh 'docker rm -f c1 || true'
                sh 'docker run -d --name c1 -p 8081:80 react-sample-image'
            } 
        }
    }
    post {
        success { emailext(
            to: 'praveenkumar.sankar08@gmail.com',
            subject: "SUCCESS: React-Job #${BUILD_NUMBER}",
            body: "React app deployed successfully. Build URL: ${BUILD_URL}"
        ) }
        failure { emailext(
            to: 'praveenkumar.sankar08@gmail.com',
            subject: "FAILED: React-Job #${BUILD_NUMBER}",
            body: "Pipeline failed. Check console output: ${BUILD_URL}"
        ) }
    }
}
