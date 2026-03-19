# Installation from Source

## Prerequisites

- **Go** 1.25 or later — [download](https://go.dev/dl/)
- **Git** — [installation guide](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
- **MariaDB** or **MySQL** — [MariaDB install guide](https://www.digitalocean.com/community/tutorials/how-to-install-mariadb-on-ubuntu-20-04)

### Notification Service

You will need at least one bot token:

- [Discord Bot](discord-bot.md)
- [Telegram Bot](telegram-bot.md)

### Database Setup

Create a database and user for Dexter. Use a [strong password](https://passwordsgenerator.net/).

For local connections:

```sql
CREATE DATABASE dexter;
CREATE USER 'dexteruser'@'localhost' IDENTIFIED BY 'yourStrongPassword';
GRANT ALL PRIVILEGES ON dexter.* TO 'dexteruser'@'localhost';
```

For remote connections (only if needed):

```sql
CREATE DATABASE dexter;
CREATE USER 'dexteruser'@'%' IDENTIFIED BY 'yourStrongPassword';
GRANT ALL PRIVILEGES ON dexter.* TO 'dexteruser'@'%';
```

## Building Dexter

1. Clone the repository:

    ```bash
    git clone https://github.com/yourorg/dexter.git
    cd dexter
    ```

2. Build the binary:

    ```bash
    go build -o dexter ./cmd/dexter
    ```

3. On first start, Dexter creates `config/local.json` from sensible defaults. Edit it to add your database credentials and bot tokens:

    ```bash
    ./dexter
    # Edit config/local.json, then restart
    ```

!!! tip "Game data"
    Dexter automatically downloads game data files (monsters, moves, items, grunts, quests, types, translations) on first startup and refreshes them every 6 hours. You can also regenerate manually with `go run ./cmd/dexter-generate`.

## Updating

```bash
git pull
go build -o dexter ./cmd/dexter
# Restart Dexter
```

## Running as a Service

### Using PM2 (recommended)

PM2 is the preferred process manager for the Golbat/Dragonite stack. If you don't already have it:

```bash
npm install -g pm2
```

Start Dexter with PM2:

```bash
pm2 start ./dexter --name dexter
```

Save the process list so it survives reboots:

```bash
pm2 save
pm2 startup   # follow the printed command to enable boot persistence
```

#### Useful PM2 commands

```bash
pm2 status                 # Overview of all processes
pm2 logs dexter            # Follow logs
pm2 restart dexter         # Restart
pm2 stop dexter            # Stop
pm2 delete dexter          # Remove from PM2
```

### Using systemd

If you prefer systemd, create `/etc/systemd/system/dexter.service`:

```ini
[Unit]
Description=Dexter Pokemon GO Alert System
After=network.target mysql.service

[Service]
Type=simple
User=dexter
WorkingDirectory=/opt/dexter
ExecStart=/opt/dexter/dexter
Restart=on-failure
RestartSec=10

[Install]
WantedBy=multi-user.target
```

Then:

```bash
sudo systemctl daemon-reload
sudo systemctl enable dexter
sudo systemctl start dexter
```

#### Useful systemd commands

```bash
sudo systemctl status dexter    # Check status
sudo systemctl restart dexter   # Restart
sudo journalctl -u dexter -f    # Follow logs
```

Next: [Basic Configuration](configuration.md)
