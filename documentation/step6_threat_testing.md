# ⚔️ Step 6: Test Security Effectiveness (Threat Simulation)

> [!CAUTION]
> **Objective:** Actively test the network defenses by simulating targeted cyber-attacks to validate Zero Trust enforcement.

## 📋 Overview
To validate the Zero Trust Architecture, we must simulate both insider (internal user going rogue) and external (unauthenticated outsider) threats.

## 🔍 Threat Simulation Scenarios

| Threat Vector | Scenario Details | Expected Outcome | Result |
| :--- | :--- | :--- | :--- |
| **Insider Threat** | Employee logged in attempts to access the administrator dashboard by manipulating the URL (e.g., navigating to `/admin_dashboard.html`). | The system denies access via RBAC and redirects the user to their permitted dashboard. | ✅ Pass |
| **External Threat** | Unauthenticated attacker attempts direct access to internal resources by typing dashboard URLs directly in the browser. | Request is blocked by the PEP, and the user is redirected to the authentication login page. | ✅ Pass |

## 🛠️ Action Items
- [x] Execute URL tampering tests
- [x] Attempt session hijacking scenarios
- [x] Document vulnerability findings
