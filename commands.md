# Linux/VPS Health Check Commands

This file contains example commands used during a basic Linux/VPS operational health check.

The commands are intended for read-only investigation and documentation. They should be adapted depending on the Linux distribution, server role, access level, and agreed client scope.

Important notes:

* Do not run commands blindly on production systems.
* Use `sudo` only when you have explicit permission.
* Avoid destructive commands unless they are clearly approved.
* Replace `example.com` with the real domain when checking SSL.
* Some commands may not be available on all distributions.

---

## 1. System overview

```bash
hostnamectl
uname -a
uptime
who
timedatectl
```

Check current user and privileges:

```bash
whoami
id
```

Check operating system release:

```bash
cat /etc/os-release
```

Check if a reboot is required on Ubuntu/Debian:

```bash
test -f /var/run/reboot-required && cat /var/run/reboot-required
```

---

## 2. CPU, RAM, swap, and load

```bash
uptime
free -h
vmstat 1 5
```

Interactive process view:

```bash
top
```

Alternative, if installed:

```bash
htop
```

Top processes by memory usage:

```bash
ps aux --sort=-%mem | head -n 15
```

Top processes by CPU usage:

```bash
ps aux --sort=-%cpu | head -n 15
```

---

## 3. Disk usage and filesystems

```bash
df -h
df -i
lsblk
mount
```

Check disk usage by top-level directories:

```bash
sudo du -h --max-depth=1 / 2>/dev/null | sort -h
```

Check disk usage in common application paths:

```bash
sudo du -h --max-depth=1 /var 2>/dev/null | sort -h
sudo du -h --max-depth=1 /var/log 2>/dev/null | sort -h
sudo du -h --max-depth=1 /home 2>/dev/null | sort -h
sudo du -h --max-depth=1 /opt 2>/dev/null | sort -h
sudo du -h --max-depth=1 /srv 2>/dev/null | sort -h
sudo du -h --max-depth=1 /var/www 2>/dev/null | sort -h
```

Find large files in common locations:

```bash
sudo find /var /home /opt /srv /var/www -type f -size +500M -exec ls -lh {} \; 2>/dev/null
```

Check log size:

```bash
sudo du -h --max-depth=1 /var/log 2>/dev/null | sort -h
```

---

## 4. Services and systemd

Check failed services:

```bash
systemctl --failed
```

List running services:

```bash
systemctl list-units --type=service --state=running
```

List enabled services:

```bash
systemctl list-unit-files --type=service --state=enabled
```

Check timers:

```bash
systemctl list-timers
```

Check a specific service:

```bash
systemctl status service-name
```

View recent logs for a specific service:

```bash
journalctl -u service-name --since "24 hours ago"
```

View boot logs with errors:

```bash
journalctl -p err -xb
```

View warnings and errors from the last 24 hours:

```bash
journalctl --since "24 hours ago" -p warning
```

---

## 5. Network configuration

```bash
ip a
ip route
ss -tulpen
```

Check listening TCP/UDP ports:

```bash
sudo ss -tulpen
```

Check active connections:

```bash
ss -tunap
```

Check DNS resolver status:

```bash
resolvectl status
```

Alternative DNS check:

```bash
cat /etc/resolv.conf
```

Test DNS resolution:

```bash
getent hosts example.com
```

Test outbound connectivity:

```bash
ping -c 4 8.8.8.8
ping -c 4 example.com
```

Trace route, if installed:

```bash
traceroute example.com
```

---

## 6. Firewall status

Ubuntu/Debian with UFW:

```bash
sudo ufw status verbose
```

iptables:

```bash
sudo iptables -L -n -v
```

nftables:

```bash
sudo nft list ruleset
```

firewalld:

```bash
sudo firewall-cmd --state
sudo firewall-cmd --list-all
```

---

## 7. SSH and access checks

Check SSH effective configuration:

```bash
sudo sshd -T | grep -E 'port|permitrootlogin|passwordauthentication|pubkeyauthentication|challengeresponseauthentication|usepam'
```

Check SSH config file:

```bash
sudo grep -Ei 'port|permitrootlogin|passwordauthentication|pubkeyauthentication|allowusers|denyusers' /etc/ssh/sshd_config
```

List users with login shells:

```bash
awk -F: '$7 ~ /(bash|sh|zsh)$/ {print $1, $3, $6, $7}' /etc/passwd
```

Check sudo group members:

```bash
getent group sudo
```

Check wheel group members, common on RHEL-based systems:

```bash
getent group wheel
```

Check recent logins:

```bash
last
```

Check failed login attempts, if available:

```bash
sudo lastb
```

Check SSH authentication logs on Ubuntu/Debian:

