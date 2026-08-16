# 🏗️ Step 3: Design a Zero Trust Framework

> [!TIP]
> **Objective:** Architect a comprehensive framework that incorporates identity, segmentation, and encryption across the enterprise.

## 📋 Overview
A comprehensive Zero Trust framework incorporates multiple layers of security to ensure data and resources are protected regardless of their location on the network.

## 🔍 Core Pillars of the Framework

| Pillar | Strategy & Enforcement |
| :--- | :--- |
| **Identity Verification** | Continuously authenticate and authorize all users, devices, and applications. Utilize centralized Identity and Access Management (IAM) systems. |
| **Micro-segmentation** | Divide the network into smaller, isolated segments. This restricts lateral movement for attackers, ensuring a compromise in one segment does not spread. |
| **Encryption** | Data must be encrypted both at rest (stored data) and in transit (data moving across networks). Utilize TLS/SSL for all web communications. |

## 🛠️ Action Items
- [x] Draft network segmentation maps
- [x] Define encryption standards (TLS 1.3, AES-256)
- [x] Integrate IAM identity providers
