pipeline {
    agent any

    environment {
        SLACK_WEBHOOK_URL = credentials('slack-webhook')
    }

    stages {
        stage('Checkout') {
            steps {
                echo "🔄 Cloning repository..."
                git url: 'https://github.com/sunitasalanke/Amazon.git', branch: 'master'
            }
        }

        stage('Build') {
            steps {
                echo '🔧 Building the application...'
                // Add your real build command here
            }
        }

        stage('Notify Success') {
            steps {
                script {
                    def message = '{"text": "✅ *Build Successful* for `Amazon` pipeline (branch: master)"}'
                    sh """
                        curl -X POST -H 'Content-type: application/json' \
                        --data '${message}' ${SLACK_WEBHOOK_URL}
                    """
                }
            }
        }
    }

    post {
        failure {
            script {
                def failMsg = '{"text": "❌ *Build Failed* for `Amazon` pipeline (branch: master). Please check logs."}'
                sh """
                    curl -X POST -H 'Content-type: application/json' \
                    --data '${failMsg}' ${SLACK_WEBHOOK_URL}
                """
            }
        }
    }
}
