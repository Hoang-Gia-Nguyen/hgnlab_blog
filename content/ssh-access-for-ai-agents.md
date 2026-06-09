+++
title = "SSH Access for AI Agents and Automation Tools — A Quick Security Guide"
date = 2026-06-09
draft = false
description = "A practical guide to safely provisioning SSH access for AI coding agents and automation tools using dedicated users and temporary key pairs."
authors = ["hgn"]
[taxonomies]
categories = ["Self-hosting"]
tags = ["ssh", "security", "ai-agents", "automation", "devops"]

[extra]
#cover.image = "images/ssh-access-for-ai-agents-cover.png"
#cover.alt = "Illustration of an AI agent connecting to a server via SSH"
+++

You've built a homelab server, and now you want AI agents (like Codex CLI, Cursor, or GitHub Copilot) or automation tools (Ansible, CI/CD runners) to help you manage it. But giving them full SSH access with your personal key? That's a risk you don't need to take.

This guide walks through a quick, repeatable security pattern: **dedicated user + temporary key pair + restricted access**. Follow these steps every time you provision access for a tool or agent.

## The Threat

The risk isn't about trust — it's about **blast radius**. If an AI agent's session is compromised, or a CI pipeline leaks credentials, an attacker could:

- Wipe your entire server
- Steal sensitive data (configs, passwords, database dumps)
- Use your server as a launchpad for attacks

Your personal SSH key grants full access to everything your user can do. A dedicated user with limited permissions contains the damage to a single, controlled scope.

## Step 1: Create a Dedicated User

Pick a name that identifies *what* is connecting. Never use a personal account.

```bash
# Create a user without a home directory initially
sudo useradd -m -s /bin/bash ai-agent

# Optional: lock password to prevent login via password
sudo passwd -l ai-agent
```

The `-m` flag creates a home directory at `/home/ai-agent/`, which is where you'll place the SSH authorized keys.

> **Pro tip:** For stricter setups, use `-s /usr/sbin/nologin` to prevent shell access entirely — the agent will only be able to run commands specified in `authorized_keys` via `command=` restrictions.

## Step 2: Restrict What the User Can Do

### Option A: Lock the user to specific commands

Edit the user's `~/.ssh/authorized_keys` file and prefix the public key with a `command=` restriction:

```bash
# This key can only run ~/allowed-scripts/deploy.sh
command="/home/ai-agent/allowed-scripts/deploy.sh",no-port-forwarding,no-X11-forwarding,no-agent-forwarding ssh-ed25519 AAAAC3...
```

Drop the agent into its own directory and only give write access where needed:

```bash
sudo mkdir -p /home/ai-agent/allowed-scripts
sudo chown -R ai-agent:ai-agent /home/ai-agent
sudo chmod 755 /home/ai-agent
sudo chmod 750 /home/ai-agent/.ssh
```

### Option B: Restrict with `Match` blocks in `sshd_config`

```bash
sudo tee -a /etc/ssh/sshd_config << 'EOF'

# Restrict ai-agent user
Match User ai-agent
    AllowTcpForwarding no
    X11Forwarding no
    PermitTTY no
    ForceCommand /usr/sbin/nologin
EOF

sudo systemctl restart sshd
```

## Step 3: Generate a Temporary Key Pair

Create a **dedicated key pair** for this specific agent or tool. Never reuse keys across tools.

```bash
# Generate an Ed25519 key — faster and more secure than RSA
ssh-keygen -t ed25519 -f ~/temp-ssh-keys/ai-agent-key -C "ai-agent-temp-20260609" -N ""
```

This creates two files:

- `~/temp-ssh-keys/ai-agent-key` — **private key** (give this to the agent)
- `~/temp-ssh-keys/ai-agent-key.pub` — public key (goes on the server)

> **Why Ed25519?** It's the modern default. Smaller keys, faster authentication, and excellent security. Only fall back to RSA (`-t rsa -b 4096`) if the remote system is old.

## Step 4: Copy the Public Key to the Target Machine

```bash
# Copy the public key into the user's authorized_keys
sudo mkdir -p /home/ai-agent/.ssh
cat ~/temp-ssh-keys/ai-agent-key.pub | sudo tee -a /home/ai-agent/.ssh/authorized_keys
sudo chown -R ai-agent:ai-agent /home/ai-agent/.ssh
sudo chmod 700 /home/ai-agent/.ssh
sudo chmod 600 /home/ai-agent/.ssh/authorized_keys
```

