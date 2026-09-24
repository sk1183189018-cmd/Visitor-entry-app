Building Visitor & Entry Management System

A production-oriented visitor, resident, guard, and building entry management platform for residential societies, apartments, offices, and managed buildings.

The system provides secure visitor registration, resident approval, guard-controlled entry/exit, QR-based visitor registration, notifications, automatic reporting, role-based access control, and audit logging.

---

1. Project Overview

The Building Visitor & Entry Management System manages the complete lifecycle of a visitor:

Visitor
   │
   ├── Scan Building QR / Open Visitor Web
   │
   ▼
Visitor Registration
   │
   ▼
Select Building → Floor → Flat → Resident
   │
   ▼
Resident Notification
   │
   ├── Approve
   │
   └── Reject
   │
   ▼
Guard Verification
   │
   ▼
Entry Recorded
   │
   ▼
Visitor Inside
   │
   ▼
Exit Recorded
   │
   ▼
Duration Calculated
   │
   ▼
Database
   │
   ▼
Automatic Excel Report

The database remains the authoritative source of information. Excel files are generated automatically from database records for reporting and administrative use.

---

2. Current Deployment

The first deployment is designed for one building.

Building Structure

Building
│
├── Floor 1
│   ├── Flat 101
│   ├── Flat 102
│   └── Flat 103
│
├── Floor 2
│   ├── Flat 201
│   ├── Flat 202
│   └── Flat 203
│
└── Floor 3
    ├── Flat 301
    ├── Flat 302
    └── Flat 303

Total:

- 1 Building
- 3 Floors
- 9 Flats

The architecture is multi-building ready, so additional buildings can be added later without rebuilding the entire application.

---

3. Applications

Admin

Admin has authorized management access to the building.

Admin can manage:

- Building
- Floors
- Flats/Units
- Residents
- Guards
- Security gates
- Visitors
- Visitor history
- Entry/exit records
- Deliveries
- Workers
- Vehicles
- QR codes
- Approvals
- Notifications
- Reports
- Automatic Excel exports
- Audit logs
- Security settings
- System settings

Sensitive information such as full mobile numbers is available only according to the admin's permissions.

---

Guard

The Guard application is designed for daily gate operations.

Guard can:

- Register visitors
- Scan visitor QR codes
- Search existing visitors
- Verify visitor information
- Request resident approval
- View approval status
- Record entry
- Record exit
- View active visitors
- View permitted history
- Register deliveries
- Register workers
- Register vehicles
- Work with limited connectivity using offline synchronization

Guards cannot access information outside their assigned permissions.

---

Resident

Residents can manage visitors related to their own flat.

Resident can:

- Receive visitor notifications
- Approve visitors
- Reject visitors
- Create pre-approved visitors
- View own visitor history
- View active visitor information
- Receive entry/exit notifications
- Manage permitted profile information

Residents cannot access another resident's private information.

---

Visitor

A visitor does not need to install an application.

Visitor access is browser-based.

The visitor can:

1. Scan the building QR code.
2. Open the Visitor Web page.
3. Enter required information.
4. Select the destination flat/resident.
5. Submit the request.
6. Wait for approval.
7. Show the visitor status/pass to the guard.

Example:

https://visitor.example.com/register/<secure-token>

The actual production URL will be configured during deployment.

Visitor Web exposes only the information required for the visitor workflow.

---

4. Visitor Registration

A visitor may provide:

- Full name
- Mobile number
- Purpose of visit
- Visitor type
- Building
- Floor
- Flat
- Resident/host
- Vehicle number
- Photo, if required by building policy

The backend validates all submitted information before storing it.

---

5. Resident Approval

Example:

A visitor wants to visit Flat 203.

Visitor
   │
   ▼
Flat 203 selected
   │
   ▼
System identifies resident
   │
   ▼
Notification sent
   │
   ├── APPROVE
   │
   └── REJECT

If approved:

Approved
   ↓
Guard notified
   ↓
Identity/details verified
   ↓
Entry recorded

If rejected:

Rejected
   ↓
Guard notified
   ↓
Entry denied

The exact approval rules are configurable by authorized administrators.

---

6. Entry and Exit Tracking

A visitor has one logical visit record.

Example:

Visitor: Rahul Sharma

Entry:
23-09-2026 10:32:00

Exit:
23-09-2026 12:15:00

Duration:
1 hour 43 minutes

