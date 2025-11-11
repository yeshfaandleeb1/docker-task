pipeline {
    agent any

    environment {
        BACKEND_PATH = "docker-task/GreenX_DCS_Assesment_Tool-main/GreenX_DCS_Assesment_Tool_Backend"
        FRONTEND_PATH = "docker-task/GreenX_DCS_Assesment_Tool-main/greenx-assessment-tool-frontend"
        BACKEND_IMAGE = "greenx-backend:latest"
        FRONTEND_IMAGE = "greenx-frontend:latest"
        EMAIL_RECIPIENT = "yeshfaandleeb05@gmail.com"
    }

    stages {
        stage('Checkout') {
            steps {
                echo "📦 Checking out code..."
                checkout scm
                sh 'echo "Workspace after checkout:" && ls -R | grep Dockerfile || true'
            }
        }

        stage('Build Backend Image') {
            steps {
                echo "🐍 Building Backend Docker image..."
                sh '''
                    echo "Building from path: ${BACKEND_PATH}"
                    docker build -t ${BACKEND_IMAGE} -f ${BACKEND_PATH}/Dockerfile ${BACKEND_PATH}
                '''
            }
        }

        stage('Build Frontend Image') {
            steps {
                echo "🌐 Building Frontend Docker image..."
                sh '''
                    docker build -t ${FRONTEND_IMAGE} -f ${FRONTEND_PATH}/Dockerfile ${FRONTEND_PATH}
                '''
            }
        }

        stage('List Docker Images') {
            steps {
                sh 'docker images'
            }
        }
    }

    post {
        success {
            echo "✅ Build successful."
            emailext(
                subject: "✅ Jenkins Build Success: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: "🎉 Build Successful!\\nJob: ${env.JOB_NAME}\\nBuild: ${env.BUILD_NUMBER}\\nURL: ${env.BUILD_URL}",
                to: "${EMAIL_RECIPIENT}"
            )
        }

        failure {
            echo "❌ Build failed."
            emailext(
                subject: "❌ Jenkins Build Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: "⚠️ Build Failed!\\nJob: ${env.JOB_NAME}\\nBuild: ${env.BUILD_NUMBER}\\nURL: ${env.BUILD_URL}",
                to: "${EMAIL_RECIPIENT}"
            )
        }
    }
}
