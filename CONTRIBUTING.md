Contributing Guide

Building Visitor & Entry Management System

Thank you for contributing to the Building Visitor & Entry Management System.

This project is intended to be developed as a secure, production-oriented application. Contributions should maintain the project's security, reliability, privacy, and maintainability standards.

---

1. Development Principles

All contributions should follow these principles:

- Security first
- Privacy by default
- Backend-enforced authorization
- Database as the source of truth
- Clean and maintainable code
- Strong validation
- Automated testing
- Clear documentation
- Backward compatibility where practical
- No fake or placeholder production functionality

---

2. Before Making Changes

Before implementing a significant feature:

1. Understand the existing architecture.
2. Check whether the feature already exists.
3. Identify affected applications and services.
4. Review database implications.
5. Review security and privacy implications.
6. Determine required tests.
7. Update documentation when necessary.

Large architectural changes should be discussed before implementation.

---

3. Repository Structure

The repository is organized into:

apps/
    User-facing applications

services/
    Backend and background services

packages/
    Shared libraries and modules

database/
    Database-related resources

infrastructure/
    Deployment infrastructure

security/
    Security-related configuration and documentation

docs/
    Project documentation

tests/
    Automated tests

scripts/
    Development and deployment utilities

Contributors should place code in the appropriate location rather than creating duplicate implementations.

---

4. Branching

Use descriptive branches.

Examples:

feature/visitor-registration
feature/qr-visitor-flow
feature/excel-reports
feature/resident-approval
fix/duplicate-entry
fix/qr-validation
security/api-rate-limit
refactor/visitor-service
docs/deployment-guide

Avoid vague branch names such as:

test
new
update
abc
final
final2

---

5. Commits

Commits should describe one logical change.

Good examples:

feat: add visitor registration API
feat: add resident approval workflow
fix: prevent duplicate visitor exit
fix: validate QR expiration
security: add rate limiting to OTP endpoint
docs: update deployment guide
test: add visitor entry integration tests

Avoid commits such as:

changes
done
update
final
working

---

6. Pull Requests

A pull request should clearly explain:

- What changed
- Why it changed
- Which applications/services are affected
- Database changes
- Security impact
- Privacy impact
- Testing performed
- Deployment considerations

Example:

## Summary

Added resident visitor approval workflow.

## Changes

- Added approval API
- Added resident approval screen
- Added guard approval-status handling
- Added notification event
- Added database status transition

## Security

- Backend verifies resident ownership of the target unit.

## Testing

- Unit tests
- API integration tests
- Approval workflow test

---

7. Database Changes

Database changes must be handled through migrations.

Do not modify production databases manually as part of normal development.

A database change should include:

Schema change
     ↓
Migration
     ↓
Updated types/models
     ↓
Tests
     ↓
Documentation if required

Migrations should be reviewed carefully before production deployment.

---

8. API Changes

API endpoints must include appropriate:

- Authentication
- Authorization
- Input validation
- Error handling
- Rate limiting where necessary
- Tests
- Documentation

Never assume that frontend restrictions provide API security.

For example, if residents should only access their own visitors, the backend must verify ownership on every relevant request.

---

9. Privacy

Contributors must protect personal information.

Potentially sensitive information includes:

- Mobile numbers
- Names
- Visitor photos
- Vehicle numbers
- Flat/unit information
- Entry/exit history
- Authentication information

Do not expose information simply because it is available in the database.

Use the minimum information required for each role.

---

10. Test Data

Never commit real resident or visitor information.

Use synthetic test data.

Example:

Name:
Test Visitor

Mobile:
9000000000

Flat:
203

Vehicle:
TEST-0001

Production personal data must not be used in development or testing without an approved privacy-preserving process.

---

11. Authentication and Authorization

When adding a protected feature:

1. Define who can access it.
2. Define the required permission.
3. Enforce authorization on the backend.
4. Add unauthorized-access tests.
5. Verify that users cannot access another organization's/building's data.

Example:

Resident
   ↓
Own authorized visitor records

Not:

Resident
   ↓
All visitor records

---

12. Visitor Workflow Integrity

Changes to visitor workflows must preserve valid state transitions.

Example:

REGISTERED
    ↓
PENDING_APPROVAL
    ↓
APPROVED / REJECTED
    ↓
ENTRY_RECORDED
    ↓
INSIDE
    ↓
EXIT_RECORDED
    ↓
COMPLETED

