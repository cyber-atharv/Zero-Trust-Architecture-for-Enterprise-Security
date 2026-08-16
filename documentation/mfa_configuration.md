# MFA Configuration

## Overview

Multi-Factor Authentication (MFA) adds a second layer of identity verification beyond passwords. This document outlines MFA requirements, supported methods, and setup procedures for all users and systems.

---

## MFA Requirements

| User Type | MFA Required | Enforcement Level |
|-----------|-------------|-------------------|
| Viewer | Optional | Encouraged |
| Developer | ✅ Yes | Mandatory |
| DevOps Engineer | ✅ Yes | Mandatory |
| Security Analyst | ✅ Yes | Mandatory |
| Admin | ✅ Yes | Mandatory + Hardware Key |
| Super Admin | ✅ Yes | Mandatory + Hardware Key |
| Service Accounts | N/A | Certificate-based auth |

---

## Supported MFA Methods

### 1. TOTP (Time-Based One-Time Password)
- Apps: **Google Authenticator**, **Authy**, **Microsoft Authenticator**
- Generates a 6-digit code every 30 seconds.
- Recommended for developers and standard users.

### 2. Hardware Security Keys (FIDO2 / WebAuthn)
- Devices: **YubiKey**, **Google Titan Key**
- Phishing-resistant. Required for Admin and Super Admin roles.
- Must be registered and stored securely when not in use.

### 3. Push Notification (Duo / Okta Verify)
- A push notification is sent to a registered mobile device.
- User approves or denies the login attempt.
- Suitable for general workforce use.

### 4. SMS / Email OTP *(Deprecated — Legacy Use Only)*
- Not recommended due to SIM-swapping and phishing risks.
- Will be fully disabled for all roles by **Q3 2026**.

---

## Setup Instructions

### TOTP Setup (Google Authenticator / Authy)

```bash
1. Log in to your account dashboard.
2. Navigate to Settings > Security > Two-Factor Authentication.
3. Select "Authenticator App" and click "Enable".
4. Scan the displayed QR code with your authenticator app.
5. Enter the 6-digit code from the app to confirm setup.
6. Save your backup codes in a secure location.
```

### Hardware Key Setup (YubiKey)

```bash
1. Insert your YubiKey into a USB port.
2. Navigate to Settings > Security > Hardware Key.
3. Click "Register Security Key".
4. Touch the YubiKey button when prompted.
5. Name the key (e.g., "YubiKey - Primary") and save.
6. Register a backup key if available.
```

### Duo Push Notification Setup

```bash
1. Download the Duo Mobile app on your smartphone.
2. In your account, go to Settings > Security > Duo MFA.
3. Click "Enroll Device" and scan the QR code.
4. Approve a test push notification to complete setup.
```

---

## MFA Recovery

- **Backup Codes**: Generated at MFA setup. Store offline. Each code is single-use.
- **Admin Recovery**: Users who lose MFA access must contact the IT helpdesk with verified identity documents.
- **Hardware Key Replacement**: Report a lost key immediately. It will be revoked within 1 hour.

---

## MFA Bypass Policy

- MFA bypass is **not permitted** for Admin or Super Admin accounts under any circumstances.
- Temporary bypass for other roles requires:
  - Written justification
  - Security team approval
  - A maximum window of **4 hours**
  - Full audit log entry

---

## Monitoring & Alerts

- Failed MFA attempts (3+) trigger an account lock and security alert.
- Unusual login locations with MFA approval trigger a review notification.
- All MFA events (success, failure, bypass) are logged to the SIEM platform.

---

## Related Documents

- [RBAC Policy](./rbac_policy.md)
- [Insider Threat Test](./insider_threat_test.md)
- [External Threat Test](./external_threat_test.md)

---

*Last updated: June 2026 | Owner: IT Security Team*
