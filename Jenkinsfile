pipeline {
    agent any

    environment {
        DOCKERHUB_NAMESPACE = 'yeshfaandleeb01'
        IMAGE_NAME = 'greenx-app'
        TARGET_HOST = '3.239.118.152'
        APP_PORT = '8000'
    }

    stages {
        stage('Checkout') {
            steps {
                sshagent(['github-ssh']) {
                    checkout([$class: 'GitSCM',
                        branches: [[name: '*/main']],
                        userRemoteConfigs: [[
                            url: "git@github.com:yeshfaandleeb1/docker-task.git",
                            credentialsId: 'github-ssh'
                        ]]
                    ])
                }
            }
        }

        stage('Build and Push Docker Image') {
            steps {
                script {
                    def tag = "${DOCKERHUB_NAMESPACE}/${IMAGE_NAME}:${BUILD_NUMBER}"
                    sh "docker build -t ${tag} ."

                    withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DH_USER', passwordVariable: 'DH_PASS')]) {
                        sh """
                            echo "$DH_PASS" | docker login -u "$DH_USER" --password-stdin
                            docker push ${tag}
                            docker tag ${tag} ${DOCKERHUB_NAMESPACE}/${IMAGE_NAME}:latest
                            docker push ${DOCKERHUB_NAMESPACE}/${IMAGE_NAME}:latest
                            docker logout
                        """
                    }
                }
            }
        }

        stage('Deploy to Target Server') {
            steps {
                sshagent(['target-ssh']) {
                    sh """
                        ssh -o StrictHostKeyChecking=no ubuntu@${TARGET_HOST} '
                            docker pull ${DOCKERHUB_NAMESPACE}/${IMAGE_NAME}:latest &&
                            docker rm -f ${IMAGE_NAME} || true &&
                            docker run -d --name ${IMAGE_NAME} -p ${APP_PORT}:${APP_PORT} ${DOCKERHUB_NAMESPACE}/${IMAGE_NAME}:latest
                        '
                    """
                }
            }
        }
    }

    post {
        success {
            echo "✅ Deployment succeeded!"
        }
        failure {
            echo "❌ Deployment failed!"
        }
    }
}
