---
layout: post
title:  "Preparing a New Debian Server for Web Services A Comprehensive Setup Guide.md"
date: 2025-06-25 07:38:29 +0800
tags: ['运维']
slug: p20250625073829

---

# Preparing a New Debian Server for Web Services: A Comprehensive Setup Guide

Deploying web applications on a Debian server requires careful preparation to ensure security, performance, and maintainability. This guide outlines the essential configurations for setting up a Debian server to host web services driven by tools like **PM2** or **Gunicorn**, following the **Principle of Least Privilege (POLP)**. From user management to network security, these steps provide a production-ready foundation.

---

## 1. Creating Dedicated Service Users

To isolate web applications and minimize security risks, create dedicated system users without login privileges.

### Steps:
- **Create a system user** for running the web application (e.g., `webapp`):
  ```bash
  sudo adduser --system --group --no-create-home --shell /usr/sbin/nologin webapp
  ```
  - `--system`: Assigns a system UID/GID (< 1000).
  - `--no-create-home`: Avoids creating a home directory.
  - `--shell /usr/sbin/nologin`: Prevents interactive login.
- **Verify user properties**:
  ```bash
  id webapp
  ```
- **Optional: Create an SFTP user** for file uploads (e.g., `webdeploy`):
  ```bash
  sudo adduser --system --group --shell /bin/false webdeploy
  sudo usermod -aG sftp-only webdeploy
  ```
  Configure SSH for chrooted SFTP in `/etc/ssh/sshd_config`:
  ```text
  Match Group sftp-only
      ChrootDirectory /srv/your-app/uploads
      ForceCommand internal-sftp
      AllowTcpForwarding no
  ```
  Restart SSH: `sudo systemctl restart sshd`.

### Why?
- System users reduce attack surfaces by prohibiting shell access.
- SFTP users allow secure file uploads without compromising server access.

---

## 2. Setting Up File System Permissions

Organize the application directory with strict permissions to protect code, logs, and sensitive files.

### Directory Structure:
```text
/srv/your-app/
├── app/       # Application code (755/644)
├── logs/      # Logs (775)
├── uploads/   # User uploads (770)
└── .env       # Configuration file (640)
```

### Steps:
- **Set ownership**:
  ```bash
  sudo chown -R webapp:webapp /srv/your-app
  ```
- **Apply permissions**:
  ```bash
  sudo find /srv/your-app/app -type d -exec chmod 755 {} \;
  sudo find /srv/your-app/app -type f -exec chmod 644 {} \;
  sudo chmod 770 /srv/your-app/uploads
  sudo chmod 640 /srv/your-app/.env
  ```
- **Protect sensitive files** (optional):
  ```bash
  sudo chattr +i /srv/your-app/.env
  ```

### Why?
- Fine-grained permissions ensure only authorized users/processes access specific resources.
- Immutable attributes (`chattr +i`) prevent accidental modifications to critical files.

---

## 3. Configuring a Management User with `sudo` Privileges

Directly using the `root` user is risky and violates POLP. Instead, create a management user with limited `sudo` privileges.

### Steps:
- **Create a management user** (e.g., `deployer`):
  ```bash
  sudo adduser deployer
  sudo passwd deployer
  ```
- **Grant `sudo` access**:
  ```bash
  sudo usermod -aG sudo deployer
  ```
- **Optional: Restrict `sudo` commands** via `/etc/sudoers`:
  ```bash
  sudo visudo
  ```
  Add:
  ```text
  deployer ALL=(ALL) NOPASSWD: /bin/systemctl restart pm2-webapp, /bin/systemctl start pm2-webapp, /bin/systemctl stop pm2-webapp, /usr/bin/chown, /usr/bin/chmod
  ```
- **Configure SSH access**:
  - Disable root login in `/etc/ssh/sshd_config`:
    ```text
    PermitRootLogin no
    AllowUsers deployer
    ```
  - Set up SSH key authentication:
    ```bash
    su - deployer
    mkdir ~/.ssh
    chmod 700 ~/.ssh
    nano ~/.ssh/authorized_keys  # Add public key
    chmod 600 ~/.ssh/authorized_keys
    ```
  - Restart SSH: `sudo systemctl restart sshd`.

### Why?
- A `deployer` user with `sudo` provides controlled access for administrative tasks (e.g., service restarts, dependency installation).
- SSH key authentication and restricted `sudo` commands enhance security.

---

## 4. Configuring Web Services (PM2/Gunicorn)

Run web services as the `webapp` user to avoid root privileges.

### PM2 Setup:
- **Generate PM2 service**:
  ```bash
  sudo -u webapp pm2 init
  ```
- **Create systemd unit**:
  ```bash
  sudo nano /etc/systemd/system/pm2-webapp.service
  ```
  Content:
  ```ini
  [Unit]
  Description=PM2 for webapp
  After=network-online.target
  Wants=network-online.target
  
  [Service]
  User=webapp
  Group=webapp
  WorkingDirectory=/srv/your-app
  ExecStart=/usr/bin/pm2 start --no-daemon /srv/your-app/ecosystem.config.js
  Restart=always
  MemoryMax=512M
  CPUQuota=50%
  
  [Install]
  WantedBy=multi-user.target
  ```