```bash
sudo grep -i "failed password" /var/log/auth.log | tail -n 20
sudo grep -i "accepted" /var/log/auth.log | tail -n 20
```

Check SSH authentication logs on RHEL-based systems:

```bash
sudo grep -i "failed password" /var/log/secure | tail -n 20
sudo grep -i "accepted" /var/log/secure | tail -n 20
```

---

## 8. Package updates

Ubuntu/Debian:

```bash
sudo apt update
apt list --upgradable
```

Check held packages:

```bash
apt-mark showhold
```

RHEL/CentOS/AlmaLinux/Rocky Linux:

```bash
sudo dnf check-update
```

Older CentOS systems:

```bash
sudo yum check-update
```

Check package manager errors on Ubuntu/Debian:

```bash
sudo tail -n 100 /var/log/apt/history.log
sudo tail -n 100 /var/log/dpkg.log
```

---

## 9. Logs and recurring errors

Recent system errors:

```bash
journalctl -p err --since "24 hours ago"
```

Recent warnings and errors:

```bash
journalctl -p warning --since "24 hours ago"
```

Kernel messages:

```bash
dmesg -T | tail -n 100
```

Authentication logs on Ubuntu/Debian:

```bash
sudo tail -n 100 /var/log/auth.log
```

System logs on Ubuntu/Debian:

```bash
sudo tail -n 100 /var/log/syslog
```

System logs on RHEL-based systems:

```bash
sudo tail -n 100 /var/log/messages
sudo tail -n 100 /var/log/secure
```

Check log directory size:

```bash
sudo du -h --max-depth=1 /var/log 2>/dev/null | sort -h
```

Check log rotation configuration:

```bash
ls -la /etc/logrotate.d/
cat /etc/logrotate.conf
```

---

## 10. Nginx checks

Check if Nginx is installed:

```bash
nginx -v
```

Check Nginx service status:

```bash
systemctl status nginx
```

Test Nginx configuration:

```bash
sudo nginx -t
```

List enabled sites on Ubuntu/Debian:

```bash
ls -la /etc/nginx/sites-enabled/
```

List available sites on Ubuntu/Debian:

```bash
ls -la /etc/nginx/sites-available/
```

List Nginx config directories:

```bash
ls -la /etc/nginx/
```

Check recent Nginx access logs:

```bash
sudo tail -n 100 /var/log/nginx/access.log
```

Check recent Nginx error logs:

```bash
sudo tail -n 100 /var/log/nginx/error.log
```

Find server blocks:

```bash
sudo grep -R "server_name" /etc/nginx/ 2>/dev/null
```

---

## 11. Apache checks

Check if Apache is installed:

```bash
apache2 -v
```

Alternative on RHEL-based systems:

```bash
httpd -v
```

Check Apache status on Ubuntu/Debian:

```bash
systemctl status apache2
```

Check Apache status on RHEL-based systems:

```bash
systemctl status httpd
```

Test Apache configuration on Ubuntu/Debian:

```bash
sudo apachectl configtest
```

Test Apache configuration on RHEL-based systems:

```bash
sudo httpd -t
```

List enabled sites on Ubuntu/Debian:

```bash
ls -la /etc/apache2/sites-enabled/
```

Check Apache logs on Ubuntu/Debian:

```bash
sudo tail -n 100 /var/log/apache2/access.log
sudo tail -n 100 /var/log/apache2/error.log
```

Check Apache logs on RHEL-based systems:

```bash
sudo tail -n 100 /var/log/httpd/access_log
sudo tail -n 100 /var/log/httpd/error_log
```

---

## 12. SSL certificate checks

Check SSL certificate for a domain:

```bash
openssl s_client -connect example.com:443 -servername example.com </dev/null
```

Show certificate dates:

```bash
echo | openssl s_client -connect example.com:443 -servername example.com 2>/dev/null | openssl x509 -noout -dates
```

Show certificate issuer and subject:

```bash
echo | openssl s_client -connect example.com:443 -servername example.com 2>/dev/null | openssl x509 -noout -issuer -subject
```

Check Certbot certificates, if Certbot is used:

```bash
sudo certbot certificates
```

Check Certbot renewal timer:

```bash
systemctl list-timers | grep certbot
```

Test Certbot renewal without changing certificates:

```bash
sudo certbot renew --dry-run
```

---

## 13. Docker checks

Check Docker version:

```bash
docker --version
```

Check Docker service:

```bash
systemctl status docker
```

List running containers:

```bash
docker ps
```

List all containers:

```bash
docker ps -a
```

List images:

```bash
docker images
```

List volumes:

```bash
docker volume ls
```

List networks:

```bash
docker network ls
```

Check Docker disk usage:

```bash
docker system df
```

Check Docker Compose version:

```bash
docker compose version
```

Check Docker Compose services:

