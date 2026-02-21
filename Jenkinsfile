pipeline {
    agent { label 'docker' } // Runs on Node 2 (Docker Agent)
    
    environment {
        DOCKERHUB_CREDENTIALS_ID = 'docker-hub-cred'
        DOCKERHUB_REPO = 'arunkumarravi08/horilla' // Updated to match your Docker Hub username
        SONARQUBE_SERVER = 'SonarQube'
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('SonarQube Analysis') {
            steps {
                script {
                    def scannerHome = tool 'SonarQube Scanner'
                    withSonarQubeEnv("${SONARQUBE_SERVER}") {
                        sh "${scannerHome}/bin/sonar-scanner \
                        -Dsonar.projectKey=horilla \
                        -Dsonar.sources=."
                    }
                }
            }
        }
        
        stage('Docker Build') {
            steps {
                sh "docker build -t ${DOCKERHUB_REPO}:latest -t ${DOCKERHUB_REPO}:${BUILD_NUMBER} ."
            }
        }
        
        stage('Trivy Security Scan') {
            steps {
                sh "trivy image --severity HIGH,CRITICAL --format table ${DOCKERHUB_REPO}:latest"
            }
        }
        
        stage('Docker Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: "${DOCKERHUB_CREDENTIALS_ID}", passwordVariable: 'DOCKERHUB_PASSWORD', usernameVariable: 'DOCKERHUB_USERNAME')]) {
                    sh "echo ${DOCKERHUB_PASSWORD} | docker login -u ${DOCKERHUB_USERNAME} --password-stdin"
                    sh "docker push ${DOCKERHUB_REPO}:latest"
                    sh "docker push ${DOCKERHUB_REPO}:${BUILD_NUMBER}"
                }
            }
        }
        
        stage('Trigger Deployment (Ansible)') {
            steps {
                echo "Triggering Ansible on Node 3..."
                // Logic to trigger Node 3 (e.g. via ssh or remote trigger)
            }
        }
    }
}


        stage('Trigger Deployment (Ansible)') {
            steps {
                sshagent(['docker-agent-cred']) {
                    sh "ssh -o StrictHostKeyChecking=no ubuntu@10.10.1.82 'ansible-playbook -i inventory.ini deploy_horilla.yml --extra-vars \"db_url=postgres://postgres:postgres@db:5432/horilla\"'"
                }
            }
        }
