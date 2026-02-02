# OpenClaw (Clawbot) Docker Image

Pre-built Docker image for [OpenClaw](https://github.com/openclaw/openclaw) — run your AI assistant in minutes without building from source.

This repository provides:

* A **one-command installer** (recommended)
* A **Docker Compose setup** for manual control
* A **pre-built image** hosted on GHCR

---

## One-Line Install (Recommended)

### Linux / macOS

```bash
bash <(curl -fsSL https://raw.githubusercontent.com/elyasmoshirpanahi/openclaw-docker/main/install.sh)
```

### Windows (PowerShell)

```powershell
irm https://raw.githubusercontent.com/elyasmoshirpanahi/openclaw-docker/main/install.ps1 | iex
```

> **Windows note:** Docker Desktop must be installed and running.
> WSL2 users can use the Linux command inside WSL.

### What the installer does

* ✅ Checks Docker & Docker Compose
* ✅ Creates a user-scoped install directory
* ✅ Pulls the OpenClaw Docker image
* ✅ Runs the onboarding wizard
* ✅ Starts the OpenClaw gateway via Docker Compose

---

## Installer Options

### Linux / macOS

```bash
# Pull image only
bash <(curl -fsSL https://raw.githubusercontent.com/elyasmoshirpanahi/openclaw-docker/main/install.sh) --pull-only

# Skip onboarding (already configured)
bash <(curl -fsSL https://raw.githubusercontent.com/elyasmoshirpanahi/openclaw-docker/main/install.sh) --skip-onboard

# Do not start gateway automatically
bash <(curl -fsSL https://raw.githubusercontent.com/elyasmoshirpanahi/openclaw-docker/main/install.sh) --no-start

# Custom install directory
bash <(curl -fsSL https://raw.githubusercontent.com/elyasmoshirpanahi/openclaw-docker/main/install.sh) --install-dir /opt/openclaw
```

### Windows (PowerShell)

```powershell
# Pull image only
irm https://raw.githubusercontent.com/elyasmoshirpanahi/openclaw-docker/main/install.ps1 | iex -PullOnly

# Skip onboarding
irm https://raw.githubusercontent.com/elyasmoshirpanahi/openclaw-docker/main/install.ps1 | iex -SkipOnboard

# Do not start gateway
irm https://raw.githubusercontent.com/elyasmoshirpanahi/openclaw-docker/main/install.ps1 | iex -NoStart
```

---

## Manual Install (Advanced)

### Requirements

* Docker
* Docker Compose
* Run as a **normal user** (not root)
* User must be in the `docker` group on Linux

```bash
sudo usermod -aG docker $USER
# log out and back in
```

---

## Using Docker Compose (Recommended for Manual Setup)

```bash
git clone https://github.com/elyasmoshirpanahi/openclaw-docker.git
cd openclaw-docker
```

### First-time onboarding

```bash
docker-compose run --rm openclaw-cli onboard
```

### Start the gateway (important)

```bash
docker-compose up -d openclaw-gateway
```

Verify:

```bash
docker-compose ps
docker logs -f openclaw-gateway
```

Health check:

```bash
curl http://127.0.0.1:18789/health
```

### Run the TUI

```bash
docker-compose run --rm openclaw-cli tui
```

> ⚠️ **Important**
>
> * Do **not** use `openclaw-cli gateway start` in Docker environments
> * That command relies on `systemctl --user` and is **not supported in containers**
> * Always manage the gateway with **Docker Compose**

---

## Configuration

Configuration is stored on the host and persists across restarts:

```text
~/.openclaw/
├── openclaw.json
├── identity/
└── workspace/
```

Configured during onboarding:

* AI provider (OpenAI, Anthropic, etc.)
* Agents and channels
* Gateway settings

---

## Volumes

| Container Path                   | Purpose           |
| -------------------------------- | ----------------- |
| `/home/node/.openclaw`           | Config + identity |
| `/home/node/.openclaw/workspace` | Agent workspace   |

---

## Ports

| Port    | Purpose                 |
| ------- | ----------------------- |
| `18789` | Gateway API + WebSocket |
| `18790` | Dashboard               |

---

## Docker Image Tags

| Tag      | Description                   |
| -------- | ----------------------------- |
| `latest` | Latest stable                 |
| `vX.Y.Z` | Specific release              |
| `main`   | Latest main branch (unstable) |

---

## Links

* 🌐 Website: [https://openclaw.ai](https://openclaw.ai)
* 📘 Docs: [https://docs.openclaw.ai](https://docs.openclaw.ai)
* 💻 Core Repo: [https://github.com/openclaw/openclaw](https://github.com/openclaw/openclaw)
* 💬 Discord: [https://discord.gg/clawd](https://discord.gg/clawd)

---

## License

This Docker packaging is maintained by
**Elyas Moshirpanahi** — [https://elyasmoshirpanahi.com](https://elyasmoshirpanahi.com)

OpenClaw is MIT licensed.
See the [original OpenClaw repository](https://github.com/openclaw/openclaw).

---

