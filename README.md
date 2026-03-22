# 🏠 Private NAS on Windows (Immich + Tailscale)

Build a professional, self-hosted photo NAS on Windows with Immich for photo management and Tailscale for secure remote access.

![Windows](https://img.shields.io/badge/OS-Windows%2010%2F11-0078D6?logo=windows&logoColor=white)
![Docker](https://img.shields.io/badge/Container-Docker-2496ED?logo=docker&logoColor=white)
![Immich](https://img.shields.io/badge/App-Immich-4250AF)
![Tailscale](https://img.shields.io/badge/Remote-Tailscale-242424?logo=tailscale&logoColor=white)

## Contents

- [Why this project](#why-this-project)
- [Stack](#stack)
- [Requirements](#requirements)
- [Project layout](#project-layout)
- [Quick start (10 steps)](#quick-start-10-steps)
- [24/7 operation notes (important)](#247-operation-notes-important)
- [Security best practices](#security-best-practices)
- [Troubleshooting](#troubleshooting)
- [Future improvements](#future-improvements)

## Why this project

- Private photo backup without public cloud lock-in
- Google Photos alternative with your own storage
- Remote access from anywhere using WireGuard-based networking (Tailscale)
- Beginner-friendly homelab setup

## Stack

- Windows 10/11
- Docker Desktop
- Immich + Redis + PostgreSQL (pgvecto-rs)
- Tailscale

## Requirements

- 8 GB RAM minimum
- 20 GB free disk minimum (more recommended)
- Stable internet connection
- External SSD strongly recommended

## Project layout

```text
F:\NAS\immich\
├── library
└── postgres
```

## Quick start (10 steps)

### 1) Create folders

```powershell
mkdir F:\NAS
cd F:\NAS
mkdir immich
cd immich
mkdir library
mkdir postgres
```

### 2) Install Docker Desktop

```powershell
winget install Docker.DockerDesktop
```

Then enable:

- Use WSL 2
- Start Docker on login

Wait until Docker shows **running**.

### 3) Create compose file

Use `docker-compose.yml` from this repository, or create your own in `F:\NAS\immich`.

### 4) Start Immich

```powershell
cd F:\NAS\immich
docker compose up -d
docker ps
```

Expected containers:

- `immich_server`
- `immich_redis`
- `immich_postgres`

### 5) Open Immich web UI

Go to:

```text
http://localhost:2283
```

Create your admin account.

### 6) Connect mobile app

Install Immich app (iOS/Android), find local IP:

```powershell
ipconfig
```

Use server URL format:

```text
http://<YOUR_LOCAL_IP>:2283
```

### 7) Install Tailscale

```powershell
winget install Tailscale.Tailscale
tailscale up
```

### 8) Access from anywhere

```powershell
tailscale ip -4
```

Use:

```text
http://<YOUR_TAILSCALE_IP>:2283
```

### 9) Ensure auto-start

- `restart: always` is already configured in compose
- Keep Docker Desktop on startup

### 10) Back up regularly

Back up `F:\NAS\immich` to:

- External SSD
- Optional cloud backup

## 24/7 operation notes (important)

- Keep the NAS PC powered on for continuous uploads and remote access
- Do not frequently shut down if you rely on automatic phone sync
- Keep internet active for Tailscale remote connectivity
- Keep Docker Desktop running at all times
- Use a UPS to reduce corruption risk during power loss
- Monitor disk growth in `library` and `postgres`

## Security best practices

- Use strong, unique passwords
- Keep Windows, Docker, and Immich updated
- Never expose port `2283` directly to the public internet
- Access remotely via Tailscale only

## Troubleshooting

### Immich does not open

```powershell
docker ps
docker compose restart
```

### Tailscale cannot connect

```powershell
tailscale up
```

## Included file

This repository includes:

- `docker-compose.yml` for Immich + Redis + PostgreSQL

## Future improvements

- Add automated backup schedule
- Add Cloudflare Tunnel (optional)
- Add custom domain (optional)
- Extend to full homelab stack (e.g., Nextcloud)

## Author

Created by **TechWithSuki**.
