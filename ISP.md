# Information Security Policy

**Loaded**

*Author: Roberto Hasfura*
*Last updated: February 10, 2026*
*Version: 1.0*

---

## 1. Purpose

This Information Security Policy ("ISP") establishes the security principles, standards, and practices governing the development, operation, and maintenance of Loaded ("the App"). The purpose of this policy is to protect the confidentiality, integrity, and availability of all data processed by the App, with particular emphasis on consumer financial data obtained through third-party integrations such as Plaid.

## 2. Scope

This policy applies to all systems, infrastructure, code, data stores, and accounts associated with the App, including:

- Application source code and configuration
- Local and remote environments where the App is developed or deployed
- Third-party API credentials and access tokens (e.g., Plaid)
- Consumer financial data, including transaction history and account metadata
- Any device used to develop, test, or operate the App

## 3. Roles and Responsibilities

Loaded is developed and operated by a single individual. As such, Roberto Hasfura serves as the sole operator and is responsible for all aspects of information security, including:

- Application development and secure coding practices
- Infrastructure provisioning and maintenance
- Access control and credential management
- Incident detection, response, and remediation
- Policy review and updates

## 4. Access Control

### 4.1 Principle of Least Privilege

Access to all systems and data is restricted to the minimum level necessary to operate the App. As a single-operator application, only Roberto Hasfura has access to production systems, source code, and stored data.

### 4.2 Authentication

- All access to systems that store or process consumer data requires authentication.
- Multi-factor authentication (MFA) is enabled on all accounts that support it, including cloud providers, GitHub, email, and the Plaid developer dashboard.
- SSH key-based authentication is used for remote server access where applicable. Password-based SSH access is disabled.

### 4.3 Role-Based Access Control (RBAC)

A single administrative role exists with full access to the App and its supporting infrastructure. No other roles are provisioned. Should additional users be granted access in the future, distinct roles with scoped permissions will be defined before access is granted.

### 4.4 Consumer-Facing Application Authentication

The consumer-facing interface where Plaid Link is deployed is protected by authentication. Only authorized household members may access the application.

### 4.5 Access Reviews

Access to all systems and third-party services is reviewed on a quarterly basis to confirm that no unauthorized access has been granted and that all credentials remain valid and necessary.

## 5. Data Protection

### 5.1 Data Classification

| Classification | Description | Examples |
|---|---|---|
| Sensitive | Consumer financial data, API credentials, access tokens | Plaid access tokens, transaction data, account metadata |
| Internal | Application configuration, source code, budget targets | Environment variables, database schemas, categorization rules |
| Public | Published policies and documentation | Privacy policy, this ISP |

### 5.2 Encryption

- **In transit**: All network communication uses TLS 1.2 or higher. Unencrypted HTTP connections are not permitted for any endpoint that transmits sensitive data.
- **At rest**: Devices that store consumer data use full-disk encryption (e.g., FileVault, LUKS, BitLocker). Database files containing sensitive data are stored on encrypted volumes.

### 5.3 Credential and Token Management

- Plaid API keys, access tokens, and secrets are never hardcoded in source code.
- Credentials are stored in environment variables or encrypted configuration files excluded from version control (e.g., via `.gitignore` or `.env` files).
- Plaid access tokens are stored securely and are never exposed in client-side code, logs, or public repositories.

### 5.4 Data Retention and Deletion

- Transaction data is retained for as long as it serves the App's budgeting and financial analysis purposes.
- When a linked financial account is removed, the associated Plaid access token is revoked via the Plaid API (`/item/remove`).
- Users may request complete deletion of their data by contacting rhasfura@gmail.com. Upon such request, all associated records and tokens will be deleted within 30 days.
- Backups containing sensitive data, if any, are subject to the same retention and deletion practices.

## 6. Secure Development Practices

### 6.1 Coding Standards

- Input from external sources (including Plaid API responses) is validated and sanitized before use.
- Dependencies are sourced from trusted package registries (npm, PyPI) and are pinned to specific versions where practical.
- Secrets and credentials are excluded from version control via `.gitignore` and are never committed to any repository, public or private.

### 6.2 Dependency Management

