# 🌐 HTML + Docker + Cloudflare — Static Site Deployment Guide

A lightweight and secure setup for deploying a static HTML/CSS website using Docker and a Cloudflare Tunnel, with a domain managed through Cloudflare.
This guide demonstrates how to containerize and serve your site with Nginx, keeping your setup minimal, portable, and production-ready.

---

## 🚀 Overview

This project shows how to:
- Serve static HTML/CSS files through an Nginx container.
- Expose your containerized site securely via a Cloudflare Tunnel.
- Manage everything easily using Docker Compose.
- Host your own domain through Cloudflare (no VPS required).

---

## 🧱 Project Structure
```
.
├── compose.yaml              # Docker Compose configuration
├── nginx.conf                # Nginx main config
├── security-headers.conf     # Additional HTTP security headers
├── cloudflared/              # Cloudflare Tunnel (local only; excluded from Git)
│   ├── config.yml            # Actual tunnel config (not committed)
│   ├── cert.pem              # Certificate file (not committed)
│   └── credentials.json      # Credentials file (not committed)
└── site/
    └── src/
        ├── index.html        # Main HTML page
        └── assets/
            ├── style.css     # CSS stylesheet
            └── image.jpg
```
---

## ⚙️ Prerequisites

Before starting, make sure you have the following installed:

- [Docker Desktop for Windows](https://www.docker.com/products/docker-desktop/)
- [Cloudflare account](https://www.cloudflare.com/) (for domain and DNS)
- [PowerShell](https://learn.microsoft.com/en-us/powershell/) (recommended for commands below)

---

## 🧰 Environment Variables

This project does not require a `.env` file by default.  
All necessary configuration values are defined within the Docker and Cloudflare setup.  
If you prefer to use one later for convenience, see the `.env.example` section in this guide.

## .env.example
NEXT_PUBLIC_SITE_URL=https://example.com
CLOUDFLARE_TUNNEL_ID=00000000-0000-0000-0000-000000000000

---

## ▶️ Build and Run with Docker

# 1  Build and start
docker compose up -d --build

# 2  Visit
http://localhost:8080

# 3  Stop
docker compose down

---

## 🌩️ Connect Your Cloudflare Domain

Open Cloudflare → Zero Trust → Access → Tunnels

Create a tunnel (or reuse one)

Copy its Tunnel Token

Add it to your .env or compose.yaml

Run docker compose up -d

Your site is now live at your Cloudflare domain

## Example safe config to commit as cloudflared/config.example.yml:

tunnel: <TUNNEL-NAME>
credentials-file: /etc/cloudflared/credentials.json  # never commit real file
ingress:
  - hostname: your-domain.example
    service: http://web:80
  - service: http_status:404

---

## 🧹 Security Practices

- Cloudflared/ is git-ignored to protect secrets
- Enable HTTPS-only mode in Cloudflare for full encryption.
- Real credentials and tunnel IDs stay local
- Security-headers.conf adds standard protections:
  - X-Content-Type-Options: nosniff
  - X-Frame-Options: DENY
  - Referrer-Policy: strict-origin-when-cross-origin
  - Permissions-Policy: camera=(), microphone=(), geolocation=()

---

## 🧠 Troubleshooting
```
| Issue                 | Fix                                                          |
| --------------------- | ------------------------------------------------------------ |
| 404 errors            | Ensure files are in `site/src` and `nginx.conf` root matches |
| Tunnel not connecting | Re-authenticate `cloudflared` or refresh token               |
| CSS missing           | Check path `/assets/style.css`                               |
| Port conflict         | Edit `compose.yaml` → change `"8080:80"`                     |
```
---

## ✅ Result

Your HTML/CSS site runs via Nginx + Docker, served safely through Cloudflare Tunnel.
The setup is portable, lightweight, and free of secrets.

---

## 📄 License

Licensed under the MIT License — see LICENSE.

