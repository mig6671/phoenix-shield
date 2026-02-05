# PhoenixShield 🔥🛡️

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Bash](https://img.shields.io/badge/bash-4.0+-blue.svg)](https://www.gnu.org/software/bash/)
[![OpenClaw Skill](https://img.shields.io/badge/OpenClaw-Skill-orange.svg)](https://openclaw.ai)

> **"Like the Phoenix, your system rises from its own backup"**

Self-healing backup and update system with intelligent rollback capabilities.

---

## 🎯 The Problem

System updates can fail, leaving services broken and causing costly downtime:

```
Production Server
    ↓
Running update...
    ↓
❌ Update fails!
    ↓
System broken 😱
    ↓
Manual recovery (hours?)
```

**Real-world impact:**
- Hours of downtime during recovery
- Lost revenue and customer trust
- Manual, error-prone rollback processes
- Stressful emergency situations

---

## 💡 The Solution

**PhoenixShield** provides a complete safety net with automatic recovery when things go wrong.

```
Production Server
    ↓
PhoenixShield: Pre-flight checks ✅
    ↓
PhoenixShield: Create snapshot 💾
    ↓
Running update...
    ↓
❌ Update fails!
    ↓
PhoenixShield: Auto-rollback 🔄
    ↓
System restored ✅
    ↓
PhoenixShield: 24h monitoring 📊
```

**Downtime: Minutes instead of hours** | **Recovery: Automatic** | **Stress: Zero**

---

## ✨ Key Features

### 🔄 **Automatic Recovery**
- Self-heals when updates fail
- No manual intervention required
- Multiple recovery strategies

### 🧪 **Canary Testing**
- Test updates before production
- Isolated test environment
- Catch failures early

### 📊 **24/7 Monitoring**
- 4-phase post-update monitoring
- Early warning system
- Stability tracking

### ⚡ **Smart Rollback**
- Only revert changed components
- Preserve user data
- Minimize downtime

### 🛡️ **Zero-Downtime**
- Graceful degradation when possible
- Service-by-service recovery
- Keep critical systems running

---

## 📦 Installation

### From ClawHub (Recommended)
```bash
clawhub install phoenix-shield
```

### From Source
```bash
git clone https://github.com/mig6671/phoenix-shield.git
cd phoenix-shield
chmod +x scripts/phoenix-shield
sudo cp scripts/phoenix-shield /usr/local/bin/
```

---

## 🚀 Quick Start

### 1. Initialize PhoenixShield

```bash
phoenix-shield init --project myapp --backup-dir /var/backups
```

### 2. Create Pre-Update Snapshot

```bash
phoenix-shield snapshot --name "pre-update-$(date +%Y%m%d)"
```

### 3. Safe Update with Auto-Recovery

```bash
phoenix-shield update \
  --command "npm update" \
  --health-check "curl -f http://localhost/health" \
  --auto-rollback
```

### 4. Monitor Post-Update

```bash
phoenix-shield monitor --duration 24h --interval 5m
```

---

## 🎬 Demo

```bash
# Initialize for your project
$ phoenix-shield init production-api
🔥 Initializing PhoenixShield for project: production-api
✅ PhoenixShield initialized
   Config: ./phoenix-shield.yaml
   Backups: /var/backups/phoenix/production-api

# Run pre-flight checks
$ phoenix-shield preflight
🧪 Running pre-flight checks...
✅ Disk space OK (52GB available)
✅ Backup directory writable
✅ No critical build processes detected
✅ Network connectivity OK
✅ All pre-flight checks passed

# Create snapshot
$ phoenix-shield snapshot
📸 Creating snapshot: pre-deploy-20260205
✅ Snapshot created

# Deploy with protection
$ phoenix-shield deploy \
  --command "apt upgrade -y" \
  --health-check "systemctl status nginx" \
  --rollback-on-failure
🚀 Starting safe deployment
   Command: apt upgrade -y
✅ All pre-flight checks passed
   Snapshot created: /var/backups/phoenix/snapshots/pre-deploy-20260205
✅ Command executed successfully
✅ Health checks passed
   Starting post-deployment monitoring...
✅ Deployment complete
   Monitor: Run 'phoenix-shield status' to check
```

---

## 🔥 Real-World Use Cases

### Safe OpenClaw Update

```bash
#!/bin/bash
# Update OpenClaw with PhoenixShield protection

phoenix-shield preflight || exit 1

phoenix-shield snapshot --name "openclaw-$(date +%Y%m%d)"

phoenix-shield deploy \
  --command "npm install -g openclaw@latest && cd /usr/lib/node_modules/openclaw && npm update" \
  --health-check "openclaw --version" \
  --health-check "openclaw doctor" \
  --rollback-on-failure

phoenix-shield monitor --duration 2h
```

### Ubuntu Server Update

```bash
phoenix-shield deploy \
  --command "apt update && apt upgrade -y" \
  --health-check "systemctl status nginx" \
  --health-check "systemctl status mysql" \
  --pre-hook "/root/notify-start.sh" \
  --post-hook "/root/notify-complete.sh" \
  --auto-rollback
```

### Multi-Server Deployment

```bash
# Update multiple servers with PhoenixShield
SERVERS="server1 server2 server3"

for server in $SERVERS; do
  phoenix-shield deploy \
    --target "$server" \
    --command "apt upgrade -y" \
    --batch-size 1 \
    --rollback-on-failure
done
```

---

## 📚 Commands Reference

| Command | Description |
|---------|-------------|
| `init` | Initialize PhoenixShield for project |
| `snapshot` | Create system snapshot |
| `backup` | Create backup (full/incremental) |
| `preflight` | Run pre-update checks |
| `canary` | Test update in isolated environment |
| `deploy` | Execute update with protection |
| `monitor` | Start post-update monitoring |
| `rollback` | Rollback to previous state |
| `status` | Show current status |
| `history` | Show update history |
| `verify` | Verify backup integrity |

---

## ⚙️ Configuration

Create `phoenix-shield.yaml`:

```yaml
project: my-production-app
backup:
  directory: /var/backups/phoenix
  retention: 10  # Keep last 10 backups
  compression: gzip

health_checks:
  - command: "curl -f http://localhost/health"
    interval: 30s
    retries: 3
  - command: "systemctl status nginx"
    interval: 60s

monitoring:
  enabled: true
  duration: 24h
  intervals:
    critical: 1m    # 0-5 min
    normal: 5m      # 5-30 min
    extended: 30m   # 30-120 min
    stability: 2h   # 2-24h

rollback:
  strategy: smart  # smart, full, manual
  auto_rollback: true
  max_attempts: 3

notifications:
  on_start: true
  on_success: true
  on_failure: true
  on_rollback: true
```

---

## 🏗️ Architecture

```
┌─────────────────────────────────────┐
│        PhoenixShield Core           │
├─────────────────────────────────────┤
│ PreFlight │ Deploy │ Monitor │ Roll │
├─────────────────────────────────────┤
│   Backup Engine  │  Health Engine   │
├─────────────────────────────────────┤
│      Snapshots   │   Recovery       │
├─────────────────────────────────────┤
│   Config │ State │ Logs │ Metrics   │
└─────────────────────────────────────┘
```

### The 5 Recovery Strategies

1. **Soft Recovery** - Restart services
2. **Config Recovery** - Revert configuration
3. **Package Recovery** - Downgrade packages
4. **Full Recovery** - Complete system restore
5. **Emergency Mode** - Minimal services, notify admin

---

## 🎓 Best Practices

### Always Use Preflight
```bash
# Bad
phoenix-shield deploy --command "apt upgrade"

# Good
phoenix-shield preflight && \
phoenix-shield deploy --command "apt upgrade"
```

### Test Rollback Before Production
```bash
phoenix-shield snapshot --name test
phoenix-shield deploy --command "echo test"
phoenix-shield rollback --dry-run  # See what would happen
```

### Monitor Critical Updates
```bash
phoenix-shield deploy --command "major-update.sh"
phoenix-shield monitor --duration 48h  # Extended monitoring
```

---

## 🔒 Security

- Backups are encrypted at rest
- Integrity verification with checksums
- Secure handling of credentials
- Audit trail for all operations

---

## 🤝 Contributing

We welcome contributions! See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

---

## 📄 License

MIT License - Free for personal and commercial use.

---

## 🔗 Links

- **ClawHub:** https://clawhub.com/skills/phoenix-shield
- **GitHub:** https://github.com/mig6671/phoenix-shield
- **Author:** @mig6671

---

<p align="center">
  <strong>Like the Phoenix, your system rises from backup 🔥🛡️</strong>
</p>
