pipeline {
    agent any

    stages {
        stage('Clean Workspace') {
            steps {
                cleanWs()
                echo '🧹 Workspace cleaned.'
            }
        }

        stage('Build Backend Image') {
            steps {
                dir('docker-task/GreenX_DCS_Assesment_Tool-main/GreenX_DCS_Assesment_Tool_Backend') {
                    echo '🐍 Building Backend Docker image...'
                    sh 'docker build -t greenx-backend:latest -f Dockerfile .'
                }
            }
        }

        stage('Build Frontend Image') {
            steps {
                dir('docker-task/GreenX_DCS_Assesment_Tool-main/greenx-assessment-tool-frontend') {
                    echo '🌐 Building Frontend Docker image...'
                    sh 'docker build -t greenx-frontend:latest -f Dockerfile .'
                }
            }
        }

        stage('List Docker Images') {
            steps {
                echo '📦 Listing built Docker images...'
                sh 'docker images | grep greenx'
            }
        }
    }

    post {
        success {
            echo '✅ Build successful.'
            emailext (
                subject: "✅ Jenkins Build SUCCESS: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: "Build completed successfully.\nCheck console: ${env.BUILD_URL}",
                to: 'yeshfaandleeb05@gmail.com'
            )
        }

        failure {
            echo '❌ Build failed.'
            emailext (
                subject: "❌ Jenkins Build FAILED: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: "Build failed. Check logs: ${env.BUILD_URL}",
                to: 'yeshfaandleeb05@gmail.com'
            )
        }
    }
}
