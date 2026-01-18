
---

# 🔐 Vaultwarden Self-Hosted Deployment Guide

Objective: Deploy a secure, enterprise-grade password manager for the team using self-hosted infrastructure.

Requirement: Docker & Docker Compose installed. HTTPS is mandatory for Bitwarden clients to function.

---

## 1. Directory Structure

Create a directory on the server for the service:

Bash

```
mkdir vaultwarden
cd vaultwarden
mkdir vw-data
touch docker-compose.yml
```

---

## 2. Docker Compose Configuration

Copy the following into `docker-compose.yml`.

**Important:** Generate a secure Admin Token before deploying. You can run `openssl rand -base64 48` in your terminal to get one.

YAML

```
version: '3'
services:
  vaultwarden:
    image: vaultwarden/server:latest
    container_name: vaultwarden
    restart: always
    volumes:
      - ./vw-data:/data
    environment:
      - WEBSOCKET_ENABLED=true
      # Step 1: Set to 'true' to allow team members to create accounts.
      # Step 2: Set to 'false' immediately after onboarding to secure the server.
      - SIGNUPS_ALLOWED=true
      # Security token for the /admin page
      - ADMIN_TOKEN=REPLACE_WITH_LONG_RANDOM_STRING
      # Your full domain URL
      - DOMAIN=https://pass.yourcompany.com
    ports:
      - 127.0.0.1:8080:80    # Web Interface mapped to local 8080
      - 127.0.0.1:3012:3012  # WebSocket port
```

---

## 3. Reverse Proxy (Nginx) with HTTPS

Vaultwarden requires HTTPS. Use Nginx as a reverse proxy.

Below is the configuration snippet for your Nginx site config (usually in /etc/nginx/sites-available/pass.yourcompany.com).

**Crucial:** The `Upgrade` and `Connection` headers are required for WebSockets (real-time sync) to work.

Nginx

```
server {
    listen 443 ssl http2;
    server_name pass.yourcompany.com;

    # SSL Certificates (Let's Encrypt / Certbot paths)
    ssl_certificate /etc/letsencrypt/live/pass.yourcompany.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/pass.yourcompany.com/privkey.pem;

    # Main Traffic
    location / {
        proxy_pass http://127.0.0.1:8080;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    # WebSocket Traffic (Notifications)
    location /notifications/hub {
        proxy_pass http://127.0.0.1:3012;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
    }

    # Notifications endpoint negotiation
    location /notifications/hub/negotiate {
        proxy_pass http://127.0.0.1:8080;
    }
}
```

---

## 4. Onboarding SOP (Standard Operating Procedure)

1. Launch Service:
    
    Run docker-compose up -d.
    
2. Team Registration:
    
    All 3 team members must navigate to https://pass.yourcompany.com and create their accounts immediately.
    
3. **Lockdown (Security Hardening):**
    
    - Edit `docker-compose.yml`.
        
    - Change `SIGNUPS_ALLOWED=true` to `SIGNUPS_ALLOWED=false`.
        
    - Run `docker-compose up -d` to apply changes.
        
    - _Result:_ No one else can register an account on this server.
        
4. **Create Organization:**
    
    - Log in to the web vault.
        
    - Go to **Organizations** -> **New Organization**.
        
    - Name it (e.g., "Company Secrets").
        
    - Invite the other 2 members.
        

---

## 5. Usage Guidelines

### Storing Secrets

- **Personal Vault:** For individual passwords (e.g., personal Gmail). Private to the user.
    
- **Organization Vault:** For shared infrastructure (AWS, Database, SSH Keys). Visible to the team.
    

### Sharing Secrets (The "Send" Feature)

**NEVER** paste passwords directly into Discord/Slack.

1. Go to the Web Vault or Mobile App.
    
2. Click **Send** (Paper airplane icon).
    
3. Select the item or paste text.
    
4. Options:
    
    - **Deletion:** "1 hour" or "1 access".
        
    - **Password:** Optional.
        
5. Copy the link and paste it into Discord.
    

---

## 6. Automated Backup Strategy

Risk: If the server disk fails, all passwords are lost.

Solution: A daily cron job to backup the SQLite database and attachments.

Create a script `backup.sh`:

Bash

```
#!/bin/bash
# Define paths
DATA_DIR="/path/to/vaultwarden/vw-data"
BACKUP_DIR="/path/to/backups"
TIMESTAMP=$(date +"%Y%m%d")

# 1. Backup Database (Safe SQLite dump)
sqlite3 $DATA_DIR/db.sqlite3 ".backup '$BACKUP_DIR/vw-backup-$TIMESTAMP.sqlite3'"

# 2. Backup Attachments & Config
tar -czf $BACKUP_DIR/vw-attachments-$TIMESTAMP.tar.gz -C $DATA_DIR attachments config.json rsa_key*

# 3. Retention: Delete backups older than 30 days
find $BACKUP_DIR -name "vw-*" -mtime +30 -delete

# 4. (Optional) Sync to Cloud Storage using Rclone
# rclone copy $BACKUP_DIR remote:backup-folder
```

Add to Crontab (crontab -e) to run daily at 3 AM:

0 3 * * * /bin/bash /path/to/backup.sh