```bash
docker compose ps
```

Check container logs:

```bash
docker logs container-name --tail 100
```

Check container restart policies:

```bash
docker inspect --format='{{.Name}} {{.HostConfig.RestartPolicy.Name}}' $(docker ps -aq)
```

Find Docker Compose files in common locations:

```bash
sudo find /home /opt /srv /var/www -name "docker-compose.yml" -o -name "compose.yml" 2>/dev/null
```

---

## 14. Cron jobs and scheduled tasks

Current user cron jobs:

```bash
crontab -l
```

Root cron jobs:

```bash
sudo crontab -l
```

System cron directories:

```bash
ls -la /etc/cron.d/
ls -la /etc/cron.daily/
ls -la /etc/cron.hourly/
ls -la /etc/cron.weekly/
ls -la /etc/cron.monthly/
```

Systemd timers:

```bash
systemctl list-timers
```

---

## 15. Backup basics

Look for backup-related scripts in common locations:

```bash
sudo find /home /opt /srv /var/www /etc -iname "*backup*" 2>/dev/null
```

Check cron jobs for backup references:

```bash
sudo grep -R "backup" /etc/cron* 2>/dev/null
```

Check common backup directories:

```bash
ls -la /backup 2>/dev/null
ls -la /backups 2>/dev/null
ls -la /var/backups 2>/dev/null
```

Check recent files in common backup directories:

```bash
sudo find /backup /backups /var/backups -type f -mtime -30 -exec ls -lh {} \; 2>/dev/null
```

Check if rsync is installed:

```bash
rsync --version
```

Check if restic is installed:

```bash
restic version
```

Check if borg is installed:

```bash
borg --version
```

---

## 16. Application directories

Common web application paths:

```bash
ls -la /var/www 2>/dev/null
ls -la /srv 2>/dev/null
ls -la /opt 2>/dev/null
```

Check ownership and permissions:

```bash
ls -la /var/www 2>/dev/null
ls -la /srv 2>/dev/null
ls -la /opt 2>/dev/null
```

Find `.env` files in common application paths:

```bash
sudo find /var/www /srv /opt /home -name ".env" 2>/dev/null
```

Important: do not expose or copy secrets from `.env` files. Only document whether environment files exist and whether permissions look appropriate.

Check permissions of `.env` files:

```bash
sudo find /var/www /srv /opt /home -name ".env" -exec ls -lh {} \; 2>/dev/null
```

---

## 17. Security baseline observations

Check listening services:

```bash
sudo ss -tulpen
```

Check users with UID 0:

```bash
awk -F: '$3 == 0 {print $1}' /etc/passwd
```

Check world-writable directories, limited to common paths:

```bash
sudo find /var/www /srv /opt /home -type d -perm -0002 -exec ls -ld {} \; 2>/dev/null
```

Check files with SUID bit, common security review item:

```bash
sudo find / -perm -4000 -type f -exec ls -lh {} \; 2>/dev/null
```

Check automatic updates on Ubuntu/Debian:

```bash
systemctl status unattended-upgrades
apt-cache policy unattended-upgrades
```

Check fail2ban, if installed:

```bash
systemctl status fail2ban
sudo fail2ban-client status
```

---

## 18. Cloud/VPS metadata checks

Check public IP from server perspective:

```bash
curl -4 ifconfig.me
```

Alternative:

```bash
curl -4 https://icanhazip.com
```

Check cloud-init status, if used:

```bash
cloud-init status
```

Check cloud-init logs:

```bash
sudo tail -n 100 /var/log/cloud-init.log
sudo tail -n 100 /var/log/cloud-init-output.log
```

---

## 19. Documentation notes to collect

During the health check, document the following:

```text
Server purpose:
Main applications:
Main domains:
Public IP:
Operating system:
Web server:
Database:
Container runtime:
Deployment method:
Backup method:
Monitoring method:
Firewall status:
SSH access method:
Known risks:
Recommended next steps:
```

---

## 20. Final report structure

Use the collected information to prepare a report with this structure:

```text
1. Executive summary
2. Scope
3. Server overview
4. Critical findings
5. High priority findings
6. Medium priority findings
7. Low priority findings
8. Recommendations
9. Commands/checks performed
10. Technical appendix
11. Final note
```

---

## 21. Commands to avoid unless explicitly approved

Avoid running these commands on client systems unless the client clearly approves the action and you understand the impact:

```bash
rm -rf
reboot
shutdown
systemctl restart service-name
systemctl stop service-name
apt upgrade
apt full-upgrade
dnf update
yum update
docker system prune
docker volume prune
docker compose down
ufw enable
ufw disable
iptables -F
mkfs
fdisk
parted
```

These commands can change system state, remove data, restart services, interrupt production workloads, or break access.
