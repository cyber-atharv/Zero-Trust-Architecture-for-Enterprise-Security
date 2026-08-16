# RBAC Policy

## Overview

Role-Based Access Control (RBAC) defines how permissions are assigned to users based on their roles within the organization. This policy governs access to systems, data, and infrastructure across all environments.

---

## Roles & Permissions Matrix

| Role | Read | Write | Delete | Admin | Deploy |
|------|------|-------|--------|-------|--------|
| Viewer | ✅ | ❌ | ❌ | ❌ | ❌ |
| Developer | ✅ | ✅ | ❌ | ❌ | ❌ |
| DevOps Engineer | ✅ | ✅ | ✅ | ❌ | ✅ |
| Security Analyst | ✅ | ✅ | ❌ | ❌ | ❌ |
| Admin | ✅ | ✅ | ✅ | ✅ | ✅ |
| Super Admin | ✅ | ✅ | ✅ | ✅ | ✅ |

---

## Role Definitions

### Viewer
- Read-only access to dashboards and reports.
- Cannot modify, delete, or deploy any resources.
- Suitable for stakeholders and auditors.

### Developer
- Read and write access to development and staging environments.
- Cannot delete production resources or access admin panels.
- Must be assigned to a specific project scope.

### DevOps Engineer
- Full access to infrastructure, CI/CD pipelines, and deployment workflows.
- Can delete non-critical resources with approval.
- No access to user management or billing.

### Security Analyst
- Read and write access to logs, alerts, and security configurations.
- Can create and update threat detection rules.
- Cannot delete audit logs or modify RBAC policies.

### Admin
- Full access to all application-level settings.
- Can manage users, roles, and permissions within their domain.
- Cannot modify global platform configurations.

### Super Admin
- Unrestricted access to all systems and configurations.
- Reserved for designated security leads only.
- All actions are logged and reviewed weekly.

---

## Policy Rules

1. **Least Privilege Principle** — Users are granted the minimum permissions required to perform their job function.
2. **Separation of Duties** — No single user should have conflicting roles (e.g., Developer + Admin in production).
3. **Role Assignment Approval** — Role assignments above Developer level require approval from the Security team.
4. **Periodic Review** — All role assignments are reviewed quarterly. Unused roles are revoked.
5. **Temporary Elevated Access** — Temporary role escalation is allowed for up to 24 hours with a logged justification.
6. **Service Accounts** — Service accounts follow the same RBAC model. They must be tied to a specific owner.

---

## Role Assignment Process

```
1. User submits access request via ticketing system.
2. Direct manager approves or denies the request.
3. Security team reviews requests for Admin or higher roles.
4. IT provisions the role within 1 business day of approval.
5. Access is logged in the IAM audit trail.
```

---

## Audit & Compliance

- All permission changes are recorded in the centralized audit log.
- Quarterly access reviews are conducted by the Security team.
- Any unauthorized privilege escalation triggers an immediate incident alert.
- RBAC policies are reviewed annually or after any major organizational change.

---

## Related Documents

- [MFA Configuration](./mfa_configuration.md)
- [Insider Threat Test](./insider_threat_test.md)
- [Setup Guide for Docker](./setup_guide_docker.md)

---

*Last updated: June 2026 | Owner: Security Team*
