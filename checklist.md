# Linux/VPS Health Check Checklist

## 1. System overview

- Operating system and version
- Kernel version
- Hostname
- Uptime
- Current logged-in users
- Timezone
- Reboot requirement

## 2. Resources

- CPU load
- RAM usage
- Swap usage
- Disk usage
- Largest directories/files
- Inode usage

## 3. Services

- Running services
- Failed systemd units
- Recently restarted services
- Critical services status
- Cron jobs and timers

## 4. Network

- Open listening ports
- Publicly exposed services
- Firewall status
- Basic DNS resolution
- Active connections

## 5. SSH and access

- SSH port
- Root login status
- Password authentication status
- Existing users with shell access
- Sudo users
- SSH key usage

## 6. Updates and packages

- Available system updates
- Security updates
- Unused packages
- Package manager errors

## 7. Logs

- Recent system errors
- Authentication failures
- Service-specific errors
- Disk, memory, or kernel warnings
- Repeated log patterns

## 8. Web server

- Nginx/Apache status
- Enabled virtual hosts
- SSL certificate status
- Redirects
- Basic configuration issues

## 9. Docker

- Docker service status
- Running containers
- Restart policies
- Exposed ports
- Volumes
- Image/container disk usage
- Docker Compose files

## 10. Backup basics

- Existing backup scripts
- Cron backup jobs
- Backup destination
- Last successful backup evidence
- Restore documentation

## 11. Documentation

- Server purpose
- Main services
- Deployment method
- Access method
- Known risks
- Recommended next steps
