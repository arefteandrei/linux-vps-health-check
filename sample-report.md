# Linux/VPS Health Check — Sample Report

## Client

Demo Server / Sample Environment

## Date

YYYY-MM-DD

## Scope

This report covers a basic operational health check for a Linux VPS server.

The review includes:

* System overview
* CPU, RAM, swap, uptime, and load
* Disk usage and inode usage
* Running and failed services
* Open ports and exposed services
* SSH configuration
* Firewall status
* Available updates
* Logs and recurring errors
* Nginx/Apache basic checks
* SSL certificate status
* Docker/Docker Compose checks, if applicable
* Backup observations
* Documentation gaps
* Practical recommendations

This is not a full penetration test, compliance audit, managed security assessment, or managed IT contract.

---

## 1. Executive summary

The server is operational, but several areas require attention.

The most important findings are related to SSH access configuration, exposed services, missing backup validation, package updates, Docker restart policies, and incomplete operational documentation.

No critical outage was observed during this sample review. However, the server would benefit from basic hardening, clearer backup validation, and improved documentation.

---

## 2. Server overview

| Item              | Status                  |
| ----------------- | ----------------------- |
| Operating system  | Ubuntu Server XX.XX     |
| Kernel            | x.x.x                   |
| Uptime            | X days                  |
| Main role         | Web application server  |
| Web server        | Nginx                   |
| Container runtime | Docker                  |
| Public IP         | Present                 |
| Firewall          | Active / Inactive       |
| SSH access        | Enabled                 |
| Backup evidence   | Present / Not confirmed |
| Monitoring        | Basic / Not confirmed   |
| Documentation     | Partial / Missing       |

---

## 3. Overall status

| Area             | Status       | Notes                                      |
| ---------------- | ------------ | ------------------------------------------ |
| System resources | OK           | No immediate CPU/RAM issue observed        |
| Disk usage       | Warning      | Disk usage should be monitored             |
| Services         | OK           | No critical failed service observed        |
| SSH access       | Warning      | Password authentication should be reviewed |
| Firewall         | Warning      | Exposed ports should be confirmed          |
| Updates          | Warning      | Pending updates should be reviewed         |
| Logs             | Warning      | Recurring warnings should be checked       |
| Web server       | OK           | Nginx is running                           |
| SSL              | OK / Warning | Certificate validity should be confirmed   |
| Docker           | Warning      | Restart policies should be reviewed        |
| Backups          | Warning      | Restore validation not confirmed           |
| Documentation    | Warning      | Operational documentation is incomplete    |

---

## 4. Findings

## Critical findings

No critical findings were observed in this sample report.

---

## High priority findings

### 1. SSH password authentication is enabled

**Risk:**
If password authentication is enabled, the server may be more exposed to brute-force login attempts.

**Recommendation:**
Confirm key-based SSH access, then disable password authentication if it is not required.

**Suggested action:**
Review `/etc/ssh/sshd_config` and validate the effective SSH configuration using:

```bash
sudo sshd -T
```

---

### 2. Backup validation was not confirmed

**Risk:**
Backups may exist, but they may not be usable during a real incident if restore procedures were never tested.

**Recommendation:**
Define and test a restore procedure.

**Suggested action:**

* Identify backup location.
* Confirm backup schedule.
* Confirm retention period.
* Perform a controlled restore test.
* Document the restore process.

---

## Medium priority findings

### 1. Several packages require updates

**Risk:**
Pending updates may include security fixes and stability improvements.

**Recommendation:**
Review available updates and apply them during a controlled maintenance window.

**Suggested action:**

For Ubuntu/Debian systems:

```bash
sudo apt update
apt list --upgradable
```

---

### 2. Docker containers do not all have restart policies

**Risk:**
Some containers may not restart automatically after a server reboot, Docker restart, or container failure.

**Recommendation:**
Add appropriate restart policies in `docker-compose.yml`.

**Example:**

```yaml
restart: unless-stopped
```

---

### 3. Server documentation is incomplete

**Risk:**
Troubleshooting, handover, and recovery become harder when the server has no clear documentation.

**Recommendation:**
Create a short operational README for the server.

