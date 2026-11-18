# Zero-Trust Healthcare Security Architecture (ZTNA & SIEM)

A comprehensive, containerized hospital management security architecture implementing a Zero-Trust model. This project integrates strict Identity & Access Management (IAM), Web Application Firewall (WAF) protection, and an advanced Wazuh SIEM pipeline featuring custom rule tuning (Level 15 alerts) and automated Active Response (IPS).

---

## Architecture & Tech Stack

Our system is built using modern containerized microservices and security agents:
- **Core Application:** OpenEMR (Hospital Management System) backed by secure MySQL database containers with encrypted volumes.
- **Identity & Access Management (IAM):** Keycloak delivering OpenID Connect (OIDC) Single Sign-On (SSO) and Role-Based Access Control (RBAC).
- **Authentication Proxy:** OAuth2 Proxy managing session security, token validation, and secure cookie handling.
- **Edge Security & WAF:** Nginx Reverse Proxy integrated with **ModSecurity WAF** to inspect traffic and block common web vulnerabilities (SQLi, XSS, RCE).
- **SIEM & Threat Intelligence:** **Wazuh (Manager, Indexer, and Dashboard)** deployed for real-time log analysis, endpoint security, and automated threat mitigation.

---

## Wazuh SIEM & Security Monitoring Integration

Wazuh plays a central role in our Zero-Trust architecture by providing continuous monitoring, threat detection, and automated incident response:
1. **Wazuh Agent Deployment:** Installed and configured on the infrastructure to monitor system logs, audit trails, and critical application directories.
2. **Custom Rules & Severity Tuning (Level 15):** 
   - Configured custom rules (`local_rules.xml`) to detect specific security events such as WAF intrusion attempts (Rule ID 100002) and unauthorized file modifications.
   - Escalated critical attack patterns to **Level 15** alerts to trigger immediate high-priority incident flags.
3. **File Integrity Monitoring (FIM):** Actively tracks changes, additions, or deletions within sensitive system files and configuration paths, instantly logging anomalies.
4. **Active Response (IPS):** 
   - Integrated automated active response scripts (`firewall-drop`).
   - When Wazuh detects repeated attacks or critical Level 15 threats, it dynamically commands the firewall to automatically block the attacker's IP address in real time.

---

## Key Security Controls

- **Strict Access Enforcement (RBAC):** Users must authenticate via Keycloak, where granular Realm Roles (`doctor`, `admin`) govern access permissions before any application request is served.
- **Web Defense & Filtering:** ModSecurity inspects HTTP payloads at the proxy level, neutralizing malicious requests and script injections.
- **Automated Threat Mitigation:** Combines SIEM log aggregation with Active Response to shift from passive monitoring to active, automated prevention.

---

## Repository Structure

```text
├── certs/                # SSL/TLS certificates for secure HTTPS deployment
├── db-keys/              # Database configuration files and encryption keys
├── docker-compose.yml    # Main orchestration file for containerized services
├── real-ip.conf          # Nginx real IP header forwarding configuration
├── waf-bypass.conf       # Custom ModSecurity rules and bypass testing vectors
├── shell.php             # Controlled test scripts used for WAF and SIEM validation
├── test.txt              # Sample validation files
└── .gitignore            # Exclusion rules for sensitive environment data and logs
