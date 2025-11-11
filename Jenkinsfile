pipeline {
    agent any

    environment {
        BACKEND_PATH = "GreenX_DCS_Assesment_Tool-main/GreenX_DCS_Assesment_Tool_Backend"
        FRONTEND_PATH = "GreenX_DCS_Assesment_Tool-main/greenX-assessment-tool-frontend"
        RECIPIENT = "yeshfaandleeb05@gmail.com"
    }

    stages {
        stage('Clean Workspace') {
            steps {
                echo "🧹 Cleaning workspace..."
                cleanWs()
            }
        }

        stage('Checkout Code') {
            steps {
                echo "📦 Checking out repository..."
                checkout scm
                sh 'ls -R | grep Dockerfile || true'
            }
        }

        stage('Build Backend Image') {
            steps {
                echo "🐍 Building Backend Docker image..."
                sh '''
                    echo "Current Directory: $(pwd)"
                    docker build -t greenx-backend:latest \
                    -f ${WORKSPACE}/${BACKEND_PATH}/Dockerfile \
                    ${WORKSPACE}/${BACKEND_PATH}
                '''
            }
        }

        stage('Build Frontend Image') {
            steps {
                echo "🌐 Building Frontend Docker image..."
                sh '''
                    docker build -t greenx-frontend:latest \
                    -f ${WORKSPACE}/${FRONTEND_PATH}/Dockerfile \
                    ${WORKSPACE}/${FRONTEND_PATH}
                '''
            }
        }

        stage('List Docker Images') {
            steps {
                echo "📋 Listing Docker images..."
                sh 'docker images'
            }
        }
    }

    post {
        success {
            echo "✅ Build succeeded!"
            emailext(
                to: "${RECIPIENT}",
                subject: "✅ Jenkins Build Success: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """
                Hi,

                The Jenkins build for '${env.JOB_NAME}' completed successfully.

                Build URL: ${env.BUILD_URL}

                Regards,
                Jenkins Automation
                """
            )
        }

        failure {
            echo "❌ Build failed!"
            emailext(
                to: "${RECIPIENT}",
                subject: "❌ Jenkins Build Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """
                Hi,

                The Jenkins build for '${env.JOB_NAME}' failed.

                Build URL: ${env.BUILD_URL}

                Please review the logs and fix the issue.

                Regards,
                Jenkins Automation
                """
            )
        }
    }
}
