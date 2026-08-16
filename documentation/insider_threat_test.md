# Insider Threat Test

## Overview

Insider threat testing evaluates how well the system detects, responds to, and limits damage caused by malicious or negligent actions from users who already have legitimate access. Tests are conducted periodically by the Security team with controlled, pre-approved scenarios.

---

## Threat Categories Tested

| Category | Description | Risk Level |
|----------|-------------|------------|
| Privilege Abuse | User exploits their own role to access restricted data | High |
| Data Exfiltration | Unauthorized copying or transfer of sensitive data | Critical |
| Credential Sharing | Sharing login credentials with unauthorized individuals | High |
| Policy Bypass | Attempting to disable logging, MFA, or audit trails | Critical |
| Lateral Movement | Moving across systems beyond assigned scope | High |
| Sabotage | Deliberate deletion or corruption of data/configs | Critical |

---

## Test Scenarios

### Scenario 1: Unauthorized Data Access (Privilege Abuse)

**Objective**: Verify that a Developer cannot access production database records.

**Steps**:
```
1. Log in as a Developer-role user.
2. Attempt to query the production database directly via CLI.
3. Attempt to access the admin dashboard at /admin.
4. Attempt to download a full export of the user table.
```

**Expected Result**: All actions blocked. Alerts triggered in SIEM within 2 minutes.

---

### Scenario 2: Data Exfiltration via USB / Cloud Upload

**Objective**: Detect bulk data transfer to external destinations.

**Steps**:
```
1. Log in as a Security Analyst.
2. Download 500+ sensitive log files to a local machine.
3. Attempt to upload the archive to a personal cloud storage account.
4. Attempt to email a compressed archive to an external address.
```

**Expected Result**: DLP policy blocks upload/email. Incident ticket auto-created.

---

### Scenario 3: Audit Log Tampering

**Objective**: Confirm audit logs are immutable and tampering is detected.

**Steps**:
```
1. Log in as a DevOps Engineer.
2. Attempt to delete or modify entries in /var/log/audit.log.
3. Attempt to disable logging via system service command.
4. Attempt to clear Docker container logs.
```

**Expected Result**: Write operations denied. Tampering attempt flagged as a critical alert.

---

### Scenario 4: Credential Sharing Simulation

**Objective**: Detect simultaneous logins from geographically disparate locations.

**Steps**:
```
1. Log in as Developer A from Location 1 (e.g., Delhi).
2. Simultaneously log in with the same credentials from Location 2 (e.g., Singapore).
3. Observe detection response.
```

**Expected Result**: Second session blocked. User notified. Security alert raised.

---

### Scenario 5: Lateral Movement

**Objective**: Ensure a compromised low-privilege account cannot pivot to high-value systems.

**Steps**:
```
1. Log in as a Viewer account.
2. Attempt SSH into internal servers using default or guessed credentials.
3. Attempt to enumerate network shares and internal APIs.
4. Attempt to install unauthorized software.
```

**Expected Result**: All lateral movement blocked. Network segmentation holds. Alert generated.

---

## Detection Controls Verified

- [ ] SIEM alert triggered within defined SLA (≤ 5 minutes)
- [ ] DLP policy enforcement active
- [ ] Audit log integrity maintained
- [ ] Account lockout triggered after policy violation
- [ ] Incident ticket auto-created in ticketing system
- [ ] Security team notified via alert channel

---

## Test Schedule

| Test Type | Frequency | Responsible Team |
|-----------|-----------|-----------------|
| Privilege Abuse | Monthly | Security Red Team |
| Data Exfiltration | Quarterly | Security + IT |
| Log Tampering | Quarterly | Security Ops |
| Credential Sharing | Monthly | Identity & Access |
| Lateral Movement | Bi-annually | Red Team |

---

## Reporting

All insider threat test results are documented in [Results](./results.md) with:
- Test date and tester name
- Pass / Fail for each detection control
- Time-to-detect metrics
- Remediation actions if any control failed

---

## Related Documents

- [RBAC Policy](./rbac_policy.md)
- [External Threat Test](./external_threat_test.md)
- [Results](./results.md)

---

*Last updated: June 2026 | Owner: Security Red Team*
