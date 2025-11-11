pipeline {
    agent any

    environment {
        BACKEND_PATH = "GreenX_DCS_Assesment_Tool-main/GreenX_DCS_Assesment_Tool_Backend"
        FRONTEND_PATH = "GreenX_DCS_Assesment_Tool-main/greenx-assessment-tool-frontend"
        BACKEND_IMAGE = "greenx-backend:latest"
        FRONTEND_IMAGE = "greenx-frontend:latest"
        EMAIL_RECIPIENT = "yeshfaandleeb05@gmail.com"
    }

    stages {
        stage('Clean Workspace') {
            steps {
                echo "🧹 Cleaning workspace..."
                cleanWs()
            }
        }

        stage('Build Backend Image') {
            steps {
                echo "🐍 Building Backend Docker image..."
                sh '''
                    docker build -t ${BACKEND_IMAGE} \
                    -f ${BACKEND_PATH}/Dockerfile ${BACKEND_PATH}
                '''
            }
        }

        stage('Build Frontend Image') {
            steps {
                echo "🌐 Building Frontend Docker image..."
                sh '''
                    docker build -t ${FRONTEND_IMAGE} \
                    -f ${FRONTEND_PATH}/Dockerfile ${FRONTEND_PATH}
                '''
            }
        }

        stage('List Docker Images') {
            steps {
                echo "📦 Listing Docker images..."
                sh 'docker images'
            }
        }
    }

    post {
        success {
            echo "✅ Build successful."
            emailext (
                subject: "✅ Jenkins Build Success: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """
🎉 Jenkins Build Successful!

Project: ${env.JOB_NAME}
Build Number: ${env.BUILD_NUMBER}
Status: SUCCESS ✅
URL: ${env.BUILD_URL}
                """,
                to: "${EMAIL_RECIPIENT}"
            )
        }

        failure {
            echo "❌ Build failed."
            emailext (
                subject: "❌ Jenkins Build Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """
⚠️ Jenkins Build Failed!

Project: ${env.JOB_NAME}
Build Number: ${env.BUILD_NUMBER}
Status: FAILED ❌
URL: ${env.BUILD_URL}
                """,
                to: "${EMAIL_RECIPIENT}"
            )
        }
    }
}
