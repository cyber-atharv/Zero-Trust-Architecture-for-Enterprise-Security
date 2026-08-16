# 🛂 Step 5: Implement Access Control Policies using IAM

> [!WARNING]
> **Objective:** Enforce authorization boundaries via Identity & Access Management (IAM) preventing lateral escalation.

## 📋 Overview
Identity & Access Management (IAM) is crucial to defining and enforcing access control policies within a Zero Trust Architecture. All requests must pass through a Policy Enforcement Point (PEP).

## 🔍 IAM Configuration Details

| IAM Component | Enforcement Strategy |
| :--- | :--- |
| **Administrator Role** | Full access to all security logs, registered users, and configuration settings. |
| **Employee Role** | Limited access mapped strictly to their specific tasks and quick actions. |
| **Policy Enforcement Point (PEP)**| Gateway inspects the IAM policies attached to the user session. Unauthorized requests (e.g., Employee to `/admin`) are actively blocked. |
| **Session Management** | Active sessions are continuously monitored. Suspicious activity triggers re-authentication. |

## 🛠️ Action Items
- [x] Implement authorization gateways for `/admin` vs `/employee`
- [x] Set up session timeouts and re-authentication logic
