Security Policy

Building Visitor & Entry Management System

Security is a core requirement of the Building Visitor & Entry Management System.

This project handles sensitive information including visitor identity, mobile numbers, resident information, entry/exit records, visitor photos, security events, and administrative data.

All contributors and deployments must follow secure development and operational practices.

---

1. Supported Versions

Security fixes should be applied to the currently supported production release and the active development branch.

Version| Security Support
Production| Supported
Development| Supported
Deprecated releases| Not guaranteed

Production deployments should use a documented release version.

---

2. Reporting a Security Vulnerability

If you discover a security vulnerability, do not publicly disclose the vulnerability before it has been investigated.

Report security issues privately to the project security contact configured by the project owner.

A security report should contain:

- Short description of the vulnerability
- Affected component
- Steps to reproduce
- Expected behavior
- Actual behavior
- Potential security impact
- Relevant logs or screenshots, if safe to provide
- Suggested mitigation, if known

Do not include real resident or visitor personal information in a vulnerability report.

---

3. Sensitive Information

The application may process:

- Visitor names
- Visitor mobile numbers
- Resident names
- Resident mobile numbers
- Flat/unit information
- Visitor photos
- Vehicle numbers
- Entry timestamps
- Exit timestamps
- Approval records
- Guard information
- Authentication information
- Audit information

Sensitive information must only be accessible to authorized users.

---

4. Privacy Rules

The application must enforce privacy at the backend/API level.

Frontend hiding is not considered a security control.

For example:

Admin
9876543210

Resident
98******10

Unauthorized Resident
No access

A user must never be able to bypass privacy restrictions by directly calling an API.

---

5. Authentication

Authentication must use secure mechanisms.

Requirements include:

- Secure password hashing
- Secure session/token handling
- Token expiration
- Refresh-token protection where applicable
- Login rate limiting
- Brute-force protection
- Account lockout or progressive protection where appropriate
- Secure logout
- Device/session management
- Multi-factor authentication where required

Authentication secrets must never be stored in source code.

---

6. Authorization

Every protected API operation must verify:

1. User identity
2. User role
3. User permissions
4. Organization/building scope
5. Resource ownership where applicable

Example:

Resident A
   ↓
Can access
   ↓
Resident A's authorized flat/visitor information

Resident A
   ↓
Cannot access
   ↓
Resident B's private information

---

7. Role-Based Access Control

The system supports role-based authorization.

Initial roles include:

SUPER_ADMIN
ADMIN
GUARD
RESIDENT
VISITOR

Permissions should be managed independently from application UI components.

The backend must remain the final authorization authority.

---

8. API Security

All protected APIs must implement appropriate:

- Authentication
- Authorization
- Input validation
- Schema validation
- Rate limiting
- Request size limits
- Error handling
- Logging
- Abuse protection

API responses must not expose unnecessary internal information.

Database errors, stack traces, secrets, and internal implementation details must not be returned to clients.

---

9. Input Validation

All external input must be treated as untrusted.

Validation is required for:

- Names
- Mobile numbers
- Email addresses
- Flat/unit identifiers
- Visitor purposes
- Vehicle numbers
- QR tokens
- IDs
- Query parameters
- File uploads
- API payloads

Validation must occur on the server.

Client-side validation alone is insufficient.

---

10. QR Security

QR codes must not expose sensitive information.

Do not place confidential information directly inside a QR code.

QR access tokens should be:

- Unpredictable
- Validated by the backend
- Revocable where appropriate
- Expirable where appropriate
- Protected against replay
- Associated with the correct building/gate/workflow

Example:

QR
 ↓
Secure token
 ↓
Backend validation
 ↓
Allowed workflow

---

11. Visitor Photos and File Uploads

Uploaded visitor photographs and documents must be treated as untrusted files.

The system should validate:

- File type
- File size
- File extension
- MIME type
- Storage path
- Access permissions

Files should not be directly executable.

Private files should use controlled access rather than unrestricted public URLs.

---

12. Mobile Number Protection

Full mobile numbers must not be exposed to unauthorized users.

Example:

Authorized Admin:
9876543210

Resident:
98******10

Visitor:
Not available

Logs should also avoid unnecessarily storing full sensitive values.

---

13. Audit Logging

Security-sensitive actions should be recorded.

Examples:

LOGIN
LOGOUT
LOGIN_FAILED
PASSWORD_CHANGED
ROLE_CHANGED
PERMISSION_CHANGED
VISITOR_CREATED
VISITOR_APPROVED
VISITOR_REJECTED
ENTRY_RECORDED
EXIT_RECORDED
SENSITIVE_DATA_VIEWED
REPORT_EXPORTED
QR_CREATED
QR_REVOKED
ACCOUNT_DISABLED

