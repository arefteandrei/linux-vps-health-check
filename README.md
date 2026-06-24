# Linux/VPS Health Check

A practical operational review for Linux VPS servers used by IT providers, web agencies, developers, and small technical teams.

This health check focuses on common operational risks and misconfigurations, including system resources, disk usage, running services, logs, SSH exposure, firewall status, updates, web server configuration, Docker Compose deployments, SSL certificates, and backup basics.

The goal is not to replace a full security audit, penetration test, or managed IT contract. The goal is to provide a clear technical snapshot of the server, identify visible operational risks, and document practical recommendations.

## Who this is for

- IT providers needing white-label Linux/VPS support
- Web agencies managing client VPS servers
- Developers deploying applications to VPS environments
- Small technical teams needing an external operational review
- Servers with unclear configuration or missing documentation

## Areas covered

- System overview
- CPU, RAM, uptime, and load
- Disk usage and inode usage
- Running and failed services
- Open ports and exposed services
- SSH configuration
- Firewall status
- Available updates
- Logs and recurring errors
- Nginx/Apache basic checks
- SSL certificate status
- Docker/Docker Compose checks, if applicable
- Backup observations
- Documentation gaps
- Recommendations and next steps

## Deliverable

The final deliverable is a written report containing:

- Executive summary
- Technical findings
- Risk classification
- Recommended remediation steps
- Commands/checks performed
- Technical appendix

## Important note

This is a basic operational health check, not a full security audit, compliance assessment, or penetration test.
