# Node 4: Terraform + Ansible Installation

Connect to your EC2-4 instance and follow these steps:

### 1. Update System and Install Ansible
```bash
sudo apt update -y
sudo apt install software-properties-common -y
sudo add-apt-repository --yes --update ppa:ansible/ansible
sudo apt install ansible -y
```

### 2. Install HashiCorp Key and Repo for Terraform
```bash
wget -O- https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
```

### 3. Install Terraform
```bash
sudo apt update -y
sudo apt install terraform -y
```

### 4. Install AWS CLI
```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
sudo apt install unzip -y
unzip awscliv2.zip
sudo ./aws/install
```

### 5. Verify Tool Versions
```bash
ansible --version
terraform --version
aws --version
```