The README should include:

* Server purpose
* Main applications
* Domains
* Public IP
* Main services
* Important paths
* Deployment steps
* Backup method
* Restore procedure
* Access method
* Known risks

---

### 4. Firewall rules require review

**Risk:**
Unnecessary exposed ports increase the attack surface.

**Recommendation:**
Expose only the required ports.

Common examples:

| Port | Service    | Usually required?                            |
| ---- | ---------- | -------------------------------------------- |
| 22   | SSH        | Yes, but should be restricted where possible |
| 80   | HTTP       | Yes, for web servers                         |
| 443  | HTTPS      | Yes, for web servers                         |
| 3306 | MySQL      | Usually no public exposure                   |
| 5432 | PostgreSQL | Usually no public exposure                   |
| 6379 | Redis      | Usually no public exposure                   |

---

## Low priority findings

### 1. Log rotation should be reviewed

**Risk:**
Large logs can consume disk space and cause service issues.

**Recommendation:**
Review `/etc/logrotate.conf` and `/etc/logrotate.d/`.

---

### 2. Unused packages should be reviewed

**Risk:**
Unused packages increase maintenance overhead and may increase the attack surface.

**Recommendation:**
Review installed packages and remove only what is clearly unused and safe to remove.

---

### 3. Naming conventions should be improved

**Risk:**
Unclear directory, service, or container names make troubleshooting harder.

**Recommendation:**
Use clear names for services, containers, directories, and deployment files.

---

## 5. Recommendations summary

| Priority | Recommendation                                                  |
| -------- | --------------------------------------------------------------- |
| High     | Confirm SSH key-based access and review password authentication |
| High     | Validate backup and restore process                             |
| Medium   | Review and apply pending updates                                |
| Medium   | Confirm firewall rules and exposed ports                        |
| Medium   | Add Docker restart policies                                     |
| Medium   | Improve operational documentation                               |
| Low      | Review log rotation                                             |
| Low      | Review unused packages                                          |
| Low      | Standardize naming conventions                                  |

---

## 6. Technical appendix

The following checks were included in the review:

### System

* Operating system and version
* Kernel version
* Hostname
* Uptime
* Logged-in users
* Timezone
* Reboot requirement

### Resources

* CPU load
* RAM usage
* Swap usage
* Disk usage
* Inode usage
* Large files and directories

### Services

* Running services
* Failed systemd units
* Enabled services
* Systemd timers
* Recent service logs

### Network

* IP configuration
* Routing
* Listening ports
* Exposed services
* DNS resolution
* Active connections

### SSH and access

* SSH effective configuration
* SSH port
* Root login status
* Password authentication status
* Existing users with shell access
* Sudo/wheel group members
* Recent logins
* Failed login attempts

### Firewall

* UFW status
* iptables rules
* nftables rules
* firewalld status, if applicable

### Updates

* Available package updates
* Security update visibility
* Package manager logs
* Held packages, if applicable

### Logs

* Recent system errors
* Recent warnings
* Kernel messages
* Authentication logs
* Web server logs
* Log directory size
* Log rotation configuration

### Web server

* Nginx/Apache status
* Configuration test
* Enabled sites
* Access logs
* Error logs
* Server blocks / virtual hosts

### SSL

* Certificate validity dates
* Certificate issuer and subject
* Certbot certificates, if applicable
* Renewal timer, if applicable

### Docker

* Docker service status
* Running containers
* Stopped containers
* Images
* Volumes
* Networks
* Disk usage
* Compose services
* Container logs
* Restart policies

### Backups

* Backup scripts
* Backup cron jobs
* Backup directories
* Recent backup files
* Backup tooling, if present
* Restore documentation

---

## 7. Final note

The server is suitable for continued use after the recommended operational improvements are reviewed and applied.

The most important next steps are:

1. Reduce SSH access risk.
2. Confirm firewall exposure.
3. Validate backup and restore reliability.
4. Apply updates during a maintenance window.
5. Improve server documentation.
6. Add Docker restart policies where needed.

This report provides an operational snapshot and should be reviewed together with the server owner, IT provider, web agency, or development team responsible for the environment.
