# BMAD-Method Development Container

Docker-in-Docker development environment for [BMAD-METHOD](https://github.com/bmad-code-org/BMAD-METHOD) - a comprehensive AI-powered development methodology.

## Table of Contents
- [Prerequisites](#prerequisites)
- [Quick Start](#quick-start) ⭐ *Complete setup in 10 minutes*
- [Container Setup](#container-setup)
- [Working with the Container](#working-with-the-development-container)
- [BMAD-Method Installation](#bmad-method-setup)
- [Claude Code Integration](#additional-setup)
- [Usage Monitoring & Costs](#usage-monitoring--costs) 💰 *Track your AI usage*
- [Troubleshooting](#troubleshooting)
- [Resources](#resources)

## Prerequisites

Before you begin, ensure you have the following installed on your local machine:

1. **Visual Studio Code** - [Download VS Code](https://code.visualstudio.com/)
2. **Docker Desktop** - [Download Docker](https://www.docker.com/products/docker-desktop/)
   - Windows: Docker Desktop for Windows
   - macOS: Docker Desktop for Mac
   - Linux: Docker Engine and Docker Compose
3. **Dev Containers extension** for VS Code
   - Install from VS Code Extensions marketplace or run:
   ```bash
   code --install-extension ms-vscode-remote.remote-containers
   ```

**Note:** This container includes Node.js 20+ which is required for BMAD-Method.

## Quick Start

**TL;DR**: Get the complete BMAD + Claude Code environment ready in ~15 minutes:

1. **Setup Container**:
   ```bash
   git clone https://github.com/fabriciopsouza/bmad-method-dind.git
   cd bmad-method-dind
   ```

2. **Open in VS Code**: Click "Reopen in Container" when prompted

3. **Install Claude Code**:
   ```bash
   npm install -g @anthropic-ai/claude-code@latest
   claude --dangerously-skip-permissions
   ```
   Exit Claude Code after initial setup

4. **Setup GitHub Integration**:
   ```bash
   gh auth login
   claude
   /install-github-app
   ```
   Follow the prompts for authentication

5. **BMAD-Method**: already installed (v6, `_bmad/`). To update:
   ```bash
   npx bmad-method@latest install --directory . --modules bmm --tools claude-code --action update --yes
   ```

6. **Restart Claude Code** (Important for BMAD integration):
   ```bash
   # Exit Claude Code if currently running (Ctrl+C or 'exit')
   # Then restart to load BMAD integration
   claude
   ```

7. **Start Development**: 
   - Use `claude` to enter Claude Code environment
   - Ask for `bmad-help` to get the recommended next BMAD skill

8. **Optional - Set up Usage Monitoring**:
   ```bash
   # Install monitoring tools (recommended for cost tracking)
   curl -LsSf https://astral.sh/uv/install.sh | sh
   uv tool install claude-monitor
   ```

🎉 **You're ready!** You now have a complete AI-powered development environment with BMAD methodology and Claude Code integration.

## Container Setup

### 1. Clone the Repository

```bash
git clone https://github.com/fabriciopsouza/bmad-method-dind.git
cd bmad-method-dind
```

### 2. Open in VS Code with Remote Containers

There are multiple ways to open the project in a Dev Container:

#### Option A: Using Command Palette
1. Open VS Code
2. Open the cloned folder: `File > Open Folder...`
3. Press `F1` or `Ctrl+Shift+P` (Windows/Linux) or `Cmd+Shift+P` (macOS)
4. Type and select: `Remote-Containers: Reopen in Container`

#### Option B: Using the Quick Action
1. Open the folder in VS Code
2. When VS Code detects the `.devcontainer` folder, a notification will appear
3. Click `Reopen in Container`

#### Option C: From VS Code Terminal
```bash
code .
# Then use Option A or B
```

### 3. Wait for Container Build

The first time you open the project in a container:
- VS Code will build the Docker image based on the configuration
- This may take several minutes depending on your internet connection
- You'll see progress in the VS Code terminal

### 4. Verify the Environment

Once the container is running, verify the setup:

```bash
# Check Docker is available (Docker-in-Docker)
docker --version

# Check Node.js is installed
node --version
npm --version

# Check GitHub CLI is available
gh --version
```

## Working with the Development Container

### Container Features

This development container includes:
- **Docker-in-Docker**: Run Docker commands inside the container
- **Node.js LTS**: With npm, yarn, and pnpm package managers
- **GitHub CLI**: For GitHub integration
- **VS Code Extensions**: Markdown support pre-configured

### Common Commands

#### Rebuild Container
If you make changes to the container configuration:
1. Press `F1` → `Remote-Containers: Rebuild Container`

#### Restart Container
1. Press `F1` → `Remote-Containers: Restart Container`

#### Open Terminal in Container
1. Press `` Ctrl+` `` or `View > Terminal`
2. The terminal automatically opens inside the container

### Port Forwarding

To forward ports from the container to your local machine:
1. Add to `.devcontainer/devcontainer.json`:
   ```json
   "forwardPorts": [3000, 8080]
   ```
2. Or forward manually: Press `F1` → `Forward a Port`

## Claude Code Integration

### Claude Code

#### Install Claude
```bash
npm install -g @anthropic-ai/claude-code@latest
```

#### Initialize Claude & set credentials
```bash
claude --dangerously-skip-permissions
```

Exit Claude Code

### GitHub Integration

Configure GitHub CLI (if not already configured):
```bash
gh auth login
```

#### Authenticate to GitHub
Follow the prompts to authenticate with your GitHub account:
1. Go to the provided URL: `https://github.com/login/device`
2. Paste the authentication code (XXXX-XXXX)

#### Initialize GitHub App
```bash
claude
/install-github-app
```
And Follow the installation path

⚠️ **Authentication Note**: Browser links might not work for authentication calls through host OS and non-default browsers. Be prepared to copy & paste URLs during authentication stages.

📖 **For detailed troubleshooting**: [Claude Code Action Setup Guide](https://github.com/anthropics/claude-code-action/blob/main/docs/setup.md)

### BMAD-Method Setup

This repository ships BMAD-METHOD **v6** already installed for Claude Code:

| Path | Content |
|---|---|
| `_bmad/` | BMAD core + `bmm` module (installer-managed; customize only in `_bmad/custom/`) |
| `.claude/skills/bmad-*` | BMAD skills exposed to Claude Code |
| `_bmad-output/` | Artifacts produced by the workflows (git-ignored) |

#### Install or update
```bash
# Update the existing installation (non-interactive)
npx bmad-method@latest install --directory . --modules bmm --tools claude-code --action update --yes

# Restart Claude Code afterwards: skills are loaded at startup
```

#### Getting started
1. Open Claude Code in the project folder.
2. Ask for `bmad-help`: it inspects the project state and recommends the next skill.
3. Typical flow: `bmad-product-brief` → `bmad-prd` → `bmad-ux` → `bmad-architecture` →
   `bmad-create-epics-and-stories` → `bmad-sprint-planning` → `bmad-build` → `bmad-code-review` → `bmad-retrospective`.
4. Agent personas: `bmad-agent-analyst`, `bmad-agent-pm`, `bmad-agent-ux-designer`, `bmad-agent-architect`, `bmad-agent-dev`.

Personal answers (name, communication language) live in `_bmad/config.user.toml`; it and the generated
`_bmad/*/config.yaml` are git-ignored. Team overrides go in
`_bmad/custom/config.toml`; use the `bmad-customize` skill to author them.

## Usage Monitoring & Costs

Track your Claude Code usage and costs to manage your AI development budget effectively.

### Install UV Package Manager

First, install `uv` - a fast Python package installer:
```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

### Install Claude Monitor

Install the [Claude Code Usage Monitor](https://github.com/Maciek-roboblog/Claude-Code-Usage-Monitor) for real-time usage tracking:
```bash
uv tool install claude-monitor
```

### Real-Time Usage Monitoring

Monitor your Claude usage in real-time with your specific plan and timezone:
```bash
# For Claude Pro Max plan in Europe/Helsinki timezone
claude-monitor --plan max20 --timezone Europe/Helsinki

# Adjust plan and timezone as needed:
# Plans: pro5, max20 (number represents daily message limit)
# Timezone: Your local timezone (e.g., America/New_York, Asia/Tokyo)
```

### Daily Usage Reports

Get daily usage summaries using ccusage:
```bash
npx ccusage@latest
```

### Monitoring Best Practices

- **Set up monitoring early**: Install monitoring tools before heavy development sessions
- **Check usage regularly**: Review daily reports to track consumption patterns
- **Plan usage accordingly**: Use monitoring data to optimize your development workflow
- **Consider timezone settings**: Accurate timezone ensures correct daily reset tracking

## Troubleshooting

### Container Won't Start
- Ensure Docker Desktop is running
- Check Docker Desktop settings for resource allocation (CPU, Memory)
- Try: `docker system prune` to clean up Docker resources

### Permission Issues
- The container runs as a non-root user by default
- To run as root, uncomment in `.devcontainer/devcontainer.json`:
  ```json
  "remoteUser": "root"
  ```

### Slow Performance
- Increase Docker Desktop resource limits
- Windows: Ensure WSL 2 is enabled for better performance
- macOS: Check Docker Desktop preferences for resource allocation

### Extension Issues
- Extensions are installed in the container, not locally
- To add more extensions, modify `.devcontainer/devcontainer.json`:
  ```json
  "customizations": {
    "vscode": {
      "extensions": [
        "extension.id.here"
      ]
    }
  }
  ```

### BMAD-Method Issues

```bash
# Installation fails: clear the npm cache and retry
npm cache clean --force
npx bmad-method@latest install --directory . --modules bmm --tools claude-code --yes

# Skills missing in Claude Code: restart Claude Code; check the skills exist
ls .claude/skills | grep bmad-
```

- BMAD requires Node.js 20+ (`node --version`); the container includes it.
- Do not edit files under `_bmad/` outside `_bmad/custom/`: the installer overwrites them.

### Usage Monitoring Issues

#### Claude Monitor Installation Problems
```bash
# If uv installation fails, try alternative method
pip install uv
uv tool install claude-monitor

# Or install directly with pip
pip install claude-monitor
```

#### Monitoring Command Issues
```bash
# If claude-monitor command not found, check PATH
echo $PATH
which claude-monitor

# Refresh shell after uv installation
source ~/.bashrc  # or ~/.zshrc
```

#### Timezone Configuration Problems
- Use standard timezone format: `Area/City` (e.g., `Europe/Helsinki`, `America/New_York`)
- List available timezones: `timedatectl list-timezones` (Linux)
- For Windows WSL: Use Linux timezone format

#### Usage Tracking Not Working
- Ensure Claude Code is running when using monitoring tools
- Check if monitoring tools have proper permissions
- Verify Claude Code authentication is active

## Resources

### Development Container
- [VS Code Remote Containers Documentation](https://code.visualstudio.com/docs/remote/containers)
- [Dev Container Specification](https://containers.dev/)
- [Docker Desktop Documentation](https://docs.docker.com/desktop/)

### BMAD-Method
- [BMAD-Method Repository](https://github.com/bmad-code-org/BMAD-METHOD)
- [BMAD User Guide](https://github.com/bmad-code-org/BMAD-METHOD/blob/main/docs/user-guide.md)
- [Discord Community](https://discord.gg/gk8jAdXWmj)

### Additional Tools
- [Claude Code Documentation](https://github.com/anthropics/claude-code-action/blob/main/docs/setup.md)
- [GitHub CLI Documentation](https://cli.github.com/manual/)

### Usage Monitoring Tools
- [Claude Code Usage Monitor](https://github.com/Maciek-roboblog/Claude-Code-Usage-Monitor) - Real-time usage tracking
- [UV Package Manager](https://astral.sh/uv/) - Fast Python package installer
- [ccusage](https://www.npmjs.com/package/ccusage) - Daily usage reports


