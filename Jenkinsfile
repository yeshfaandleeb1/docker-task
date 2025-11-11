pipeline {
    agent any

    environment {
        BACKEND_PATH = "GreenX_DCS_Assesment_Tool-main/GreenX_DCS_Assesment_Tool_Backend"
        FRONTEND_PATH = "GreenX_DCS_Assesment_Tool-main/GreenX_DCS_Assesment_Tool_Frontend"
        BACKEND_IMAGE = "greenx-backend:latest"
        FRONTEND_IMAGE = "greenx-frontend:latest"
    }

    stages {
        stage('Clean Workspace') {
            steps {
                cleanWs()
                echo '🧹 Workspace cleaned.'
            }
        }

        stage('Build Backend Image') {
            steps {
                dir("${BACKEND_PATH}") {
                    echo '🐍 Building Backend Docker image...'
                    sh 'docker build -t ${BACKEND_IMAGE} .'
                }
            }
        }

        stage('Build Frontend Image') {
            steps {
                dir("${FRONTEND_PATH}") {
                    echo '⚙️ Building Frontend Docker image...'
                    sh 'docker build -t ${FRONTEND_IMAGE} .'
                }
            }
        }

        stage('List Docker Images') {
            steps {
                echo '📦 Listing all Docker images...'
                sh 'docker images'
            }
        }
    }

    post {
        success {
            echo '✅ Build completed successfully!'
            emailext (
                subject: "✅ Jenkins Build SUCCESS: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """
🎉 Build Successful!

Job: ${env.JOB_NAME}
Build Number: ${env.BUILD_NUMBER}
Build URL: ${env.BUILD_URL}
            """,
                to: "yeshfaandleeb05@gmail.com"
            )
        }

        failure {
            echo '❌ Build failed.'
            emailext (
                subject: "❌ Jenkins Build FAILED: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """
🚨 Build Failed!

Job: ${env.JOB_NAME}
Build Number: ${env.BUILD_NUMBER}
Logs: ${env.BUILD_URL}
            """,
                to: "yeshfaandleeb05@gmail.com"
            )
        }
    }
}
