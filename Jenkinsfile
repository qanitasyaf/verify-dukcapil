pipeline {
    agent any
    tools {
        maven 'Maven 3.9.10'
        jdk 'Temurin JDK 21'
    }

    environment {
        FRONTEND_IMAGE_NAME = 'frontend-secure-onboarding-system'
        GCP_PROJECT_ID = 'model-parsec-465503-p3'
        GCR_HOSTNAME = 'gcr.io'
        GCR_KEY_FILE_PATH = ''
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/alvarolt17/frontend-secure-onboarding-system', branch: 'feature/docker-support'
            }
        }

        stage('Docker Build and Push Frontend') {
            steps {
                script {
                    def imageTag = "${env.BUILD_NUMBER}"
                    def fullImageName = "${GCR_HOSTNAME}/${GCP_PROJECT_ID}/${FRONTEND_IMAGE_NAME}:${imageTag}"

                    echo "Membangun Docker image Frontend: ${fullImageName}"

                    withCredentials([file(credentialsId: 'gcr-credential-id', variable: 'GCR_KEY_FILE_PATH')]) {
                        sh "docker login -u _json_key --password-stdin https://${GCR_HOSTNAME} < ${GCR_KEY_FILE_PATH}"
                        sh "docker build -t ${fullImageName} -f Dockerfile ."
                        sh "docker push ${fullImageName}"

                        echo "Docker image Frontend ${fullImageName} berhasil dibangun dan didorong ke GCR."
                    }

                    env.BACKEND_DEPLOY_IMAGE = fullImageName
                }
            }
        }
    }

    post {
        success {
            echo "Pipeline Docker Build berhasil 🚀"
        }
        failure {
            echo "Pipeline Docker Build gagal 💥"
        }
        always {
            cleanWs()
        }
    }
}