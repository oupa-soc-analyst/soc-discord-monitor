# 🛡️ SOC Discord Monitor — Production SIEM Pipeline

> Real-time threat detection for SSH brute-force, PII leakage, phishing URLs, and TOR exit node activity — confirmed live in production.

![Python](https://img.shields.io/badge/Python-3.11-blue?style=flat-square&logo=python)
![Wazuh](https://img.shields.io/badge/SIEM-Wazuh-orange?style=flat-square)
![Splunk](https://img.shields.io/badge/Threat_Hunting-Splunk_SPL-green?style=flat-square)
![Status](https://img.shields.io/badge/Status-Live_In_Production-brightgreen?style=flat-square)
![TryHackMe](https://img.shields.io/badge/TryHackMe-Top_20%25_Global-red?style=flat-square)

---

## 📌 What This Does

This is a production-deployed SOC monitoring pipeline built entirely by a self-taught analyst. It watches for real threats and fires Discord alerts in real time — no enterprise budget, no team, no formal IT background.

| Threat Type | Detection Method | Response |
|---|---|---|
| SSH Brute-Force | 8+ failed logins threshold | Discord alert + iptables block |
| TOR Exit Node Traffic | IP matched against TOR list | Discord CRITICAL alert |
| PII Leakage | Regex: SSN / Credit Card / Email | Discord CRITICAL alert |
| Phishing URLs | VirusTotal API scan | Discord HIGH alert |

---

## 🏗️ Architecture

```
Log Sources
    │
    ▼
Wazuh SIEM (Rule 1001)
    │
    ├──► PowerShell Webhook Script
    │         │
    │         ▼
    │    Discord Alerts
    │    (severity-coded embeds)
    │
    ▼
Python Monitor (soc_monitor.py)
    │
    ├──► PII Regex Engine (SSN / CC / Email)
    ├──► VirusTotal API (URL scanning)
    ├──► TOR Exit Node Checker
    ├──► Brute-force Counter (threshold: 8 fails)
    │
    ▼
SQLite Incident Log
    │
    └──► !incidents audit command
```

---

## ⚡ Alert Examples

**CRITICAL — PII Leak Detected**
```
🔴 [CRITICAL] PII LEAK DETECTED
Type    : SSN Pattern
Source  : auth.log / process: webapp
Time    : 2025-03-14 09:42:11 UTC
Action  : Logged to incidents DB
```

**HIGH — SSH Brute-Force from TOR Node**
```
🟠 [HIGH] BRUTE-FORCE DETECTED
Source IP  : 185.230.122.10
TOR Node   : ✅ Confirmed
Fail Count : 14 attempts
Rule       : Wazuh 1001
Action     : iptables DROP applied
```

---

## 🔧 Tech Stack

- **SIEM:** Wazuh (Rule 1001), Splunk SPL
- **Alerting:** Discord Webhooks (severity-coded embeds)
- **Scripting:** Python 3.11, PowerShell
- **Detection:** Regex (PII/SSN/CC), VirusTotal API, TOR IP lists
- **Response:** iptables, fail2ban
- **Logging:** SQLite + `!incidents` audit command
- **Packaging:** uv (Python)

---

## 📂 Project Structure

```
soc-discord-monitor/
├── soc_monitor.py          # Main monitoring engine
├── pii_detector.py         # Regex patterns: SSN, CC, email
├── virustotal_scan.py      # Phishing URL checking
├── tor_checker.py          # TOR exit node lookup
├── wazuh_webhook.ps1       # PowerShell → Discord bridge
├── playbooks/
│   ├── ssh_bruteforce.md   # Incident response playbook
│   ├── pii_leak.md         # PII response playbook
│   └── phishing.md         # Phishing response playbook
├── incidents.db            # SQLite log (auto-generated)
└── README.md
```

---

## 🚀 Setup

```bash
# Clone
git clone https://github.com/oupa-soc-analyst/soc-discord-monitor
cd soc-discord-monitor

# Install dependencies
pip install -r requirements.txt

# Set your Discord webhook
export DISCORD_WEBHOOK=your_webhook_url

# Run
python soc_monitor.py
```

---

## 📋 Incident Response Playbooks

Documented playbooks included in `/playbooks/`:

- **SSH Brute-Force:** Detect → Identify → Block (iptables) → Verify TOR → fail2ban → Document
- **PII Leak:** Alert → Identify source process → Isolate → Preserve logs → Escalate → POPIA compliance
- **Phishing URL:** VirusTotal check → WHOIS → AbuseIPDB → DNS block → Alert users

---

## 👤 About

Built by **Oupa Nelson Nyoni** — self-taught SOC analyst from Limpopo, South Africa.

- 🎯 TryHackMe: [tryhackme.com/p/madipapa](https://tryhackme.com/p/madipapa) — Top 20% globally, Level 6 Voyager, 24 badges
- 💼 LinkedIn: [linkedin.com/in/oupa-nyoni-4b9b45245](https://linkedin.com/in/oupa-nyoni-4b9b45245)
- 📧 oupanelson547@gmail.com

> *"I built this because I couldn't afford to wait for a job to start learning. So I built the job first."*

---

## ⚠️ Legal

This tool is built for defensive security monitoring of systems you own or have written permission to monitor. Unauthorised use against third-party systems is illegal.
