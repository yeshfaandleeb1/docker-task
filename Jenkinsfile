pipeline {
    agent any

    stages {

        stage('Clean Workspace') {
            steps {
                cleanWs()
                echo "🧹 Workspace cleaned."
            }
        }

        stage('Build Backend Image') {
            steps {
                dir('GreenX_DCS_Assesment_Tool-main/GreenX_DCS_Assesment_Tool_Backend') {
                    echo "🐍 Building Backend Docker image..."
                    sh '''
                    docker build -t greenx-backend:latest .
                    '''
                }
            }
        }

        stage('Build Frontend Image') {
            steps {
                dir('GreenX_DCS_Assesment_Tool-main/greenX-assessment-tool-frontend') {
                    echo "🧱 Building Frontend Docker image..."
                    sh '''
                    docker build -t greenx-frontend:latest .
                    '''
                }
            }
        }

        stage('List Docker Images') {
            steps {
                echo "📦 Listing all Docker images..."
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