### Gunicorn Setup:
- **Create systemd unit**:
  ```bash
  sudo nano /etc/systemd/system/gunicorn.service
  ```
  Content:
  ```ini
  [Unit]
  Description=Gunicorn for webapp
  After=network-online.target
  Wants=network-online.target
  
  [Service]
  User=webapp
  Group=webapp
  WorkingDirectory=/srv/your-app
  ExecStart=/usr/bin/gunicorn --workers 3 --bind 0.0.0.0:8000 app:app
  Restart=always
  AmbientCapabilities=CAP_NET_BIND_SERVICE
  NoNewPrivileges=true
  StandardOutput=append:/srv/your-app/logs/gunicorn.log
  StandardError=append:/srv/your-app/logs/gunicorn.error.log
  
  [Install]
  WantedBy=multi-user.target
  ```

- **Enable and start services**:
  ```bash
  sudo systemctl daemon-reload
  sudo systemctl enable pm2-webapp gunicorn
  sudo systemctl start pm2-webapp gunicorn
  ```

### Why?
- Non-root execution reduces risks if the application is compromised.
- Systemd ensures service reliability and resource limits.

---

## 5. Network Security Hardening

Secure network access to minimize exposure.

### Steps:
- **Configure UFW firewall**:
  ```bash
  sudo apt install ufw
  sudo ufw allow 22/tcp
  sudo ufw allow 80/tcp
  sudo ufw allow 443/tcp
  sudo ufw enable
  ```
- **Allow non-root port binding** (80/443):
  ```bash
  sudo setcap 'cap_net_bind_service=+ep' $(which python3)
  sudo setcap 'cap_net_bind_service=+ep' $(which node)
  ```
- **Install Fail2Ban**:
  ```bash
  sudo apt install fail2ban
  sudo systemctl enable fail2ban
  ```

### Why?
- UFW limits open ports to essential services.
- `setcap` avoids running services as root for low ports.
- Fail2Ban protects against brute-force attacks.

---

## 6. Log Management and Auditing

Ensure logs are rotated and access is monitored.

### Log Rotation:
- **Configure `logrotate`**:
  ```bash
  sudo nano /etc/logrotate.d/webapp-logs
  ```
  Content:
  ```text
  /srv/your-app/logs/*.log {
      daily
      missingok
      rotate 30
      compress
      delaycompress
      notifempty
      create 664 webapp webapp
      su webapp webapp
      sharedscripts
      postrotate
          systemctl reload pm2-webapp
      endscript
  }
  ```

### Auditing:
- **Install and configure `auditd`**:
  ```bash
  sudo apt install auditd
  sudo auditctl -w /srv/your-app -p wa -k webapp_access
  sudo auditctl -w /srv/your-app/uploads -p wa -k webapp_uploads
  ```
  Check logs: `sudo ausearch -k webapp_access`.

### Why?
- Log rotation prevents disk exhaustion.
- Auditing tracks file access for security investigations.

---

## 7. Backup and Disaster Recovery

Prepare for data loss or server failure.

### Steps:
- **Backup critical files**:
  ```bash
  sudo mkdir -p /backups
  sudo tar czvf - /etc/passwd /etc/group /etc/shadow /etc/sudoers.d/ /srv/your-app/.env | gpg --symmetric -o /backups/webapp-config-$(date +%F).tar.gz.gpg
  ```
- **Automate backups** via cron:
  ```bash
  sudo crontab -e
  ```
  Add:
  ```bash
  0 2 * * * /bin/bash /path/to/backup-script.sh
  ```
- **User restoration script**:
  ```bash
  sudo nano /usr/local/bin/restore-users.sh
  ```
  Content:
  ```bash
  #!/bin/bash
  adduser --system --group --no-create-home webapp
  chown -R webapp:webapp /srv/your-app
  setcap 'cap_net_bind_service=+ep' $(which node)
  systemctl daemon-reload
  systemctl restart pm2-webapp
  ```
  Make executable: `sudo chmod +x /usr/local/bin/restore-users.sh`.

### Why?
- Encrypted backups protect sensitive data.
- Automated scripts ensure quick recovery.

---

## 8. Verification Checklist

Ensure the setup is correct:
- **User validation**:
  ```bash
  id webapp  # Check UID/GID and shell
  sudo -u webapp whoami  # Confirm identity
  ```
- **Service validation**:
  ```bash
  ps aux | grep 'pm2\|gunicorn'  # Verify non-root execution
  sudo systemctl status pm2-webapp gunicorn
  ```
- **Port binding test**:
  ```bash
  sudo -u webapp node -e "require('http').createServer((_,res)=>res.end('OK')).listen(80)"
  ```
- **Log permissions**:
  ```bash
  ls -l /srv/your-app/logs
  ```

---

## 9. Additional Recommendations

- **Enable SELinux/AppArmor** (if applicable):
  ```bash
  sudo apt install apparmor
  sudo aa-status
  ```
- **Set up monitoring** (e.g., Prometheus, Zabbix) for CPU, memory, and service health.
- **Use containers** (e.g., Docker) for additional isolation.
- **Test disaster recovery** monthly to validate backups and scripts.
- **Secure HTTPS** with Let’s Encrypt:
  ```bash
  sudo apt install certbot python3-certbot-nginx
  sudo certbot --nginx
  ```

---

## Conclusion

This guide provides a secure and maintainable foundation for hosting web services on a Debian server. By isolating users, restricting permissions, and hardening the system, you can deploy PM2 or Gunicorn-driven applications with confidence. Regularly audit configurations and test recovery procedures to ensure long-term reliability.

For specific use cases (e.g., CI/CD integration, multi-app setups), tailor these steps to your needs. Happy deploying!