# Zero Trust Architecture — Enterprise Security Simulation

A hands-on simulation of a **Zero Trust Architecture (ZTA)** framework built with Docker, Nginx, and vanilla web components. The project demonstrates core principles of Zero Trust — *"never trust, always verify"* — by enforcing explicit authentication, role-based access control (RBAC), session validation, and container-level isolation.

Developed by **Atharv Hogade** during the Cybersecurity internship program at **Novitech R&D Pvt Ltd** (2026).

---

## Overview

Traditional perimeter security assumes that anything inside the internal network is trustworthy. In modern cloud and enterprise environments, this assumption fails against credential theft, insider threats, and lateral movement.

This project implements a practical Zero Trust prototype where:
- Every request is validated before granting access.
- Access to resources is strictly constrained by user role (Least Privilege).
- Direct page visits and URL manipulation bypasses are blocked using client/server session checks.
- Application components run inside an isolated, hardened Docker container with security headers.

---

## System Architecture & Access Flow

```text
               +-----------------------------+
               |        User Browser         |
               +--------------+--------------+
                              |
                              v
               +-----------------------------+
               |   Nginx Container (:9090)   |
               |   - Security Headers        |
               |   - No-Cache Policy         |
               +--------------+--------------+
                              |
                              v
               +-----------------------------+
               |      Authentication Hub     |
               |         (login.html)        |
               +--------------+--------------+
                              |
              +---------------+---------------+
              |                               |
       [ Role: Admin ]                [ Role: Employee ]
              |                               |
              v                               v
    +-------------------+           +-------------------+
    |  Admin Dashboard  |           | Employee Dashboard|
    | - User Management |           | - Daily Tasks     |
    | - Security Alerts |           | - Profile / Logs  |
    | - Threat Monitor  |           | - Team Directory  |
    +-------------------+           +-------------------+
              \                               /
               \                             /
                v                           v
             +---------------------------------+
             | Session Check & RBAC Validation |
             | (Direct URL Access Blocked)     |
             +---------------------------------+
```

---

## Key Features

- **Strict Identity Verification**: Login gateway with credential validation and session token generation.
- **Role-Based Access Control (RBAC)**: Distinct dashboards with privilege separation between `admin` and `employee`.
- **Route Guarding**: Client-side session checks (`sessionStorage`) prevent unauthenticated users or lower-privileged roles from opening protected dashboard pages directly.
- **Hardened Nginx Deployment**:
  - `X-Frame-Options: SAMEORIGIN` (prevents clickjacking)
  - `X-Content-Type-Options: nosniff` (MIME-sniffing protection)
  - `X-XSS-Protection: 1; mode=block`
  - Strict no-cache headers for HTML files to avoid stale auth states
- **Container Isolation**: Multi-stage/lightweight Alpine-based container packaging.

---

## Test Accounts

The prototype comes with pre-configured mock identities to test access controls:

| Username | Password | Role | Target Dashboard | Access Scope |
| :--- | :--- | :--- | :--- | :--- |
| `admin` | `admin123` | Administrator | `admin_dashboard.html` | Full access: user management, security monitoring, threat logs |
| `employee` | `emp123` | Standard Employee | `employee_dashboard.html` | Limited access: workspace tools, assigned tasks, personal logs |

---

## Getting Started

### Prerequisites

- [Docker](https://docs.docker.com/get-docker/) installed and running
- (Optional) [Docker Compose](https://docs.docker.com/compose/)

### Option 1: Run with Docker CLI

```bash
# 1. Build the Docker image
docker build -t zta-security ./docker

# 2. Run the container on port 9090
docker run -d -p 9090:80 --name zta-portal zta-security

# 3. Check status
docker ps
```

### Option 2: Run with Docker Compose

```bash
cd docker
docker compose up -d --build
```

Open your browser and navigate to:
```
http://localhost:9090
```

---

## Threat Testing & Verification

The project includes test cases simulating real-world attack scenarios:

| Test Case | Scenario | Expected Behavior | Status |
| :--- | :--- | :--- | :--- |
| **Unauthenticated Access** | Attempt to open `/admin_dashboard.html` without logging in | Redirected immediately to `login.html` | Passed |
| **Privilege Escalation** | Log in as `employee` and attempt manual navigation to `/admin_dashboard.html` | Access denied / redirected back | Passed |
| **Invalid Credentials** | Submit incorrect username/password combinations | Error notification displayed, access blocked | Passed |
| **Session Expiry / Logout** | Clear session storage or click logout | Session invalidated, access to dashboards revoked | Passed |
| **Container Health** | Container healthcheck endpoint query | Nginx returns HTTP 200 OK | Passed |

Detailed documentation for threat testing can be found in the [`documentation/`](./documentation/) directory:
- [RBAC Policy Specification](./documentation/rbac_policy.md)
- [External Threat Testing](./documentation/external_threat_test.md)
- [Insider Threat Testing](./documentation/insider_threat_test.md)
- [Docker Setup Guide](./documentation/setup_guide_docker.md)
- [Results & Improvements](./documentation/results.md)

---

## Project Structure

```
Zero-Trust-Architecture-for-Enterprise-Security/
├── docker/
│   ├── Dockerfile              # Alpine-Nginx container definition
│   ├── docker-compose.yml      # Service configuration & port mapping
│   └── nginx.conf              # Web server config with security headers
├── web/
│   ├── login.html              # Authentication portal
│   ├── admin_dashboard.html    # Admin command center & threat monitor
│   └── employee_dashboard.html # Standard user dashboard
├── documentation/              # Step-by-step guides & test reports
│   ├── rbac_policy.md
│   ├── external_threat_test.md
│   ├── insider_threat_test.md
│   ├── mfa_configuration.md
│   ├── setup_guide_docker.md
│   └── results.md
├── screenshots/                # Deployment and testing captures
├── report/                     # Project report documents
├── presentation/               # Slide deck blueprints
└── README.md
```

---

## Roadmap & Planned Improvements

- [ ] **Multi-Factor Authentication (MFA)**: Add TOTP / Authenticator app support.
- [ ] **Backend Identity Provider (IdP)**: Integrate Keycloak or Okta via OpenID Connect (OIDC).
- [ ] **JWT Verification**: Transition from client-side session checks to cryptographic JSON Web Tokens.
- [ ] **Centralized SIEM**: Forward access and audit logs to Elasticsearch / Splunk for real-time alerting.
- [ ] **Kubernetes Network Policies**: Implement micro-segmentation across pod namespaces.

---

## Author

- **Atharv Hogade**
- Cybersecurity Intern, Novitech R&D Pvt Ltd

## License

This project is open source and available under the [MIT License](./LICENSE).

