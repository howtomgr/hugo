# hugo Installation Guide

hugo is a free and open-source static site generator. Hugo provides fast static site generator with flexible themes

## Table of Contents
1. [Prerequisites](#prerequisites)
2. [Supported Operating Systems](#supported-operating-systems)
3. [Installation](#installation)
4. [Configuration](#configuration)
5. [Service Management](#service-management)
6. [Troubleshooting](#troubleshooting)
7. [Security Considerations](#security-considerations)
8. [Performance Tuning](#performance-tuning)
9. [Backup and Restore](#backup-and-restore)
10. [System Requirements](#system-requirements)
11. [Support](#support)
12. [Contributing](#contributing)
13. [License](#license)
14. [Acknowledgments](#acknowledgments)
15. [Version History](#version-history)
16. [Appendices](#appendices)

## 1. Prerequisites

- **Hardware Requirements**:
  - CPU: 1 core minimum
  - RAM: 512MB minimum
  - Storage: 1GB for sites
  - Network: Build tool
- **Operating System**: 
  - Linux: Any modern distribution (RHEL, Debian, Ubuntu, CentOS, Fedora, Arch, Alpine, openSUSE)
  - macOS: 10.14+ (Mojave or newer)
  - Windows: Windows Server 2016+ or Windows 10
  - FreeBSD: 11.0+
- **Network Requirements**:
  - Port N/A (default hugo port)
  - Dev server on 1313
- **Dependencies**:
  - See official documentation for specific requirements
- **System Access**: root or sudo privileges required


## 2. Supported Operating Systems

This guide supports installation on:
- RHEL 8/9 and derivatives (CentOS Stream, Rocky Linux, AlmaLinux)
- Debian 11/12
- Ubuntu 20.04/22.04/24.04 LTS
- Arch Linux (rolling release)
- Alpine Linux 3.18+
- openSUSE Leap 15.5+ / Tumbleweed
- SUSE Linux Enterprise Server (SLES) 15+
- macOS 12+ (Monterey and later) 
- FreeBSD 13+
- Windows 10/11/Server 2019+ (where applicable)

## 3. Installation

### RHEL/CentOS/Rocky Linux/AlmaLinux

```bash
# Install EPEL repository if needed
sudo dnf install -y epel-release

# Install hugo
sudo dnf install -y hugo

# Enable and start service
sudo systemctl enable --now hugo

# Configure firewall
sudo firewall-cmd --permanent --add-port=N/A/tcp
sudo firewall-cmd --reload

# Verify installation
hugo --version
```

### Debian/Ubuntu

```bash
# Update package index
sudo apt update

# Install hugo
sudo apt install -y hugo

# Enable and start service
sudo systemctl enable --now hugo

# Configure firewall
sudo ufw allow N/A

# Verify installation
hugo --version
```

### Arch Linux

```bash
# Install hugo
sudo pacman -S hugo

# Enable and start service
sudo systemctl enable --now hugo

# Verify installation
hugo --version
```

### Alpine Linux

```bash
# Install hugo
apk add --no-cache hugo

# Enable and start service
rc-update add hugo default
rc-service hugo start

# Verify installation
hugo --version
```

### openSUSE/SLES

```bash
# Install hugo
sudo zypper install -y hugo

# Enable and start service
sudo systemctl enable --now hugo

# Configure firewall
sudo firewall-cmd --permanent --add-port=N/A/tcp
sudo firewall-cmd --reload

# Verify installation
hugo --version
```

### macOS

```bash
# Using Homebrew
brew install hugo

# Start service
brew services start hugo

# Verify installation
hugo --version
```

### FreeBSD

```bash
# Using pkg
pkg install hugo

# Enable in rc.conf
echo 'hugo_enable="YES"' >> /etc/rc.conf

# Start service
service hugo start

# Verify installation
hugo --version
```

### Windows

```bash
# Using Chocolatey
choco install hugo

# Or using Scoop
scoop install hugo

# Verify installation
hugo --version
```

## Initial Configuration

### Basic Configuration

```bash
# Create configuration directory
sudo mkdir -p /etc/hugo

# Set up basic configuration
# See official documentation for detailed configuration options

# Test configuration
hugo --version
```

## 5. Service Management

### systemd (RHEL, Debian, Ubuntu, Arch, openSUSE)

```bash
# Enable service
sudo systemctl enable hugo

# Start service
sudo systemctl start hugo

# Stop service
sudo systemctl stop hugo

# Restart service
sudo systemctl restart hugo

# Check status
sudo systemctl status hugo

# View logs
sudo journalctl -u hugo -f
```

### OpenRC (Alpine Linux)

```bash
# Enable service
rc-update add hugo default

# Start service
rc-service hugo start

# Stop service
rc-service hugo stop

# Restart service
rc-service hugo restart

# Check status
rc-service hugo status
```

### rc.d (FreeBSD)

```bash
# Enable in /etc/rc.conf
echo 'hugo_enable="YES"' >> /etc/rc.conf

# Start service
service hugo start

# Stop service
service hugo stop

# Restart service
service hugo restart

# Check status
service hugo status
```

### launchd (macOS)

```bash
# Using Homebrew services
brew services start hugo
brew services stop hugo
brew services restart hugo

# Check status
brew services list | grep hugo
```

### Windows Service Manager

```powershell
# Start service
net start hugo

# Stop service
net stop hugo

# Using PowerShell
Start-Service hugo
Stop-Service hugo
Restart-Service hugo

# Check status
Get-Service hugo
```

## Advanced Configuration

See the official documentation for advanced configuration options.

## Reverse Proxy Setup

### nginx Configuration

```nginx
upstream hugo_backend {
    server 127.0.0.1:N/A;
}

server {
    listen 80;
    server_name hugo.example.com;
    return 301 https://$server_name$request_uri;
}

server {
    listen 443 ssl http2;
    server_name hugo.example.com;

    ssl_certificate /etc/ssl/certs/hugo.example.com.crt;
    ssl_certificate_key /etc/ssl/private/hugo.example.com.key;

    location / {
        proxy_pass http://hugo_backend;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

### Apache Configuration

```apache
<VirtualHost *:80>
    ServerName hugo.example.com
    Redirect permanent / https://hugo.example.com/
</VirtualHost>

<VirtualHost *:443>
    ServerName hugo.example.com
    
    SSLEngine on
    SSLCertificateFile /etc/ssl/certs/hugo.example.com.crt
    SSLCertificateKeyFile /etc/ssl/private/hugo.example.com.key
    
    ProxyRequests Off
    ProxyPreserveHost On
    
    ProxyPass / http://127.0.0.1:N/A/
    ProxyPassReverse / http://127.0.0.1:N/A/
</VirtualHost>
```

### HAProxy Configuration

```haproxy
frontend hugo_frontend
    bind *:80
    bind *:443 ssl crt /etc/ssl/certs/hugo.pem
    redirect scheme https if !{ ssl_fc }
    default_backend hugo_backend

backend hugo_backend
    balance roundrobin
    server hugo1 127.0.0.1:N/A check
```

## Security Configuration

### Basic Security Setup

```bash
# Set appropriate permissions
sudo chown -R hugo:hugo /etc/hugo
sudo chmod 750 /etc/hugo

# Configure firewall
sudo firewall-cmd --permanent --add-port=N/A/tcp
sudo firewall-cmd --reload

# Enable SELinux policies (if applicable)
sudo setsebool -P httpd_can_network_connect on
```

## Database Setup

See official documentation for database configuration requirements.

## Performance Optimization

### System Tuning

```bash
# Basic system tuning
echo 'net.core.somaxconn = 65535' | sudo tee -a /etc/sysctl.conf
echo 'net.ipv4.tcp_max_syn_backlog = 65535' | sudo tee -a /etc/sysctl.conf
sudo sysctl -p
```

## Monitoring

### Basic Monitoring

```bash
# Check service status
sudo systemctl status hugo

# View logs
sudo journalctl -u hugo -f

# Monitor resource usage
top -p $(pgrep hugo)
```

## 9. Backup and Restore

### Backup Script

```bash
#!/bin/bash
# Basic backup script
BACKUP_DIR="/backup/hugo"
DATE=$(date +%Y%m%d_%H%M%S)

mkdir -p "$BACKUP_DIR"
tar -czf "$BACKUP_DIR/hugo-backup-$DATE.tar.gz" /etc/hugo /var/lib/hugo

echo "Backup completed: $BACKUP_DIR/hugo-backup-$DATE.tar.gz"
```

### Restore Procedure

```bash
# Stop service
sudo systemctl stop hugo

# Restore from backup
tar -xzf /backup/hugo/hugo-backup-*.tar.gz -C /

# Start service
sudo systemctl start hugo
```

## 6. Troubleshooting

### Common Issues

1. **Service won't start**:
```bash
# Check logs
sudo journalctl -u hugo -n 100
sudo tail -f /var/log/hugo/hugo.log

# Check configuration
hugo --version

# Check permissions
ls -la /etc/hugo
```

2. **Connection issues**:
```bash
# Check if service is listening
sudo ss -tlnp | grep N/A

# Test connectivity
telnet localhost N/A

# Check firewall
sudo firewall-cmd --list-all
```

3. **Performance issues**:
```bash
# Check resource usage
top -p $(pgrep hugo)

# Check disk I/O
iotop -p $(pgrep hugo)

# Check connections
ss -an | grep N/A
```

## Integration Examples

### Docker Compose Example

```yaml
version: '3.8'
services:
  hugo:
    image: hugo:latest
    ports:
      - "N/A:N/A"
    volumes:
      - ./config:/etc/hugo
      - ./data:/var/lib/hugo
    restart: unless-stopped
```

## Maintenance

### Update Procedures

```bash
# RHEL/CentOS/Rocky/AlmaLinux
sudo dnf update hugo

# Debian/Ubuntu
sudo apt update && sudo apt upgrade hugo

# Arch Linux
sudo pacman -Syu hugo

# Alpine Linux
apk update && apk upgrade hugo

# openSUSE
sudo zypper update hugo

# FreeBSD
pkg update && pkg upgrade hugo

# Always backup before updates
tar -czf /backup/hugo-pre-update-$(date +%Y%m%d).tar.gz /etc/hugo

# Restart after updates
sudo systemctl restart hugo
```

### Regular Maintenance

```bash
# Log rotation
sudo logrotate -f /etc/logrotate.d/hugo

# Clean old logs
find /var/log/hugo -name "*.log" -mtime +30 -delete

# Check disk usage
du -sh /var/lib/hugo
```

## Additional Resources

- Official Documentation: https://docs.hugo.org/
- GitHub Repository: https://github.com/hugo/hugo
- Community Forum: https://forum.hugo.org/
- Best Practices Guide: https://docs.hugo.org/best-practices

---

**Note:** This guide is part of the [HowToMgr](https://howtomgr.github.io) collection. Always refer to official documentation for the most up-to-date information.
