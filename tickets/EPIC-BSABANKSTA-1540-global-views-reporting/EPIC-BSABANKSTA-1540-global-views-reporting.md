# EPIC-BSABANKSTA-1540: Global Views and Reporting

> **Epic ID:** BSABANKSTA-1540
> **Batch:** 2 (New)
> **Feature Reference:** F-004 — Global Views and Reporting
> **Total Story Points:** 21
> **Figma Frames:** None — All UI Specifications Generated via SOP (Global Rule #8)
> **Status:** Draft — Ready for Refinement

---

## Epic Summary

### Strategic Goal

Provide BSA/AML compliance officers and administrators with comprehensive, organization-wide reporting and global confirmation views that aggregate data across all project spaces, enabling enterprise-level compliance monitoring, cross-project trend analysis, and regulatory reporting readiness. This epic transforms siloed project-level confirmation data into a unified global perspective, empowering the institution to demonstrate holistic BSA program oversight during regulatory examinations and audits.

### Business Context

BSA/AML compliance operations require holistic visibility across all active confirmation projects. Individual project spaces — created and managed via BSABANKSTA-1305 (Batch 1, Feature F-001) — contain project-scoped data. However, regulatory reporting, executive oversight, and compliance audits require aggregated views that span all projects simultaneously. Regulatory bodies such as FinCEN (Financial Crimes Enforcement Network) and OFAC (Office of Foreign Assets Control) mandate enterprise-wide compliance monitoring, making cross-project visibility a non-negotiable operational requirement.

The Global Views and Reporting epic addresses this gap by:

- **Providing cross-project confirmation data aggregation and consolidation** — MongoDB aggregation pipelines merge confirmation records across all project spaces into unified reporting views, eliminating the need for manual cross-project data compilation
- **Enabling global filtering, searching, and sorting across all project spaces** — Multi-dimensional filtering (by date range, project space, confirmation status, confirmation type) empowers compliance officers to isolate specific data subsets across the entire BSA program
- **Supporting compliance officers in identifying patterns, anomalies, and trends across the organization's BSA program** — Aggregated summary statistics surface key metrics (total confirmations, status distribution, project space activity) at a glance
- **Delivering reporting views that align with FinCEN and OFAC regulatory reporting requirements** — Cross-project data aggregation provides the foundation for regulatory report preparation and audit evidence compilation
- **Reducing the manual effort of compiling cross-project compliance data** — Automated aggregation replaces manual spreadsheet-based data consolidation, saving analyst time and reducing error risk

These capabilities are critical for regulatory compliance because BSA/AML examinations and audits require demonstrable evidence of enterprise-wide monitoring and reporting. Without global views, the institution risks regulatory findings for insufficient program-level oversight.

### Key Features to be Implemented

- **Cross-Project Confirmation Data Aggregation**: MongoDB aggregation pipeline for consolidating confirmation data across all project spaces into a single queryable dataset, with role-based access control filtering ensuring users only see data from project spaces they are authorized to access
- **Global Reporting Views**: Table-based reporting interfaces with sortable columns, multi-dimensional filtering, and cursor-based infinite scroll (per CC-RQ-001), providing a comprehensive cross-project confirmation list
- **Data Aggregation Logic for Shared All Confirmations Surface**: Backend services powering the All Confirmations Page — a shared surface where BSABANKSTA-1536 (in BSABANKSTA-1531, Application Frame) provides the navigation container and page shell, and this epic provides the data aggregation backend, API endpoints, and query logic
- **Cross-Project Filtering and Search**: Multi-dimensional filtering capabilities including date ranges, project space selection, confirmation status, and confirmation types — enabling precise data retrieval across the entire BSA program
- **Confirmation Summary Statistics**: Aggregated summary metrics (total confirmations, status distribution, project space activity counts) displayed in summary cards above the main data table, providing at-a-glance organizational metrics
- **Lazy Load / Infinite Scroll**: All list and table views use cursor-based infinite scroll with React Intersection Observer API (no pagination per CC-RQ-001 / Global Rule #4)

### Out of Scope

- **Mobile-specific layouts** — All views target desktop and responsive web only (per CC-RQ-003)
- **Traditional pagination** — Globally replaced by lazy load / infinite scroll (per Global Rule #4 / CC-RQ-001)
- **Project-level CRUD operations** — Covered by BSABANKSTA-1305 (Create/Modify Project Space) in Batch 1
- **Navigation framework and page shell for the All Confirmations Page** — Covered by BSABANKSTA-1536 in BSABANKSTA-1531 (Application Frame and Global Navigation)
- **Administrative reporting (Integration Audit Trail)** — Covered by BSABANKSTA-1458 in BSABANKSTA-131 (BSA Admin Persona)
- **Database schema migrations** — No data model specifications or migration files are included (Constraint C-004)
- **Direct FinCEN/OFAC system integrations** — External regulatory system connections are not specified
- **Batch 3+ reporting features** — Only the stories identified in this epic are in scope
- **Real-time data streaming or push notifications for report updates** — Reporting views load data on demand via API; no WebSocket or push-based updates
- **Data export to PDF/CSV** — While mentioned as a potential capability, specific export functionality is deferred unless explicitly required in the source requirements
- **Personalized/user-specific dashboard views** — Covered by BSABANKSTA-1572 (My Projects Dashboard)

---

## User Stories Index

| Story ID | Title (Verb-Noun) | Story Points | Figma Frames | Status |
|----------|-------------------|:------------:|--------------|--------|
| BSABANKSTA-1541 | View Global Confirmations Report | 8 | None — Generated UI Specifications Required | Draft |
| BSABANKSTA-1542 | Filter Cross-Project Reporting Data | 8 | None — Generated UI Specifications Required | Draft |
| BSABANKSTA-1543 | Aggregate Confirmation Summary Statistics | 5 | None — Generated UI Specifications Required | Draft |

**Total Estimated Story Points: 21** (Fibonacci sum: 8 + 8 + 5)

> **Note on Story Extraction:** Story IDs BSABANKSTA-1541, BSABANKSTA-1542, and BSABANKSTA-1543 were derived from the functional requirements documented for F-004 (Global Views and Reporting) in the project's operational specification and cross-epic references. The three stories decompose F-004's capabilities into: (1) the primary cross-project reporting view with infinite scroll, (2) multi-dimensional filtering and search, and (3) aggregated summary statistics. This decomposition follows the single-responsibility principle and ensures each story meets INVEST criteria.

> **Decomposition Analysis (Global Rule #2):** Each story was evaluated for AC count and workflow complexity. No story exceeds 10 ACs or contains multiple distinct workflows requiring further decomposition.

---

## Dependencies

### Epics This Epic Depends On

| Epic ID | Epic Name | Batch | Dependency Type | Description |
|---------|-----------|:-----:|-----------------|-------------|
| **BSABANKSTA-1305** | Create/Modify Project Space | 1 | **Data Dependency (Critical)** | All reporting views aggregate data from project space entities created and managed by F-001. BSABANKSTA-1305 is the data producer for confirmation records, project metadata, and entity lifecycle data that this epic consumes for cross-project reporting. Without F-001 project space data, reporting views have no data source. The MongoDB aggregation pipelines query the Project Spaces collection managed by F-001. |
| **BSABANKSTA-1531** | Application Frame and Global Navigation | 2 | **Navigation Dependency + Shared Surface** | The Application Frame provides navigation routing to reporting views via the global navigation framework. Additionally, the All Confirmations Page (BSABANKSTA-1536 in BSABANKSTA-1531) provides the navigation container and page shell while this epic provides the data aggregation backend. This is a bidirectional shared-surface dependency. |
| **Auth0** | Auth0 Identity Provider | External Service | **Authentication & RBAC** | User authentication and role-based access control. Reporting views must respect the authenticated user's project space access permissions for data filtering — BSA Administrators see all project spaces; BSA Analysts see only their assigned project spaces. |

### Epics That Depend On This Epic

| Epic ID | Epic Name | Batch | Dependency Type | Description |
|---------|-----------|:-----:|-----------------|-------------|
| **BSABANKSTA-1531** | Application Frame and Global Navigation | 2 | **Shared Surface (All Confirmations)** | The All Confirmations Page in F-005 depends on F-004's data aggregation logic to populate the confirmations table. BSABANKSTA-1536 (View All Confirmations) depends on the reporting API endpoints provided by this epic for cross-project confirmation data retrieval, cursor-based infinite scroll data loading, and sort/filter query execution. |
| **BSABANKSTA-1572** | My Projects Dashboard | 2 | **Related Reporting** | Dashboard may reference global reporting data for summary statistics or cross-project comparisons. Both epics consume F-001 project space data at different aggregation levels — dashboard provides user-specific views while this epic provides organization-level aggregation. |
| **BSABANKSTA-131** | BSA Admin Persona | 2 | **Related Reporting Patterns** | Admin reporting (BSABANKSTA-1458 Integration Audit Trail) is a related reporting capability. Shared reporting UI patterns (infinite scroll tables, filtering, event display) and potentially shared API infrastructure for cursor-based data retrieval. |

### Story-Level Dependency Map

| Source Story (This Epic) | Target Story / Epic | Dependency Type | Description |
|--------------------------|---------------------|-----------------|-------------|
| BSABANKSTA-1541 (View Global Confirmations Report) | BSABANKSTA-1305 (Project Spaces, Batch 1) | Data Dependency | Cross-project report queries the Project Spaces MongoDB collection to aggregate confirmation records across all project spaces. |
| BSABANKSTA-1541 (View Global Confirmations Report) | BSABANKSTA-1536 (View All Confirmations, BSABANKSTA-1531) | Shared Surface | This story provides the data aggregation backend; BSABANKSTA-1536 provides the navigation container and page shell. API contract must align between frontend page component and backend data layer. |
| BSABANKSTA-1541 (View Global Confirmations Report) | BSABANKSTA-1531 (Application Frame) | Navigation Routing | Reporting views are accessed via navigation routes defined in the Application Frame's global navigation framework. |
| BSABANKSTA-1542 (Filter Cross-Project Reporting Data) | BSABANKSTA-1305 (Project Spaces, Batch 1) | Data Dependency | Filter dimensions (project spaces, confirmation statuses, confirmation types) are derived from the F-001 data model. Available filter values are populated from the project space data. |
| BSABANKSTA-1542 (Filter Cross-Project Reporting Data) | BSABANKSTA-1536 (View All Confirmations, BSABANKSTA-1531) | Shared Surface | Filter state is managed as part of the All Confirmations Page — filter changes trigger new data fetches from this epic's API endpoints. |
| BSABANKSTA-1543 (Aggregate Confirmation Summary Statistics) | BSABANKSTA-1305 (Project Spaces, Batch 1) | Data Dependency | Summary statistics are computed from project space confirmation data via MongoDB aggregation pipelines ($group, $count, $sum). |
| BSABANKSTA-1543 (Aggregate Confirmation Summary Statistics) | BSABANKSTA-1572 (My Projects Dashboard) | Related Data | Summary statistics may provide organizational context complementing the personalized dashboard's user-specific project views. |
| All stories (this epic) | CC-RQ-001 (Cross-Cutting Requirement) | UI Pattern Dependency | All list and table views implement the Lazy Load / Infinite Scroll pattern using cursor-based data loading with React Intersection Observer API. No traditional pagination controls are rendered. |
| All stories (this epic) | Auth0 (External Service) | Authentication & Authorization | All reporting endpoints require authenticated sessions. RBAC enforcement filters data by user's accessible project spaces. |

---

## System Placeholders

> **Global Rule #7 Compliance:** All placeholders listed below are configurable deployment variables. They must NOT be hard-coded in the application. Each placeholder must be resolved at deployment time through environment variables, configuration files, or a runtime configuration service.

| Placeholder | Description | Used By | Example Value |
|-------------|-------------|---------|---------------|
| `[Application Name]` | Configurable application title displayed in report page headers, browser tab titles, and breadcrumbs. Must be a configurable deployment variable. | All stories — page titles, report headers | `BSA Banking Confirmations` |
| `[Report Date Format]` | Configurable date format for reporting views. Deployment-specific to accommodate institutional date formatting preferences. | All stories — date columns, date range filters, summary statistics | `MM/DD/YYYY` |
| `[Default Batch Size]` | Configurable number of records to fetch per infinite scroll batch. Deployment-specific based on network and performance characteristics. | BSABANKSTA-1541, BSABANKSTA-1542 — infinite scroll batch size | `25` |
| `[API Base URL]` | Deployment-specific base URL for all reporting API endpoints. Varies by environment (development, staging, production). | All stories — API calls | `https://api.bsa-confirmations.internal/v1` |
| `[MongoDB Connection String]` | Deployment-specific database connection string for MongoDB aggregation pipelines. | All stories — data layer | `mongodb+srv://...` |
| `[Auth0 Domain]` | Auth0 tenant domain for authentication and JWT validation. Deployment-specific. | All stories — authentication | `bsa-confirmations.us.auth0.com` |
| `[Auth0 Client ID]` | Auth0 application client identifier for the SPA. Deployment-specific. | All stories — authentication | `a1b2c3d4e5f6g7h8i9j0` |
| `[Auth0 Audience]` | Auth0 API audience identifier for JWT token scoping and backend authorization. | All stories — API authorization | `https://api.bsa-confirmations.internal` |
| `[Max Aggregation Timeout]` | Maximum allowed execution time for MongoDB aggregation queries before timeout. Prevents long-running queries from blocking resources. | BSABANKSTA-1541, BSABANKSTA-1543 — aggregation pipelines | `30000` (milliseconds) |
| `[Summary Statistics Cache TTL]` | Cache duration for summary statistics data to reduce MongoDB aggregation load. | BSABANKSTA-1543 — summary statistics | `300` (seconds / 5 minutes) |

### Implementation Notes

- All placeholders must be injected via environment variables at build time (for static values) or runtime configuration (for dynamic values).
- The React application should consume these via a centralized configuration module (e.g., `src/config/appConfig.ts`) that reads from `import.meta.env` (Vite environment variables) or a runtime configuration endpoint.
- Auth0-related placeholders are consumed by the `@auth0/auth0-react` `Auth0Provider` component configuration.
- Database and API-related placeholders are consumed by the Flask backend configuration module.

---

## Definition of Done (Epic-Level)

The following checklist defines the completion criteria for the Global Views and Reporting epic. All items must be satisfied before the epic can be considered complete and accepted.

### Functional Completion

- [ ] All 3 user stories (BSABANKSTA-1541, BSABANKSTA-1542, BSABANKSTA-1543) are completed and accepted by the Product Owner
- [ ] All acceptance criteria across all stories pass BDD (Given/When/Then) validation
- [ ] Cross-project data aggregation correctly consolidates data from ALL project spaces via MongoDB aggregation pipelines
- [ ] All reporting views display accurate, up-to-date data reflecting the current state of all project spaces
- [ ] All list and table views use lazy load / infinite scroll — no pagination controls exist (per CC-RQ-001)
- [ ] Filtering and sorting work correctly across all reporting views:
  - [ ] Date range filtering returns records within the specified date range
  - [ ] Project space filtering isolates records from selected project spaces
  - [ ] Confirmation status filtering returns records matching selected statuses
  - [ ] Column sorting (ascending/descending) correctly reorders data
  - [ ] Multiple filters can be applied simultaneously (composable filters)
  - [ ] Clearing filters resets the view to the unfiltered state
- [ ] Data access respects user permissions — BSA Administrators see all project spaces; BSA Analysts see only assigned project spaces
- [ ] Summary statistics correctly compute aggregated metrics across all accessible project spaces
- [ ] Shared All Confirmations surface verified with BSABANKSTA-1536 (navigation shell) and F-004 (data layer)

### Non-Functional Requirements

- [ ] All reporting API endpoints return data within < 2 seconds for standard queries
- [ ] Initial page loads complete within < 3 seconds
- [ ] Infinite scroll batch loads complete within < 1 second per batch
- [ ] Sort and filter operations complete within < 1 second
- [ ] MongoDB aggregation queries execute within < 3 seconds for cross-project queries
- [ ] No pagination patterns exist — all views use lazy load / infinite scroll
- [ ] WCAG 2.1 AA accessibility compliance verified across all reporting pages:
  - [ ] Keyboard navigation functional for all interactive elements (table headers, filters, buttons)
  - [ ] Screen reader announcements for sort changes, filter applications, data loading, and error states
  - [ ] Color contrast ratios meet AA minimum thresholds (4.5:1 for text)
  - [ ] `aria-sort` attributes correctly applied to sortable column headers
  - [ ] Focus indicators visible on all interactive elements

### Cross-Epic Integration

- [ ] Cross-epic dependency links verified bidirectionally with all related epics:
  - [ ] BSABANKSTA-1305 (Create/Modify Project Space) — data dependency verified
  - [ ] BSABANKSTA-1531 (Application Frame) — navigation routing and shared All Confirmations surface verified
  - [ ] BSABANKSTA-131 (BSA Admin Persona) — related reporting patterns aligned
  - [ ] BSABANKSTA-1572 (My Projects Dashboard) — organizational vs. personalized view relationship documented
- [ ] Shared All Confirmations Page surface verified:
  - [ ] Frontend page shell (BSABANKSTA-1536 in BSABANKSTA-1531) correctly renders data from F-004 backend
  - [ ] API contract between frontend and backend aligns (request/response schema)
  - [ ] Infinite scroll triggers data fetching from the Global Reporting API endpoints

### Configuration and Deployment

- [ ] All configurable placeholders resolve correctly per deployment environment
- [ ] `[Application Name]` displays correctly in report headers and page titles
- [ ] `[Report Date Format]` formats dates consistently across all reporting views
- [ ] `[Default Batch Size]` configures the correct infinite scroll batch size
- [ ] Auth0 configuration variables connect to the correct tenant
- [ ] `[MongoDB Connection String]` connects to the correct database

### Quality Assurance

- [ ] ALL Generated UI Specifications reviewed by design team (no Figma frames for this epic — DESIGN REVIEW REQUIRED)
- [ ] Unit tests pass for all backend API endpoints (pytest)
- [ ] Unit tests pass for all MongoDB aggregation pipelines (pytest)
- [ ] Component tests pass for all React components (Vitest + @testing-library/react)
- [ ] BDD acceptance tests pass (behave)
- [ ] E2E test suite covering all reporting workflows passes (Playwright):
  - [ ] Navigate to reporting view → data loads with infinite scroll
  - [ ] Apply filters → data re-loads with filter parameters
  - [ ] Sort columns → data re-sorts correctly
  - [ ] Scroll to end → end-of-list indicator appears
  - [ ] Empty state → displayed when no data matches filters
  - [ ] Error state → displayed with retry on API failure
  - [ ] RBAC → administrator sees all data; analyst sees only assigned project spaces
- [ ] Integration testing with upstream data sources (F-001 project spaces) passes
- [ ] Documentation is complete and reviewed

---

## Design Token Manifest

> **Global Rule #8, Step 1 — Design Token Manifest:** The following manifest was constructed from the Figma BSA Wireframes file (`6fQyfvBUqImyavY8Fw47FV`) Assets panel. Since F-004 has NO dedicated Figma frames, this manifest governs ALL Generated UI Specifications for every story in this epic.

### Colors

| Token | Value | Usage |
|-------|-------|-------|
| Brand Primary | `blue-600` (#2563EB) | Primary actions, active states, links |
| Brand Primary Hover | `blue-700` (#1D4ED8) | Hover states for primary actions |
| Surface White | `white` (#FFFFFF) | Card backgrounds, page background |
| Surface Gray Light | `gray-50` (#F9FAFB) | Table header background, alternate row background |
| Surface Gray | `gray-100` (#F3F4F6) | Hover row background |
| Border Light | `gray-200` (#E5E7EB) | Table borders, card borders, dividers |
| Border Medium | `gray-300` (#D1D5DB) | Input borders, filter control borders |
| Text Primary | `gray-900` (#111827) | Headings, primary text content |
| Text Secondary | `gray-600` (#4B5563) | Descriptions, secondary labels |
| Text Tertiary | `gray-500` (#6B7280) | Placeholder text, muted content |
| Text Muted | `gray-400` (#9CA3AF) | End-of-list indicator, disabled text |
| Success | `green-600` (#16A34A) | Active/completed status indicators |
| Success Background | `green-100` (#DCFCE7) | Success badge background |
| Error | `red-600` (#DC2626) | Error text, error icons |
| Error Background | `red-50` (#FEF2F2) | Error state container background |
| Error Border | `red-200` (#FECACA) | Error state container border |
| Warning | `amber-600` (#D97706) | Warning/pending status indicators |
| Warning Background | `amber-100` (#FEF3C7) | Warning badge background |
| Info | `blue-600` (#2563EB) | Informational indicators, active filters |

### Typography

| Token | Value | Usage |
|-------|-------|-------|
| Font Family | System default (Inter, -apple-system, sans-serif) | All text |
| Heading Large | `text-2xl font-semibold` (24px, 600 weight) | Page titles |
| Heading Medium | `text-xl font-semibold` (20px, 600 weight) | Section titles |
| Heading Small | `text-lg font-semibold` (18px, 600 weight) | Card titles, summary card headings |
| Body | `text-base font-normal` (16px, 400 weight) | Body text, descriptions |
| Body Small | `text-sm font-normal` (14px, 400 weight) | Table cell content, filter labels |
| Caption | `text-xs font-medium` (12px, 500 weight) | Table headers, badge text, labels |
| Caption Uppercase | `text-xs font-medium uppercase tracking-wider` | Table column headers |

### Spacing

| Token | Value | Usage |
|-------|-------|-------|
| Page Padding X | `px-4 sm:px-6 lg:px-8` (16/24/32px) | Page container horizontal padding |
| Page Padding Y | `py-8` (32px) | Page container vertical padding |
| Section Gap | `mb-6` (24px) | Between page header and content sections |
| Card Padding | `p-6` (24px) | Summary card internal padding |
| Card Gap | `gap-6` (24px) | Between summary cards in grid |
| Filter Panel Padding | `p-4` (16px) | Filter panel internal padding |
| Filter Panel Margin Bottom | `mb-6` (24px) | Between filter panel and data table |
| Table Cell Padding | `px-6 py-4` (24px horizontal, 16px vertical) | Table cell content padding |
| Table Header Padding | `px-6 py-3` (24px horizontal, 12px vertical) | Table header cell padding |

### Border Radii

| Token | Value | Usage |
|-------|-------|-------|
| Small | `rounded-sm` (2px) | Small UI elements |
| Medium | `rounded-md` (6px) | Inputs, buttons, summary cards, filter controls |
| Large | `rounded-lg` (8px) | Filter panel, modal containers |
| Extra Large | `rounded-xl` (12px) | Large container elements |
| Full | `rounded-full` | Status badges, circular indicators |

### Effects

| Token | Value | Usage |
|-------|-------|-------|
| Shadow Small | `shadow-sm` | Summary cards, filter panel |
| Shadow Medium | `shadow-md` | Hover states, dropdown menus |
| Shadow Large | `shadow-lg` | Modals, overlays |
| Ring Focus | `ring-2 ring-blue-500` | Focus state for interactive elements |
| Transition Standard | `transition-all duration-150` | Hover transitions, state changes |
| Spin Animation | `animate-spin` | Loading spinner animation |

---

## Component Mapping Reference

| UI Element | TailwindCSS Pattern | Token Priority | Notes |
|-----------|-------------------|----------------|-------|
| Report Page Container | `max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8` | Priority 2 — Semantic | Standard page container |
| Report Header | `text-2xl font-semibold text-gray-900 mb-6` | Priority 2 — Semantic | Page title styling |
| Report Description | `text-base text-gray-600 mb-8` | Priority 2 — Semantic | Subtitle / context |
| Data Table | `table-auto w-full` with `overflow-auto` container | Priority 1 — Reuse from BSABANKSTA-1536 | Match All Confirmations pattern |
| Table Header Row | `bg-gray-50 border-b border-gray-200` with `text-xs font-medium text-gray-500 uppercase tracking-wider` | Priority 1 — Reuse | Standard header pattern |
| Sortable Column Header | `cursor-pointer hover:bg-gray-100` with sort indicator icon + `aria-sort` | Priority 2 — Semantic | Accessibility required |
| Table Body Row | `border-b border-gray-200 hover:bg-gray-50` | Priority 1 — Reuse | Standard row pattern |
| Table Cell | `px-6 py-4 text-sm text-gray-900` | Priority 1 — Reuse | Standard cell styling |
| Filter Panel | `bg-white rounded-lg shadow-sm border border-gray-200 p-4 mb-6` | Priority 2 — Semantic | Above table |
| Filter Input | `border border-gray-300 rounded-md px-3 py-2 text-sm focus:ring-2 focus:ring-blue-500 focus:border-blue-500` | Priority 2 — Semantic | Standard form control |
| Filter Select Dropdown | `border border-gray-300 rounded-md px-3 py-2 text-sm bg-white` | Priority 2 — Semantic | Dropdown filter control |
| Date Range Picker | `border border-gray-300 rounded-md px-3 py-2` with calendar icon | Priority 2 — Semantic | For date filtering |
| Filter Chip / Tag | `inline-flex items-center px-3 py-1 rounded-full text-sm bg-blue-100 text-blue-800` with remove icon | Priority 2 — Semantic | Active filter indicator |
| Clear Filters Button | `text-sm text-blue-600 hover:text-blue-800 font-medium` | Priority 2 — Semantic | Reset all filters |
| Summary Card | `bg-white rounded-md shadow-sm p-6` in `grid grid-cols-1 md:grid-cols-3 gap-6` | Priority 2 — Semantic | Summary statistics |
| Summary Card Metric | `text-3xl font-bold text-gray-900` | Priority 2 — Semantic | Large metric number |
| Summary Card Label | `text-sm text-gray-500 mt-1` | Priority 2 — Semantic | Metric description |
| Status Badge | `inline-flex items-center px-2.5 py-0.5 rounded-full text-xs font-medium` with semantic colors | Priority 2 — Semantic | Confirmation status |
| InfiniteScrollTrigger | `overflow-auto` container with Intersection Observer trigger row | Priority 1 — Reuse shared | Shared pattern |
| Loading Indicator | `flex justify-center py-4` with `animate-spin h-6 w-6 text-blue-600` | Priority 2 — Semantic | During data fetch |
| Loading Skeleton Row | `animate-pulse bg-gray-200 h-12 rounded` | Priority 2 — Semantic | Table row placeholder |
| Empty State | `flex flex-col items-center justify-center py-16 text-gray-500` | Priority 1 — Reuse shared | No data display |
| End-of-List | `text-center py-4 text-gray-400` with divider | Priority 1 — Reuse shared | All records loaded |
| Error State / Retry | `bg-red-50 border border-red-200 rounded-md p-4` with retry button | Priority 1 — Reuse shared | API failure |

---

## Technology Stack References

| Category | Package | Version | Usage in Sub-Tasks |
|----------|---------|---------|-------------------|
| Backend Runtime | Python | 3.13.x | Backend runtime environment |
| Backend Framework | Flask | 3.1.3 | API blueprint routes for reporting endpoints |
| Validation | Marshmallow | ≥3.26.2 | Request/response schema serialization |
| Database | PyMongo | ≥4.7.0 | MongoDB aggregation pipeline queries |
| Authentication | authlib / pyjwt | ≥1.6.8 / ≥2.10.1 | JWT validation for RBAC |
| CORS | Flask-CORS | ≥6.0.2 | Cross-origin configuration |
| AI Integration | LangChain | ≥1.2.5 | Blitzy Platform communication and AI-assisted processing |
| Backend Testing | pytest | ≥9.0.2 | Unit tests for API and data layer |
| BDD Testing | behave | 1.x | Acceptance criteria validation |
| Frontend Framework | React | 19.2.4 | UI components |
| Type Safety | TypeScript | 5.9.x | Frontend type definitions |
| CSS Framework | TailwindCSS | 4.2.1 | Design token application and styling |
| Build Tool | Vite | ≥7.3.1 | Frontend build with @tailwindcss/vite |
| Routing | React Router | 7.x | Client-side navigation |
| Auth | @auth0/auth0-react | SPA SDK | Session management and RBAC |
| Frontend Testing | Vitest | ≥4.0.18 | Unit tests |
| Component Testing | @testing-library/react | 16.x | Component behavior tests |
| E2E Testing | Playwright | ≥1.55.1 | End-to-end workflow tests |
| Database | MongoDB | ≥8.0.17 (Atlas) | Reporting Views collection, aggregation pipelines |

---
---

# STORY DOCUMENTATION

> The following sections contain complete 14-section story documentation for each user story in this epic. Stories are separated by horizontal rules (`---`).

---

# View Global Confirmations Report

**Story ID:** BSABANKSTA-1541
**Epic:** BSABANKSTA-1540 — Global Views and Reporting
**Batch:** 2

---

## User Story

**As a** BSA Analyst or BSA Administrator,
**I want** to view a cross-project global confirmations report with sortable columns and cursor-based infinite scroll,
**So that** I can monitor and review all confirmation records aggregated across all accessible project spaces in a single unified view, improving compliance oversight efficiency and enabling enterprise-level BSA program monitoring.

---

## INVEST Validation

| Principle | Validation | Notes |
|-----------|------------|-------|
| **Independent** | ✅ Pass | The global confirmations report view can be developed independently from the filtering (BSABANKSTA-1542) and summary statistics (BSABANKSTA-1543) stories. Data loading, table rendering, and infinite scroll are self-contained. The data aggregation API can be built independently from the page shell (BSABANKSTA-1536). |
| **Negotiable** | ✅ Pass | Column configuration, default sort order, batch size, and table styling are negotiable implementation details. The core requirement is a cross-project data table with infinite scroll and column sorting. |
| **Valuable** | ✅ Pass | Provides organization-wide confirmation visibility critical for BSA/AML compliance monitoring. Enables compliance officers and administrators to review all confirmation records without switching between individual project spaces, directly supporting regulatory oversight requirements. |
| **Estimable** | ✅ Pass | The table-with-infinite-scroll pattern is well-understood. The closest visual reference is the All Confirmations Page (BSABANKSTA-1536, frame `8241-118805`). MongoDB aggregation pipeline patterns for cross-project data are established. Estimation is feasible. |
| **Small** | ✅ Pass | Single table-view page with cross-project data loading, sortable columns, and infinite scroll. Well-scoped to a single page interaction pattern (navigate → load → scroll → sort) without distinct multi-workflow branches. |
| **Testable** | ✅ Pass | Can verify: data loading from multiple project spaces, column sorting (ascending/descending), infinite scroll triggering and appending, empty state, error state with retry, end-of-list indicator, and role-based access filtering. |

---

## Non-Functional Requirements

| NFR ID | Category | Requirement | Target |
|--------|----------|-------------|--------|
| NFR-1541-01 | Performance | Initial page load to first meaningful paint | < 3 seconds |
| NFR-1541-02 | Performance | Next batch of records loads when scroll threshold is reached | < 1 second |
| NFR-1541-03 | Performance | Client-side sort operation triggers data re-fetch | < 1 second |
| NFR-1541-04 | Performance | API response time for confirmation data retrieval | < 2 seconds |
| NFR-1541-05 | Performance | MongoDB aggregation pipeline execution for cross-project query | < 3 seconds |
| NFR-1541-06 | Accessibility | WCAG 2.1 AA compliance for data tables | Proper `<th>`/`<td>` semantics, `aria-sort` attributes on sortable headers, screen reader announcements for sort changes and data loading |
| NFR-1541-07 | Responsiveness | Desktop-first responsive layout | No mobile-specific layouts per CC-RQ-003; responsive breakpoints for tablet and desktop |
| NFR-1541-08 | Data Freshness | Report data reflects the latest state from all project spaces | Each new batch fetched during infinite scroll retrieves current data |
| NFR-1541-09 | Scalability | Infinite scroll handles large datasets without degrading performance | No memory leaks from DOM element accumulation; consider virtualized rendering for very large datasets |

---

## Acceptance Criteria

### AC1: Navigate to Global Confirmations Report

```gherkin
Scenario: User navigates to the global confirmations report
  Given the user is authenticated and has access to the application
  And the global navigation or reporting menu is visible
  When the user clicks on the "All Confirmations" or "Global Report" item in the navigation
  Then the Global Confirmations Report page is displayed
  And the report data table is rendered with column headers
  And the page URL reflects the reporting route
  And the page title includes "[Application Name] — Global Confirmations Report"
```

### AC2: Cross-Project Data Loading with Infinite Scroll

```gherkin
Scenario: Initial data load uses cursor-based lazy loading
  Given the user has navigated to the Global Confirmations Report page
  When the page renders for the first time
  Then the initial batch of confirmation records from ALL accessible project spaces is displayed in the table
  And data is loaded using cursor-based lazy loading (NOT traditional pagination)
  And a loading indicator is shown while the initial batch is being fetched
  And the number of records in the initial batch matches the configured "[Default Batch Size]"
  And each record displays the originating project space name for cross-project context
```

### AC3: Infinite Scroll Trigger

```gherkin
Scenario: Next batch loads automatically when user scrolls to bottom
  Given the Global Confirmations Report table is displaying the current batch of records
  And additional confirmation records exist beyond the current batch
  When the user scrolls to the bottom of the currently loaded records
  And the Intersection Observer detects the scroll threshold element
  Then the next batch of confirmation records is automatically fetched from the API using the cursor token
  And the newly fetched records are appended to the bottom of the existing table rows
  And a loading indicator is displayed at the bottom of the table during the fetch
  And the user's scroll position is preserved after new records are appended
```

### AC4: End-of-List Indicator

```gherkin
Scenario: End-of-list indicator displays when all records are loaded
  Given the user has scrolled through all available confirmation records
  And no additional records remain to be fetched (next_cursor is null)
  When the user reaches the end of the loaded data
  Then an end-of-list indicator is displayed below the table (e.g., "All confirmations loaded")
  And no further API calls are triggered for additional data
  And the Intersection Observer is disconnected to prevent unnecessary observations
```

### AC5: Sortable Columns

```gherkin
Scenario: User sorts report data by clicking a column header
  Given the Global Confirmations Report table is displaying records
  And the table has sortable column headers with sort indicators
  When the user clicks on a sortable column header
  Then the table data is re-loaded from the first batch sorted by that column in ascending order
  And the active sort column header displays an ascending sort indicator
  And the aria-sort attribute on the active column is set to "ascending"
  And a loading indicator is shown during the sort re-fetch

Scenario: User toggles sort direction
  Given the report table is sorted by a column in ascending order
  When the user clicks on the same column header again
  Then the sort direction toggles to descending order
  And data is re-loaded from the first batch with the new sort direction
  And the sort indicator updates to reflect descending order
  And the aria-sort attribute updates to "descending"
```

### AC6: Cross-Project Confirmation Aggregation

```gherkin
Scenario: Confirmation records are aggregated across all accessible project spaces
  Given the user has access to multiple project spaces
  When the Global Confirmations Report page loads
  Then confirmation records are aggregated across ALL accessible project spaces
  And each confirmation record displays its originating project space name for context
  And records from different project spaces are interleaved based on the active sort order
  And the total record count reflects the aggregate across all accessible project spaces
```

### AC7: Empty State Display

```gherkin
Scenario: Empty state displays when no confirmation records exist
  Given the user has no project spaces assigned or no confirmation records exist across any accessible project space
  When the Global Confirmations Report page loads
  Then a meaningful empty state message is displayed (e.g., "No confirmations found")
  And the empty state includes a descriptive subtitle and an appropriate icon
  And no error message is shown (this is a valid data state, not an error)
```

### AC8: Error State with Retry

```gherkin
Scenario: Error state with retry displays when API call fails
  Given the user is on the Global Confirmations Report page
  And the API call to retrieve confirmation data fails due to a server or network error
  When the error is detected
  Then an error message is displayed describing the issue (e.g., "Unable to load report data")
  And a "Retry" button is displayed below the error message
  When the user clicks the "Retry" button
  Then the API call is retried
  And a loading indicator is shown during the retry attempt
```

### AC9: Role-Based Data Access

```gherkin
Scenario: BSA Analyst sees only confirmations for accessible project spaces
  Given a user with the BSA Analyst role is authenticated
  And the analyst has access to a subset of project spaces
  When the analyst navigates to the Global Confirmations Report
  Then only confirmation records from project spaces the analyst has access to are displayed

Scenario: BSA Administrator sees confirmations across all project spaces
  Given a user with the BSA Administrator role is authenticated
  When the administrator navigates to the Global Confirmations Report
  Then confirmation records from ALL project spaces in the system are displayed
```

---

## Sub-Tasks

### Model

| Sub-Task ID | Description | Technical Details |
|-------------|-------------|-------------------|
| ST-1541-M1 | MongoDB aggregation pipeline for cross-project confirmation data retrieval | Design a MongoDB aggregation pipeline using PyMongo ≥4.7.0 that queries across the Project Spaces collection (`BSABANKSTA-1305`), applying `$match` for RBAC-based project space filtering, `$lookup` for project space name resolution, and `$sort` for column ordering. Implement cursor-based data loading using compound keys (sort field + `_id`). |
| ST-1541-M2 | Cursor-based data loading model for infinite scroll | Implement cursor token generation and parsing using base64-encoded compound keys. Define configurable batch size via `[Default Batch Size]` (default: 25 records). Support forward-only cursor traversal with `next_cursor` token. |
| ST-1541-M3 | Marshmallow schema for global confirmations report response | Define `GlobalConfirmationsResponseSchema` (Marshmallow ≥3.26.2) with fields: `data` (list of confirmation objects including `project_space_name`), `next_cursor` (string or null), `total_count` (integer), `sort_by` (string), `sort_direction` (string). Include nested `ConfirmationItemSchema`. |
| ST-1541-M4 | Sort parameter model | Define `SortParameterSchema` (Marshmallow ≥3.26.2) with fields: `sort_by` (enum of sortable column names), `sort_direction` (enum: `asc`, `desc`). Validate against allowed sortable columns. |

### API

| Sub-Task ID | Description | Technical Details |
|-------------|-------------|-------------------|
| ST-1541-A1 | Flask blueprint route for `GET /api/reports/confirmations` | Implement Flask 3.1.3 blueprint endpoint with query parameters: `cursor` (optional string), `limit` (optional integer, default from `[Default Batch Size]`, max 100), `sort_by` (optional string, default `created_date`), `sort_direction` (optional string, default `desc`). Require JWT authentication via authlib/pyjwt. |
| ST-1541-A2 | Response contract definition | Response JSON: `{ "data": [...], "next_cursor": "string|null", "total_count": number, "sort_by": "string", "sort_direction": "string" }`. Each item in `data` includes confirmation fields plus `project_space_id` and `project_space_name`. |
| ST-1541-A3 | Cross-project data aggregation endpoint logic | Server-side aggregation merges confirmation records across all project spaces accessible to the authenticated user. Apply sort parameters before cursor slicing. Use PyMongo aggregation pipeline with `$facet` for parallel total count and cursor-sliced results. |
| ST-1541-A4 | RBAC middleware for reporting endpoints | Apply role-based access control: BSA Administrator sees all project spaces; BSA Analyst sees only assigned project spaces. Extract accessible project space IDs from JWT claims or user profile. Inject into aggregation query `$match` filter. Configure Flask-CORS ≥6.0.2 for cross-origin requests. |

### Component

| Sub-Task ID | Description | Technical Details |
|-------------|-------------|-------------------|
| ST-1541-C1 | React `GlobalConfirmationsReportPage` component | Top-level page component (React 19.2.4 + TypeScript 5.9.x) with React Router 7.x route registration at `/reports/confirmations`. TailwindCSS 4.2.1 layout: `max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8`. Manages page-level state (loading, error, data, sort). |
| ST-1541-C2 | `ReportDataTable` component | Data table with TailwindCSS: `table-auto w-full` in `overflow-auto` container. Sortable column headers with click handlers, sort indicator icons, and `aria-sort` attributes. Column header: `bg-gray-50 border-b border-gray-200 text-xs font-medium text-gray-500 uppercase tracking-wider`. Body rows: `border-b border-gray-200 hover:bg-gray-50`. |
| ST-1541-C3 | `InfiniteScrollTrigger` component | Invisible sentinel element rendered after the last table row. Uses Intersection Observer API to detect viewport intersection. Triggers data fetch callback. Disconnects observer when `next_cursor` is null. |
| ST-1541-C4 | `EndOfListIndicator` component | Rendered when all data loaded (`next_cursor` is null and data is non-empty). TailwindCSS: `text-center py-4 text-gray-400` with `border-t border-gray-200` divider above. Displays "All confirmations loaded". |
| ST-1541-C5 | `EmptyState` component (shared) | Rendered when no records exist. TailwindCSS: `flex flex-col items-center justify-center py-16 text-gray-500`. Includes icon, `text-lg font-semibold` heading, `text-sm text-gray-400` subtitle. |
| ST-1541-C6 | `ErrorStateRetry` component (shared) | Rendered on API failure. TailwindCSS: `bg-red-50 border border-red-200 rounded-md p-4`. Error message in `text-red-700`, "Retry" button styled as primary action. |
| ST-1541-C7 | Column header sort indicators | Visual sort direction indicators (chevron up/down icons) within `<th>` elements. Applies `aria-sort="ascending"`, `aria-sort="descending"`, or `aria-sort="none"`. TailwindCSS: `cursor-pointer hover:bg-gray-100`. |
| ST-1541-C8 | Loading skeleton/indicator | Loading indicator during initial fetch: skeleton rows (`animate-pulse bg-gray-200 h-12 rounded`). During infinite scroll: spinner at bottom (`flex justify-center py-4` with `animate-spin h-6 w-6 text-blue-600`). |

### Logic

| Sub-Task ID | Description | Technical Details |
|-------------|-------------|-------------------|
| ST-1541-L1 | Infinite scroll data fetching with cursor management | Custom React hook (`useInfiniteReportData`) managing cursor state, data accumulation, loading state, error state, and total count. Handles fetch, append, and reset operations. Uses `AbortController` for fetch cancellation on sort change. |
| ST-1541-L2 | Client-side sort state management | Sort state (column + direction) managed via React state or URL search parameters (`useSearchParams`). Sort change triggers: (1) cancel pending fetch via AbortController, (2) reset accumulated data, (3) refetch from first batch with new sort parameters and fresh cursor. |
| ST-1541-L3 | Cross-project data display logic | Frontend receives pre-merged data from API. Display logic maps each confirmation record to its `project_space_name`. Handles potential data shape variations. Formats dates using `[Report Date Format]` placeholder. |
| ST-1541-L4 | RBAC enforcement — client-side route guard | React Router route guard via `@auth0/auth0-react` ensures only authenticated users access the report. Role-based data filtering is enforced server-side; client renders whatever data the API returns. |
| ST-1541-L5 | Error handling and retry logic | Centralized error handling for API failures. Retry logic resets error state and re-invokes the last failed API call. Handles both initial load errors (full-page error) and infinite scroll batch errors (inline error below loaded rows). |
| ST-1541-L6 | React Router integration | Register `/reports/confirmations` route. Integrate with global navigation for route highlighting. Coordinate with BSABANKSTA-1536's `/confirmations` route (shared surface). |

### Testing

| Sub-Task ID | Description | Technical Details |
|-------------|-------------|-------------------|
| ST-1541-T1 | Unit tests (pytest ≥9.0.2) for reporting API endpoint | Test cursor-based data loading logic, sort parameter validation, RBAC filtering (admin vs. analyst), empty result set, error responses, boundary conditions (first page, last page, invalid cursor). |
| ST-1541-T2 | Unit tests (pytest ≥9.0.2) for MongoDB aggregation pipeline | Test cross-project aggregation correctness, cursor generation/parsing, sort application, RBAC filter injection, large dataset handling, timeout enforcement via `[Max Aggregation Timeout]`. Mock PyMongo collections. |
| ST-1541-T3 | Component tests (@testing-library/react 16.x) for `GlobalConfirmationsReportPage` | Test initial data loading, table rendering, sort interaction, empty state rendering, error state rendering with retry, and loading state transitions. |
| ST-1541-T4 | Component tests for `ReportDataTable` | Test column header rendering, sort click handling, `aria-sort` attribute management, row rendering with project space origin. |
| ST-1541-T5 | Component tests for `InfiniteScrollTrigger` | Test Intersection Observer setup, callback invocation on intersection, and observer disconnection when no more data. |
| ST-1541-T6 | BDD tests (behave 1.x) for acceptance criteria | Implement feature files mapping to all 9 AC scenarios (AC1–AC9). Validate end-to-end behavior using Given/When/Then steps. |
| ST-1541-T7 | E2E tests (Playwright ≥1.55.1) for full report flow | Test complete flow: navigation → initial load → scroll-triggered batch → column sorting → empty state → error state with retry → RBAC enforcement. |
| ST-1541-T8 | Accessibility tests | Verify `aria-sort` attributes, keyboard navigation through sortable headers, screen reader announcements for sort changes and loading states, focus management. |
| ST-1541-T9 | Performance tests | Verify < 3s initial page load to first meaningful paint under representative data volume. |

---

## Edge Cases

| # | Edge Case | Expected Behavior | Category |
|---|-----------|-------------------|----------|
| 1 | **Massive Dataset** — Hundreds of thousands of confirmation records across hundreds of project spaces | Infinite scroll handles gracefully with consistent performance. MongoDB aggregation pipeline uses indexed queries and cursor-based slicing to avoid full-collection scans. Frontend uses virtualized rendering if DOM node count exceeds threshold. No memory leaks from accumulating rows. | Performance / Scalability |
| 2 | **Network Interruption During Scroll** — API call for next batch fails mid-scroll due to network loss | Display inline error message below the last loaded row with a retry option. Preserve all previously loaded records in the table. Do not clear or reset existing data. Resume loading from the same cursor position on retry. | Error Handling |
| 3 | **Zero Accessible Projects** — Authenticated user has no project space access permissions assigned | Display the shared Empty State component with an appropriate message (e.g., "No project spaces assigned. Contact your administrator for access."). Do not show an error state — this is a valid authorization state, not an API failure. | Authorization / Empty State |
| 4 | **Sort During Lazy Load** — User clicks a sort column header while a lazy load fetch is in progress | Cancel the pending fetch request via `AbortController`. Reset accumulated data and cursor state. Reload from the first batch using the new sort parameters. Display loading indicator during the fresh fetch. | User Interaction / Race Condition |
| 5 | **Concurrent Data Changes** — New confirmations are created or modified in another project space while user is viewing the report | Newly loaded batches via infinite scroll reflect the current database state. Previously loaded records maintain their position and content (eventual consistency model). No automatic refresh of already-rendered rows. Users can manually refresh by navigating away and back. | Data Consistency |

---

## Dependencies

### Cross-Epic Dependencies

| Dependency | Epic | Type | Description |
|-----------|------|------|-------------|
| **BSABANKSTA-1305** | Create/Modify Project Space (Batch 1) | **Data Dependency (Critical)** | All confirmation records originate from project spaces created and managed by F-001. The MongoDB aggregation pipeline queries the Project Spaces collection established by this epic. Without F-001 data, the report has no data source. |
| **BSABANKSTA-1531** | Application Frame and Global Navigation | **Navigation Routing** | Reporting views are accessed via the global navigation framework provided by the Application Frame. Route registration and navigation menu integration depend on F-005. |
| **BSABANKSTA-1536** | View All Confirmations (in BSABANKSTA-1531) | **Shared Surface (Critical)** | This story provides the data aggregation backend and API endpoints; BSABANKSTA-1536 provides the navigation container and page shell. The API contract (request parameters, response schema) must align between both stories. |
| **BSABANKSTA-1572** | My Projects Dashboard | **Related View** | Users may navigate between the personalized dashboard and the global report for different data perspectives (user-specific vs. cross-project). |
| **BSABANKSTA-131** | BSA Admin Persona | **Indirect** | Administrator role definitions determine RBAC rules governing whether a user sees all data (admin) or a filtered subset (analyst). |

### Cross-Cutting Dependencies

| Dependency | Reference | Description |
|-----------|-----------|-------------|
| Lazy Load / Infinite Scroll | CC-RQ-001 | Implements the shared infinite scroll pattern mandated across all list views. Uses cursor-based data loading with Intersection Observer API. |
| Shared Empty State Component | Shared UI Component | Reuses the application-wide Empty State display pattern. |
| Shared Error State / Retry Component | Shared UI Component | Reuses the application-wide Error State with retry button pattern. |
| Shared End-of-List Indicator | Shared UI Component | Reuses the application-wide End-of-List indicator pattern. |
| Auth0 Authentication | External Service | Requires authenticated user session for RBAC enforcement and project space access determination. |

### Bidirectional Dependency Links

- **This story → BSABANKSTA-1536**: This story's API endpoints power the data layer for the All Confirmations page shell
- **BSABANKSTA-1536 → This story**: The All Confirmations page shell depends on this story's data aggregation backend
- **This story → BSABANKSTA-1305**: Data dependency on project space entities and confirmation records
- **BSABANKSTA-1305 → This story**: Batch 1 epic updated to reference this epic's reporting views in its workflow diagram

---

## Story Estimation Guidance

| Attribute | Value |
|-----------|-------|
| **Story Points** | **8** (Fibonacci) |
| **Complexity** | High |
| **Uncertainty** | Moderate |
| **Effort** | Significant |

### Rationale

- **High complexity**: Cross-project data aggregation via MongoDB pipeline with `$lookup`, `$match`, and `$facet` stages; cursor-based infinite scroll implementation with Intersection Observer API; multi-column sortable table with `aria-sort` attributes and three distinct UI states (empty, error, end-of-list) in addition to the primary data table; and RBAC-filtered data retrieval.
- **Moderate uncertainty**: This is a shared surface between two epics (F-005 provides page shell via BSABANKSTA-1536; F-004 provides data aggregation). The API contract between frontend and backend requires coordination. The exact data model for cross-project confirmation aggregation must align with F-001 project space entity structures from Batch 1.
- **Significant effort**: Full table component with sortable headers, infinite scroll trigger with Intersection Observer, cursor management, error handling with retry, empty state, end-of-list indicator, loading skeletons, and RBAC enforcement. Backend requires MongoDB aggregation pipeline with cursor-based data loading and cross-project data merging.
- **8 points** reflects the combination of complex UI (infinite scroll table with sort), backend aggregation pipeline, cross-epic coordination across three epics, and comprehensive state management (loading, error, empty, end-of-list).

---

## Refinement Notes

### Global Rule #2 — Proactive Story Decomposition
- **Status:** No decomposition required.
- **Rationale:** This story contains 9 acceptance criteria (AC1–AC9) with a single user workflow (viewing cross-project confirmation data in a table with infinite scroll and sorting). The AC count is below the >10 threshold. The story focuses on a single cohesive page interaction pattern (navigate → load → scroll → sort) without distinct multi-workflow branches.

### Global Rule #3 — Verb-Noun Title Standardization
- **Original Title:** "Global Views — All Confirmations Report"
- **Standardized Title:** "View Global Confirmations Report"
- **Rationale:** Applied Verb-Noun format. "Global Views —" is organizational metadata removed from the title. "View" is the verb describing the user action; "Global Confirmations Report" is the noun phrase describing the target.

### Global Rule #4 — Pagination to Lazy Load Override
- **Override Status:** ✅ **MANDATORY OVERRIDE APPLIED**
- **Details:** All pagination patterns have been replaced with lazy load / infinite scroll using cursor-based data loading. The implementation specifies cursor-based API data loading (cursor token + batch size), Intersection Observer API for scroll detection, automatic next-batch fetching, and end-of-list indicator when `next_cursor` is null.
- **Reference:** CC-RQ-001 — Global pagination-to-lazy-load override
- **Impact:** All acceptance criteria reference infinite scroll behavior. No "page number", "next page", "previous page", or "results per page" controls exist.

### Global Rule #5 — Jira Source of Truth
- **Applied:** Jira/PDF requirement text was used as the authoritative source of truth. F-004 has NO dedicated Figma frames, so no direct Jira-Figma comparison points exist. The closest visual reference is the All Confirmations Page from BSABANKSTA-1536 (frame `8241-118805`), which was used as a design pattern reference (Priority 1: Reuse Similar Existing Component).

### Global Rule #6 — NFR Elevation
- **Applied:** Performance targets extracted into the dedicated Non-Functional Requirements section with specific measurable thresholds: < 3s initial page load, < 1s infinite scroll batch, < 1s sort operation, < 2s API response, < 3s MongoDB aggregation, WCAG 2.1 AA accessibility.

### Global Rule #8 — Generated UI Specifications
- **Status:** ✅ Applied — ALL UI specifications are generated since F-004 has no Figma frames.
- **Assessment:** Every UI element in this story requires a Generated UI Specification. The three-tier priority hierarchy was applied: data table and shared components reuse patterns from BSABANKSTA-1536 (Priority 1); page layout and form controls use semantic TailwindCSS tokens (Priority 2).

---

## Discrepancy Review

F-004 (Global Views and Reporting) has no dedicated Figma wireframes. All UI specifications are requirements-driven and generated using the Generated UI Specifications SOP (Global Rule #8). No direct Figma-Jira discrepancy comparison is applicable for this epic.

The closest visual reference — the All Confirmations Page (BSABANKSTA-1536, frame `8241-118805`) in BSABANKSTA-1531 — was used as a design pattern reference for the data table layout. Any traditional pagination controls that may be depicted in that Figma frame are overridden per Global Rule #4 in favor of lazy load / infinite scroll.

---

## Generated UI Specifications

> **DESIGN REVIEW REQUIRED:** The following UI specifications were automatically generated based on the existing design system tokens, as no explicit mockup was provided for these elements. Please review for accuracy and design intent before development.

### Report Page Layout

| UI Element | Design Token / Attribute | Value | Rationale |
|-----------|------------------------|-------|-----------|
| Page container | `max-width`, `margin`, `padding` | `max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8` | Standard page container consistent with application-wide layout patterns (Priority 2: Semantic Token) |
| Page heading | `font-size`, `font-weight`, `color`, `margin-bottom` | `text-2xl font-semibold text-gray-900 mb-2` | Primary page title styling (Priority 2: Semantic Token) |
| Page description | `font-size`, `color`, `margin-bottom` | `text-base text-gray-600 mb-8` | Contextual description below heading (Priority 2: Semantic Token) |

### Data Table

| UI Element | Design Token / Attribute | Value | Rationale |
|-----------|------------------------|-------|-----------|
| Table container | `overflow`, `border`, `border-radius` | `overflow-auto border border-gray-200 rounded-lg` | Scrollable container for horizontal overflow on smaller viewports — reuses All Confirmations table pattern from BSABANKSTA-1536 (Priority 1: Reuse Similar Existing Component) |
| Table element | `width`, `layout` | `table-auto w-full` | Full-width table with automatic column sizing (Priority 1: Reuse) |
| Table header row | `background`, `border` | `bg-gray-50 border-b border-gray-200` | Subtle background distinguishing header from body (Priority 1: Reuse) |
| Table header cell | `padding`, `font`, `color`, `text-transform` | `px-6 py-3 text-xs font-medium text-gray-500 uppercase tracking-wider text-left` | Compact, uppercase header text (Priority 1: Reuse) |
| Sortable header | `cursor`, `hover`, `aria` | `cursor-pointer hover:bg-gray-100` with sort chevron icon and `aria-sort` attribute | Interactive sort headers with accessibility support (Priority 2: Semantic Token) |
| Table body row | `border`, `hover` | `border-b border-gray-200 hover:bg-gray-50` | Subtle row separation with hover highlight (Priority 1: Reuse) |
| Table body cell | `padding`, `font`, `color` | `px-6 py-4 text-sm text-gray-900` | Standard cell content styling (Priority 1: Reuse) |
| Project space name cell | `font`, `color` | `text-sm text-gray-500` | Subdued text for contextual project space column (Priority 2: Semantic Token) |

### Empty State Display

| UI Element | Design Token / Attribute | Value | Rationale |
|-----------|------------------------|-------|-----------|
| Container layout | `display`, `flex-direction`, `align-items`, `justify-content` | `flex flex-col items-center justify-center` | Centered column layout — reuses shared Empty State pattern (Priority 1: Reuse) |
| Container padding | `padding-y` | `py-16` (64px vertical) | Generous whitespace distinguishing empty state from loading state |
| Text color | `color` | `text-gray-500` (#6B7280) | Subdued, non-error messaging color |
| Icon | Component | Placeholder document/search outline icon | Visual context for empty state |
| Heading | `font-size`, `font-weight` | `text-lg font-semibold` | Clear heading: "No confirmations found" |
| Subtitle | `font-size`, `color` | `text-sm text-gray-400` | Secondary: "There are no confirmation records in your accessible project spaces" |

### End-of-List Indicator

| UI Element | Design Token / Attribute | Value | Rationale |
|-----------|------------------------|-------|-----------|
| Container | `text-align`, `padding-y` | `text-center py-4` | Centered below table — reuses shared pattern (Priority 1: Reuse) |
| Divider | `border-top` | `border-t border-gray-200` | Subtle horizontal separator |
| Text | `font-size`, `color` | `text-sm text-gray-400` | Muted: "All confirmations loaded" |

### Error State with Retry

| UI Element | Design Token / Attribute | Value | Rationale |
|-----------|------------------------|-------|-----------|
| Container background | `background-color` | `bg-red-50` (#FEF2F2) | Light red background — reuses shared Error State (Priority 1: Reuse) |
| Container border | `border`, `border-color`, `border-radius` | `border border-red-200 rounded-md` | Subtle red border reinforcing error |
| Container padding | `padding` | `p-4` | Standard container padding |
| Error text | `color`, `font-size` | `text-red-700 text-sm` | Clear error message text |
| Retry button | `background`, `color`, `padding`, `border-radius` | `bg-blue-600 text-white px-4 py-2 rounded-md hover:bg-blue-700 mt-3` | Primary action button for retry |

### Loading Indicator

| UI Element | Design Token / Attribute | Value | Rationale |
|-----------|------------------------|-------|-----------|
| Initial load — skeleton rows | `animation`, `background`, `height`, `border-radius` | `animate-pulse bg-gray-200 h-12 rounded` (repeated for batch size) | Skeleton placeholder matching table row height (Priority 2: Semantic Token) |
| Scroll load — spinner | `display`, `padding`, `animation`, `size`, `color` | `flex justify-center py-4` with `animate-spin h-6 w-6 text-blue-600` | Centered spinner below loaded rows during fetch (Priority 2: Semantic Token) |

---

## Figma Mockup Link

No dedicated Figma wireframes exist for F-004 (Global Views and Reporting). All UI specifications are generated using the design token manifest from the BSA Wireframes Figma file. For reference, the closest visual analog is the All Confirmations Page (shared surface with F-005):

- **All Confirmations Page (Closest Visual Reference):** [https://www.figma.com/design/6fQyfvBUqImyavY8Fw47FV/BSA-Wireframes?node-id=8241-118805](https://www.figma.com/design/6fQyfvBUqImyavY8Fw47FV/BSA-Wireframes?node-id=8241-118805) — BSABANKSTA-1536 in BSABANKSTA-1531 (Application Frame)

---

## Definition of Done (Story-Level)

- [ ] All acceptance criteria (AC1–AC9) pass BDD validation
- [ ] Unit tests written and passing (pytest ≥9.0.2 for API, Vitest ≥4.0.18 for components)
- [ ] Component tests written and passing (@testing-library/react 16.x)
- [ ] E2E tests written and passing (Playwright ≥1.55.1)
- [ ] BDD tests written and passing (behave 1.x)
- [ ] Code reviewed and approved
- [ ] Cross-project data aggregation verified with multiple project spaces
- [ ] Infinite scroll implementation verified (no pagination controls rendered)
- [ ] Column sorting functional (ascending/descending with `aria-sort`)
- [ ] Empty state, error state with retry, and end-of-list indicator implemented
- [ ] WCAG 2.1 AA accessibility compliance verified (keyboard navigation, screen reader, color contrast)
- [ ] NFRs validated (< 3s initial load, < 1s scroll batch, < 2s API response, < 3s aggregation)
- [ ] Generated UI Specifications reviewed by design team (DESIGN REVIEW REQUIRED)
- [ ] Cross-epic dependency links verified bidirectionally (BSABANKSTA-1305, BSABANKSTA-1531, BSABANKSTA-1536)
- [ ] RBAC enforcement verified (admin sees all; analyst sees assigned only)
- [ ] Documentation updated


---
---

# Filter Cross-Project Reporting Data

**Story ID:** BSABANKSTA-1542
**Epic:** BSABANKSTA-1540 — Global Views and Reporting
**Batch:** 2

---

## User Story

**As a** BSA Analyst or BSA Administrator,
**I want** to apply multi-dimensional filters to the global confirmations report — including date ranges, project spaces, confirmation statuses, and confirmation types,
**So that** I can isolate specific data subsets across all project spaces, enabling precise compliance analysis, targeted regulatory reporting preparation, and efficient investigation of specific confirmation patterns.

---

## INVEST Validation

| Principle | Validation | Notes |
|-----------|------------|-------|
| **Independent** | ✅ Pass | The filtering capability can be developed independently from the base report view (BSABANKSTA-1541) and summary statistics (BSABANKSTA-1543). While the filter UI integrates into the report page, the filter state management, filter API parameters, and filter panel component are self-contained. |
| **Negotiable** | ✅ Pass | Specific filter dimensions (which columns are filterable), filter control types (dropdown vs. text input vs. date picker), and filter panel layout are negotiable implementation details. The core requirement is multi-dimensional filtering across the global report. |
| **Valuable** | ✅ Pass | Multi-dimensional filtering is essential for compliance analysis. Compliance officers must isolate specific date ranges for regulatory reporting periods, specific project spaces for focused audits, and specific confirmation statuses for workflow monitoring. Without filtering, the global report is a flat data dump with limited analytical value. |
| **Estimable** | ✅ Pass | Filter patterns (dropdown selectors, date range pickers, clear/reset controls) are well-established UI patterns. The backend implementation extends the existing reporting API with additional query parameters. Estimation is feasible based on the number of filter dimensions. |
| **Small** | ✅ Pass | Focused on a single capability — applying, composing, and clearing filters on the global report. Does not include data loading (BSABANKSTA-1541) or summary statistics (BSABANKSTA-1543). |
| **Testable** | ✅ Pass | Can verify: each filter dimension applies correctly, multiple filters compose, filters reset, data re-loads with filter parameters, empty state when no matches, filter state persists during infinite scroll, and active filter indicators display. |

---

## Non-Functional Requirements

| NFR ID | Category | Requirement | Target |
|--------|----------|-------------|--------|
| NFR-1542-01 | Performance | Filter application triggers data re-fetch and render | < 1 second |
| NFR-1542-02 | Performance | API response time with filter parameters applied | < 2 seconds |
| NFR-1542-03 | Performance | MongoDB aggregation with filter `$match` stages | < 3 seconds |
| NFR-1542-04 | Performance | Filter control interaction (open dropdown, select value) | < 200 milliseconds |
| NFR-1542-05 | Accessibility | WCAG 2.1 AA for filter controls | All filter inputs have associated labels, keyboard operable, screen reader announces filter changes and result count updates |
| NFR-1542-06 | Responsiveness | Desktop-first responsive filter panel | Filter panel wraps gracefully on smaller viewports per CC-RQ-003 |
| NFR-1542-07 | Data Accuracy | Filtered results exactly match applied criteria | No records outside filter parameters appear in results |

---

## Acceptance Criteria

### AC1: Filter Panel Display

```gherkin
Scenario: Filter panel is displayed above the report data table
  Given the user is on the Global Confirmations Report page
  When the page loads
  Then a filter panel is displayed above the data table
  And the filter panel contains controls for: Date Range, Project Space, Confirmation Status, and Confirmation Type
  And all filter controls are in their default (unfiltered) state
  And the filter panel is visually distinct from the data table area
```

### AC2: Date Range Filter

```gherkin
Scenario: User filters confirmations by date range
  Given the user is on the Global Confirmations Report page
  And the filter panel is visible
  When the user selects a start date and end date in the Date Range filter
  Then the report data is re-loaded with only confirmation records within the selected date range
  And the data table displays only records matching the date range
  And the infinite scroll resets to load from the first batch with the new filter
  And the active date range filter is indicated visually (e.g., highlighted input, filter chip)

Scenario: User clears the date range filter
  Given a date range filter is currently applied
  When the user clears the date range filter (e.g., clicks clear/X on the date input)
  Then the date range filter is removed
  And the report data is re-loaded without the date range constraint
```

### AC3: Project Space Filter

```gherkin
Scenario: User filters confirmations by specific project space(s)
  Given the user is on the Global Confirmations Report page
  And the Project Space filter dropdown lists all project spaces accessible to the user
  When the user selects one or more project spaces from the dropdown
  Then the report data is re-loaded with only confirmation records from the selected project space(s)
  And the selected project space(s) are indicated as active filters

Scenario: BSA Administrator sees all project spaces in filter dropdown
  Given a BSA Administrator is on the Global Confirmations Report page
  When the administrator opens the Project Space filter dropdown
  Then ALL project spaces in the system are listed as filter options

Scenario: BSA Analyst sees only assigned project spaces in filter dropdown
  Given a BSA Analyst is on the Global Confirmations Report page
  When the analyst opens the Project Space filter dropdown
  Then only project spaces the analyst is assigned to are listed as filter options
```

### AC4: Confirmation Status Filter

```gherkin
Scenario: User filters confirmations by status
  Given the user is on the Global Confirmations Report page
  And the Confirmation Status filter lists available statuses
  When the user selects one or more statuses (e.g., "Pending", "Completed", "In Review")
  Then the report data is re-loaded with only records matching the selected status(es)
  And the active status filter(s) are visually indicated
```

### AC5: Confirmation Type Filter

```gherkin
Scenario: User filters confirmations by type
  Given the user is on the Global Confirmations Report page
  And the Confirmation Type filter lists available types
  When the user selects one or more confirmation types
  Then the report data is re-loaded with only records matching the selected type(s)
  And the active type filter(s) are visually indicated
```

### AC6: Composable Filters

```gherkin
Scenario: User applies multiple filters simultaneously
  Given the user is on the Global Confirmations Report page
  When the user selects a date range AND a project space AND a confirmation status
  Then all three filters are applied simultaneously
  And the report data displays only records matching ALL applied filters (AND logic)
  And all active filters are visually indicated
  And the total record count reflects the filtered result set

Scenario: Active filter indicators display applied filters
  Given multiple filters are applied to the report
  Then each active filter is displayed as a filter chip/tag showing the filter name and value
  And each filter chip has a remove (X) button to clear that individual filter
```

### AC7: Clear All Filters

```gherkin
Scenario: User clears all applied filters at once
  Given one or more filters are currently applied to the report
  And a "Clear All Filters" action is visible
  When the user clicks "Clear All Filters"
  Then all filters are removed simultaneously
  And all filter controls reset to their default (unfiltered) state
  And the report data is re-loaded without any filter constraints
  And the infinite scroll resets to the first batch
```

### AC8: Filter Persistence During Infinite Scroll

```gherkin
Scenario: Filters persist when user scrolls to load more data
  Given one or more filters are applied to the report
  And the user scrolls to trigger infinite scroll for the next batch
  When the next batch of data is fetched
  Then the filter parameters are included in the API request for the next batch
  And the newly loaded records also match the applied filter criteria
  And filter controls remain in their selected state
```

### AC9: Empty State After Filtering

```gherkin
Scenario: Empty state displays when no records match applied filters
  Given the user has applied filters to the report
  And no confirmation records match the combination of applied filters
  When the filter query completes
  Then a filtered empty state message is displayed (e.g., "No confirmations match the selected filters")
  And the empty state includes a suggestion to adjust or clear filters
  And the "Clear All Filters" action remains accessible
```

---

## Sub-Tasks

### Model

| Sub-Task ID | Description | Technical Details |
|-------------|-------------|-------------------|
| ST-1542-M1 | Filter parameter Marshmallow schema | Define `ReportFilterSchema` (Marshmallow ≥3.26.2) with fields: `date_range_start` (optional datetime), `date_range_end` (optional datetime), `project_space_ids` (optional list of strings), `confirmation_statuses` (optional list of enum values), `confirmation_types` (optional list of enum values). Include validation for date range logic (start <= end). |
| ST-1542-M2 | Extended MongoDB aggregation pipeline with filter stages | Extend the aggregation pipeline from BSABANKSTA-1541 to inject `$match` stages for each active filter dimension. Implement dynamic pipeline construction — only include `$match` stages for filters that are actively applied. Maintain cursor-based infinite scroll compatibility. |
| ST-1542-M3 | Filter options data model | Define models for retrieving available filter values: list of accessible project spaces (filtered by RBAC), list of confirmation statuses, list of confirmation types. These populate the filter control dropdowns. |

### API

| Sub-Task ID | Description | Technical Details |
|-------------|-------------|-------------------|
| ST-1542-A1 | Extended `GET /api/reports/confirmations` with filter parameters | Extend the reporting endpoint from BSABANKSTA-1541 to accept additional query parameters: `date_start` (ISO datetime), `date_end` (ISO datetime), `project_space_ids` (comma-separated), `statuses` (comma-separated), `types` (comma-separated). All filter parameters are optional. |
| ST-1542-A2 | `GET /api/reports/filter-options` endpoint | New Flask 3.1.3 blueprint endpoint returning available filter values for the authenticated user: `{ "project_spaces": [...], "statuses": [...], "types": [...] }`. Project spaces filtered by RBAC. Require JWT authentication. |
| ST-1542-A3 | Filter parameter validation middleware | Validate filter parameters using `ReportFilterSchema`. Return 400 Bad Request with descriptive error for invalid parameters (e.g., invalid date format, unknown status value, end date before start date). |
| ST-1542-A4 | Response contract with filter metadata | Extend response JSON to include `filters_applied`: `{ "date_range": {...}, "project_space_ids": [...], "statuses": [...], "types": [...] }` reflecting the server-side interpretation of applied filters. |

### Component

| Sub-Task ID | Description | Technical Details |
|-------------|-------------|-------------------|
| ST-1542-C1 | `FilterPanel` component | Container component with TailwindCSS: `bg-white rounded-lg shadow-sm border border-gray-200 p-4 mb-6`. Wraps individual filter controls in a responsive flex/grid layout. |
| ST-1542-C2 | `DateRangeFilter` component | Two date input controls (start/end) with TailwindCSS: `border border-gray-300 rounded-md px-3 py-2 text-sm focus:ring-2 focus:ring-blue-500`. Calendar icon, clear button per input. Validation: end >= start. |
| ST-1542-C3 | `ProjectSpaceFilter` component | Multi-select dropdown populated via `GET /api/reports/filter-options`. TailwindCSS: `border border-gray-300 rounded-md px-3 py-2 text-sm bg-white`. Shows selected count when collapsed. RBAC-filtered options. |
| ST-1542-C4 | `StatusFilter` component | Multi-select dropdown for confirmation statuses with semantic color indicators matching status badges. |
| ST-1542-C5 | `TypeFilter` component | Multi-select dropdown for confirmation types. |
| ST-1542-C6 | `ActiveFilterChips` component | Displays active filters as removable chips/tags: `inline-flex items-center px-3 py-1 rounded-full text-sm bg-blue-100 text-blue-800` with remove (X) icon. |
| ST-1542-C7 | `ClearAllFilters` button | Text button: `text-sm text-blue-600 hover:text-blue-800 font-medium`. Visible when at least one filter is active. |

### Logic

| Sub-Task ID | Description | Technical Details |
|-------------|-------------|-------------------|
| ST-1542-L1 | Filter state management | Custom React hook (`useReportFilters`) managing filter state as a composable object. Each filter dimension is independently settable and clearable. State changes trigger data re-fetch via the `useInfiniteReportData` hook from BSABANKSTA-1541, resetting cursor and accumulated data. |
| ST-1542-L2 | Filter-to-query parameter serialization | Serialize filter state object to URL query parameters for API calls and browser URL (for shareable/bookmarkable filtered views via `useSearchParams`). Deserialize on page load to restore filter state from URL. |
| ST-1542-L3 | Debounced filter application | Debounce filter changes (e.g., 300ms delay after last interaction) to avoid excessive API calls during rapid filter adjustments. Cancel in-flight requests via `AbortController` when new filter combination is applied. |
| ST-1542-L4 | Filter options loading | Fetch available filter options on page mount via `GET /api/reports/filter-options`. Cache options during the session. Update project space list if user navigates away and back. |
| ST-1542-L5 | RBAC-filtered filter options | Project Space filter dropdown only shows project spaces accessible to the authenticated user. BSA Administrator sees all; BSA Analyst sees only assigned project spaces. Filter options endpoint enforces this server-side. |

### Testing

| Sub-Task ID | Description | Technical Details |
|-------------|-------------|-------------------|
| ST-1542-T1 | Unit tests (pytest ≥9.0.2) for filter parameter validation | Test each filter dimension validation, combined filter validation, invalid inputs (bad dates, unknown statuses), and empty filters. |
| ST-1542-T2 | Unit tests (pytest ≥9.0.2) for filtered aggregation pipeline | Test MongoDB aggregation with each filter type applied individually and in combination. Verify correct `$match` stage injection. Test edge cases: date range boundaries, empty filter values. |
| ST-1542-T3 | Unit tests (pytest ≥9.0.2) for filter-options endpoint | Test RBAC filtering of project space list (admin vs. analyst), available status/type enumeration, and authentication requirement. |
| ST-1542-T4 | Component tests (@testing-library/react 16.x) for `FilterPanel` | Test filter control rendering, interaction (select/deselect values), filter chip display, clear all action, and responsive layout. |
| ST-1542-T5 | Component tests for individual filter controls | Test each filter component: `DateRangeFilter` (date selection, validation, clear), `ProjectSpaceFilter` (multi-select, RBAC-filtered options), `StatusFilter`, `TypeFilter`. |
| ST-1542-T6 | BDD tests (behave 1.x) for acceptance criteria | Implement feature files mapping to all 9 AC scenarios (AC1–AC9). |
| ST-1542-T7 | E2E tests (Playwright ≥1.55.1) for filter workflow | Test complete filter flow: apply date range → add project space filter → verify composable filtering → clear individual filter → clear all → verify empty state after filtering. |

---

## Edge Cases

| # | Edge Case | Expected Behavior | Category |
|---|-----------|-------------------|----------|
| 1 | **Rapid Filter Changes** — User quickly changes multiple filter values in succession | Debounce filter application (300ms). Cancel in-flight API requests via `AbortController` when a new filter combination is submitted. Only the most recent filter state is fetched. No stale data appears from cancelled requests. | User Interaction / Race Condition |
| 2 | **No Matching Records** — Applied filter combination results in zero matches | Display filtered empty state with message: "No confirmations match the selected filters." Include suggestion to adjust or clear filters. "Clear All Filters" remains accessible. | Empty State / Boundary |
| 3 | **Invalid Date Range** — User selects end date before start date | Client-side validation prevents submission. Display inline validation error: "End date must be after start date." If bypassed, server returns 400 Bad Request with descriptive error. | Input Validation |
| 4 | **Filter with Infinite Scroll** — User applies filters after scrolling deep into data | Filter application resets the cursor to the beginning. All previously accumulated data is cleared. New data loads from the first batch matching the filter criteria. Scroll position resets to top. | State Management |
| 5 | **Stale Filter Options** — A project space is created or removed while user is on the page | Filter options are loaded once on page mount and cached. Stale options are acceptable during the session. If a user selects a removed project space, the API returns zero results for that space — not an error. New project spaces appear on next page load. | Data Consistency |

---

## Dependencies

### Cross-Epic Dependencies

| Dependency | Epic | Type | Description |
|-----------|------|------|-------------|
| **BSABANKSTA-1541** | View Global Confirmations Report (this epic) | **Internal Dependency** | Filter controls integrate into the report page created by BSABANKSTA-1541. Filter state changes trigger data re-fetch via the infinite scroll data hook. |
| **BSABANKSTA-1305** | Create/Modify Project Space (Batch 1) | **Data Dependency** | Filter dimensions (project spaces, confirmation statuses, types) are derived from the F-001 data model. Available project spaces in the filter dropdown are populated from the project space entities. |
| **BSABANKSTA-1536** | View All Confirmations (BSABANKSTA-1531) | **Shared Surface** | Filter controls may be rendered on the shared All Confirmations page shell. API filter parameters extend the shared reporting endpoint contract. |
| **BSABANKSTA-1531** | Application Frame and Global Navigation | **Navigation Routing** | Reporting views with filters are accessed via the Application Frame's navigation. |
| **Auth0** | External Service | **RBAC** | Project Space filter options are RBAC-filtered per user role. |

### Cross-Cutting Dependencies

| Dependency | Reference | Description |
|-----------|-----------|-------------|
| Lazy Load / Infinite Scroll | CC-RQ-001 | Filter changes reset infinite scroll cursor and reload from first batch. |
| Auth0 Authentication | External Service | RBAC enforcement for filter options (project space visibility). |

### Bidirectional Dependency Links

- **This story → BSABANKSTA-1541**: Extends the reporting API and page with filter capabilities
- **BSABANKSTA-1541 → This story**: Base report view provides the page and data hook that filters modify
- **This story → BSABANKSTA-1305**: Filter values derived from F-001 data model
- **This story → BSABANKSTA-1536**: Filter may be rendered on shared All Confirmations surface

---

## Story Estimation Guidance

| Attribute | Value |
|-----------|-------|
| **Story Points** | **8** (Fibonacci) |
| **Complexity** | High |
| **Uncertainty** | Moderate |
| **Effort** | Significant |

### Rationale

- **High complexity**: Four distinct filter dimensions (date range, project space, status, type) each requiring their own UI control, validation logic, and backend query parameter handling. Composable filter logic (AND combination) with debouncing and AbortController for request management. Filter state serialization to URL for shareable views. Dynamic MongoDB pipeline construction with multiple optional `$match` stages.
- **Moderate uncertainty**: The exact filter dimensions and available values depend on the F-001 data model (BSABANKSTA-1305). The interaction between filter state, infinite scroll cursor management, and sort state requires careful coordination. The filter options endpoint requires RBAC awareness.
- **Significant effort**: Four filter control components, a filter panel container, active filter chip display, clear all functionality, filter state management hook, URL serialization, debouncing, server-side filter validation, dynamic pipeline construction, and comprehensive testing across all filter combinations.
- **8 points** reflects the multi-component UI (four filter controls + chips + clear all), complex state management (composable filters with debounce), backend pipeline extension, and RBAC-aware filter options.

---

## Refinement Notes

### Global Rule #2 — Proactive Story Decomposition
- **Status:** No decomposition required.
- **Rationale:** This story contains 9 acceptance criteria (AC1–AC9) focused on a single capability (multi-dimensional filtering). The AC count is below the >10 threshold. All ACs relate to the same workflow pattern (apply filter → data reloads → visual indication) without distinct multi-workflow branches.

### Global Rule #3 — Verb-Noun Title Standardization
- **Original Title:** "Global Views — Cross-Project Filtering and Search"
- **Standardized Title:** "Filter Cross-Project Reporting Data"
- **Rationale:** Applied Verb-Noun format. "Filter" is the verb; "Cross-Project Reporting Data" is the noun phrase. Removed "Global Views —" organizational prefix and simplified "Filtering and Search" to "Filter" since this story focuses on structured filtering rather than free-text search.

### Global Rule #4 — Pagination to Lazy Load Override
- **Override Status:** ✅ **MANDATORY OVERRIDE APPLIED**
- **Details:** Filter changes reset the infinite scroll cursor and reload from the first batch. No traditional pagination controls are rendered or referenced. Filter state is compatible with cursor-based data loading.
- **Reference:** CC-RQ-001

### Global Rule #5 — Jira Source of Truth
- **Applied:** Requirements text is the authoritative source. F-004 has no Figma frames for direct comparison. Filter panel design is generated using the design token manifest.

### Global Rule #6 — NFR Elevation
- **Applied:** Performance targets extracted into NFR section: < 1s filter application, < 2s API with filters, < 3s MongoDB with filters, < 200ms filter control interaction, WCAG 2.1 AA for filter controls.

### Global Rule #8 — Generated UI Specifications
- **Status:** ✅ Applied — All filter UI elements require Generated UI Specifications since F-004 has no Figma frames. Filter panel design uses semantic TailwindCSS tokens (Priority 2).

---

## Discrepancy Review

F-004 (Global Views and Reporting) has no dedicated Figma wireframes. All UI specifications are requirements-driven and generated using the Generated UI Specifications SOP (Global Rule #8). No direct Figma-Jira discrepancy comparison is applicable for this epic.

---

## Generated UI Specifications

> **DESIGN REVIEW REQUIRED:** The following UI specifications were automatically generated based on the existing design system tokens, as no explicit mockup was provided for these elements. Please review for accuracy and design intent before development.

### Filter Panel

| UI Element | Design Token / Attribute | Value | Rationale |
|-----------|------------------------|-------|-----------|
| Panel container | `background`, `border-radius`, `shadow`, `border`, `padding`, `margin` | `bg-white rounded-lg shadow-sm border border-gray-200 p-4 mb-6` | Visually distinct filter area above data table (Priority 2: Semantic Token) |
| Panel layout | `display`, `flex-wrap`, `gap` | `flex flex-wrap items-end gap-4` | Responsive wrapping layout for filter controls |
| Filter label | `font-size`, `font-weight`, `color`, `margin` | `text-sm font-medium text-gray-700 mb-1` | Standard form label styling |

### Date Range Filter

| UI Element | Design Token / Attribute | Value | Rationale |
|-----------|------------------------|-------|-----------|
| Date input | `border`, `border-radius`, `padding`, `font-size`, `focus` | `border border-gray-300 rounded-md px-3 py-2 text-sm focus:ring-2 focus:ring-blue-500 focus:border-blue-500` | Standard form control styling (Priority 2: Semantic Token) |
| Date separator | `color`, `padding` | `text-gray-400 px-2` text "to" | Connecting text between start and end date |
| Calendar icon | `size`, `color`, `position` | `h-4 w-4 text-gray-400` positioned inside input | Visual indicator for date input type |
| Clear button | `size`, `color`, `cursor` | `h-4 w-4 text-gray-400 hover:text-gray-600 cursor-pointer` (X icon) | Clear individual date input |

### Project Space / Status / Type Filter Dropdowns

| UI Element | Design Token / Attribute | Value | Rationale |
|-----------|------------------------|-------|-----------|
| Dropdown trigger | `border`, `border-radius`, `padding`, `font-size`, `background` | `border border-gray-300 rounded-md px-3 py-2 text-sm bg-white` | Standard select/dropdown styling (Priority 2: Semantic Token) |
| Dropdown menu | `background`, `border`, `shadow`, `border-radius`, `max-height` | `bg-white border border-gray-200 rounded-md shadow-lg max-h-60 overflow-auto` | Dropdown options container |
| Option item | `padding`, `hover`, `font-size` | `px-3 py-2 text-sm hover:bg-gray-100 cursor-pointer` | Individual selectable option |
| Selected indicator | `color` | `text-blue-600` checkmark icon | Indicates selected option |
| Selected count badge | `font-size`, `background`, `color`, `border-radius`, `padding` | `text-xs bg-blue-100 text-blue-800 rounded-full px-2 py-0.5` | Shows count of selected items when collapsed |

### Active Filter Chips

| UI Element | Design Token / Attribute | Value | Rationale |
|-----------|------------------------|-------|-----------|
| Chip container area | `display`, `flex-wrap`, `gap`, `margin` | `flex flex-wrap gap-2 mt-3` | Wrapping area below filter controls |
| Filter chip | `display`, `padding`, `border-radius`, `font-size`, `background`, `color` | `inline-flex items-center px-3 py-1 rounded-full text-sm bg-blue-100 text-blue-800` | Active filter indicator (Priority 2: Semantic Token) |
| Remove icon | `size`, `color`, `margin`, `cursor` | `h-3.5 w-3.5 ml-1.5 text-blue-600 hover:text-blue-800 cursor-pointer` | Remove individual filter |

### Clear All Filters

| UI Element | Design Token / Attribute | Value | Rationale |
|-----------|------------------------|-------|-----------|
| Button | `font-size`, `color`, `font-weight`, `cursor` | `text-sm text-blue-600 hover:text-blue-800 font-medium cursor-pointer` | Text-button to reset all filters (Priority 2: Semantic Token) |

---

## Figma Mockup Link

No dedicated Figma wireframes exist for F-004 (Global Views and Reporting). All UI specifications are generated using the design token manifest from the BSA Wireframes Figma file. For reference, the closest visual analog is the All Confirmations Page (shared surface with F-005):

- **All Confirmations Page (Closest Visual Reference):** [https://www.figma.com/design/6fQyfvBUqImyavY8Fw47FV/BSA-Wireframes?node-id=8241-118805](https://www.figma.com/design/6fQyfvBUqImyavY8Fw47FV/BSA-Wireframes?node-id=8241-118805) — BSABANKSTA-1536 in BSABANKSTA-1531 (Application Frame)

---

## Definition of Done (Story-Level)

- [ ] All acceptance criteria (AC1–AC9) pass BDD validation
- [ ] Unit tests written and passing (pytest ≥9.0.2 for API filter validation and pipeline, Vitest ≥4.0.18 for filter components)
- [ ] Component tests written and passing (@testing-library/react 16.x for FilterPanel, DateRangeFilter, all dropdowns)
- [ ] E2E tests written and passing (Playwright ≥1.55.1 for complete filter workflow)
- [ ] BDD tests written and passing (behave 1.x)
- [ ] Code reviewed and approved
- [ ] All four filter dimensions functional (date range, project space, status, type)
- [ ] Composable filters work correctly (AND logic across dimensions)
- [ ] Active filter chips display and are individually removable
- [ ] Clear All Filters resets all controls and reloads data
- [ ] Filter state persists during infinite scroll (subsequent batches include filter params)
- [ ] Filtered empty state displays when no records match
- [ ] Debounced filter application prevents excessive API calls
- [ ] Filter state serialized to URL for shareable/bookmarkable views
- [ ] RBAC-filtered project space options verified (admin sees all; analyst sees assigned)
- [ ] Infinite scroll resets correctly when filters change
- [ ] WCAG 2.1 AA accessibility verified for all filter controls
- [ ] NFRs validated (< 1s filter application, < 2s API with filters)
- [ ] Generated UI Specifications reviewed by design team (DESIGN REVIEW REQUIRED)
- [ ] Cross-epic dependency links verified bidirectionally
- [ ] Documentation updated

---
---

# Aggregate Confirmation Summary Statistics

**Story ID:** BSABANKSTA-1543
**Epic:** BSABANKSTA-1540 — Global Views and Reporting
**Batch:** 2

---

## User Story

**As a** BSA Analyst or BSA Administrator,
**I want** to view aggregated summary statistics for confirmations across all project spaces — including total counts, status breakdowns, and trend indicators,
**So that** I can quickly assess the overall state of the organization's BSA confirmation program, identify bottlenecks or anomalies at a glance, and support executive-level compliance reporting without manually compiling data from individual projects.

---

## INVEST Validation

| Principle | Validation | Notes |
|-----------|------------|-------|
| **Independent** | ✅ Pass | Summary statistics are a distinct display layer that aggregates data independently from the tabular report view (BSABANKSTA-1541) and filter controls (BSABANKSTA-1542). While the summary cards appear on the same page, the statistics computation, display components, and API endpoint are self-contained. |
| **Negotiable** | ✅ Pass | The specific metrics displayed (total count, status breakdown, project count, trend direction), the visual presentation (cards, charts, counters), and the exact aggregation logic are negotiable. The core requirement is a high-level summary view of cross-project confirmation data. |
| **Valuable** | ✅ Pass | Summary statistics provide immediate, at-a-glance insight into the organization's confirmation program health. Compliance officers and administrators can identify trends (increasing/decreasing volumes), spot status imbalances (e.g., disproportionate pending count), and prepare executive summaries without scrolling through detail records. |
| **Estimable** | ✅ Pass | Summary card components are standard UI patterns. The MongoDB aggregation pipeline for computing totals and breakdowns is well-defined. Estimation is based on the number of metric cards and the complexity of the aggregation queries. |
| **Small** | ✅ Pass | Focused on a single capability — computing and displaying aggregated summary metrics. Does not include detail-level data display (BSABANKSTA-1541) or interactive filtering (BSABANKSTA-1542). |
| **Testable** | ✅ Pass | Can verify: summary cards render with correct totals, status breakdown percentages match raw data, trend indicators reflect period-over-period changes, summary responds to applied filters, and RBAC enforcement limits aggregation scope. |

---

## Non-Functional Requirements

| NFR ID | Category | Requirement | Target |
|--------|----------|-------------|--------|
| NFR-1543-01 | Performance | Summary statistics API response time | < 2 seconds |
| NFR-1543-02 | Performance | MongoDB aggregation pipeline for summary metrics | < 3 seconds |
| NFR-1543-03 | Performance | Summary card rendering (client-side) | < 500 milliseconds |
| NFR-1543-04 | Performance | Initial page load including summary statistics | < 3 seconds |
| NFR-1543-05 | Accessibility | WCAG 2.1 AA for summary cards | Screen reader announces each metric value and label; sufficient color contrast for all text; trend indicators have text alternatives |
| NFR-1543-06 | Responsiveness | Desktop-first responsive summary card grid | Cards reflow from 4-column to 2-column to 1-column on smaller viewports per CC-RQ-003 |
| NFR-1543-07 | Data Accuracy | Summary statistics match the detail data | Totals in summary cards equal the count of records in the detail table under the same filter scope |

---

## Acceptance Criteria

### AC1: Summary Statistics Card Display

```gherkin
Scenario: Summary statistics cards display above the report data table
  Given the user is on the Global Confirmations Report page
  When the page loads
  Then a summary statistics section is displayed above the filter panel and data table
  And the summary section contains metric cards for:
    | Metric                 | Description                                |
    | Total Confirmations    | Count of all confirmations across projects  |
    | Confirmations by Status| Breakdown by status (Pending, In Review, Completed, etc.) |
    | Active Project Spaces  | Count of project spaces with confirmations  |
    | Period Trend           | Comparison to previous period (e.g., +12% from last month) |
```

### AC2: Total Confirmations Count

```gherkin
Scenario: Total confirmations count displays accurately
  Given the user is on the Global Confirmations Report page
  And confirmations exist across multiple project spaces
  When the summary statistics load
  Then the "Total Confirmations" card displays the total count of all confirmations accessible to the user
  And the count is formatted with thousands separators (e.g., "12,345")
  And the card includes a descriptive label "Total Confirmations"
```

### AC3: Status Breakdown

```gherkin
Scenario: Confirmation status breakdown displays correctly
  Given the user is on the Global Confirmations Report page
  And confirmations exist in various statuses
  When the summary statistics load
  Then the "Confirmations by Status" section displays a breakdown of counts per status
  And each status shows its count and percentage of total (e.g., "Pending: 234 (18%)")
  And each status uses a semantic color indicator matching the status badge colors
```

### AC4: Active Project Spaces Count

```gherkin
Scenario: Active project spaces count displays correctly
  Given the user is on the Global Confirmations Report page
  And confirmations span multiple project spaces
  When the summary statistics load
  Then the "Active Project Spaces" card displays the count of distinct project spaces that have at least one confirmation
  And the count reflects only project spaces accessible to the current user (RBAC enforcement)
```

### AC5: Period Trend Indicator

```gherkin
Scenario: Period trend indicator shows directional change
  Given the user is on the Global Confirmations Report page
  When the summary statistics load
  Then the "Period Trend" card displays a comparison metric (e.g., percentage change from the previous 30-day period)
  And an upward trend is indicated with a green arrow and positive percentage
  And a downward trend is indicated with a red arrow and negative percentage
  And a flat trend (< 1% change) is indicated with a neutral gray indicator
```

### AC6: Summary Statistics Respond to Filters

```gherkin
Scenario: Summary statistics update when filters are applied
  Given the user is on the Global Confirmations Report page
  And one or more filters are applied (via BSABANKSTA-1542)
  When the filter is applied and data reloads
  Then the summary statistics cards recalculate to reflect only the filtered data subset
  And the "Total Confirmations" count reflects the filtered total
  And the "Confirmations by Status" breakdown reflects filtered data
  And the "Active Project Spaces" count reflects only project spaces with filtered results
  And the "Period Trend" recalculates based on the filtered data scope
```

### AC7: RBAC-Scoped Aggregation

```gherkin
Scenario: BSA Administrator sees organization-wide summary statistics
  Given a BSA Administrator is on the Global Confirmations Report page
  When the summary statistics load
  Then all metrics aggregate data from ALL project spaces in the system

Scenario: BSA Analyst sees scoped summary statistics
  Given a BSA Analyst is on the Global Confirmations Report page
  When the summary statistics load
  Then all metrics aggregate data only from project spaces the analyst is assigned to
```

### AC8: Loading State for Summary Statistics

```gherkin
Scenario: Loading skeleton displays while summary statistics are being computed
  Given the user navigates to the Global Confirmations Report page
  When the summary statistics API request is in progress
  Then skeleton placeholder cards are displayed in the summary section
  And each skeleton card has the same dimensions as the final card
  And the skeleton cards use a subtle animation to indicate loading
```

### AC9: Error State for Summary Statistics

```gherkin
Scenario: Error state displays when summary statistics fail to load
  Given the user is on the Global Confirmations Report page
  When the summary statistics API request fails (network error, server error)
  Then an error message is displayed in the summary section: "Unable to load summary statistics"
  And a "Retry" button is available to re-attempt the API request
  And the data table below continues to function independently (summary failure does not block detail view)
```

---

## Sub-Tasks

### Model

| Sub-Task ID | Description | Technical Details |
|-------------|-------------|-------------------|
| ST-1543-M1 | Summary statistics MongoDB aggregation pipeline | Define a dedicated aggregation pipeline using PyMongo ≥4.7.0 that computes: `$group` by status for status breakdown, `$count` for total confirmations, `$group` by `project_space_id` with `$addToSet` for distinct project count, and a date-windowed comparison for period trend. Pipeline must accept filter parameters from BSABANKSTA-1542 as optional `$match` stages. |
| ST-1543-M2 | Summary statistics response schema | Define `SummaryStatisticsSchema` (Marshmallow ≥3.26.2) with fields: `total_confirmations` (integer), `status_breakdown` (list of objects with `status`, `count`, `percentage`), `active_project_spaces` (integer), `period_trend` (object with `direction`, `percentage_change`, `current_period_count`, `previous_period_count`). |
| ST-1543-M3 | Period trend calculation model | Define the period comparison logic: default 30-day window (current 30 days vs. previous 30 days). Compute percentage change. Handle edge case of zero previous-period data. |

### API

| Sub-Task ID | Description | Technical Details |
|-------------|-------------|-------------------|
| ST-1543-A1 | `GET /api/reports/summary` endpoint | New Flask 3.1.3 blueprint endpoint returning aggregated summary statistics. Accept optional filter parameters (same as BSABANKSTA-1542: `date_start`, `date_end`, `project_space_ids`, `statuses`, `types`). Response: `{ "total_confirmations": N, "status_breakdown": [...], "active_project_spaces": N, "period_trend": {...} }`. Require JWT authentication. |
| ST-1543-A2 | RBAC middleware for summary endpoint | Apply RBAC filtering to the aggregation query — BSA Administrator aggregates all data; BSA Analyst aggregates only from assigned project spaces. Reuse RBAC middleware from BSABANKSTA-1541. |
| ST-1543-A3 | Caching strategy for summary statistics | Implement server-side caching (e.g., in-memory with TTL or Redis) for summary statistics to avoid re-computing expensive aggregation on every request. Cache key includes user role scope and active filter parameters. Invalidate on data changes. |

### Component

| Sub-Task ID | Description | Technical Details |
|-------------|-------------|-------------------|
| ST-1543-C1 | `SummaryStatisticsSection` container component | Container using TailwindCSS: `grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6 mb-8`. Wraps individual metric cards. Placed above the filter panel in the page layout. |
| ST-1543-C2 | `MetricCard` reusable component | Card component: `bg-white rounded-md shadow-sm p-6 border border-gray-100`. Displays a label (text-sm font-medium text-gray-500), value (text-2xl font-bold text-gray-900), and optional sub-text or trend indicator. |
| ST-1543-C3 | `StatusBreakdownCard` component | Extends `MetricCard` with a mini status breakdown list. Each status row shows: colored dot (matching status badge), status name, count, and percentage. |
| ST-1543-C4 | `TrendIndicator` component | Inline component showing directional arrow (↑ green / ↓ red / → gray) with percentage text. Uses semantic colors: `text-green-600` for positive, `text-red-600` for negative, `text-gray-500` for flat. |
| ST-1543-C5 | `SummarySkeletonLoader` component | Skeleton cards matching `MetricCard` dimensions: `bg-gray-200 rounded-md animate-pulse` with placeholder bars for label and value. |
| ST-1543-C6 | `SummaryErrorState` component | Error state within the summary section: `bg-red-50 border border-red-200 rounded-md p-4 text-center` with error message and `Retry` button. |

### Logic

| Sub-Task ID | Description | Technical Details |
|-------------|-------------|-------------------|
| ST-1543-L1 | Summary statistics data hook | Custom React hook (`useSummaryStatistics`) that fetches summary data from `GET /api/reports/summary` on page load and when filters change. Accepts filter state object from `useReportFilters` (BSABANKSTA-1542). Returns `{ data, isLoading, error, refetch }`. |
| ST-1543-L2 | Filter-responsive summary updates | When filter state changes (via BSABANKSTA-1542), the summary statistics hook re-fetches with the updated filter parameters. Debounced in coordination with the detail data fetch to avoid duplicate requests. |
| ST-1543-L3 | Number formatting utilities | Utility functions for formatting: thousands separators (e.g., 12,345), percentage formatting (e.g., 18.2%), trend percentage with sign (e.g., +12.4%, -3.1%). Use `Intl.NumberFormat` for locale-aware formatting. |
| ST-1543-L4 | Period trend computation logic | Backend logic to compute period-over-period comparison: default 30-day current window vs. previous 30-day window. Calculate percentage change: `((current - previous) / previous) * 100`. Handle edge cases: previous = 0 (show "N/A" or "New"), both = 0 (flat). |
| ST-1543-L5 | RBAC-scoped aggregation | Inject user's accessible project space IDs into the aggregation `$match` stage for BSA Analyst role. BSA Administrator has no project space filter injected. |

### Testing

| Sub-Task ID | Description | Technical Details |
|-------------|-------------|-------------------|
| ST-1543-T1 | Unit tests (pytest ≥9.0.2) for summary aggregation pipeline | Test aggregation with known data: verify total count, status breakdown percentages, distinct project count, and period trend calculation. Test with empty data, single project, multiple projects. |
| ST-1543-T2 | Unit tests (pytest ≥9.0.2) for summary API endpoint | Test response structure, RBAC scoping (admin vs. analyst), filter parameter pass-through, error handling, and caching behavior. |
| ST-1543-T3 | Unit tests (pytest ≥9.0.2) for period trend computation | Test percentage change calculation: positive trend, negative trend, flat trend, zero previous period, zero both periods, very large changes. |
| ST-1543-T4 | Component tests (@testing-library/react 16.x) for `SummaryStatisticsSection` | Test card rendering with mock data, loading skeleton display, error state display, retry button functionality, and responsive layout. |
| ST-1543-T5 | Component tests for `MetricCard` and `TrendIndicator` | Test number formatting, trend direction colors, accessibility (aria-labels, screen reader text for trend direction), and edge case values. |
| ST-1543-T6 | BDD tests (behave 1.x) for acceptance criteria | Implement feature files mapping to all 9 AC scenarios (AC1–AC9). |
| ST-1543-T7 | E2E tests (Playwright ≥1.55.1) for summary statistics workflow | Test: page load → summary cards display → apply filter → summary updates → error simulation → retry → verify RBAC scoping with different user roles. |

---

## Edge Cases

| # | Edge Case | Expected Behavior | Category |
|---|-----------|-------------------|----------|
| 1 | **Zero Confirmations** — No confirmation data exists in any project space | Summary cards display zero values: "Total Confirmations: 0", "Active Project Spaces: 0". Status breakdown shows "No data available." Period trend shows "N/A — insufficient data." | Empty State / Boundary |
| 2 | **Zero Previous Period Data** — Current period has data but previous 30 days has none | Period trend indicator shows "New" or "N/A" instead of a percentage, since division by zero is not meaningful. Arrow is omitted; a neutral gray text indicates insufficient comparison data. | Calculation / Boundary |
| 3 | **Very Large Numbers** — Millions of confirmations across thousands of projects | Numbers formatted with thousands separators (e.g., "1,234,567"). Percentage breakdown rounds to one decimal place. Aggregation pipeline uses indexed fields and `$limit` where possible to maintain < 3s response time. | Performance / Scalability |
| 4 | **Summary API Fails but Detail Data Loads** — Network error only on summary endpoint | Summary section displays error state with retry button. The filter panel and data table below continue to function independently — summary failure does not block the detail report view. | Partial Failure / Resilience |
| 5 | **Rapid Filter Changes Affect Summary** — User changes filters quickly while summary is loading | Previous summary request is cancelled via `AbortController`. Only the latest filter combination triggers a summary computation. Skeleton loader re-appears during each transition. No stale summary data is displayed. | Race Condition / User Interaction |

---

## Dependencies

### Cross-Epic Dependencies

| Dependency | Epic | Type | Description |
|-----------|------|------|-------------|
| **BSABANKSTA-1541** | View Global Confirmations Report (this epic) | **Internal Dependency** | Summary statistics are displayed on the same page as the detail report. The summary section is positioned above the filter panel and data table. Page layout coordination required. |
| **BSABANKSTA-1542** | Filter Cross-Project Reporting Data (this epic) | **Internal Dependency** | Summary statistics recalculate when filters are applied. The summary data hook consumes the filter state from `useReportFilters`. |
| **BSABANKSTA-1305** | Create/Modify Project Space (Batch 1) | **Data Dependency** | Summary metrics aggregate across project space entities managed by F-001. Active project space count derived from F-001 data. |
| **BSABANKSTA-1536** | View All Confirmations (BSABANKSTA-1531) | **Shared Surface** | Summary statistics may also appear on the shared All Confirmations page shell if the reporting page is rendered within it. |
| **BSABANKSTA-1531** | Application Frame and Global Navigation | **Navigation Routing** | Reporting page (including summary) is accessible via the Application Frame's navigation. |
| **Auth0** | External Service | **RBAC** | Summary aggregation scope is filtered by user role and project space access. |

### Cross-Cutting Dependencies

| Dependency | Reference | Description |
|-----------|-----------|-------------|
| Auth0 Authentication | External Service | RBAC enforcement for aggregation scope (admin sees all; analyst sees assigned). |

### Bidirectional Dependency Links

- **This story → BSABANKSTA-1541**: Summary section integrates into the report page layout
- **This story → BSABANKSTA-1542**: Summary recalculates based on filter state changes
- **BSABANKSTA-1542 → This story**: Filter changes trigger summary data re-fetch
- **This story → BSABANKSTA-1305**: Aggregation queries span project space data from F-001
- **This story → BSABANKSTA-1536**: Summary may render on shared All Confirmations surface

---

## Story Estimation Guidance

| Attribute | Value |
|-----------|-------|
| **Story Points** | **5** (Fibonacci) |
| **Complexity** | Moderate |
| **Uncertainty** | Low-Moderate |
| **Effort** | Moderate |

### Rationale

- **Moderate complexity**: Four summary metric cards with distinct aggregation logic (total count, status breakdown with percentages, distinct project count, period trend with comparison). The MongoDB aggregation pipeline requires multiple stages (`$match`, `$group`, `$count`, date-windowed comparison). The trend indicator requires period-over-period calculation with edge case handling. However, the UI components are relatively straightforward card displays.
- **Low-Moderate uncertainty**: Summary card patterns are well-established. The main uncertainty is in the period trend calculation logic (30-day window definition, handling zero-data periods) and ensuring summary data stays consistent with the detail table under all filter combinations.
- **Moderate effort**: Backend aggregation pipeline, dedicated API endpoint with RBAC, 6 frontend components (container, 3 card types, skeleton, error state), data hook with filter reactivity, number formatting utilities, and comprehensive testing.
- **5 points** (rather than 8 for the other stories) because the UI is simpler (read-only display cards vs. interactive tables/filters), there are fewer acceptance criteria, and the primary complexity is in the backend aggregation rather than multi-faceted frontend interaction.

---

## Refinement Notes

### Global Rule #2 — Proactive Story Decomposition
- **Status:** No decomposition required.
- **Rationale:** This story contains 9 acceptance criteria (AC1–AC9) focused on a single display capability (summary statistics). The AC count is below the >10 threshold. All ACs relate to the same workflow (load summary → display metrics → respond to filters) without distinct multi-workflow branches.

### Global Rule #3 — Verb-Noun Title Standardization
- **Original Title:** "Global Views — Reporting Dashboard Summary"
- **Standardized Title:** "Aggregate Confirmation Summary Statistics"
- **Rationale:** Applied Verb-Noun format. "Aggregate" is the verb (reflecting the core action of computing cross-project summaries); "Confirmation Summary Statistics" is the noun phrase. Removed "Global Views —" organizational prefix and "Dashboard" (which conflates with BSABANKSTA-1572 My Projects Dashboard).

### Global Rule #4 — Pagination to Lazy Load Override
- **Override Status:** ✅ **NOT DIRECTLY APPLICABLE** — Summary statistics are aggregated metrics displayed in cards, not a paginated list. However, the summary statistics integrate with the infinite scroll data view (BSABANKSTA-1541) and respond to filter changes that affect infinite scroll behavior. No pagination references exist in this story's requirements.
- **Reference:** CC-RQ-001

### Global Rule #5 — Jira Source of Truth
- **Applied:** Requirements text is the authoritative source. F-004 has no Figma frames for direct comparison. Summary card design is generated using the design token manifest.

### Global Rule #6 — NFR Elevation
- **Applied:** Performance targets extracted into NFR section: < 2s API response, < 3s MongoDB aggregation, < 500ms client render, < 3s initial page load, WCAG 2.1 AA for summary cards.

### Global Rule #8 — Generated UI Specifications
- **Status:** ✅ Applied — All summary card UI elements require Generated UI Specifications since F-004 has no Figma frames. Card designs use the Priority 2 semantic token approach with standard card and grid patterns.

---

## Discrepancy Review

F-004 (Global Views and Reporting) has no dedicated Figma wireframes. All UI specifications are requirements-driven and generated using the Generated UI Specifications SOP (Global Rule #8). No direct Figma-Jira discrepancy comparison is applicable for this epic.

---

## Generated UI Specifications

> **DESIGN REVIEW REQUIRED:** The following UI specifications were automatically generated based on the existing design system tokens, as no explicit mockup was provided for these elements. Please review for accuracy and design intent before development.

### Summary Statistics Section Layout

| UI Element | Design Token / Attribute | Value | Rationale |
|-----------|------------------------|-------|-----------|
| Section container | `display`, `grid`, `gap`, `margin` | `grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6 mb-8` | Responsive 4-column grid that collapses to 2 and then 1 column (Priority 2: Semantic Token) |

### Metric Card (Total Confirmations, Active Project Spaces)

| UI Element | Design Token / Attribute | Value | Rationale |
|-----------|------------------------|-------|-----------|
| Card container | `background`, `border-radius`, `shadow`, `padding`, `border` | `bg-white rounded-md shadow-sm p-6 border border-gray-100` | Standard summary card matching dashboard card pattern (Priority 1: Reuse from BSABANKSTA-1572 Dashboard Card Grid pattern) |
| Metric label | `font-size`, `font-weight`, `color` | `text-sm font-medium text-gray-500` | Descriptive label above the metric value |
| Metric value | `font-size`, `font-weight`, `color` | `text-2xl font-bold text-gray-900` | Prominent numeric display |
| Sub-text (optional) | `font-size`, `color` | `text-xs text-gray-400 mt-1` | Additional context below the value |

### Status Breakdown Card

| UI Element | Design Token / Attribute | Value | Rationale |
|-----------|------------------------|-------|-----------|
| Card container | `background`, `border-radius`, `shadow`, `padding`, `border` | `bg-white rounded-md shadow-sm p-6 border border-gray-100` | Matches other metric cards for visual consistency |
| Status row | `display`, `justify`, `padding` | `flex items-center justify-between py-1.5` | Rows for each status entry |
| Status dot | `size`, `border-radius`, `color` | `h-2.5 w-2.5 rounded-full` with semantic colors: green (Completed), yellow (Pending), blue (In Review), gray (Draft) | Color-coded status indicator (Priority 2: Semantic Token) |
| Status name | `font-size`, `color` | `text-sm text-gray-700` | Status label text |
| Status count | `font-size`, `font-weight`, `color` | `text-sm font-semibold text-gray-900` | Numeric count per status |
| Status percentage | `font-size`, `color` | `text-xs text-gray-400 ml-1` | Parenthetical percentage of total |

### Trend Indicator

| UI Element | Design Token / Attribute | Value | Rationale |
|-----------|------------------------|-------|-----------|
| Trend container | `display`, `items` | `inline-flex items-center gap-1 mt-2` | Inline with metric value or below it |
| Up arrow | `color`, `size` | `text-green-600 h-4 w-4` (↑ SVG icon) | Positive trend direction (Priority 2: Semantic Token) |
| Down arrow | `color`, `size` | `text-red-600 h-4 w-4` (↓ SVG icon) | Negative trend direction |
| Flat indicator | `color`, `size` | `text-gray-500 h-4 w-4` (→ SVG icon) | No significant change |
| Percentage text (positive) | `font-size`, `font-weight`, `color` | `text-sm font-medium text-green-600` | "+12.4%" |
| Percentage text (negative) | `font-size`, `font-weight`, `color` | `text-sm font-medium text-red-600` | "-3.1%" |
| Percentage text (flat) | `font-size`, `font-weight`, `color` | `text-sm font-medium text-gray-500` | "0.0%" or "N/A" |
| Period label | `font-size`, `color` | `text-xs text-gray-400` | "vs. previous 30 days" |

### Summary Skeleton Loader

| UI Element | Design Token / Attribute | Value | Rationale |
|-----------|------------------------|-------|-----------|
| Skeleton card | `background`, `border-radius`, `animation` | `bg-gray-200 rounded-md animate-pulse p-6` | Matches MetricCard dimensions (Priority 2: Semantic Token) |
| Skeleton label bar | `width`, `height`, `background`, `border-radius` | `w-24 h-3 bg-gray-300 rounded` | Placeholder for metric label |
| Skeleton value bar | `width`, `height`, `background`, `border-radius`, `margin` | `w-16 h-7 bg-gray-300 rounded mt-3` | Placeholder for metric value |

### Summary Error State

| UI Element | Design Token / Attribute | Value | Rationale |
|-----------|------------------------|-------|-----------|
| Error container | `background`, `border`, `border-radius`, `padding`, `text-align` | `bg-red-50 border border-red-200 rounded-md p-4 text-center col-span-full` | Spans full grid width (Priority 1: Reuse shared Error State pattern) |
| Error message | `font-size`, `color` | `text-sm text-red-700` | "Unable to load summary statistics" |
| Retry button | `font-size`, `font-weight`, `color`, `padding`, `border-radius`, `background` | `text-sm font-medium text-red-700 hover:text-red-800 bg-red-100 hover:bg-red-200 px-3 py-1.5 rounded-md mt-2` | Retry action button |

---

## Figma Mockup Link

No dedicated Figma wireframes exist for F-004 (Global Views and Reporting). All UI specifications are generated using the design token manifest from the BSA Wireframes Figma file. For reference, the closest visual analogs are:

- **All Confirmations Page (Reporting Table Reference):** [https://www.figma.com/design/6fQyfvBUqImyavY8Fw47FV/BSA-Wireframes?node-id=8241-118805](https://www.figma.com/design/6fQyfvBUqImyavY8Fw47FV/BSA-Wireframes?node-id=8241-118805) — BSABANKSTA-1536 in BSABANKSTA-1531 (Application Frame)
- **My Projects Dashboard (Card Grid Reference):** [https://www.figma.com/design/6fQyfvBUqImyavY8Fw47FV/BSA-Wireframes?node-id=7646-212204](https://www.figma.com/design/6fQyfvBUqImyavY8Fw47FV/BSA-Wireframes?node-id=7646-212204) — BSABANKSTA-1572 (My Projects Dashboard) — used as Priority 1 reuse reference for card grid layout and card styling

---

## Definition of Done (Story-Level)

- [ ] All acceptance criteria (AC1–AC9) pass BDD validation
- [ ] Unit tests written and passing (pytest ≥9.0.2 for aggregation pipeline and API, Vitest ≥4.0.18 for components)
- [ ] Component tests written and passing (@testing-library/react 16.x for SummaryStatisticsSection, MetricCard, TrendIndicator)
- [ ] E2E tests written and passing (Playwright ≥1.55.1 for summary statistics workflow)
- [ ] BDD tests written and passing (behave 1.x)
- [ ] Code reviewed and approved
- [ ] All four summary metric cards render correctly with real data
- [ ] Status breakdown shows accurate counts and percentages
- [ ] Period trend indicator shows correct direction and percentage
- [ ] Summary statistics respond to filter changes from BSABANKSTA-1542
- [ ] Skeleton loading state displays during API fetch
- [ ] Error state with retry button displays on API failure
- [ ] Summary section independent of detail table (summary failure does not block table)
- [ ] RBAC enforcement verified (admin sees all; analyst sees assigned project spaces only)
- [ ] Number formatting uses thousands separators and locale-aware formatting
- [ ] WCAG 2.1 AA accessibility verified (screen reader, color contrast, trend text alternatives)
- [ ] NFRs validated (< 2s API, < 3s aggregation, < 500ms render, < 3s page load)
- [ ] Generated UI Specifications reviewed by design team (DESIGN REVIEW REQUIRED)
- [ ] Cross-epic dependency links verified bidirectionally
- [ ] Documentation updated