The system calculates the duration automatically.

The exit operation updates the existing visit/entry record rather than creating an unrelated duplicate record.

---

7. Automatic Excel Reporting

Excel generation is automatic.

Guards and visitors do not manually maintain Excel files.

The workflow is:

App / Web
    ↓
Backend API
    ↓
Database
    ↓
Background Worker
    ↓
Excel Generator
    ↓
Report Storage

Possible reports:

Visitor_Report_23-09-2026.xlsx
Visitor_Report_24-09-2026.xlsx
Monthly_Visitor_Report_September_2026.xlsx
Yearly_Visitor_Report_2026.xlsx
Master-Visitor-Report.xlsx

Possible columns:

Column| Description
Sr No.| Report sequence
Date| Visit date
Building| Building
Floor| Destination floor
Flat| Destination flat
Visitor Name| Visitor name
Mobile| Authorized mobile display
Visitor Type| Guest/delivery/worker/etc.
Purpose| Visit purpose
Host| Resident/host
Entry Time| Entry timestamp
Exit Time| Exit timestamp
Duration| Automatically calculated
Vehicle Number| Vehicle information
Guard| Guard handling the visit
Status| Approved/rejected/completed/etc.

Excel is a reporting format, not the primary database.

---

8. Privacy

The system uses role-based data access.

Example:

Admin

Mobile: 9876543210

Resident

Mobile: 98******10

Other Resident

Mobile: Not available

Visitor

Resident's full mobile number:
Not available

The backend enforces these permissions. Hiding a field only in the frontend is not considered sufficient security.

---

9. Roles

The initial role model includes:

SUPER_ADMIN
ADMIN
GUARD
RESIDENT
VISITOR

Future roles can be added without redesigning the complete authorization system.

Permissions are separated from roles so that administrators can have different levels of access.

---

10. Security

The production system is designed with:

- Secure authentication
- Role-based access control
- API-level authorization
- Input validation
- Rate limiting
- Secure session/token handling
- Password protection
- Sensitive-data protection
- Encryption where appropriate
- Audit logs
- Security event logging
- Secure file storage
- Database backups
- Recovery procedures
- Environment separation
- API security
- Access control policies
- Data retention policies

Security-sensitive operations should be auditable.

---

11. Audit Logging

Important actions are recorded in an audit trail.

Examples:

Admin created resident
Admin updated flat
Guard registered visitor
Resident approved visitor
Resident rejected visitor
Guard recorded entry
Guard recorded exit
Admin viewed sensitive information
Admin exported visitor report
QR code generated
User permissions changed

An audit record can contain:

Actor
Action
Resource
Resource ID
Timestamp
IP information
Device/session information
Result
Metadata

---

12. QR System

QR codes are used for visitor access.

Possible QR types:

Building QR
Gate QR
Temporary Visitor QR
Pre-Approval QR
Visitor Pass QR

QR tokens should not expose sensitive database IDs directly.

Secure, short-lived or revocable tokens should be used where appropriate.

---

13. Notifications

The notification system can support:

Resident notification
Guard notification
Admin notification
Visitor status notification
Entry notification
Exit notification
Security notification

Possible delivery channels:

- Push notification
- SMS
- Email
- WhatsApp, where an approved provider/API is integrated

External messaging services require their own provider accounts and APIs.

---

14. Offline Support

The Guard application may need to operate during temporary network interruptions.

The architecture therefore supports:

Online
   ↓
Local secure queue
   ↓
Network unavailable
   ↓
Operations stored locally
   ↓
Network restored
   ↓
Synchronization
   ↓
Server reconciliation

Synchronization must be idempotent so that the same operation is not accidentally recorded multiple times.

---

15. Architecture

High-level architecture:

                    ┌─────────────────────┐
                    │    Visitor Web      │
                    └──────────┬──────────┘
                               │
┌──────────────┐       ┌───────▼────────┐       ┌───────────────┐
│  Guard App   │──────▶│   Backend API  │◀──────│ Resident App  │
└──────────────┘       └───────┬────────┘       └───────────────┘
                               │
                       ┌───────▼────────┐
                       │    Database    │
                       └───────┬────────┘
                               │
                  ┌────────────▼────────────┐
                  │      Background Worker  │
                  └────────────┬────────────┘
                               │
              ┌────────────────┴────────────────┐
              │                                 │
       ┌──────▼──────┐                   ┌──────▼──────┐
       │ Excel/Report│                   │Notification │
       │ Generation  │                   │   Service   │
       └─────────────┘                   └─────────────┘

                       ┌─────────────────┐
                       │    Admin Web    │
                       └─────────────────┘

