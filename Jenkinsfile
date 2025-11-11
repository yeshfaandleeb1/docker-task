pipeline {
    agent any

    environment {
        DOCKERHUB_NAMESPACE = 'yeshfaandleeb01'
        BACKEND_IMAGE = 'greenx-backend'
        FRONTEND_IMAGE = 'greenx-frontend'
        TARGET_HOST = '3.239.118.152'
    }

    stages {
        stage('Checkout') {
            steps {
                sshagent(['github-ssh']) {
                    checkout([$class: 'GitSCM',
                        branches: [[name: '*/main']],
                        userRemoteConfigs: [[
                            url: 'git@github.com:yeshfaandleeb1/docker-task.git',
                            credentialsId: 'github-ssh'
                        ]]
                    ])
                }
            }
        }

        stage('Build & Push Backend') {
            steps {
                dir('GreenX_DCS_Assessment_Tool-main/GreenX_DCS_Assessment_Tool_Backend') {
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DH_USER', passwordVariable: 'DH_PASS')]) {
                        sh '''
                            docker build -t ${DOCKERHUB_NAMESPACE}/${BACKEND_IMAGE}:${BUILD_NUMBER} .
                            echo "$DH_PASS" | docker login -u "$DH_USER" --password-stdin
                            docker push ${DOCKERHUB_NAMESPACE}/${BACKEND_IMAGE}:${BUILD_NUMBER}
                            docker tag ${DOCKERHUB_NAMESPACE}/${BACKEND_IMAGE}:${BUILD_NUMBER} ${DOCKERHUB_NAMESPACE}/${BACKEND_IMAGE}:latest
                            docker push ${DOCKERHUB_NAMESPACE}/${BACKEND_IMAGE}:latest
                            docker logout
                        '''
                    }
                }
            }
        }

        stage('Build & Push Frontend') {
            steps {
                dir('GreenX_DCS_Assessment_Tool-main/greenX-assessment-tool-frontend') {
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DH_USER', passwordVariable: 'DH_PASS')]) {
                        sh '''
                            docker build -t ${DOCKERHUB_NAMESPACE}/${FRONTEND_IMAGE}:${BUILD_NUMBER} .
                            echo "$DH_PASS" | docker login -u "$DH_USER" --password-stdin
                            docker push ${DOCKERHUB_NAMESPACE}/${FRONTEND_IMAGE}:${BUILD_NUMBER}
                            docker tag ${DOCKERHUB_NAMESPACE}/${FRONTEND_IMAGE}:${BUILD_NUMBER} ${DOCKERHUB_NAMESPACE}/${FRONTEND_IMAGE}:latest
                            docker push ${DOCKERHUB_NAMESPACE}/${FRONTEND_IMAGE}:latest
                            docker logout
                        '''
                    }
                }
            }
        }

        stage('Deploy to EC2') {
            steps {
                sshagent(['target-ssh']) {
                    sh '''
                        ssh -o StrictHostKeyChecking=no ubuntu@${TARGET_HOST} "
                            docker pull ${DOCKERHUB_NAMESPACE}/${BACKEND_IMAGE}:latest &&
                            docker rm -f ${BACKEND_IMAGE} || true &&
                            docker run -d --name ${BACKEND_IMAGE} -p 8000:8000 ${DOCKERHUB_NAMESPACE}/${BACKEND_IMAGE}:latest &&
                            
                            docker pull ${DOCKERHUB_NAMESPACE}/${FRONTEND_IMAGE}:latest &&
                            docker rm -f ${FRONTEND_IMAGE} || true &&
                            docker run -d --name ${FRONTEND_IMAGE} -p 3000:3000 ${DOCKERHUB_NAMESPACE}/${FRONTEND_IMAGE}:latest
                        "
                    '''
                }
            }
        }
    }

    post {
        success { echo "✅ Build and Deployment Successful!" }
        failure { echo "❌ Pipeline Failed!" }
    }
}
