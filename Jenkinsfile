pipeline {
    agent none // We specify agents per stage

    environment {
        DOCKER_IMAGE = "arunkumarravi08/horilla-app"
        DOCKER_HUB_CREDS = credentials('docker-hub-creds')
    }

    stages {
        stage('Checkout') {
            agent { label 'built-in' } // Run checkout on controller
            steps {
                checkout scm
            }
        }

        stage('Security Scan (Trivy)') {
            agent { label 'docker-node' } // Run on Node 3
            steps {
                script {
                    echo "Running Security Scan on Node 3 (Worker Node)..."
                    sh 'trivy fs . --severity HIGH,CRITICAL'
                }
            }
        }

        stage('Build Docker Image') {
            agent { label 'docker-node' } // Run on Node 3
            steps {
                script {
                    echo "Building Docker Image on Node 3..."
                    sh "docker build -t ${DOCKER_IMAGE}:${BUILD_NUMBER} ."
                    sh "docker build -t ${DOCKER_IMAGE}:latest ."
                }
            }
        }

        stage('Push to Docker Hub') {
            agent { label 'docker-node' } // Run on Node 3
            steps {
                script {
                    echo "Logging into Docker Hub and Pushing Image from Node 3..."
                    sh "echo ${DOCKER_HUB_CREDS_PSW} | docker login -u ${DOCKER_HUB_CREDS_USR} --password-stdin"
                    sh "docker push ${DOCKER_IMAGE}:${BUILD_NUMBER}"
                    sh "docker push ${DOCKER_IMAGE}:latest"
                }
            }
        }

        stage('Deploy to EKS') {
            agent { label 'docker-node' } 
            steps {
                script {
                    echo "Deploying to EKS..."
                    // Note: Jenkins needs AWS credentials configured or an IAM role attached to the EC2 instance
                    sh "aws eks update-kubeconfig --region us-east-1 --name ${env.CLUSTER_NAME}" 
                    sh "kubectl apply -f k8s-manifests/deployment.yaml"
                    sh "kubectl rollout status deployment/horilla-app"
                }
            }
        }
    }

    post {
        always {
            echo "Pipeline finished."
        }
        success {
            echo "Build and Image Push successful!"
        }
        failure {
            echo "Pipeline failed. Check stage logs."
        }
    }
}

