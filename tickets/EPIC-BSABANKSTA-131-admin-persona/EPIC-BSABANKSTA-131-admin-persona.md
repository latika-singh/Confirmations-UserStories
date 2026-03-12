# EPIC-BSABANKSTA-131: BSA Admin Persona

> **Epic ID:** BSABANKSTA-131  
> **Batch:** 2 (New)  
> **Feature Reference:** F-002 — BSA Admin Persona  
> **Role Restriction:** BSA Administrator only  
> **Figma Coverage:** 10 frames across 3 stories  

---

## Epic Summary

### Strategic Goal

Empower BSA Administrators with a comprehensive suite of administrative tools for managing system-wide communications, user accounts, and integration audit compliance within the BSA Banking Confirmations application. This epic establishes the operational governance and regulatory traceability infrastructure required by BSA/AML compliance teams, ensuring that administrators have centralized, role-restricted control over the three pillars of administrative operations: communication (system messages), identity (user management), and accountability (audit trail).

### Business Context

BSA/AML compliance operations require robust administrative capabilities to ensure proper user access governance, system-wide communication management, and audit-grade traceability of all integration activities. Regulatory bodies — including FinCEN and OFAC — mandate that financial institutions maintain immutable audit trails, enforce strict role-based access controls, and provide administrators with tools to manage compliance communications and user access in real time.

The BSA Admin Persona centralizes these administrative functions under a role-restricted interface accessible only to users with the BSA Administrator role. This epic enables administrators to:

- **Broadcast compliance updates and system announcements** via global system messages with configurable visibility rules (active/inactive status, date-range scheduling, and audience targeting), ensuring that critical regulatory updates reach all relevant users promptly
- **Manage user accounts, roles, and access permissions** through a centralized user library that integrates with the Auth0 identity provider, providing search, filtering, sortable columns, role assignment, and account activation/deactivation capabilities
- **Monitor and investigate integration activities** through a comprehensive, append-only audit trail that captures all system integration events with multi-dimensional filtering (date range, event type, source system), event detail expansion, and chronological display

These capabilities are critical for maintaining regulatory compliance where audit traceability and access governance are mandatory operational controls. The absence of any one of these three capabilities creates compliance risk — incomplete audit trails may result in regulatory findings, unmanaged user access creates unauthorized data exposure, and missing system message functionality delays critical compliance communications.

### Key Features to be Implemented

- **System Message Management (BSABANKSTA-1579):** Modal-based CRUD interface for global system messages with visibility configuration (active/inactive, date ranges, audience targeting). Includes create, edit, and delete workflows with confirmation dialogs, field validation, and success/error state handling. Covered by 6 Figma frames representing the complete modal lifecycle.

- **User Management Library (BSABANKSTA-1500):** Table-based user directory with search functionality, multi-column filtering, sortable columns, role management, and account activation/deactivation. Integrates with Auth0 Management API for user data retrieval and role operations. Uses lazy load / infinite scroll for the user list (no pagination per CC-RQ-001). Covered by 1 Figma frame.

- **Integration Audit Trail (BSABANKSTA-1458):** Chronological event log displaying all system integration activities with multi-dimensional filtering (date range, event type, source system), inline event detail expansion, and append-only immutable data model ensuring audit-grade integrity. Uses lazy load / infinite scroll for the event log (no pagination per CC-RQ-001). Covered by 3 Figma frames.

- **Role-Based Access Control (RBAC):** All administrative features in this epic are restricted exclusively to users with the BSA Administrator role. BSA Analyst users must not be able to see, access, or navigate to any Admin Settings or administrative interfaces. RBAC enforcement is validated at both the frontend routing level and backend API authorization layer.

