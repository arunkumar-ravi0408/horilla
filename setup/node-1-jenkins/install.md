# Node 1: Jenkins Orchestrator Installation

Connect to your EC2-1 instance and follow these steps:

### 1. Update System and Install Java
```bash
sudo apt update -y
sudo apt install openjdk-17-jdk -y
```

### 2. Add Jenkins Repository and Key
```bash
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
```

### 3. Install Jenkins
```bash
sudo apt update -y
sudo apt install jenkins -y
```

### 4. Start and Enable Jenkins
```bash
sudo systemctl start jenkins
sudo systemctl enable jenkins
```

### 5. Access Jenkins
- Open your browser and navigate to `http://<ec2-1-public-ip>:8080`.
- Retrieve the initial admin password using:
  ```bash
  sudo cat /var/lib/jenkins/secrets/initialAdminPassword
  ```
