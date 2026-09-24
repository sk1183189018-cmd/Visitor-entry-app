Changelog

All notable changes to the Building Visitor & Entry Management System will be documented in this file.

The format follows a structured release history so that development, staging, and production changes can be tracked clearly.

---

[Unreleased]

Project Foundation

- Defined production-oriented project architecture.
- Defined single-building initial deployment.
- Defined future multi-building architecture.
- Defined visitor browser-based registration workflow.
- Defined Guard App and Guard Web workflow.
- Defined Resident App and Resident Web workflow.
- Defined Admin Web management workflow.
- Defined future Super Admin architecture.
- Defined role-based access control.
- Defined backend-enforced privacy rules.
- Defined visitor entry and exit lifecycle.
- Defined automatic duration calculation.
- Defined automatic Excel reporting.
- Defined QR-based visitor registration.
- Defined resident approval workflow.
- Defined notification architecture.
- Defined audit logging requirements.
- Defined offline synchronization requirements for guard operations.
- Defined development, testing, staging, and production environments.
- Defined security and privacy requirements.

Initial Building Configuration

The first deployment is configured conceptually for:

Building
│
├── Floor 1
│   ├── 101
│   ├── 102
│   └── 103
│
├── Floor 2
│   ├── 201
│   ├── 202
│   └── 203
│
└── Floor 3
    ├── 301
    ├── 302
    └── 303

Total:

- 1 building
- 3 floors
- 9 flats

---

[0.1.0] - Planned

Foundation

Planned initial development release.

Expected scope:

- Repository foundation
- Monorepo configuration
- Shared configuration
- Type system
- Database foundation
- Authentication foundation
- Authorization foundation
- Development environment

---

[0.2.0] - Planned

Core Backend

Expected scope:

- User management
- Organizations
- Buildings
- Floors
- Units
- Residents
- Guards
- Visitors
- Visit records
- Entry records
- Exit records
- API validation
- API authorization
- Audit events

---

[0.3.0] - Planned

Visitor Web

Expected scope:

- QR-based access
- Visitor registration
- Building/floor/flat selection
- Host selection
- Visitor status
- Secure visitor token
- Visitor pass workflow

---

[0.4.0] - Planned

Guard Application

Expected scope:

- Guard authentication
- Visitor registration
- QR scanner
- Approval status
- Entry recording
- Exit recording
- Active visitors
- Visitor history
- Delivery management
- Worker management
- Vehicle management
- Offline synchronization

---

[0.5.0] - Planned

Resident Applications

Expected scope:

- Resident authentication
- Resident dashboard
- Visitor approvals
- Visitor rejection
- Pre-approved visitors
- Visitor history
- Visitor status
- Entry notifications
- Exit notifications
- Profile management

---

[0.6.0] - Planned

Administration

Expected scope:

- Admin authentication
- Building management
- Floor management
- Unit management
- Resident management
- Guard management
- Gate management
- Visitor management
- Entry/exit management
- QR management
- Notification management
- Audit logs
- System settings

---

[0.7.0] - Planned

Reports & Excel

Expected scope:

- Automatic daily visitor reports
- Monthly reports
- Yearly reports
- Master visitor report
- Excel generation worker
- Report download
- Report filtering
- Report access control
- Scheduled report generation

Example:

Visitor_Report_23-09-2026.xlsx
Visitor_Report_September_2026.xlsx
Master-Visitor-Report.xlsx

---

[0.8.0] - Planned

Notifications

Expected scope:

- Push notifications
- Notification events
- Resident approval notifications
- Guard approval-status notifications
- Entry notifications
- Exit notifications
- SMS provider integration
- Email provider integration

External providers will be configured separately from the core application.

---

[0.9.0] - Planned

Security & Production Hardening

Expected scope:

- Rate limiting
- Security headers
- Improved audit logging
- Session management
- Secret management
- Database backup automation
- Recovery procedures
- File access controls
- Security testing
- Dependency scanning
- API security testing
- Performance testing

---

[1.0.0] - Planned

Production Release

The first production release will be considered complete only after:

- Core visitor workflow is operational
- Entry/exit tracking is operational
- Resident approval is operational
- Guard workflow is operational
- Admin management is operational
- Automatic Excel reporting is operational
- Authentication and authorization are tested
- Security controls are implemented
- Backups are configured
- Monitoring is configured
- Production deployment is tested
- Critical end-to-end workflows pass testing

---

Versioning Rules

The project will use semantic versioning:

MAJOR.MINOR.PATCH

MAJOR

Used for incompatible or major architectural/API changes.

Example:

1.0.0 → 2.0.0

MINOR

Used for backward-compatible feature additions.

Example:

1.0.0 → 1.1.0

PATCH

Used for backward-compatible bug and security fixes.

Example:

1.1.0 → 1.1.1

---

Change Categories

Changes may be categorized as:

- Added
- Changed
- Deprecated
- Removed
- Fixed
- Security
- Performance
- Documentation

---

Release Principle

A version should represent the actual state of the software.

Planned functionality must not be described as implemented functionality.

Production releases should be tagged in Git so that every deployed version can be identified and reproduced.
