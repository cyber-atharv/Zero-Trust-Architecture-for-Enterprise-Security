# External Threat Test

## Overview

External threat testing simulates attacks originating from outside the organization's network perimeter. These tests validate the effectiveness of perimeter defenses, application security controls, and incident response against real-world attack vectors.

---

## Threat Categories Tested

| Category | Description | Risk Level |
|----------|-------------|------------|
| Reconnaissance | Information gathering and OSINT | Low |
| Brute Force / Credential Stuffing | Automated login attempts | High |
| SQL Injection | Database manipulation via input fields | Critical |
| Cross-Site Scripting (XSS) | Injecting malicious scripts into web pages | High |
| API Abuse | Exploiting exposed or undocumented API endpoints | High |
| DDoS Simulation | Flooding services to cause availability failure | Critical |
| Phishing | Social engineering via email or fake login pages | High |
| Container Escape | Breaking out of Docker container isolation | Critical |

---

## Test Scenarios

### Scenario 1: Reconnaissance & OSINT

**Objective**: Identify what an attacker can learn before touching the system.

**Steps**:
```
1. Run passive DNS enumeration on the target domain.
2. Search for exposed credentials on public breach databases.
3. Enumerate subdomains using tools like Subfinder or Amass.
4. Check for exposed .git directories, .env files, or admin panels.
5. Review job listings for technology stack disclosures.
```

**Expected Result**: No sensitive data publicly exposed. OSINT findings documented for remediation.

---

### Scenario 2: Brute Force & Credential Stuffing

**Objective**: Confirm account lockout and rate limiting policies hold under automated attack.

**Steps**:
```
1. Target the /login endpoint with a list of 1000 common credentials.
2. Use a distributed attack from multiple IPs to bypass IP-based rate limiting.
3. Attempt login with credentials from known breach datasets.
```

**Expected Result**: Account locked after 5 failed attempts. IP flagged and blocked by WAF. Alert triggered.

---

### Scenario 3: SQL Injection

**Objective**: Verify all database-facing inputs are properly sanitized.

**Steps**:
```
1. Identify input fields, search boxes, and URL parameters.
2. Test for basic SQL injection: ' OR '1'='1
3. Test for blind SQLi using time-based payloads.
4. Use sqlmap against authenticated endpoints with a test account.
```

**Expected Result**: All injections blocked. WAF logs the attempt. No data returned from DB.

---

### Scenario 4: Cross-Site Scripting (XSS)

**Objective**: Ensure the application does not execute injected scripts.

**Steps**:
```
1. Submit <script>alert('XSS')</script> in all text input fields.
2. Test stored XSS via profile fields, comments, or file names.
3. Test reflected XSS in URL parameters and error messages.
4. Test DOM-based XSS via hash fragments.
```

**Expected Result**: Scripts sanitized and not executed. Content Security Policy (CSP) header blocks execution.

---

### Scenario 5: API Abuse

**Objective**: Validate API security controls against unauthorized access and enumeration.

**Steps**:
```
1. Enumerate API endpoints via Swagger/OpenAPI docs or fuzzing.
2. Attempt to access endpoints without an auth token.
3. Attempt BOLA (Broken Object Level Authorization) by changing resource IDs.
4. Test rate limiting on high-frequency endpoints.
5. Attempt to inject oversized payloads to test input validation.
```

**Expected Result**: Unauthenticated requests rejected with 401. BOLA attempts blocked. Rate limiting enforced.

---

### Scenario 6: Container Escape

**Objective**: Confirm Docker containers cannot be used to access the host system.

**Steps**:
```
1. Gain shell access inside a running container.
2. Attempt to mount the host filesystem.
3. Attempt to access the Docker socket (/var/run/docker.sock).
4. Attempt privilege escalation using kernel exploits.
5. Try to communicate with other containers on the default bridge network.
```

**Expected Result**: Container confined to its namespace. No host filesystem access. Docker socket not exposed.

---

## Tools Used

| Tool | Purpose |
|------|---------|
| Nmap / Masscan | Port scanning and service enumeration |
| Burp Suite | Web application testing |
| sqlmap | SQL injection detection |
| OWASP ZAP | Automated vulnerability scanning |
| Metasploit | Exploitation framework |
| Subfinder / Amass | Subdomain enumeration |
| Hydra | Brute force testing |
| Trivy | Container vulnerability scanning |

---

## Detection Controls Verified

- [ ] WAF blocks injection and scripting payloads
- [ ] Rate limiting enforced on auth endpoints
- [ ] SIEM alert triggered for scanning activity
- [ ] Container isolation holds under escape attempts
- [ ] API returns no sensitive data to unauthenticated requests
- [ ] Security headers present (CSP, HSTS, X-Frame-Options)

---

## Test Schedule

| Test Type | Frequency | Responsible Team |
|-----------|-----------|-----------------|
| Full Penetration Test | Bi-annually | External Pen Test Firm |
| API Security Review | Quarterly | Security Engineering |
| Vulnerability Scan | Weekly (automated) | Security Ops |
| Red Team Exercise | Annually | Red Team |

---

## Related Documents

- [Insider Threat Test](./insider_threat_test.md)
- [MFA Configuration](./mfa_configuration.md)
- [Results](./results.md)
- [Setup Guide for Docker](./setup_guide_docker.md)

---

*Last updated: June 2026 | Owner: Security Engineering*
