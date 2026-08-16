# Step 6: Test Security Effectiveness (Threat Simulation)

To validate the Zero Trust Architecture, we must simulate both insider and external threats.

## 1. Insider Threat Simulation
**Scenario:** An employee attempts to access the administrator dashboard by manipulating the URL (e.g., navigating to `/admin_dashboard.html`).
**Test Steps:**
- Log in with Employee credentials.
- Change the URL path to restricted admin pages.
**Expected Result:** The system denies access and redirects the user to their permitted dashboard or logs them out. (Authorization Enforcement)

## 2. External Threat Simulation
**Scenario:** An unauthenticated attacker attempts direct access to internal resources by typing dashboard URLs directly in the browser.
**Test Steps:**
- Clear browser sessions/cookies.
- Attempt to access `/employee_dashboard.html` directly.
**Expected Result:** The request is blocked, and the user is redirected to the authentication login page. (Continuous Verification)
