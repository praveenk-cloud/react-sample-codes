
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
        always {
            script {

                def repoName = 'react-sample-codes'

                def buildNumber = env.BUILD_NUMBER

                def commitId = sh(
                    script: 'git rev-parse --short HEAD',
                    returnStdout: true
                ).trim()

                def msg = sh(
                    script: 'git log -1 --pretty=%s',
                    returnStdout: true
                ).trim()

                def commitName = sh(
                    script: 'git log -1 --pretty=%an',
                    returnStdout: true
                ).trim()

                def message = """
Jenkins Notification

Repository: ${repoName}
Build Number: ${buildNumber}
Commit ID: ${commitId}
Commit Message: ${msg}
Commit Name: ${commitName}
Build Status: ${currentBuild.currentResult}
"""

                withCredentials([
                    string(credentialsId: 'telegram-api-token1',
                           variable: 'TOKEN'),

                    string(credentialsId: 'telegram-id-1',
                           variable: 'CHAT_ID')
                ]) {

                    withEnv(["MESSAGE=${message}"]) {
                        sh '''
                            curl --fail -sS -X POST \
                            "https://api.telegram.org/bot${TOKEN}/sendMessage" \
                            --data-urlencode "chat_id=${CHAT_ID}" \
                            --data-urlencode "text=${MESSAGE}"
                        '''
                    }
                }
            }
        }
    }
}
