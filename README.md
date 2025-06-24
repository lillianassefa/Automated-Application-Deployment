# Automated Application Deployment

A complete Infrastructure as Code (IaC) solution for automated deployment of Flask applications using Terraform and Ansible.

##  Architecture

This project implements a modern CI/CD pipeline using:
- **Terraform**: Infrastructure provisioning on AWS
- **Ansible**: Application deployment and configuration management
- **Flask**: Python web application
- **Nginx**: Reverse proxy and web server
- **Gunicorn**: WSGI server for production deployment

## 📁 Project Structure

```
Automated-Application-Deployment/
├── terraform/
│   └── main.tf                 # Terraform infrastructure configuration
├── ansible/
│   ├── inventory.yml           # Ansible inventory with host definitions
│   └── deploy_flask_app.yml    # Ansible playbook for Flask deployment
└── README.md                   # Project documentation
```

##  Quick Start

### Prerequisites

1. **AWS CLI** configured with appropriate credentials
2. **Terraform** (version >= 1.0)
3. **Ansible** (version >= 2.9)
4. **SSH key pair** for EC2 instance access

### Step 1: Infrastructure Provisioning

```bash
# Navigate to terraform directory
cd terraform

# Initialize Terraform
terraform init

# Plan the infrastructure
terraform plan

# Apply the infrastructure
terraform apply
```

### Step 2: Application Deployment

```bash
# Navigate to ansible directory
cd ../ansible

# Set the server IP (replace with your EC2 instance IP)
export APP_SERVER_IP=$(terraform -chdir=../terraform output -raw public_ip)

# Run the Ansible playbook
ansible-playbook -i inventory.yml deploy_flask_app.yml
```

## 📋 Configuration

### Terraform Variables

The following variables can be customized in `terraform/main.tf`:

- `aws_region`: AWS region (default: us-west-2)
- `instance_type`: EC2 instance type (default: t3.micro)
- `app_name`: Application name (default: flask-app)

### Ansible Variables

Configure the following in `ansible/inventory.yml`:

- `ansible_host`: Target server IP address
- `ansible_user`: SSH user (default: ubuntu)
- `app_port`: Application port (default: 5000)
- `app_directory`: Application installation directory (default: /opt/flask-app)

## 🔧 Features

### Infrastructure (Terraform)
- ✅ AWS EC2 instance with Ubuntu 22.04
- ✅ Security group with SSH, HTTP, and Flask app ports
- ✅ Automatic package installation via user data
- ✅ Output variables for easy integration

### Application Deployment (Ansible)
- ✅ Python virtual environment setup
- ✅ Flask application with modern UI
- ✅ Gunicorn WSGI server configuration
- ✅ Nginx reverse proxy setup
- ✅ Systemd service management
- ✅ Health checks and deployment verification

##  Accessing the Application

After successful deployment, access your Flask application at:
- **Local**: `http://localhost:5000`
- **Public**: `http://<EC2_PUBLIC_IP>:80`

##  Management Commands

### Check Application Status
```bash
# SSH into the server
ssh -i ~/.ssh/id_rsa ubuntu@<EC2_PUBLIC_IP>

# Check Flask service status
sudo systemctl status flask-app

# Check Nginx status
sudo systemctl status nginx

# View application logs
sudo journalctl -u flask-app -f
```

### Update Application
```bash
# Run the Ansible playbook again
ansible-playbook -i inventory.yml deploy_flask_app.yml
```

### Destroy Infrastructure
```bash
# Navigate to terraform directory
cd terraform

# Destroy all resources
terraform destroy
```

##  Security Considerations

- SSH access is configured for the ubuntu user
- Security group allows HTTP (80) and Flask app (5000) traffic
- Application runs behind Nginx reverse proxy
- Systemd service runs with appropriate user permissions

## 📝 Troubleshooting

### Common Issues

1. **SSH Connection Failed**
   - Verify your SSH key is in `~/.ssh/id_rsa`
   - Check security group allows SSH (port 22)
   - Ensure EC2 instance is running

2. **Application Not Accessible**
   - Check if Flask service is running: `sudo systemctl status flask-app`
   - Verify Nginx configuration: `sudo nginx -t`
   - Check firewall settings

3. **Terraform Errors**
   - Ensure AWS credentials are configured
   - Verify region and instance type availability
   - Check for any required variables

### Logs and Debugging

```bash
# Flask application logs
sudo journalctl -u flask-app -f

# Nginx logs
sudo tail -f /var/log/nginx/access.log
sudo tail -f /var/log/nginx/error.log

# System logs
sudo journalctl -xe
```

##  Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test the deployment
5. Submit a pull request

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

##  Acknowledgments

- HashiCorp for Terraform
- Red Hat for Ansible
- Flask community for the web framework
- AWS for cloud infrastructure
