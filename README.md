# Discord Ubuntu Manager Bot

Manage remote Ubuntu servers via SSH and Discord Slash Commands. Run it as a Docker container and control your infrastructure directly from Discord.

## Features

- **Multi-Server Support:** Manage an unlimited number of Ubuntu servers from a single bot.
- **Discord Autocomplete:** Seamlessly switch between servers in Discord using server aliases.
- **SSH Support:** Supports both **SSH Keys** and **Password-based** authentication.
- **Secure by Design:** No sensitive data is stored in the config; all secrets are passed via Environment Variables.
- **Slash Commands:**
  - `/update`: Run `apt update` and `apt upgrade` remotely.
  - `/process`: Search for running processes by name.
  - `/service`: Start, Stop, Restart, or check the Status of any systemd service.
  - `/logs`: Tail the last N lines of any log file.
  - `/disk`: Check disk space usage (`df -h`).

## Setup & Deployment

### 1. Requirements
- A Discord Bot Token (Create one at [Discord Developer Portal](https://discord.com/developers/applications)).
- Docker and Docker Compose installed on your management machine.
- Remote Ubuntu servers with SSH access.

### 2. Configuration
Clone the repository and copy the `.env.example` file to `.env`:
```bash
cp .env.example .env
```
Edit the `.env` file with your configuration:
- `DISCORD_TOKEN`: Your bot token.
- `SERVERS_JSON`: A JSON array of your servers.
- `SSH_KEY_...` or `SSH_PASS_...`: The actual secrets for each server.

### 3. Run with Docker
```bash
docker-compose up -d
```

## Security Recommendations
- **Dedicated User:** Create a dedicated user on your Ubuntu servers for the bot (e.g., `discord-bot`).
- **Sudo Access:** If you want to use `/update` or `/service`, ensure the user has `sudo` permissions. To avoid interactive prompts, add the user to `/etc/sudoers` with `NOPASSWD`.
- **SSH Keys:** Always prefer SSH Keys over passwords for better security.

## License
MIT License. Feel free to use and contribute!
