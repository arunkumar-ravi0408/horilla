# Node 2: Django VM Installation (Classic Deployment)

Connect to your EC2-2 instance and follow these steps:

### 1. Update System and Install Dependencies
```bash
sudo apt update -y
sudo apt install python3-pip python3-venv nginx -y
```

### 2. Set Up Virtual Environment
```bash
python3 -m venv venv
source venv/bin/activate
```

### 3. Install Django and Gunicorn
```bash
pip install django gunicorn
# Note: Further project-specific dependencies will be installed via requirements.txt
```

### Phase 2: Gunicorn Configuration
- [x] Create Gunicorn socket unit file
- [x] Create Gunicorn service unit file
- [x] Start and enable Gunicorn
- For now, ensure the system is ready for the deployment.

### Phase 3: Nginx Configuration
- [x] Create Nginx server block
- [x] Enable Nginx configuration
- [x] Restart Nginx and verify deployment
