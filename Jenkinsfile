pipeline {
    agent any

    options {
        timestamps()
    }

    environment {
        REPO_URL = 'https://github.com/yeshfaandleeb1/docker-task.git'
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

        stage('Checkout Code') {
            steps {
                echo '📦 Checking out repository...'
                checkout([$class: 'GitSCM',
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[
                        url: "${REPO_URL}",
                        credentialsId: 'yeshfaandleeb05@gmail.com'
                    ]]
                ])
            }
        }

        stage('Build Backend Image') {
            steps {
                echo '🐍 Building Backend Docker image...'
                sh '''
                    docker build -t greenx-backend:latest \
                    -f GreenX_DCS_Assesment_Tool-main/GreenX_DCS_Assesment_Tool_Backend/Dockerfile \
                    GreenX_DCS_Assesment_Tool-main/GreenX_DCS_Assesment_Tool_Backend
                '''
            }
        }

        stage('Build Frontend Image') {
            steps {
                echo '🌐 Building Frontend Docker image...'
                sh '''
                    docker build -t greenx-frontend:latest \
                    -f GreenX_DCS_Assesment_Tool-main/greenX-assessment-tool-frontend/Dockerfile \
                    GreenX_DCS_Assesment_Tool-main/greenX-assessment-tool-frontend
                '''
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
                    def log = currentBuild.rawBuild.getLog(100000) // capture ALL logs
                    writeFile file: LOG_FILE, text: log.join("\n")
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
                    subject: "SUCCESS: Jenkins Pipeline - ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                    to: "yeshfaandleeb05@gmail.com",
                    body: """
🎉 **BUILD SUCCESSFUL**

**Job:** ${env.JOB_NAME}  
**Build #:** ${env.BUILD_NUMBER}  
**Duration:** ${duration} seconds  

**Docker Images Built:**
- greenx-backend:latest  
- greenx-frontend:latest  

Attached:
✔ Full console log  
✔ Docker image size report  

""",
                    attachmentsPattern: "report.txt, ${LOG_FILE}"
                )
            }
        }

        failure {
            script {
                def end = System.currentTimeMillis()
                def duration = (end - START_TIME) / 1000

                emailext(
                    subject: "FAILED: Jenkins Pipeline - ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                    to: "yeshfaandleeb05@gmail.com",
                    body: """
❌ **BUILD FAILED**

**Job:** ${env.JOB_NAME}  
**Build #:** ${env.BUILD_NUMBER}  
**Duration:** ${duration} seconds  

Attached:
✔ Full console log  
""",
                    attachmentsPattern: "${LOG_FILE}"
                )
            }
        }

        always {
            echo "📧 Email Notification Completed"
        }
    }
}
