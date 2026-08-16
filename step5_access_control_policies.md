# Step 5: Implement Access Control Policies using IAM

Identity & Access Management (IAM) is crucial to defining and enforcing access control policies within a Zero Trust Architecture.

## IAM Policy Implementation

1. **Defining Roles**
   - **Administrator:** Full access to all security logs, registered users, and configuration settings.
   - **Employee:** Limited access to their specific tasks and quick actions.

2. **Policy Enforcement Point (PEP)**
   - All requests flow through a gateway that inspects the IAM policies attached to the user session.
   - If a regular employee tries to access an administrative endpoint (e.g., `/admin`), the IAM system flags this as unauthorized and blocks the request.

3. **Session Management**
   - Active sessions are continuously monitored and validated.
   - Any suspicious activity prompts a re-authentication request, enforcing the Zero Trust mindset.
