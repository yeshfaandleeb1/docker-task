pipeline {
    agent any

    environment {
        BACKEND_PATH = "${WORKSPACE}/GreenX_DCS_Assesment_Tool-main/GreenX_DCS_Assesment_Tool_Backend"
        FRONTEND_PATH = "${WORKSPACE}/GreenX_DCS_Assesment_Tool-main/greenX-assessment-tool-frontend"
    }

    stages {
        stage('Clean Workspace') {
            steps {
                cleanWs()
                echo "🧹 Workspace cleaned."
            }
        }

        stage('Build Backend Image') {
            steps {
                dir("${BACKEND_PATH}") {
                    echo "🐍 Building Backend Docker image..."
                    sh '''
                    echo "📁 Current directory: $(pwd)"
                    ls -la
                    docker build -t greenx-backend:latest -f Dockerfile .
                    '''
                }
            }
        }

        stage('Build Frontend Image') {
            steps {
                dir("${FRONTEND_PATH}") {
                    echo "🌐 Building Frontend Docker image..."
                    sh '''
                    echo "📁 Current directory: $(pwd)"
                    ls -la
                    docker build -t greenx-frontend:latest -f Dockerfile .
                    '''
                }
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
            echo "✅ Build completed successfully!"
        }
        failure {
            echo "❌ Build failed. Please check the logs."
        }
    }
}
