# OpenClaw Portainer Deployment

Deploy OpenClaw to Portainer using GitOps.

## Quick Start

1. **Add Stack in Portainer**
   - Go to Stacks → Add stack
   - Select "Repository"
   - Repository URL: `https://github.com/MLoacher/openclaw`
   - Compose path: `docker-compose.portainer.yml`

2. **Set Environment Variables**

   | Variable | Required | Description |
   |----------|----------|-------------|
   | `OPENCLAW_CONFIG_DIR` | Yes | Host path for config (e.g., `/opt/openclaw/config`) |
   | `OPENCLAW_WORKSPACE_DIR` | Yes | Host path for workspace (e.g., `/opt/openclaw/workspace`) |
   | `OPENCLAW_GATEWAY_TOKEN` | Yes | Auth token - generate with `openssl rand -hex 32` |
   | `OPENCLAW_GATEWAY_PORT` | No | Gateway port (default: `18789`) |
   | `OPENCLAW_BRIDGE_PORT` | No | Bridge port (default: `18790`) |
   | `OPENCLAW_GATEWAY_BIND` | No | Bind address: `lan` or `loopback` (default: `lan`) |

3. **Deploy Stack**
   - Click "Deploy the stack"
   - Wait for the build to complete (first build takes several minutes)

4. **Post-Deploy Setup**
   - Access Control UI at `http://<host>:18789/`
   - Paste your `OPENCLAW_GATEWAY_TOKEN` in Settings
   - Run onboarding via Portainer console:
     ```bash
     docker exec -it openclaw-gateway node dist/index.js onboard --no-install-daemon
     ```

## Adding Channels

After onboarding, add messaging channels:

```bash
# WhatsApp (QR code)
docker exec -it openclaw-gateway node dist/index.js channels login

# Telegram
docker exec -it openclaw-gateway node dist/index.js channels add --channel telegram --token <BOT_TOKEN>

# Discord
docker exec -it openclaw-gateway node dist/index.js channels add --channel discord --token <BOT_TOKEN>
```

## Updating

In Portainer:
1. Go to your stack
2. Click "Pull and redeploy"
3. The image will rebuild with latest changes

## Docs

- Full documentation: https://docs.openclaw.ai
- Docker guide: https://docs.openclaw.ai/install/docker
