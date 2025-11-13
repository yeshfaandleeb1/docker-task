pipeline {
    agent any

    environment {
        BACKEND_DIR = "GreenX_DCS_Assesment_Tool_Backend"
        FRONTEND_DIR = "greenX-assessment-tool-frontend"
    }

    stages {

        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Backend - Build Docker Image') {
            steps {
                dir("${BACKEND_DIR}") {
                    sh 'docker build -t greenx-backend .'
                }
            }
        }

        stage('Frontend - Build Docker Image') {
            steps {
                dir("${FRONTEND_DIR}") {
                    sh 'docker build -t greenx-frontend .'
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
            emailext (
                subject: "BUILD SUCCESSFUL ✔️: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """
                <h2 style='color:green;'>Build Successful 🎉</h2>
                <p><b>Job:</b> ${env.JOB_NAME}</p>
                <p><b>Build Number:</b> ${env.BUILD_NUMBER}</p>
                <p>✔️ Backend & Frontend Docker Images Built Successfully.</p>
                <p>🔗 <a href='${env.BUILD_URL}'>Click here to view console logs</a></p>
                """,
                mimeType: 'text/html',
                to: "yeshfaandleeb05@gmail.com"
            )
        }

        failure {
            emailext (
                subject: "BUILD FAILED ❌: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """
                <h2 style='color:red;'>Build Failed ❌</h2>
                <p><b>Job:</b> ${env.JOB_NAME}</p>
                <p><b>Build Number:</b> ${env.BUILD_NUMBER}</p>
                <p>⚠️ Something went wrong during Docker build.</p>
                <p>🔗 <a href='${env.BUILD_URL}'>Click here to view console logs</a></p>
                """,
                mimeType: 'text/html',
                to: "yeshfaandleeb05@gmail.com"
            )
        }
    }
}