Audit records should contain sufficient information to investigate an incident without unnecessarily storing sensitive personal information.

---

14. Database Security

The production database must:

- Require authentication
- Restrict network access
- Use encrypted connections
- Use least-privilege database accounts
- Have regular backups
- Have tested recovery procedures
- Use appropriate indexes and constraints
- Prevent unauthorized direct public access

Production database credentials must never be committed to Git.

---

15. Data Integrity

Visitor entry/exit records must maintain data integrity.

For example:

Entry
  ↓
Visitor is inside
  ↓
Exit
  ↓
Visit completed

The system must prevent invalid states such as:

- Exit before entry
- Duplicate exit
- Unauthorized entry
- Entry for a rejected visit
- Multiple conflicting active visits where prohibited

Server-side transactions and database constraints should be used where appropriate.

---

16. Offline Synchronization

The Guard App may temporarily operate without network connectivity.

Offline operations must use:

- Local secure storage
- Unique operation IDs
- Idempotency protection
- Server-side validation
- Conflict handling
- Synchronization status
- Retry controls

The same operation must not create duplicate visitor/entry records when synchronization is retried.

---

17. Secrets Management

Never commit:

API keys
Database passwords
JWT secrets
Encryption keys
SMS credentials
Email credentials
Cloud credentials
Private certificates
Production tokens

Use environment variables or a dedicated secrets-management system.

Example:

.env
.env.local
.env.production

These files must not be committed to Git.

Only safe example values belong in ".env.example".

---

18. Encryption

Sensitive communication must use encrypted transport.

Production services should use HTTPS/TLS.

Sensitive data requiring encryption at rest should be protected using appropriate platform/database/storage capabilities.

Encryption keys must be managed separately from encrypted data.

---

19. Rate Limiting

Rate limiting should be applied to sensitive endpoints, including:

- Login
- OTP requests
- OTP verification
- Visitor registration
- QR validation
- Password reset
- API authentication
- Report generation
- File uploads

Limits should be designed according to actual production traffic.

---

20. Error Handling

Production errors must not reveal:

- Database credentials
- SQL queries
- Internal file paths
- Authentication secrets
- Stack traces
- Private user information
- Internal service configuration

Users should receive safe error messages while detailed information is retained in protected server logs when necessary.

---

21. Logging

Application logs should be structured and protected.

Logs should support:

- Authentication investigation
- API errors
- Visitor workflow investigation
- Security incidents
- Synchronization failures
- Background worker failures
- Report generation failures

Sensitive personal information should not be logged unnecessarily.

---

22. Backups

Production data should have automated backups.

The backup strategy should define:

- Backup frequency
- Retention period
- Encryption
- Storage location
- Access permissions
- Recovery procedure
- Recovery testing

A backup that has never been tested for restoration should not be considered a fully verified recovery mechanism.

---

23. Dependency Security

Project dependencies must be regularly reviewed for known vulnerabilities.

Security checks should be included in CI/CD.

The project should use:

- Dependency scanning
- Static analysis where appropriate
- Secret scanning
- Container scanning where applicable
- Automated tests

---

24. Environment Separation

The following environments must remain separate:

Development
    ↓
Testing
    ↓
Staging
    ↓
Production

Production personal data must not be copied into development environments unless an approved, privacy-preserving process exists.

---

25. Production Deployment

Production deployment must use:

- HTTPS
- Secure environment configuration
- Restricted database access
- Secure secret management
- Monitoring
- Error tracking
- Backups
- Access control
- Audit logging
- Security updates

Administrative access should follow least-privilege principles.

---

26. Security Incident Response

A security incident should follow a controlled process:

Detect
  ↓
Contain
  ↓
Investigate
  ↓
Remediate
  ↓
Recover
  ↓
Review

The project owner should maintain appropriate incident records and determine whether affected users or authorities need to be notified based on applicable requirements.

---

27. Responsible Development

Contributors must not:

- Intentionally introduce security vulnerabilities
- Commit credentials
- Bypass authorization
- Disable security controls in production
- Access user data without authorization
- Publish private information
- Use production data for unauthorized testing

Security issues should be reported privately.

---

28. Security Principle

The project follows:

Secure by Design
        +
Least Privilege
        +
Backend Authorization
        +
Privacy by Default
        +
Auditable Actions
        +
Defense in Depth

Security is treated as a system-wide requirement rather than a feature added only to the frontend.