- **Lazy Load / Infinite Scroll:** All list and table views across this epic implement cursor-based infinite scroll with React Intersection Observer API, replacing all traditional pagination patterns per CC-RQ-001 (Global Rule #4). This includes the user management table, audit trail event log, and system messages list.

- **Shared Application Header Integration:** Admin Settings (containing user management and system messages access points) are mounted within the Application Header component provided by BSABANKSTA-1531 (Application Frame and Global Navigation). The Admin Settings menu item coexists alongside the Profile Dropdown (BSABANKSTA-1532) in the shared header bar, with conditional rendering based on user role.

### Out of Scope

The following items are explicitly excluded from this epic:

- **Mobile-specific layouts** — All admin interfaces target desktop and responsive web only (per CC-RQ-003). No native mobile or mobile-first layouts are produced.
- **Traditional pagination** — Globally replaced by lazy load / infinite scroll across all list and table views (per Global Rule #4 / CC-RQ-001). No page-number-based navigation is implemented.
- **Write operations to the audit trail** — The integration audit trail is append-only with system-generated events. Administrators can view and filter events but cannot create, edit, or delete audit records.
- **Direct FinCEN/OFAC system integrations** — External regulatory system connections are outside the scope of this epic. The audit trail logs integration events but does not implement the integrations themselves.
- **User self-service profile management** — Covered by BSABANKSTA-1532 (Profile Dropdown Menu) in the Application Frame epic (BSABANKSTA-1531). Users manage their own profiles through the profile dropdown, not through the admin interface.
- **Project space management** — Covered by BSABANKSTA-1305 (Create/Modify Project Space) in Batch 1. Admin users interact with project spaces through F-001 features, not through the admin persona.
- **Database schema migrations** — No data model specifications or migration files are included in this documentation phase (Constraint C-004).
- **Batch 3+ administrative features** — Only the three user stories identified in this epic are in scope. Future administrative capabilities (e.g., advanced analytics dashboards, bulk operations) are deferred to subsequent batches.
- **Advanced user provisioning workflows** — Complex user onboarding/offboarding automation beyond basic account activation/deactivation is not included.

---

## User Stories Index

| Story ID | Title | Story Points | Figma Frames | File Path |
|----------|-------|:------------:|--------------|-----------|
| BSABANKSTA-1579-A | Create Global System Message | 5 | 3 frames (create flow): `8304-120310`, `8304-120342`, `8304-123107` | [STORY-BSABANKSTA-1579-manage-global-system-messages.md](./STORY-BSABANKSTA-1579-manage-global-system-messages.md) (Create section) |
| BSABANKSTA-1579-B | Edit Global System Message | 5 | 3 frames (edit flow): `8304-120318`, `8304-123120`, `8304-123149` | [STORY-BSABANKSTA-1579-manage-global-system-messages.md](./STORY-BSABANKSTA-1579-manage-global-system-messages.md) (Edit section) |
| BSABANKSTA-1500 | Manage User Library | 8 | 1 frame: `7178-133386` | [STORY-BSABANKSTA-1500-manage-user-library.md](./STORY-BSABANKSTA-1500-manage-user-library.md) |
| BSABANKSTA-1458 | View Integration Audit Trail | 8 | 3 frames: `7408-96357`, `7437-87603`, `7485-99556` | [STORY-BSABANKSTA-1458-view-integration-audit-trail.md](./STORY-BSABANKSTA-1458-view-integration-audit-trail.md) |

> **Total Estimated Story Points: 26** (5 + 5 + 8 + 8)

### Decomposition Applied (Global Rule #2)

Story **BSABANKSTA-1579** (Manage Global System Messages) has been decomposed into two sub-stories per Global Rule #2 — the original story contained 11+ acceptance criteria with distinct create and edit workflows across 6 Figma frames:

- **STORY-BSABANKSTA-1579-A** — Create Global System Message (5 pts, 10 ACs, 3 Figma frames: `8304-120310`, `8304-120342`, `8304-123107`)
- **STORY-BSABANKSTA-1579-B** — Edit Global System Message (5 pts, 10 ACs, 3 Figma frames: `8304-120318`, `8304-123120`, `8304-123149`)

Both sub-stories are documented in [STORY-BSABANKSTA-1579-manage-global-system-messages.md](./STORY-BSABANKSTA-1579-manage-global-system-messages.md) with complete 14-section templates for each sub-story.

---

## Dependencies

### Epics This Epic Depends On

| Epic ID | Epic Name | Dependency Type | Description |
|---------|-----------|-----------------|-------------|
| **BSABANKSTA-1305** | Create/Modify Project Space (Batch 1) | **Data Dependency** | The Integration Audit Trail (BSABANKSTA-1458) logs project space lifecycle events originating from F-001. Admin workflows reference project space entities as contextual data. BSABANKSTA-1305 is the data producer; this epic is a data consumer. |
| **BSABANKSTA-1531** | Application Frame and Global Navigation | **UI Container Dependency** | Admin Settings (containing User Management and System Messages access) is mounted within the Application Header component provided by F-005. Navigation routing to all admin features is provided by the global navigation framework. Without the application frame, admin features have no navigation entry point. |
| **Auth0** | External Identity Provider (Managed Service) | **External Service Dependency** | User data is sourced from the Auth0 identity provider. Admin role validation depends on Auth0 role claims (JWT). The User Management Library (BSABANKSTA-1500) integrates with the Auth0 Management API for user data retrieval, role assignment, and account status operations. |

### Epics That Depend On This Epic

| Epic ID | Epic Name | Dependency Type | Description |
|---------|-----------|-----------------|-------------|
| **BSABANKSTA-1531** | Application Frame and Global Navigation | **Shared Application Header (Mutual)** | Admin Settings (BSABANKSTA-1500) and Profile Dropdown (BSABANKSTA-1532) coexist as sibling components in the Application Header bar. The Application Frame epic depends on this epic's Admin Settings component to render the complete header for BSA Administrator users. |
| **BSABANKSTA-1305** | Create/Modify Project Space (Batch 1) | **Reverse Data Dependency** | Admin user management operations (role changes, account deactivation) may affect user access to project spaces managed by F-001. Changes to user roles propagate through Auth0 and affect authorization across all features. |

### Story-Level Dependency Map

| Source Story (This Epic) | Target Story / Epic | Dependency Type | Description |
|--------------------------|---------------------|-----------------|-------------|
| BSABANKSTA-1579 (System Messages) | Application Header (BSABANKSTA-1531) | UI Container | System messages modal is launched from the admin area within the Application Header. The Application Header provides the navigation entry point and mounting context for the modal overlay. |
| BSABANKSTA-1500 (User Management) | BSABANKSTA-1532 — Profile Dropdown (BSABANKSTA-1531) | Shared Application Header | Admin Settings and Profile Dropdown are sibling components in the Application Header bar. Both must render correctly together, with Admin Settings visible only to BSA Administrator role. |
| BSABANKSTA-1500 (User Management) | Auth0 Management API | External Service | User data retrieval, role assignment, and account status operations depend on the Auth0 Management API. Connection configuration uses `[Auth0 Domain]`, `[Auth0 Client ID]`, and `[Auth0 Management API Audience]` deployment variables. |
| BSABANKSTA-1458 (Audit Trail) | BSABANKSTA-1305 — Project Spaces (Batch 1) | Data Dependency | Audit trail logs project space lifecycle events (create, modify, delete, status change) generated by F-001. The audit trail is a read-only consumer of these events. |
| BSABANKSTA-1458 (Audit Trail) | BSABANKSTA-1540 — Global Reporting | Related Reporting | Both epics provide reporting capabilities. The Audit Trail focuses on integration event logs while Global Reporting (F-004) focuses on cross-project confirmation data aggregation. Shared reporting UI patterns (infinite scroll, filtering, export) may be leveraged. |
| BSABANKSTA-1458 (Audit Trail) | BSABANKSTA-1572 — My Projects Dashboard | Contextual Reference | Audit events may reference project entities displayed on the My Projects Dashboard. Project identifiers in audit events link back to dashboard project cards. |
| All stories in this epic | CC-RQ-001 (Cross-Cutting Requirement) | UI Pattern Dependency | All list and table views implement lazy load / infinite scroll using cursor-based data loading with React Intersection Observer API. No traditional pagination is used. |
| All stories in this epic | Auth0 (External Service) | Authentication & Authorization | All admin features require BSA Administrator role validation via Auth0 JWT role claims. Unauthorized users receive 403 Forbidden responses and are redirected away from admin interfaces. |

---

## System Placeholders

> **Global Rule #7 Compliance:** All placeholders listed below are configurable deployment variables. They must NOT be hard-coded in the application. Each placeholder must be resolved at deployment time through environment variables, configuration files, or a runtime configuration service.

| Placeholder | Description | Used By | Example Value |
|-------------|-------------|---------|---------------|
| `[Application Name]` | Configurable application title displayed in system message templates, the admin interface header, and browser tab titles. Must be a configurable deployment variable. | System Messages (BSABANKSTA-1579), Application Header | `BSA Banking Confirmations` |
| `[Admin Email]` | Configurable administrator contact email used for system message attribution and admin notification footers. | System Messages (BSABANKSTA-1579) | `bsa-admin@institution.com` |
| `[Auth0 Domain]` | Auth0 tenant domain for authentication endpoints and user management API calls. Deployment-specific (dev/staging/prod). | User Management (BSABANKSTA-1500), All stories (auth) | `institution.auth0.com` |
| `[Auth0 Client ID]` | Auth0 application client identifier for SPA authentication. Deployment-specific. | All stories (authentication) | `abc123def456` |
| `[Auth0 Management API Audience]` | Auth0 Management API audience identifier for user management operations (user CRUD, role assignment). | User Management (BSABANKSTA-1500) | `https://institution.auth0.com/api/v2/` |
| `[Default Message Visibility Duration]` | Default visibility window (in days) for newly created system messages. Administrators can override per message. | System Messages (BSABANKSTA-1579) | `30` (days) |
| `[Audit Trail Retention Period]` | Retention period for audit log events. Must comply with regulatory requirements for BSA/AML record retention. | Audit Trail (BSABANKSTA-1458) | `7 years` |
| `[Max System Message Length]` | Maximum character count for the system message body field. Enforced by frontend validation and backend schema. | System Messages (BSABANKSTA-1579) | `2000` (characters) |
| `[API Base URL]` | Base URL for all backend API calls. Deployment-specific (dev/staging/prod environments). | All stories | `https://api.bsa-confirmations.institution.com` |
| `[Admin Documentation URL]` | URL linking to the administrator documentation or help resources from the admin interface. | User Management (BSABANKSTA-1500), Audit Trail (BSABANKSTA-1458) | `https://docs.institution.com/bsa-admin` |

---

## Definition of Done (Epic-Level)

The following checklist defines the completion criteria for the BSA Admin Persona epic. All items must be satisfied before the epic can be considered complete and accepted.

### Functional Completion

- [ ] All 4 user stories (BSABANKSTA-1579-A, BSABANKSTA-1579-B, BSABANKSTA-1500, BSABANKSTA-1458) are completed and accepted by the Product Owner
- [ ] All acceptance criteria across all stories pass BDD (Given/When/Then) validation
- [ ] System message CRUD operates correctly via the modal interface:
  - [ ] Create new system messages with all required fields and visibility configuration
  - [ ] Edit existing system messages with pre-populated fields and save confirmation
  - [ ] Delete system messages with confirmation dialog and success feedback
  - [ ] Visibility configuration (active/inactive, date ranges, audience targeting) functions as specified
- [ ] User management library displays users correctly:
  - [ ] User list loads with lazy load / infinite scroll (no pagination)
  - [ ] Search functionality filters users in real time
  - [ ] Column sorting works for all sortable columns
  - [ ] Role management actions execute successfully via Auth0 Management API
  - [ ] Account activation/deactivation toggles user status
- [ ] Integration audit trail operates correctly:
  - [ ] Chronological event log displays with lazy load / infinite scroll (no pagination)
  - [ ] Multi-dimensional filtering works (date range, event type, source system)
  - [ ] Event detail expansion renders complete event data inline
  - [ ] Audit data is read-only and immutable (no create/edit/delete operations available)
  - [ ] Timestamps display correctly with timezone awareness

### Security and Access Control

- [ ] RBAC enforcement verified — only BSA Administrator role can access all admin features
- [ ] BSA Analyst users cannot see or access Admin Settings in the Application Header
- [ ] Unauthorized API calls return 403 Forbidden with appropriate error messaging
- [ ] Auth0 integration verified:
  - [ ] Role validation works correctly via JWT role claims
  - [ ] User data retrieval from Auth0 Management API operates as expected
  - [ ] Token refresh and session management function properly
- [ ] No admin-only data is exposed to non-admin users through any API endpoint

### UI/UX and Design Compliance

- [ ] All list and table views use lazy load / infinite scroll (no pagination per CC-RQ-001)
- [ ] Shared Application Header coordination verified with BSABANKSTA-1531:
  - [ ] Admin Settings menu item renders for BSA Administrator users
  - [ ] Admin Settings menu item is hidden for BSA Analyst users
  - [ ] Profile Dropdown (BSABANKSTA-1532) and Admin Settings coexist without layout conflicts
- [ ] All 10 Figma frames reviewed against implementation:
  - [ ] 6 System Messages Modal frames (create, edit, confirmation, success, error, visibility states)
  - [ ] 1 User Management Library frame
  - [ ] 3 Integration Audit Trail frames
- [ ] Generated UI Specifications for undepicted elements (empty states, error states, end-of-list indicators) reviewed and approved by the design team
- [ ] WCAG 2.1 AA accessibility compliance verified across all admin pages and modals:
  - [ ] Keyboard navigation functional for all interactive elements
  - [ ] Screen reader announcements for modal open/close, loading states, and error messages
  - [ ] Color contrast ratios meet AA minimum thresholds
  - [ ] Focus management correct for modal dialogs (trap focus, return focus on close)

### Non-Functional Requirements

- [ ] All NFRs met per story-specific targets (response times, page load times)
- [ ] Infinite scroll performance verified — no UI jank or memory leaks during extended scrolling
- [ ] Audit trail data integrity verified:
  - [ ] Append-only constraint enforced at the database level
  - [ ] Immutable records cannot be modified through any interface
  - [ ] All timestamps stored in UTC with timezone-aware display

### Integration and Configuration

- [ ] All configurable placeholders resolve correctly per deployment environment:
  - [ ] `[Application Name]` displays correctly in system messages and admin header
  - [ ] Auth0 configuration variables (`[Auth0 Domain]`, `[Auth0 Client ID]`, `[Auth0 Management API Audience]`) connect to the correct tenant
  - [ ] `[Default Message Visibility Duration]` and `[Max System Message Length]` enforce correct limits
  - [ ] `[Audit Trail Retention Period]` is enforced at the data layer
- [ ] Cross-epic dependency links verified bidirectionally:
  - [ ] BSABANKSTA-1305 (Project Spaces) — data dependency verified
  - [ ] BSABANKSTA-1531 (Application Frame) — UI container dependency verified
  - [ ] BSABANKSTA-1540 (Global Reporting) — related reporting patterns aligned
  - [ ] BSABANKSTA-1572 (My Projects Dashboard) — contextual references verified
- [ ] Integration testing with dependent features passes:
  - [ ] F-001 Project Spaces events appear in the audit trail
  - [ ] F-005 Navigation correctly routes to and from admin features

### Testing

- [ ] Unit test coverage meets project standards for all admin feature code
- [ ] Component tests (@testing-library/react) pass for all admin React components
- [ ] BDD acceptance tests (behave) pass for all Given/When/Then scenarios
- [ ] E2E test suite (Playwright) covering all admin workflows passes:
  - [ ] System message create/edit/delete flows
  - [ ] User management search/sort/filter/manage flows
  - [ ] Audit trail browsing and filtering flows
  - [ ] RBAC enforcement (admin access granted, analyst access denied)
- [ ] Cross-browser compatibility verified (Chrome, Firefox, Edge, Safari)

### Documentation

- [ ] All story files are complete with all 14 required sections
- [ ] Cross-epic dependency documentation is accurate and bidirectional
- [ ] Refinement Notes document all global rule applications (decomposition, title changes, pagination overrides)
- [ ] Discrepancy Reviews flag all Figma-only elements not present in Jira requirements
- [ ] System Placeholders section is complete and verified against implementation

---

*Last Updated: Auto-generated as part of Batch 2 documentation pipeline*  
*Epic Owner: BSA Admin Persona Team*  
*Related Epics: [BSABANKSTA-1305](../EPIC-BSABANKSTA-1305-create-modify-project-space.md) | [BSABANKSTA-1531](../EPIC-BSABANKSTA-1531-application-frame-global-navigation/EPIC-BSABANKSTA-1531-application-frame-global-navigation.md) | [BSABANKSTA-1540](../EPIC-BSABANKSTA-1540-global-views-reporting/EPIC-BSABANKSTA-1540-global-views-reporting.md) | [BSABANKSTA-1572](../EPIC-BSABANKSTA-1572-my-projects-dashboard/EPIC-BSABANKSTA-1572-my-projects-dashboard.md)*  
*Dependency Graph: [EPIC_DEPENDENCY_GRAPH.md](../EPIC_DEPENDENCY_GRAPH.md)*
