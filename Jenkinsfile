pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "your-dockerhub-username/horilla-app"
        DOCKER_HUB_CREDS = credentials('docker-hub-creds')
    }

    stages {
        stage('Checkout') {
            steps {
                // This stage pulls the code from your repository
                checkout scm
            }
        }

        stage('Security Scan (Trivy)') {
            steps {
                script {
                    echo "Running Security Scan on Node 3..."
                    // We will trigger this on Node 3 in the next step
                    sh 'trivy fs . --severity HIGH,CRITICAL'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo "Building Docker Image..."
                    sh "docker build -t ${DOCKER_IMAGE}:${BUILD_NUMBER} ."
                    sh "docker build -t ${DOCKER_IMAGE}:latest ."
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    echo "Logging into Docker Hub and Pushing Image..."
                    sh "echo ${DOCKER_HUB_CREDS_PSW} | docker login -u ${DOCKER_HUB_CREDS_USR} --password-stdin"
                    sh "docker push ${DOCKER_IMAGE}:${BUILD_NUMBER}"
                    sh "docker push ${DOCKER_IMAGE}:latest"
                }
            }
        }
    }

    post {
        always {
            echo "Pipeline finished."
        }
        success {
            echo "Build and Deployment successful!"
        }
        failure {
            echo "Pipeline failed. Check logs."
        }
    }
}
