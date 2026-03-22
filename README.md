# 🏠 Windows NAS Setup with Immich + Tailscale

This guide explains how to build a **personal photo NAS** on Windows using **Immich** and access it securely from anywhere using **Tailscale**.

Perfect for:

* Personal photo backup
* Private cloud storage
* Google Photos alternative
* Home server beginners

---

## 🧰 Requirements

Before starting, make sure you have:

* Windows 10 or Windows 11
* At least **8GB RAM**
* Minimum **20GB free space**
* External SSD recommended
* Stable internet connection

Software required:

* Docker Desktop
* Git
* Tailscale

---

## 📁 Step 1 — Create NAS Folder Structure

Open PowerShell and run:

```powershell
mkdir F:\NAS
cd F:\NAS

mkdir immich
cd immich

mkdir library
mkdir postgres
```

Folder structure should look like:

```text
F:\NAS\immich\
 ├── library
 └── postgres
```

---

## 🐳 Step 2 — Install Docker Desktop

Install Docker using terminal:

```powershell
winget install Docker.DockerDesktop
```

After installation:

1. Open Docker Desktop
2. Enable:

```text
Use WSL 2
Start Docker on login
```

Wait until:

```text
Docker is running
```

---

## 📄 Step 3 — Create docker-compose.yml

Inside:

```text
F:\NAS\immich
```

Create:

```text
docker-compose.yml
```

Use the compose file from this repository root (`docker-compose.yml`) or copy this content:

```yaml
services:
  immich-server:
    container_name: immich_server
    image: ghcr.io/immich-app/immich-server:release
    depends_on:
      - redis
      - database
    environment:
      DB_HOSTNAME: database
      DB_USERNAME: postgres
      DB_PASSWORD: postgres
      DB_DATABASE_NAME: immich
    volumes:
      - F:/NAS/immich/library:/usr/src/app/upload
    ports:
      - "2283:2283"
    restart: always

  redis:
    container_name: immich_redis
    image: redis:6
    restart: always

  database:
    container_name: immich_postgres
    image: tensorchord/pgvecto-rs:pg14-v0.2.0
    environment:
      POSTGRES_PASSWORD: postgres
      POSTGRES_USER: postgres
      POSTGRES_DB: immich
    volumes:
      - F:/NAS/immich/postgres:/var/lib/postgresql/data
    restart: always
```

---

## ▶️ Step 4 — Start Immich NAS

Run:

```powershell
cd F:\NAS\immich
docker compose up -d
```

Wait about **1–2 minutes**.

Check running containers:

```powershell
docker ps
```

Expected output includes:

```text
immich_server
immich_redis
immich_postgres
```

---

## 🌐 Step 5 — Open Immich Web Interface

Open browser:

```text
http://localhost:2283
```

Create your:

* Admin email
* Password

Immich is now running.

---

## 📱 Step 6 — Connect Mobile Device

Install:

* Immich mobile app (iOS or Android)

Find your PC IP:

```powershell
ipconfig
```

Example:

```text
192.168.1.5
```

In Immich app:

```text
Server URL:
http://192.168.1.5:2283
```

Login with your account.

---

## 🌍 Step 7 — Install Tailscale (Remote Access)

Install Tailscale:

```powershell
winget install Tailscale.Tailscale
```

Login:

```powershell
tailscale up
```

Open browser and complete sign-in.

---

## 🔗 Step 8 — Access NAS From Anywhere

Get Tailscale IPv4:

```powershell
tailscale ip -4
```

Example:

```text
100.82.14.55
```

Use this address remotely:

```text
http://100.82.14.55:2283
```

Now your NAS works from anywhere (inside your Tailscale network).

---

## 🔄 Step 9 — Auto Start NAS

Containers restart automatically because compose uses:

```yaml
restart: always
```

Also ensure Docker Desktop is set to:

```text
Start on login
```

---

## 📦 Step 10 — Backup Recommendation

Backup folder:

```text
F:\NAS\immich
```

To:

* External SSD
* Cloud backup (optional)

Never skip backups.

---

## ⚠️ Important Notes (Read Before 24/7 Use)

* Keep the NAS PC powered on for continuous sync and remote access.
* Avoid shutting down frequently if you expect automatic mobile uploads.
* A stable internet connection is required for remote access through Tailscale.
* Docker Desktop must stay running; if Docker stops, Immich becomes unavailable.
* Use a UPS (battery backup) if possible to reduce data corruption risk during power loss.
* Check disk space regularly, especially in `library` and `postgres` folders.
* Do not expose port `2283` directly to the public internet; use Tailscale instead.

---

## 🛠️ Troubleshooting

### Immich not opening

Check containers:

```powershell
docker ps
```

Restart:

```powershell
docker compose restart
```

### Tailscale not connecting

Retry login/tunnel setup:

```powershell
tailscale up
```

---

## 🔐 Security Tips

* Use strong passwords
* Keep Docker updated
* Do not expose ports publicly
* Use Tailscale for remote access

---

## 🎉 Done!

You now have:

* Personal photo NAS
* Private cloud storage
* Remote access from anywhere

---

## 📌 Future Improvements

* Add Cloudflare Tunnel
* Use custom domain
* Add Nextcloud
* Add automatic backups

---

## 👨‍💻 Author

Created by **TechWithSuki**

Personal NAS + Homelab learning project.