- Application dependencies are reviewed for known vulnerabilities using automated tools such as `npm audit`, `pip audit`, or GitHub Dependabot.
- Critical or high-severity vulnerabilities in dependencies are patched or mitigated within 30 days of discovery.

### 6.3 Code Review

As a single-developer project, formal peer code review is not applicable. However, all changes to security-sensitive code (authentication, token handling, data storage) are reviewed with particular care before deployment.

## 7. Vulnerability Management

### 7.1 Vulnerability Scanning

- Automated dependency scanning is performed regularly using tools such as `npm audit`, `pip audit`, or equivalent.
- GitHub Dependabot or similar automated alerting is enabled on repositories containing application code.

### 7.2 Patch Management

- Critical and high-severity vulnerabilities are addressed within 30 days of identification.
- Medium and low-severity vulnerabilities are addressed within 90 days.
- Operating systems, language runtimes, and frameworks are kept on supported, actively maintained versions.

### 7.3 End-of-Life (EOL) Software

- Software components (operating systems, runtimes, frameworks, libraries) are monitored for end-of-life status.
- EOL software is replaced or upgraded before or promptly after the end-of-support date.
- No EOL software is knowingly used in production environments that process consumer data.

## 8. Infrastructure Security

### 8.1 Zero Trust Architecture

The App follows zero trust principles:

- No implicit trust is granted based on network location.
- All access to systems and data requires explicit authentication and authorization.
- External API calls (e.g., to Plaid) are authenticated with scoped credentials on every request.

### 8.2 Network Security

- Development and production environments are protected by firewalls or network-level access controls.
- Unnecessary ports and services are disabled.
- Remote access, if any, requires encrypted connections (SSH, VPN, or equivalent).

### 8.3 Device Security

- Devices used to develop or operate the App are protected with strong passwords or biometric authentication.
- Full-disk encryption is enabled on all devices that store or access consumer data.
- Operating system and security updates are applied promptly.

## 9. Incident Response

### 9.1 Detection

Unusual activity — including unexpected API calls, unauthorized access attempts, or data anomalies — is monitored through application logs, Plaid dashboard activity, and system-level audit logs where available.

### 9.2 Response Procedure

In the event of a suspected or confirmed security incident:

1. **Contain**: Immediately revoke compromised credentials and isolate affected systems.
2. **Assess**: Determine the scope and nature of the incident, including what data may have been affected.
3. **Remediate**: Patch the vulnerability, rotate all potentially compromised secrets and tokens, and restore from clean backups if necessary.
4. **Notify**: If consumer data is believed to have been compromised, affected users and relevant third parties (including Plaid) will be notified promptly.
5. **Document**: Record a summary of the incident, actions taken, and any policy changes made as a result.

### 9.3 Plaid-Specific Incident Procedures

If a Plaid access token or API key is suspected to be compromised:

- The affected access token is immediately revoked via `/item/remove`.
- The Plaid API secret is rotated via the Plaid dashboard.
- Plaid support is contacted to report the incident.

## 10. Employee Lifecycle Management

### 10.1 Current State

Loaded is operated solely by Roberto Hasfura. There are no additional employees, contractors, or contributors with access to production systems or consumer data.

### 10.2 Onboarding

Should additional personnel be granted access in the future, onboarding will include:

- Background verification appropriate to the level of access granted
- Provisioning of individual, scoped credentials (no shared accounts)
- Review of this Information Security Policy

### 10.3 Offboarding and De-Provisioning

Upon termination or transfer of any individual with access to App systems:

- All access credentials are revoked or rotated within 24 hours.
- Access to third-party services (Plaid, cloud providers, repositories) is removed.
- Any devices containing sensitive data are wiped or returned.

This process is automated where supported by the underlying platform (e.g., removing a team member from a GitHub organization or cloud IAM console automatically revokes their access).

## 11. Policy Review

This Information Security Policy is reviewed and updated at least annually, or whenever a significant change occurs in the App's architecture, data handling practices, or threat landscape.

## 12. Contact

For questions or concerns regarding this policy, contact:

**Roberto Hasfura**
📧 rhasfura@gmail.com

---

*This document constitutes the Information Security Policy for Loaded and satisfies the requirement for a published ISP as part of the Plaid production access approval process.*
