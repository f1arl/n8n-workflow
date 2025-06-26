markdown
# n8n Installation from npm with Systemd Service

![n8n Logo](https://n8n.io/n8n-logo.svg)

A step-by-step guide to install n8n workflow automation tool using npm (not Docker) and configure it as a systemd service for auto-start on Linux systems.

## 📋 Table of Contents
- [Prerequisites](#-prerequisites)
- [Installation Steps](#-installation-steps)
- [Configuration](#-configuration)
- [Managing the Service](#-managing-the-service)
- [Accessing n8n](#-accessing-n8n)
- [Troubleshooting](#-troubleshooting)
- [Updating](#-updating)
- [License](#-license)

## ⚙️ Prerequisites

- Linux system with systemd
- Node.js 16.x or later
- npm (comes with Node.js)
- sudo/root access

## 🚀 Installation Steps

### 1. Install Node.js and npm

```bash
curl -fsSL https://deb.nodesource.com/setup_16.x | sudo -E bash -
sudo apt-get install -y nodejs
2. Install n8n globally

bash
sudo npm install n8n -g
3. Create dedicated system user

bash
sudo adduser --system --group --no-create-home n8n
🔧 Configuration

Create systemd service file

bash
sudo nano /etc/systemd/system/n8n.service
Paste the following configuration (adjust values as needed):

ini
[Unit]
Description=n8n workflow automation
After=network.target

[Service]
Type=simple
User=n8n
Group=n8n
ExecStart=/usr/bin/n8n
Restart=on-failure
RestartSec=10
Environment=N8N_BASIC_AUTH_ACTIVE=true
Environment=N8N_BASIC_AUTH_USER=username
Environment=N8N_BASIC_AUTH_PASSWORD=password
Environment=NODE_ENV=production
WorkingDirectory=/home/n8n/
Environment=TZ=America/New_York

[Install]
WantedBy=multi-user.target
🛠️ Managing the Service

bash
# Reload systemd
sudo systemctl daemon-reload

# Enable auto-start
sudo systemctl enable n8n

# Start service
sudo systemctl start n8n

# Check status
sudo systemctl status n8n

# View logs
journalctl -u n8n -f
🌐 Accessing n8n

After installation, access the web interface at:

text
http://your-server-ip:5678
Use the basic authentication credentials you configured in the service file.

🐛 Troubleshooting

Common issues and solutions:

Port in use: Check if port 5678 is available
bash
sudo lsof -i :5678
Permission issues: Verify n8n user has proper access
bash
sudo chown -R n8n:n8n /path/to/n8n/directory
Service fails to start: Check detailed logs
bash
journalctl -xe
🔄 Updating

To update n8n to the latest version:

bash
sudo npm update n8n -g
sudo systemctl restart n8n
📜 License

This project is licensed under the MIT License.

Note: For production use, consider adding:

Reverse proxy (Nginx/Apache)
SSL certificates
Database configuration
Regular backups
text

This README.md includes:
- GitHub-flavored markdown formatting
- Clear section headers with emojis
- Code blocks with proper syntax highlighting
- Organized table of contents
- Visual separation of sections
- Important notes in highlighted boxes
- License information
- Considerations for production use
