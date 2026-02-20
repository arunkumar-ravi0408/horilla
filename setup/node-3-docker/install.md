# Node 3: Docker Node Installation

Connect to your EC2-3 instance and follow these steps:

### 1. Update System and Install Docker
```bash
sudo apt update -y
sudo apt install docker.io -y
```

### 2. Start and Enable Docker Service
```bash
sudo systemctl start docker
sudo systemctl enable docker
```

### 3. Add User to Docker Group
```bash
sudo usermod -aG docker $USER
# Logout and login back for changes to take effect
```

### 4. Install Trivy (Security Scanner)
```bash
wget https://github.com/aquasecurity/trivy/releases/download/v0.44.1/trivy_0.44.1_Linux-64bit.deb
sudo dpkg -i trivy_0.44.1_Linux-64bit.deb
```

### 5. Verify Installation
```bash
docker --version
trivy --version
```