Invalid transitions must be rejected by the backend.

---

13. Offline Synchronization

Changes affecting the Guard App offline system must account for:

- Unique operation IDs
- Retry behavior
- Duplicate requests
- Network interruptions
- Conflicting updates
- Server reconciliation
- Secure local storage

A retry must not create duplicate entry records.

---

14. Excel Reporting

Excel reports must be generated from authoritative database records.

Contributors must not introduce a workflow where guards or visitors manually maintain the Excel report.

The expected flow is:

Application
    ↓
API
    ↓
Database
    ↓
Background Worker
    ↓
Excel Generator

Report-generation changes should include automated tests.

---

15. Notifications

Notification functionality should be implemented through a provider abstraction.

Possible providers include:

Push
SMS
Email
WhatsApp

Provider credentials must never be committed to source control.

Development environments should use test/sandbox providers where available.

---

16. QR Codes

QR implementations must:

- Avoid exposing sensitive information
- Validate tokens server-side
- Handle expiration where applicable
- Support revocation where applicable
- Prevent unauthorized access
- Prevent replay when required

QR functionality must include security tests.

---

17. File Uploads

When adding image/document uploads:

- Validate file type
- Validate file size
- Validate MIME type
- Generate safe storage names
- Prevent executable uploads
- Restrict access
- Avoid exposing private storage publicly

Do not trust the filename supplied by the client.

---

18. Testing Requirements

New functionality should include appropriate tests.

Unit Tests

For individual functions and business logic.

Integration Tests

For interactions between services, database, and external dependencies.

API Tests

For authentication, authorization, validation, and API behavior.

End-to-End Tests

For important complete workflows.

Example:

Visitor registration
      ↓
Resident approval
      ↓
Guard entry
      ↓
Guard exit
      ↓
Report generation

---

19. Security Testing

Security-sensitive changes should test:

- Unauthorized access
- Cross-user access
- Cross-building access
- Invalid tokens
- Expired tokens
- Rate limiting
- Input validation
- File upload restrictions
- Sensitive-data exposure

---

20. Code Quality

Code should be:

- Readable
- Modular
- Testable
- Consistent
- Documented where necessary
- Free of unnecessary duplication

Avoid large functions that perform unrelated responsibilities.

Prefer clear domain modules.

---

21. Dependencies

New dependencies should be added only when justified.

Before adding a dependency, consider:

- Security
- Maintenance status
- License
- Bundle size
- Performance
- Existing alternatives
- Long-term compatibility

Do not add libraries simply because they provide a small convenience feature that can be implemented safely without them.

---

22. Secrets

Never commit:

.env
.env.production
API keys
Passwords
Private keys
Database credentials
JWT secrets
Cloud credentials
SMS credentials
Email credentials

Use ".env.example" for documenting required environment variables without real secrets.

---

23. Documentation

Documentation should be updated when changes affect:

- Architecture
- API behavior
- Database structure
- Security
- Deployment
- User workflows
- Configuration
- Environment variables

Documentation should describe actual system behavior, not planned behavior presented as completed functionality.

---

24. Backward Compatibility

API and database changes should consider existing clients.

For breaking changes:

- Clearly document the change.
- Provide a migration strategy.
- Update affected applications.
- Update tests.
- Update release notes.

---

25. Review Checklist

Before submitting a pull request:

[ ] Code follows project structure
[ ] Authentication checked
[ ] Authorization checked
[ ] Privacy checked
[ ] Input validation added
[ ] Database migration added if required
[ ] Tests added/updated
[ ] Security implications reviewed
[ ] Documentation updated if required
[ ] No secrets committed
[ ] No real personal data committed
[ ] No unnecessary dependency added
[ ] Build passes
[ ] Tests pass

---

26. Security Vulnerabilities

Do not create public issues containing undisclosed security vulnerabilities.

Follow the security reporting procedure described in:

SECURITY.md

---

27. Maintainer Review

Maintainers may request changes related to:

- Security
- Privacy
- Architecture
- Code quality
- Testing
- Performance
- Maintainability
- Documentation

A contribution may be rejected if it introduces unacceptable security, privacy, reliability, or maintenance risks.

---

28. Contribution Standard

Every contribution should move the project toward a reliable production system.

The goal is not simply to make a feature work once.

The goal is:

Correct
   +
Secure
   +
Tested
   +
Maintainable
   +
Scalable
   =
Production-ready feature