### One-liner for remote servers

If you're setting this up from another machine:

```bash
# From your local machine
ssh-copy-id -i ~/temp-ssh-keys/ai-agent-key.pub ai-agent@your-server-ip
```

Or manually:

```bash
# Copy the public key and append it remotely
cat ~/temp-ssh-keys/ai-agent-key.pub | ssh admin@your-server-ip "sudo tee -a /home/ai-agent/.ssh/authorized_keys"
```

## Step 5: Test the Connection

Before handing the key to any agent, verify the setup works:

```bash
ssh -i ~/temp-ssh-keys/ai-agent-key ai-agent@your-server-ip
```

If you've locked down the user, expect a command restriction or a denied shell — that's the desired behavior. The agent should still be able to run its specific commands.

## Step 6: Hand the Private Key to the Agent

Now you can give the **private key file path** to your AI agent or automation tool.

**For AI coding agents** (like Codex CLI, Cursor, Claude Code):

```bash
# Example: tell the agent to use this key
codex --ssh-key ~/temp-ssh-keys/ai-agent-key
```

**For Ansible:**

```ini
# ansible.cfg or inventory
[defaults]
private_key_file = ~/temp-ssh-keys/ai-agent-key
```

**For CI/CD runners** (GitHub Actions, GitLab CI):

```bash
# Store the private key as a CI secret (e.g., SSH_PRIVATE_KEY)
# Then in your pipeline:
- name: Setup SSH
  run: |
    mkdir -p ~/.ssh
    echo "${{ secrets.SSH_PRIVATE_KEY }}" > ~/.ssh/ci-key
    chmod 600 ~/.ssh/ci-key
```

## Security: Protecting the Private Key

The private key is the **crown jewel**. Treat it like a password:

- **Never paste the key content into a chat prompt.** Even with trusted LLM providers, you're exfiltrating credential material.
- **Instruct the agent not to read or print the key.** Most AI coding tools accept an `--ssh-key` flag and handle the file internally. If the agent asks for the key contents, tell it to use the file path instead.
- **Set restrictive permissions:**

  ```bash
  chmod 600 ~/temp-ssh-keys/ai-agent-key
  ```

- **Rotate regularly.** Generate a fresh key pair for each session or each CI run. With the one-liner above, this takes 30 seconds.
- **Revoke when done.** Remove the user or the key from `authorized_keys` when the task is complete:

  ```bash
  # Remove the specific key
  sudo sed -i '/ai-agent-temp-20260609/d' /home/ai-agent/.ssh/authorized_keys

  # Or remove the entire user
  sudo userdel -r ai-agent
  ```

## Quick Reference — Full Setup in One Block

```bash
# === On the target server ===

# 1. Create user
sudo useradd -m -s /bin/bash ai-agent
sudo passwd -l ai-agent

# 2. Set up SSH directory
sudo mkdir -p /home/ai-agent/.ssh

# 3. Add the public key (replace with your actual pub key)
echo "ssh-ed25519 AAAAC3..." | sudo tee /home/ai-agent/.ssh/authorized_keys

# 4. Fix permissions
sudo chown -R ai-agent:ai-agent /home/ai-agent/.ssh
sudo chmod 700 /home/ai-agent/.ssh
sudo chmod 600 /home/ai-agent/.ssh/authorized_keys

# === On your local machine ===

# 5. Generate a temp key
ssh-keygen -t ed25519 -f ~/temp-ssh-keys/ai-agent-key -C "ai-agent-$(date +%Y%m%d)" -N ""

# 6. Test it
ssh -i ~/temp-ssh-keys/ai-agent-key ai-agent@your-server

# 7. Hand the key to your agent
#    Agent uses: ~/temp-ssh-keys/ai-agent-key
```

## Recap

- **Always use a dedicated user** — never your personal account.
- **Generate one key pair per tool** — never reuse keys.
- **Restrict what the user can do** — `command=` restrictions or `Match` blocks.
- **Protect the private key** — file path only, never paste the content.
- **Rotate and revoke** — keys are temporary, not permanent.

This pattern takes five minutes to set up and saves you from a world of pain if something goes wrong. Your server stays safe, and your AI agents get the access they need.
