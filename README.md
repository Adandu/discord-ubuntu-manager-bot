# <img src="logo_hybrid.svg" width="48" height="48" valign="middle"> DiscoBunty

Manage remote Ubuntu servers via SSH and Discord Slash Commands. Run it as a Docker container and control your infrastructure directly from Discord or a secure Web Control Panel.

---

## ⚠️ Disclaimer

**This bot was created 100% using AI (Gemini CLI & Claude Code).**  
Whoever wants to use this bot, they do so at their own risk. The authors and creators are not responsible for any damage, data loss, or security breaches resulting from the use of this software. Always review the code and test in a safe environment before deploying to production.

---

## 🚀 Features

- **Web Control Panel (NEW):** Securely manage your bot configuration, servers, and view live application logs from a single-page dashboard.
- **Encrypted Configuration:** All sensitive data (Discord tokens, SSH passwords, keys) is stored encrypted in `config.json` using a master `SECRET_KEY`.
- **Connectivity Testing:** Integrated "Test Connection" button in the WebUI to verify SSH credentials before saving.
- **Multi-Server Support:** Manage an unlimited number of Ubuntu servers from a single bot.
- **Discord Autocomplete:** Seamlessly switch between servers in Discord using server aliases.
- **Secure SSH Management:** Supports **SSH Keys** (via raw string or volume mount) and **Passwords**.
- **Real-time Logging:** View live application activity directly in the WebUI or via Docker logs.
- **Security Hardened:** 
  - **RBAC:** Restrict administrative commands to specific Discord roles.
  - **CSRF & Timing Attack Protection:** Enhanced security for the Web Control Panel.
  - **Masked Secrets:** Sensitive values are hidden in the UI to prevent shoulder surfing.
- **Slash Commands:**
  - `/update`: Run `apt update` and `apt upgrade` remotely.
  - `/process`: Search for running processes by name.
  - `/service`: Start, Stop, Restart, or check the Status of any systemd service.
  - `/logs`: Tail the last N lines of any log file.
  - `/disk`: Check disk space usage (`df -h`).
  - `/server power`: Reboot or Shutdown a server with a safety password and confirmation step.
  - `/docker ps`: List all containers.
  - `/docker control`: Start, Stop, or Restart a specific container.
  - `/docker logs`: View the last N lines of container logs.
  - `/docker details`: View container image, internal IP, and port mappings.

---

## 📦 Setup & Deployment

### 1. Requirements
- A Discord Bot Token & Guild ID.
- Docker and Docker Compose installed.
- Remote Ubuntu servers with SSH access.

### 2. Configuration
The bot now uses a hybrid configuration system. Initial secrets are set via environment variables, and the rest is managed via `config.json`.

**Mandatory Environment Variables (`.env`):**
- `SECRET_KEY`: A 32+ character random string used to encrypt all local secrets. **Do not lose this.**
- `WEBUI_ENABLED`: Set to `true` to enable the dashboard.

**WebUI Access:**
Once the container is running, navigate to `http://<your-ip>:8083` to configure your bot token and servers.

### 3. Run with Docker
```bash
docker-compose up -d
```

---

## 🛡️ Security Best Practices

### Restricted Sudo Access
For maximum security, do not give the SSH user full passwordless sudo. Instead, restrict it to only the commands required by DiscoBunty.

Add the following to your `/etc/sudoers` file (replace `botuser` with your actual SSH user):

```bash
# Basic server management
botuser ALL=(ALL) NOPASSWD: /usr/bin/apt-get, /usr/bin/systemctl, /usr/bin/tail, /usr/bin/df, /usr/bin/realpath

# Docker management (if enabled)
botuser ALL=(ALL) NOPASSWD: /usr/bin/docker

# Power control (if enabled)
botuser ALL=(ALL) NOPASSWD: /usr/sbin/reboot, /usr/sbin/shutdown
```

### SSH Hardening
- **Known Hosts:** Ensure the `KNOWN_HOSTS_FILE` is correctly configured to prevent Man-in-the-Middle attacks.
- **SSH Keys:** Always prefer SSH Keys over passwords for better security.

---

## 📜 License
MIT License. Feel free to use and contribute!
