# ideasidus.dev Reverse Proxy

This repository contains the reverse proxy configuration for the `ideasidus.dev` domain. The setup currently uses Caddy as a web server/reverse proxy, but is configured to allow switching to Nginx if needed in the future.

## SSL Certificate Configuration

This setup uses DNS-01 challenge for SSL certificate validation instead of the standard HTTP-01 challenge. This is necessary because the domain redirects to an external service (Notion) which wouldn't allow Caddy to respond to HTTP challenges. The Caddy server is built with the Cloudflare DNS plugin to support this functionality.

## Configuration

The main configuration file is located at `conf/Caddyfile`, which currently redirects all traffic from `ideasidus.dev` to `https://ideasidus.notion.site`.

## Prerequisites

Before running this setup, you'll need to create a `.env` file with the required environment variables. See the `.env.example` file for the format.

## Usage

### Starting the Server

First, create your `.env` file based on `.env.example`:

```bash
cp .env.example .env
# Edit .env with your actual values
```

Then, you can start the container:

```bash
docker compose up -d
```

### Reloading Configuration

To reload Caddy after making changes to your Caddyfile:

```bash
docker compose exec -w /etc/caddy caddy caddy reload
```

### Viewing Logs

To see Caddy's 1000 most recent logs, and follow to see new ones streaming in:

```bash
docker compose logs caddy -n=1000 -f
```

## Switching to Nginx (Future Consideration)

If you decide to switch to Nginx instead of Caddy in the future:
1. Update the Dockerfile to use Nginx instead of Caddy
2. Replace the Caddyfile with an appropriate nginx.conf
3. Update the docker-compose.yml if needed

## Security

⚠️ **IMPORTANT**: The `.env` file containing secrets (like Cloudflare API tokens) is already included in `.gitignore` to prevent accidental commits. Never commit `.env` file to the repository.