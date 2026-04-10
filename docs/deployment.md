# Deployment & Setup Guide

Lyncis is built to be deployed on-premise. The backend infrastructure is fully containerized, while the agent runs as
a native binary on your target hosts.

## Deploying the Backend Stack (Docker Compose)

The backend requires the Go API server and a PostgreSQL database. We use a single `docker-compose.yml` to spin up the
entire management plane.

### Create Directory

Create a directory on your host server (e.g., `/opt/lyncis`).

### Create Docker Compose Stack

Create a `docker-compose.yml` file with the following content:

```yaml
services:
  postgres:
    image: postgres:16-alpine
    container_name: lyncis-db
    restart: unless-stopped
    environment:
      POSTGRES_USER: lyncis
      POSTGRES_PASSWORD: ChangeThisSecurePassword!
      POSTGRES_DB: lyncis
    volumes:
      - lyncis_db_data:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U lyncis -d lyncis"]
      interval: 5s
      timeout: 5s
      retries: 5

  backend:
    image: haerlemarcel/lyncis-backend:latest
    container_name: lyncis-backend
    restart: unless-stopped
    ports:
      - "8000:8000"
    environment:
      - DB_HOST=postgres
      - DB_PORT=5432
      - DB_USER=lyncis
      - DB_PASSWORD=ChangeThisSecurePassword!
      - DB_NAME=lyncis
    depends_on:
      postgres:
        condition: service_healthy

  ui:
    image: haerlemarcel/lyncis-ui:latest
    container_name: lyncis-ui
    restart: unless-stopped
    ports:
      - "80:80"
    environment:
      - LYNCIS_API_BASE_URL=http://<YOUR_SERVER_IP>:8000/api/v1/ui

volumes:
  lyncis_db_data:
```

### Start Compose Stack

Run `docker compose up -d` to start the stack.

### Access Web UI

Access the UI via http://<YOUR_SERVER_IP>.

## Deploying the Lyncis Agent

The agent is a statically compiled Go binary, meaning it requires no dependencies other than the `lynis` tool itself.

### Prerequisites on Target Host

Ensure `lynis` is installed on the machine you want to monitor:

```bash
sudo apt-get update
sudo apt-get install lynis
```

### Installation Steps

#### Binary Download

Download the latest `lyncis-agent` binary to the target host.

```bash
wwget https://github.com/marcelhaerle/lyncis-agent/releases/latest/download/lyncis-agent-linux-amd64
```

#### Make it executable

```bash
chmod +x lyncis-agent
sudo mv lyncis-agent /usr/local/bin/
```

#### Create Config Directory

The agent requires configuration to know where the central backend is located. By default, it looks for its
configuration file at `/etc/lyncis/config.json`.

```bash
sudo mkdir -p /etc/lyncis
```

#### Create Systemd Service

To keep the agent running continuously and start it on boot, set it up as a systemd service.

Create a service file at `/etc/systemd/system/lyncis-agent.service`:

```ini
[Unit]
Description=Lyncis Security Agent
After=network.target

[Service]
Type=simple
ExecStart=/usr/local/bin/lyncis-agent
Restart=always
RestartSec=10
StandardOutput=syslog
StandardError=syslog
SyslogIdentifier=lyncis-agent
Environment=LYNCIS_BACKEND_URL=<YOUR_BACKEND_URL>
Environment=LYNCIS_CONFIG_PATH=/etc/lyncis/config.json

[Install]
WantedBy=multi-user.target
```

#### Enable and Start Service

```bash
sudo systemctl daemon-reload
sudo systemctl enable lyncis-agent
sudo systemctl start lyncis-agent
```
