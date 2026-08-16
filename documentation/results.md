# Security Test Results

## Overview

This document consolidates the results of all security tests including insider threat scenarios, external penetration tests, MFA validation, and RBAC policy enforcement checks. Results are updated after every test cycle.

---

## Summary Dashboard

| Test Category | Last Run | Total Tests | Passed | Failed | Status |
|---------------|----------|-------------|--------|--------|--------|
| RBAC Enforcement | June 2026 | 12 | 11 | 1 | ⚠️ Review |
| MFA Configuration | June 2026 | 8 | 8 | 0 | ✅ Pass |
| Insider Threat | May 2026 | 5 | 4 | 1 | ⚠️ Review |
| External Threat | May 2026 | 8 | 7 | 1 | ⚠️ Review |
| Container Security | April 2026 | 6 | 6 | 0 | ✅ Pass |

---

## RBAC Enforcement Results — June 2026

| Test Case | Role Tested | Expected | Actual | Result |
|-----------|------------|----------|--------|--------|
| Developer accesses /admin | Developer | Blocked | Blocked | ✅ Pass |
| Developer queries prod DB | Developer | Blocked | Blocked | ✅ Pass |
| Viewer attempts file upload | Viewer | Blocked | Blocked | ✅ Pass |
| DevOps deploys to production | DevOps | Allowed | Allowed | ✅ Pass |
| Security Analyst deletes audit log | Security Analyst | Blocked | Blocked | ✅ Pass |
| Admin modifies global platform config | Admin | Blocked | Blocked | ✅ Pass |
| Super Admin accesses all resources | Super Admin | Allowed | Allowed | ✅ Pass |
| Developer accesses staging env | Developer | Allowed | Allowed | ✅ Pass |
| Viewer accesses analytics dashboard | Viewer | Allowed | Allowed | ✅ Pass |
| DevOps deletes production container | DevOps | Blocked | Blocked | ✅ Pass |
| Admin manages users | Admin | Allowed | Allowed | ✅ Pass |
| Temporary role escalation without approval | Developer | Blocked | **Allowed** | ❌ Fail |

**Failed Test Note**: Temporary role escalation was permitted without the required approval ticket. The approval gate in the IAM workflow had a logic error. Patch scheduled for June 12, 2026.

---

## MFA Configuration Results — June 2026

| Test Case | Method | Expected | Actual | Result |
|-----------|--------|----------|--------|--------|
| TOTP login for Developer | TOTP | Allowed | Allowed | ✅ Pass |
| Hardware key login for Admin | YubiKey | Allowed | Allowed | ✅ Pass |
| Login without MFA (Developer) | None | Blocked | Blocked | ✅ Pass |
| Brute force TOTP codes (100 attempts) | TOTP | Locked | Locked | ✅ Pass |
| MFA bypass request (Admin) | N/A | Denied | Denied | ✅ Pass |
| Backup code usage | Backup Code | Allowed | Allowed | ✅ Pass |
| Expired TOTP code (>90s old) | TOTP | Rejected | Rejected | ✅ Pass |
| Push notification approval from new device | Duo Push | Flagged | Flagged | ✅ Pass |

---

## Insider Threat Test Results — May 2026

| Scenario | Tester | Control Triggered | Time to Detect | Result |
|----------|--------|-------------------|----------------|--------|
| Privilege Abuse (DB access) | Red Team | SIEM alert + block | 1m 42s | ✅ Pass |
| Data Exfiltration (cloud upload) | Red Team | DLP policy blocked | 3m 10s | ✅ Pass |
| Audit Log Tampering | Red Team | Write denied + alert | 0m 58s | ✅ Pass |
| Credential Sharing (geo anomaly) | Red Team | Session blocked | 4m 55s | ✅ Pass |
| Lateral Movement (Viewer → Server) | Red Team | SSH blocked | **8m 22s** | ❌ Fail |

**Failed Test Note**: Lateral movement was ultimately blocked, but the SIEM alert was delayed by 8 minutes (SLA is 5 minutes). Root cause: log forwarding latency from the test subnet. Fix in progress — log pipeline reconfiguration scheduled for June 15, 2026.

---

## External Threat Test Results — May 2026

| Scenario | Tool Used | Control Triggered | Result |
|----------|-----------|-------------------|--------|
| Reconnaissance / OSINT | Subfinder, Shodan | No sensitive exposure | ✅ Pass |
| Brute Force (rate limit) | Hydra | IP blocked after 5 attempts | ✅ Pass |
| SQL Injection | sqlmap | WAF blocked all payloads | ✅ Pass |
| Stored XSS | Manual | CSP blocked script execution | ✅ Pass |
| Reflected XSS | Burp Suite | Sanitized — not executed | ✅ Pass |
| API — Unauthenticated Access | Postman | 401 returned | ✅ Pass |
| API — BOLA (Object ID manipulation) | Custom Script | **Data returned for another user** | ❌ Fail |
| Container Escape | Manual | Host filesystem inaccessible | ✅ Pass |

**Failed Test Note**: BOLA vulnerability found on the `/api/v1/users/{id}/profile` endpoint. An authenticated user was able to retrieve another user's profile by incrementing the `id` parameter. Object-level authorization check was missing on this endpoint. Fix deployed June 3, 2026 — awaiting re-test.

---

## Open Remediation Items

| ID | Issue | Severity | Owner | Target Date | Status |
|----|-------|----------|-------|-------------|--------|
| REM-001 | Temp role escalation bypass in IAM | High | IT Security | June 12, 2026 | In Progress |
| REM-002 | SIEM alert latency on test subnet | Medium | Security Ops | June 15, 2026 | In Progress |
| REM-003 | BOLA on /api/v1/users/{id}/profile | Critical | Backend Eng | June 10, 2026 | Deployed — Re-test Pending |

---

## Historical Pass Rate

| Month | Tests Run | Pass Rate |
|-------|-----------|-----------|
| January 2026 | 28 | 89% |
| February 2026 | 30 | 90% |
| March 2026 | 32 | 94% |
| April 2026 | 30 | 97% |
| May 2026 | 39 | 92% |
| June 2026 | 20 | 95% |

---

## Related Documents

- [RBAC Policy](./rbac_policy.md)
- [MFA Configuration](./mfa_configuration.md)
- [Insider Threat Test](./insider_threat_test.md)
- [External Threat Test](./external_threat_test.md)

---

*Last updated: June 2026 | Owner: Security Operations*
