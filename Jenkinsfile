
pipeline {
    agent any
    
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
            withCredentials([
                string(credentialsId: 'telegrambottoken',
                       variable: 'BOT_TOKEN'),
                string(credentialsId: 'telegramchatid',
                       variable: 'CHAT_ID')
            ]) {
                sh '''
                    curl -sS -X POST \
                    "https://api.telegram.org/bot${BOT_TOKEN}/sendMessage" \
                    --data-urlencode "chat_id=${CHAT_ID}" \
                    --data-urlencode "text=SUCCESS: ${JOB_NAME} Build #${BUILD_NUMBER} completed successfully. ${BUILD_URL}"
                '''
            }
        }

        failure {
            withCredentials([
                string(credentialsId: 'telegrambottoken',
                       variable: 'BOT_TOKEN'),
                string(credentialsId: 'telegramchatid',
                       variable: 'CHAT_ID')
            ]) {
                sh '''
                    curl -sS -X POST \
                    "https://api.telegram.org/bot${BOT_TOKEN}/sendMessage" \
                    --data-urlencode "chat_id=${CHAT_ID}" \
                    --data-urlencode "text=FAILURE: ${JOB_NAME} Build #${BUILD_NUMBER} failed. Check Jenkins console output. ${BUILD_URL}"
                '''
            }
        }

    }
}
