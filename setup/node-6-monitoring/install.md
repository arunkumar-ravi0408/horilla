# Node 6: Monitoring & Observability Installation

Connect to your EC2-6 instance and follow these steps:

### 1. Install Prometheus
```bash
sudo apt update -y
sudo apt install prometheus -y
```

### 2. Install Grafana
```bash
sudo apt install -y apt-transport-https software-properties-common wget
wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -
sudo add-apt-repository "deb https://packages.grafana.com/oss/release/$(lsb_release -cs) main"
sudo apt update && sudo apt install grafana -y
```

### 3. Enable and Start Services
```bash
sudo systemctl enable prometheus --now
sudo systemctl enable grafana-server --now
```

### 4. Access Prometheus
- Open `http://<ec2-6-public-ip>:9090`
- Verify that the service is running and collecting local metrics.

### 5. Access Grafana
- Open `http://<ec2-6-public-ip>:3000`
- Login with `admin/admin` and set a new password.
- Add Prometheus as a Data Source.
