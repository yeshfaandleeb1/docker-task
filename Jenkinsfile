pipeline {
    agent any

    options {
        timestamps()
    }

    environment {
        BACKEND_DIR = "GreenX_DCS_Assesment_Tool_Backend"
        FRONTEND_DIR = "greenX-assessment-tool-frontend"
        LOG_FILE = "pipeline_log_${env.BUILD_NUMBER}.txt"
        START_TIME = ""
    }

    stages {

        stage('Start Timer') {
            steps {
                script {
                    START_TIME = System.currentTimeMillis()
                }
            }
        }

        stage('Clean Workspace') {
            steps {
                echo '🧹 Cleaning workspace...'
                cleanWs()
            }
        }

        stage('Backend - Build Docker Image') {
            steps {
                echo '📦 Building Backend Image...'
                dir("${BACKEND_DIR}") {
                    sh 'docker build -t greenx-backend:latest .'
                }
            }
        }

        stage('Frontend - Build Docker Image') {
            steps {
                echo '🌐 Building Frontend Image...'
                dir("${FRONTEND_DIR}") {
                    sh 'docker build -t greenx-frontend:latest .'
                }
            }
        }

        stage('List Images & Save Report') {
            steps {
                echo '📋 Collecting Docker image size report...'
                sh '''
                    echo "===== DOCKER IMAGE REPORT =====" > report.txt
                    docker images | tee -a report.txt
                '''
            }
        }

        stage('Save Console Log') {
            steps {
                script {
                    def log = currentBuild.rawBuild.getLog(100000)
                    writeFile file: LOG_FILE, text: log.join("\\n")
                }
                echo "📄 Log file saved: ${LOG_FILE}"
            }
        }
    }

    post {

        success {
            script {
                def end = System.currentTimeMillis()
                def duration = (end - START_TIME) / 1000

                emailext(
                    subject: "SUCCESS ✔️: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                    to: "yeshfaandleeb05@gmail.com",
                    body: """
<h2 style='color:green;'>🎉 BUILD SUCCESSFUL</h2>
<b>Job:</b> ${env.JOB_NAME}<br>
<b>Build #:</b> ${env.BUILD_NUMBER}<br>
<b>Duration:</b> ${duration} seconds<br><br>

<b>Docker Images:</b><br>
✔ greenx-backend:latest<br>
✔ greenx-frontend:latest<br><br>

<b>Attached:</b><br>
✔ Full Console Log<br>
✔ Docker Image Report<br>
                    """,
                    mimeType: 'text/html',
                    attachmentsPattern: "report.txt, ${LOG_FILE}"
                )
            }
        }

        failure {
            script {
                def end = System.currentTimeMillis()
                def duration = (end - START_TIME) / 1000

                emailext(
                    subject: "FAILED ❌: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                    to: "yeshfaandleeb05@gmail.com",
                    body: """
<h2 style='color:red;'>❌ BUILD FAILED</h2>
<b>Job:</b> ${env.JOB_NAME}<br>
<b>Build #:</b> ${env.BUILD_NUMBER}<br>
<b>Duration:</b> ${duration} seconds<br><br>

<b>Attached:</b><br>
✔ Full Console Log<br>
                    """,
                    mimeType: 'text/html',
                    attachmentsPattern: "${LOG_FILE}"
                )
            }
        }

        always {
            echo "📧 Email Notification Stage Completed"
        }
    }
}