---

16. Multi-Building Ready Architecture

Although the initial deployment contains one building, the database model is designed around organizational ownership.

Conceptually:

Organization
   │
   ├── Building A
   │    ├── Floors
   │    └── Units
   │
   ├── Building B
   │    ├── Floors
   │    └── Units
   │
   └── Building C
        ├── Floors
        └── Units

If required later, the hierarchy can also support:

Organization
   ↓
Building
   ↓
Tower / Block
   ↓
Floor
   ↓
Unit

This allows the same backend to support apartments, societies, offices, and larger properties.

---

17. Repository Structure

Building-Entry-Management/
│
├── apps/
│   ├── visitor-web/
│   ├── guard-app/
│   ├── resident-app/
│   ├── resident-web/
│   ├── admin-web/
│   └── super-admin-web/
│
├── services/
│   ├── api/
│   ├── worker/
│   └── realtime/
│
├── packages/
│   ├── database/
│   ├── auth/
│   ├── validation/
│   ├── types/
│   ├── api-client/
│   ├── ui/
│   ├── security/
│   ├── qr/
│   ├── excel/
│   ├── reporting/
│   └── notifications/
│
├── infrastructure/
├── security/
├── storage/
├── database/
├── docs/
├── tests/
├── scripts/
└── .github/

---

18. Environment Separation

The system will use separate environments:

Development
     ↓
Testing
     ↓
Staging
     ↓
Production

Production credentials and databases must never be committed to Git.

Environment variables should be managed through secure deployment infrastructure.

---

19. Database Principle

The database is the authoritative source of truth.

Application
     ↓
API
     ↓
Database
     ↓
Reports / Excel / Notifications

Excel files must never become the primary source of visitor records.

---

20. Testing

The project will include:

- Unit tests
- Integration tests
- API tests
- Authentication tests
- Authorization tests
- Security tests
- Database tests
- Visitor-flow tests
- Entry/exit tests
- Excel generation tests
- Notification tests
- Offline synchronization tests
- End-to-end tests
- Load tests where required

---

21. Deployment

Production deployment may contain:

Web Applications
       │
       ▼
Reverse Proxy / CDN
       │
       ▼
Backend API
       │
       ├── Database
       ├── Cache
       ├── Queue
       ├── Worker
       ├── File Storage
       └── Notification Providers

The exact infrastructure provider can be selected later.

---

22. Development Roadmap

Phase 1 — Foundation

- Monorepo
- Shared configuration
- Type system
- Database foundation
- Authentication foundation
- Authorization foundation

Phase 2 — Core Backend

- Users
- Organizations
- Buildings
- Floors
- Units
- Residents
- Guards
- Visitors
- Visits
- Entry/exit

Phase 3 — Visitor Web

- QR entry
- Registration
- Visitor status
- Secure visitor pass

Phase 4 — Guard Application

- Visitor registration
- QR scanning
- Approval status
- Entry
- Exit
- Active visitors
- Offline synchronization

Phase 5 — Resident Application

- Visitor approvals
- Pre-approvals
- Visitor history
- Notifications

Phase 6 — Admin

- Building management
- Resident management
- Guard management
- Visitor management
- Reports
- Excel
- QR management
- Audit logs
- Security

Phase 7 — Production

- Automated backups
- Monitoring
- CI/CD
- Security testing
- Performance testing
- Deployment
- Disaster recovery

---

23. Initial Data

The first deployment will contain:

Building: 1

Floors:
1
2
3

Units:
101
102
103
201
202
203
301
302
303

Residents and guards should be created through the authorized administration workflow rather than hard-coded into the application.

---

24. Design Principle

The project follows these principles:

1. Security first
2. Database as the source of truth
3. Backend-enforced authorization
4. Privacy by default
5. Automatic reporting
6. No unnecessary visitor installation
7. Offline resilience for gate operations
8. Auditable important actions
9. Multi-building scalability
10. Production-oriented implementation

---

25. License

License information will be defined in the repository's "LICENSE" file.

---

Project Status

Status: Architecture and foundation phase

The system is being developed as a production-oriented building visitor and entry management platform rather than a simple frontend demonstration.
