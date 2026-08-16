# 🔐 Step 2: Define Authentication Models

> [!NOTE]
> **Objective:** Identify and define the primary authentication and authorization models required to support continuous verification.

## 📋 Overview
Robust authentication models are the gatekeepers of a Zero Trust environment. Moving beyond single-factor passwords is required to accurately establish identity confidence.

## 🔍 Authentication Mechanisms

| Model | Description | Implementation Details |
| :--- | :--- | :--- |
| **Multi-Factor Authentication (MFA)** | Requires two or more verification factors to gain access. | **Factor 1:** Something you know (Password/PIN)<br>**Factor 2:** Something you have (Token/Authenticator)<br>**Factor 3:** Something you are (Biometrics) |
| **Role-Based Access Control (RBAC)** | Restricts network access based on the roles of individual users. | Permissions are assigned to roles, not individuals. Enforces the principle of Least Privilege. |

## 🛠️ Action Items
- [x] Select an MFA provider
- [x] Map out user roles (e.g., Admin, Employee, Guest)
- [x] Assign baseline permissions to each role
