pipeline {
    agent any

    environment {
        BACKEND_DIR = "GreenX_DCS_Assesment_Tool-main/GreenX_DCS_Assesment_Tool_Backend"
        FRONTEND_DIR = "GreenX_DCS_Assesment_Tool-main/greenx-assessment-tool-frontend"
    }

    stages {
        stage('Clean Workspace') {
            steps {
                cleanWs()
                echo "🧹 Workspace cleaned."
            }
        }

        stage('Checkout Code') {
            steps {
                echo "📦 Fetching latest code..."
                checkout scm
            }
        }

        stage('Build Backend Image') {
            steps {
                dir("${BACKEND_DIR}") {
                    echo "🐍 Building Backend Docker image..."
                    sh 'docker build -t greenx-backend:latest .'
                }
            }
        }

        stage('Build Frontend Image') {
            steps {
                dir("${FRONTEND_DIR}") {
                    echo "🧱 Building Frontend Docker image..."
                    sh 'docker build -t greenx-frontend:latest .'
                }
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
            echo "✅ Build completed successfully!"
        }
        failure {
            echo "❌ Build failed. Please check the logs."
        }
    }
}


