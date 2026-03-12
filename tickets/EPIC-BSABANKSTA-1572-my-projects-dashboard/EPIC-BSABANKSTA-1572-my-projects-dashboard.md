# EPIC-BSABANKSTA-1572: My Projects Dashboard

> **Epic ID:** BSABANKSTA-1572
> **Batch:** 2 (New)
> **Feature Reference:** F-003 — My Projects Dashboard
> **Total Story Points:** 21
> **Figma Frames:** 3 frames — `7646-212204`, `7646-213105`, `7646-211031`
> **Status:** Draft — Ready for Refinement

---

## Epic Summary

### Strategic Goal

Provide each BSA/AML compliance officer with a personalized, at-a-glance dashboard view of their active confirmation projects, enabling rapid project monitoring, status assessment, and direct navigation to project details. The My Projects Dashboard serves as the primary user entry point for day-to-day project interaction, transforming the user experience from a project-search paradigm to a curated, role-specific workspace. By surfacing user-relevant project data immediately upon login, the dashboard improves operational throughput, reduces the risk of missed deadlines or overlooked project actions, and establishes a centralized hub for individual compliance workload management.

### Business Context

BSA/AML compliance operations involve managing multiple concurrent confirmation projects across various financial institutions. Each compliance officer — whether a BSA Analyst or BSA Administrator — is assigned to a portfolio of project spaces, each tracking confirmation activities for specific financial entities. The daily operational workflow requires officers to monitor the status of all their assigned projects, identify those requiring immediate attention, and navigate to project details for management actions.

Without a personalized dashboard, compliance officers must navigate through global project lists, perform repeated searches, or rely on external tracking tools to maintain awareness of their project portfolio status. This creates several operational inefficiencies and compliance risks:

- **Delayed response to project status changes** — Officers may not notice overdue deadlines, pending actions, or status transitions until they manually check each project
- **Increased time spent locating projects** — Without personalized filtering, officers must search or browse through all organizational projects to find their assigned work
- **Risk of missed deadlines** — Confirmation projects have regulatory deadlines; without a consolidated view of approaching deadlines, officers may miss critical due dates
- **Fragmented workflow context** — Switching between multiple project spaces requires navigating back and forth, losing workflow context with each navigation

The My Projects Dashboard addresses these gaps by:

- **Surfacing personalized project data immediately upon login** — The dashboard displays only the authenticated user's assigned project spaces, filtered by the user's Auth0 identity and project access permissions
- **Providing at-a-glance status assessment** — Visual status indicators on project cards show project lifecycle state (active, pending, completed, overdue), enabling rapid triage of the officer's entire portfolio
- **Enabling direct navigation to project spaces** — Each project card provides action links for immediate navigation to the corresponding F-001 project space detail view, reducing the clicks needed to reach project management screens
- **Supporting efficient portfolio monitoring** — The responsive card-grid layout displays key project metadata (name, status, key dates, confirmation counts) in a scannable format
- **Handling large portfolios with infinite scroll** — For officers with many assigned projects, cursor-based infinite scroll ensures smooth, performant data loading without traditional pagination interruptions

This personalized dashboard capability improves operational throughput, reduces compliance risk from missed deadlines, and provides the foundation for proactive project management within the BSA program.

### Key Features to be Implemented

- **Personalized Project Card Grid**: Dashboard displays project summary cards in a responsive grid layout (`grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6`) filtered to the authenticated user's assigned project spaces. Data is personalized based on the user's Auth0 identity token and project space access permissions.

- **Project Summary Cards**: Each card shows project metadata including project name, current status, key dates (created date, last modified, deadline), confirmation counts, and action indicators. Cards are styled with `bg-white rounded-md shadow-sm p-6` with hover effects (`hover:shadow-md transition-shadow duration-150`).

- **Status Indicators**: Visual status badges on project cards display the project lifecycle state using semantic color coding — active (green), pending (amber), overdue (red), and completed (gray). Status badges use `inline-flex items-center px-2.5 py-0.5 rounded-full text-xs font-medium` pattern.

- **Action Links / Navigation**: Direct navigation from dashboard cards to individual project space detail views (F-001, BSABANKSTA-1305). Each card includes a clickable action link or the entire card is clickable, routing the user to the corresponding project space via React Router.

- **Lazy Load / Infinite Scroll**: Project card grid uses cursor-based infinite scroll for users with many assigned projects. Implemented with React Intersection Observer API — no traditional pagination controls are rendered (per CC-RQ-001 / Global Rule #4). Includes loading indicators during fetch and end-of-list indicator when all cards are loaded.

- **Dashboard Filtering and Sorting**: Users can filter project cards by status (active, pending, completed, overdue) and sort by relevant criteria (name, date, status). Filtering updates the card grid dynamically.

- **Dashboard Summary Metrics**: Aggregated summary statistics displayed above the card grid showing total project count, status distribution, and projects requiring attention, providing at-a-glance portfolio overview.

- **Dashboard Data Refresh**: Mechanism to refresh dashboard data to reflect the latest project states. Users can manually trigger a refresh, and optionally an auto-refresh interval can be configured via `[Dashboard Refresh Interval]` deployment variable.

- **Empty State Handling**: When a user has no assigned projects, a meaningful empty state is displayed with a descriptive message and optional guidance for next steps.

- **Error State with Retry**: When API calls fail, an error state with a retry button is displayed. Previously loaded data is preserved during network interruptions.

### Out of Scope

The following items are explicitly excluded from this epic:

- **Mobile-specific layouts** — All dashboard views target desktop and responsive web only (per CC-RQ-003). No native mobile or mobile-first layouts are produced.
- **Traditional pagination** — Globally replaced by lazy load / infinite scroll across all card views (per Global Rule #4 / CC-RQ-001). No page-number-based navigation is implemented.
- **Project space CRUD operations** — Creating, modifying, and deleting project spaces is covered by BSABANKSTA-1305 (Create/Modify Project Space) in Batch 1. The dashboard only displays and navigates to existing project spaces.
- **Cross-project reporting and aggregated views** — Organization-level data aggregation and cross-project reporting is covered by BSABANKSTA-1540 (Global Views and Reporting). The dashboard provides user-specific views only.
- **Administrative functions** — User management, system messages, and audit trail capabilities are covered by BSABANKSTA-131 (BSA Admin Persona).
- **Navigation framework and application shell** — The persistent navigation bar, application header, and global navigation routing are covered by BSABANKSTA-1531 (Application Frame and Global Navigation). The dashboard is embedded within this shell.
- **Database schema migrations** — No data model specifications or migration files are included in this documentation phase (Constraint C-004).
- **Direct FinCEN/OFAC system integrations** — External regulatory system connections are outside the scope of this epic.
- **Batch 3+ dashboard features** — Only the stories identified in this epic are in scope. Future enhancements (e.g., dashboard customization, drag-and-drop card reordering, saved views) are deferred.
- **Real-time push notifications for project status changes** — Dashboard data is loaded on demand via API and optional polling. No WebSocket or push-based notification system is included.
- **Data export functionality** — Exporting dashboard data to PDF/CSV is not included in this epic.
- **Personalized dashboard configuration** — Users cannot customize card layout, visible fields, or dashboard preferences in this iteration.

---

## User Stories Index

| Story ID | Title (Verb-Noun) | Story Points | Figma Frames | Status |
|----------|-------------------|:------------:|--------------|--------|
| BSABANKSTA-1573 | View My Projects Dashboard | 8 | 3 frames: `7646-212204`, `7646-213105`, `7646-211031` + Partial — Generated UI Specifications Required | Draft |
| BSABANKSTA-1574 | Navigate Project Space Details | 5 | 2 frames: `7646-212204`, `7646-213105` | Draft |
| BSABANKSTA-1575 | Filter Dashboard Projects | 8 | 2 frames: `7646-213105`, `7646-211031` + Partial — Generated UI Specifications Required | Draft |

**Total Estimated Story Points: 21** (Fibonacci sum: 8 + 5 + 8)

> **Note on Story Extraction:** Story IDs BSABANKSTA-1573, BSABANKSTA-1574, and BSABANKSTA-1575 were derived from the functional requirements documented for F-003 (My Projects Dashboard) in the project's operational specification, cross-epic references, and the 3 associated Figma frames. The three stories decompose F-003's capabilities into: (1) the primary personalized dashboard view with project card grid and infinite scroll, (2) card-to-project-detail navigation and interaction, and (3) dashboard filtering, sorting, and summary metrics. This decomposition follows the single-responsibility principle and ensures each story meets INVEST criteria while maintaining clear boundaries between view rendering, navigation behavior, and data manipulation.

> **Decomposition Analysis (Global Rule #2):** Each story was evaluated for AC count and workflow complexity. No story exceeds 10 ACs or contains multiple distinct workflows requiring further decomposition. BSABANKSTA-1573 has 9 ACs (below threshold). BSABANKSTA-1574 has 7 ACs. BSABANKSTA-1575 has 9 ACs.

---

## Dependencies

### Epics This Epic Depends On

| Epic ID | Epic Name | Batch | Dependency Type | Description |
|---------|-----------|:-----:|-----------------|-------------|
| **BSABANKSTA-1305** | Create/Modify Project Space | 1 | **Data Dependency (Critical)** | Dashboard displays project space entities created and managed by F-001. BSABANKSTA-1305 is the data producer for project metadata, confirmation records, project lifecycle state, and user-project assignments. Without F-001 project space data, the dashboard has no data source and renders an empty state. The MongoDB queries in Model sub-tasks target the Project Spaces collection managed by F-001. |
| **BSABANKSTA-1531** | Application Frame and Global Navigation | 2 | **Navigation Dependency** | The Application Frame provides navigation routing to the My Projects Dashboard via the global navigation framework. The dashboard page is embedded within the Application Frame shell and accessed through the persistent header navigation. Users reach the dashboard via the global navigation menu item provided by F-005. |
| **Auth0** | Auth0 Identity Provider | External Service | **Authentication & RBAC** | User authentication, identity, and project access permissions. The dashboard must personalize data based on the authenticated user's project space assignments. Auth0 provides the user identity token (JWT) used to determine accessible project spaces. Both BSA Analyst and BSA Administrator personas access the dashboard. |

### Epics That Depend On This Epic

| Epic ID | Epic Name | Batch | Dependency Type | Description |
|---------|-----------|:-----:|-----------------|-------------|
| **BSABANKSTA-1540** | Global Views and Reporting | 2 | **Related Reporting** | Global reporting may reference dashboard-level aggregation or personalized metrics for organizational reporting context. Dashboard and Global Reporting share the same F-001 data source but at different aggregation levels — dashboard provides user-specific views while F-004 provides organization-level aggregation. |
| **BSABANKSTA-1531** | Application Frame and Global Navigation | 2 | **Navigation Destination** | The Application Frame routes to this dashboard as a primary navigation destination. The dashboard is one of the key landing pages within the application, and its route must be registered in the global navigation framework. |

### Story-Level Dependency Map

| Source Story (This Epic) | Target Story / Epic | Dependency Type | Description |
|--------------------------|---------------------|-----------------|-------------|
| BSABANKSTA-1573 (View My Projects Dashboard) | BSABANKSTA-1305 (Project Spaces, Batch 1) | Data Dependency | Dashboard card grid queries the Project Spaces MongoDB collection to retrieve user-assigned project data including project metadata, status, dates, and confirmation counts. |
| BSABANKSTA-1573 (View My Projects Dashboard) | BSABANKSTA-1531 (Application Frame) | Navigation Routing | Dashboard page is accessed via the global navigation menu. The Application Frame provides the page shell and navigation context. |
| BSABANKSTA-1573 (View My Projects Dashboard) | Auth0 (External Service) | Authentication | User identity token is required to filter project data to the authenticated user's assigned project spaces. |
| BSABANKSTA-1574 (Navigate Project Space Details) | BSABANKSTA-1305 (Project Spaces, Batch 1) | Navigation Target | Card navigation routes to the F-001 project space detail view. The target route and page component are defined by the Project Space epic. |
| BSABANKSTA-1574 (Navigate Project Space Details) | BSABANKSTA-1531 (Application Frame) | Navigation Routing | React Router navigation from dashboard to project detail uses routes defined within the Application Frame routing configuration. |
| BSABANKSTA-1575 (Filter Dashboard Projects) | BSABANKSTA-1305 (Project Spaces, Batch 1) | Data Dependency | Filter dimensions (project statuses, date ranges) and sortable fields are derived from the F-001 data model. Available filter values are populated from project space data. |
| BSABANKSTA-1575 (Filter Dashboard Projects) | BSABANKSTA-1540 (Global Views and Reporting) | Related Patterns | Shared filtering patterns (status filters, date range filters) may be reusable between dashboard and global reporting views. Consistent filter UX across F-003 and F-004 is desirable. |
| All stories (this epic) | CC-RQ-001 (Cross-Cutting Requirement) | UI Pattern Dependency | All card views implement the Lazy Load / Infinite Scroll pattern using cursor-based data loading with React Intersection Observer API. No traditional pagination controls are rendered. |
| All stories (this epic) | Auth0 (External Service) | Authentication & Authorization | All dashboard endpoints require authenticated sessions. Data filtering by user identity ensures personalized views. |

---

## System Placeholders

> **Global Rule #7 Compliance:** All placeholders listed below are configurable deployment variables. They must NOT be hard-coded in the application. Each placeholder must be resolved at deployment time through environment variables, configuration files, or a runtime configuration service.

| Placeholder | Description | Used By | Example Value |
|-------------|-------------|---------|---------------|
| `[Application Name]` | Configurable application title displayed in dashboard page header, breadcrumbs, and browser tab titles. Must be a configurable deployment variable. | All stories — page title, breadcrumbs | `BSA Banking Confirmations` |
| `[Default Card Batch Size]` | Configurable number of project cards to fetch per infinite scroll batch. Deployment-specific based on network and performance characteristics. | BSABANKSTA-1573, BSABANKSTA-1575 — infinite scroll batch size | `12` |
| `[API Base URL]` | Deployment-specific base URL for all dashboard API endpoints. Varies by environment (development, staging, production). | All stories — API calls | `https://api.bsa-confirmations.internal/v1` |
| `[MongoDB Connection String]` | Deployment-specific database connection string for MongoDB queries against the Project Spaces collection. | All stories — data layer | `mongodb+srv://...` |
| `[Auth0 Domain]` | Auth0 tenant domain for authentication endpoints and JWT validation. Deployment-specific (dev/staging/prod). | All stories — authentication | `bsa-confirmations.us.auth0.com` |
| `[Auth0 Client ID]` | Auth0 application client identifier for the SPA authentication. Deployment-specific. | All stories — authentication | `a1b2c3d4e5f6g7h8i9j0` |
| `[Auth0 Audience]` | Auth0 API audience identifier for JWT token scoping and backend authorization. | All stories — API authorization | `https://api.bsa-confirmations.internal` |
| `[Dashboard Refresh Interval]` | Optional configurable auto-refresh interval (in seconds) for dashboard data. Set to `0` to disable auto-refresh. Deployment-specific. | BSABANKSTA-1573 — data refresh | `0` (disabled by default) |
| `[Dashboard Date Format]` | Configurable date format for date display on project cards. Deployment-specific to accommodate institutional date formatting preferences. | All stories — date display | `MM/DD/YYYY` |
| `[Empty State Help URL]` | Optional configurable URL for help/documentation linked from the dashboard empty state. | BSABANKSTA-1573 — empty state CTA | `https://docs.institution.com/getting-started` |

### Implementation Notes

- All placeholders must be injected via environment variables at build time (for static values) or runtime configuration (for dynamic values).
- The React application should consume these via a centralized configuration module (e.g., `src/config/appConfig.ts`) that reads from `import.meta.env` (Vite environment variables) or a runtime configuration endpoint.
- Auth0-related placeholders are consumed by the `@auth0/auth0-react` `Auth0Provider` component configuration.
- Database and API-related placeholders are consumed by the Flask backend configuration module.

---

## Definition of Done (Epic-Level)

The following checklist defines the completion criteria for the My Projects Dashboard epic. All items must be satisfied before the epic can be considered complete and accepted.

### Functional Completion

- [ ] All 3 user stories (BSABANKSTA-1573, BSABANKSTA-1574, BSABANKSTA-1575) are completed and accepted by the Product Owner
- [ ] All acceptance criteria across all stories pass BDD (Given/When/Then) validation
- [ ] Dashboard displays personalized project cards for the authenticated user's assigned projects only
- [ ] Project card grid renders correctly with responsive column layout (`grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6`)
- [ ] Project summary cards display accurate, up-to-date project data from F-001 project spaces
- [ ] Status indicators correctly reflect project lifecycle state using semantic color coding (active=green, pending=amber, overdue=red, completed=gray)
- [ ] Action links / card navigation to individual project space detail views (F-001) function correctly
- [ ] All card views use lazy load / infinite scroll — no pagination controls exist (per CC-RQ-001)
- [ ] Dashboard filtering by status works correctly and dynamically updates the card grid
- [ ] Dashboard sorting by relevant criteria reorders project cards appropriately
- [ ] Dashboard data reflects the latest project state with configurable refresh capability
- [ ] Data access respects user permissions — users only see projects they are assigned to via Auth0 identity

### Non-Functional Requirements

- [ ] Dashboard API endpoints return data within < 2 seconds for standard queries
- [ ] Initial page loads complete within < 3 seconds
- [ ] Infinite scroll batch loads complete within < 1 second per batch
- [ ] Card interaction and navigation to project detail completes within < 500ms
- [ ] No pagination patterns exist — all views use lazy load / infinite scroll
- [ ] WCAG 2.1 AA accessibility compliance verified across all dashboard elements:
  - [ ] Keyboard navigation functional for all interactive elements (cards, links, filters, buttons)
  - [ ] Screen reader announcements for data loading, status changes, and error states
  - [ ] Color contrast ratios meet AA minimum thresholds (4.5:1 for text)
  - [ ] Focus indicators visible on all interactive elements
  - [ ] Cards are accessible via keyboard with clear focus management

### Cross-Epic Integration

- [ ] Cross-epic dependency links verified bidirectionally with all related epics:
  - [ ] BSABANKSTA-1305 (Create/Modify Project Space) — data dependency verified; project data loads correctly
  - [ ] BSABANKSTA-1531 (Application Frame) — navigation routing verified; dashboard accessible from global nav
  - [ ] BSABANKSTA-1540 (Global Views and Reporting) — organizational vs. personalized view relationship validated
  - [ ] BSABANKSTA-131 (BSA Admin Persona) — admin capabilities do not interfere with dashboard rendering
- [ ] Navigation from dashboard cards to F-001 project space detail views verified end-to-end

### Configuration and Deployment

- [ ] All configurable placeholders resolve correctly per deployment environment
- [ ] `[Application Name]` displays correctly in dashboard header and page title
- [ ] `[Default Card Batch Size]` configures the correct infinite scroll batch size
- [ ] `[Dashboard Date Format]` formats dates consistently across all project cards
- [ ] Auth0 configuration variables connect to the correct tenant and audience
- [ ] `[MongoDB Connection String]` connects to the correct database and Project Spaces collection

### Quality Assurance

- [ ] All 3 Figma frames reviewed against implementation:
  - [ ] Frame `7646-212204` — My Projects Dashboard Screen 1 (primary card-grid view)
  - [ ] Frame `7646-213105` — My Projects Dashboard Screen 2 (expanded/alternate state)
  - [ ] Frame `7646-211031` — My Projects Dashboard Screen 3 (additional view/state)
- [ ] Generated UI Specifications for undepicted elements reviewed by design team (empty state, error state, loading skeleton, end-of-list indicator)
- [ ] Unit tests pass for all backend API endpoints (pytest)
- [ ] Unit tests pass for all MongoDB queries and data layer (pytest)
- [ ] Component tests pass for all React components (Vitest + @testing-library/react)
- [ ] BDD acceptance tests pass (behave)
- [ ] E2E test suite covering all dashboard workflows passes (Playwright):
  - [ ] Login → dashboard loads with personalized project cards
  - [ ] Scroll → infinite scroll loads additional cards
  - [ ] Click card → navigates to project detail view
  - [ ] Filter by status → card grid updates dynamically
  - [ ] Empty state → displayed when user has no assigned projects
  - [ ] Error state → displayed with retry on API failure
  - [ ] End-of-list → indicator displayed when all cards loaded
- [ ] Integration testing with upstream data sources (F-001 project spaces) passes
- [ ] Documentation is complete and reviewed

---

## Design Token Manifest

> **Global Rule #8, Step 1 — Design Token Manifest:** The following manifest was constructed from the Figma BSA Wireframes file (`6fQyfvBUqImyavY8Fw47FV`) Assets panel. This manifest governs all Generated UI Specifications for stories in this epic where Figma frames do not depict specific UI elements.

### Colors

| Token | Value | Usage |
|-------|-------|-------|
| Brand Primary | `blue-600` (#2563EB) | Primary actions, active states, links, card action links |
| Brand Primary Hover | `blue-700` (#1D4ED8) | Hover states for primary actions and links |
| Surface White | `white` (#FFFFFF) | Card backgrounds, page background |
| Surface Gray Light | `gray-50` (#F9FAFB) | Page background, alternate surface |
| Surface Gray | `gray-100` (#F3F4F6) | Hover backgrounds, secondary surfaces |
| Border Light | `gray-200` (#E5E7EB) | Card borders, dividers, error state border |
| Border Medium | `gray-300` (#D1D5DB) | Input borders, filter control borders |
| Text Primary | `gray-900` (#111827) | Card titles, page heading, primary text content |
| Text Secondary | `gray-600` (#4B5563) | Descriptions, secondary labels, dashboard subtitle |
| Text Tertiary | `gray-500` (#6B7280) | Card metadata, muted content, empty state text |
| Text Muted | `gray-400` (#9CA3AF) | End-of-list indicator, disabled text |
| Success | `green-800` (#166534) | Active status badge text |
| Success Background | `green-100` (#DCFCE7) | Active status badge background |
| Error | `red-600` (#DC2626) | Error text, error icons |
| Error Text | `red-800` (#991B1B) | Overdue status badge text |
| Error Background | `red-50` (#FEF2F2) | Error state container background |
| Error Badge Background | `red-100` (#FEE2E2) | Overdue status badge background |
| Error Border | `red-200` (#FECACA) | Error state container border |
| Warning | `amber-800` (#92400E) | Pending status badge text |
| Warning Background | `amber-100` (#FEF3C7) | Pending status badge background |
| Completed Text | `gray-800` (#1F2937) | Completed status badge text |
| Completed Background | `gray-100` (#F3F4F6) | Completed status badge background |

### Typography

| Token | Value | Usage |
|-------|-------|-------|
| Font Family | System default (Inter, -apple-system, sans-serif) | All text |
| Heading Large | `text-2xl font-semibold` (24px, 600 weight) | Dashboard page title |
| Heading Medium | `text-xl font-semibold` (20px, 600 weight) | Section titles, summary headings |
| Heading Small | `text-lg font-semibold` (18px, 600 weight) | Card project name |
| Body | `text-base font-normal` (16px, 400 weight) | Dashboard description, body text |
| Body Small | `text-sm font-normal` (14px, 400 weight) | Card metadata, filter labels |
| Caption | `text-xs font-medium` (12px, 500 weight) | Status badge text, small labels |
| Link | `text-sm font-medium text-blue-600` | Card action links |

### Spacing

| Token | Value | Usage |
|-------|-------|-------|
| Page Padding X | `px-4 sm:px-6 lg:px-8` (16/24/32px) | Dashboard page container horizontal padding |
| Page Padding Y | `py-8` (32px) | Dashboard page container vertical padding |
| Section Gap | `mb-6` (24px) | Between dashboard header and card grid |
| Card Padding | `p-6` (24px) | Project card internal padding |
| Card Gap | `gap-6` (24px) | Between project cards in grid |
| Card Title Margin | `mb-2` (8px) | Below card project name |
| Badge Padding | `px-2.5 py-0.5` (10px/2px) | Status badge internal padding |
| Empty State Padding | `py-16` (64px) | Vertical padding for empty state container |
| End-of-List Padding | `py-4` (16px) | Vertical padding for end-of-list indicator |
| Error State Padding | `p-4` (16px) | Error state container padding |

### Border Radii

| Token | Value | Usage |
|-------|-------|-------|
| Small | `rounded-sm` (2px) | Small UI elements |
| Medium | `rounded-md` (6px) | Project cards, buttons, inputs, filter controls |
| Large | `rounded-lg` (8px) | Filter panel, larger containers |
| Full | `rounded-full` | Status badges, circular indicators |

### Effects

| Token | Value | Usage |
|-------|-------|-------|
| Shadow Small | `shadow-sm` | Project cards (default state) |
| Shadow Medium | `shadow-md` | Project cards (hover state), dropdown menus |
| Ring Focus | `ring-2 ring-blue-500` | Focus state for interactive elements |
| Transition Standard | `transition-shadow duration-150` | Card hover shadow transition |
| Transition All | `transition-all duration-150` | General state transitions |
| Pulse Animation | `animate-pulse` | Loading skeleton cards |

---

## Component Mapping Reference

| UI Element | TailwindCSS Pattern | Token Priority | Notes |
|-----------|-------------------|----------------|-------|
| Dashboard Page Container | `max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8` | Priority 2 — Semantic | Standard page container |
| Dashboard Header | `text-2xl font-semibold text-gray-900 mb-6` | Priority 2 — Semantic | Page title styling |
| Dashboard Description | `text-base text-gray-600 mb-8` | Priority 2 — Semantic | Subtitle/description |
| Project Card Grid | `grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6` | Priority 1 — From Figma frames | Responsive card layout |
| Project Card | `bg-white rounded-md shadow-sm p-6 hover:shadow-md transition-shadow duration-150` | Priority 1 — From Figma frames | Card with hover effect |
| Card Title | `text-lg font-semibold text-gray-900 mb-2` | Priority 2 — Semantic | Project name in card |
| Card Metadata | `text-sm text-gray-500` | Priority 2 — Semantic | Dates, counts, etc. |
| Status Badge (Active) | `inline-flex items-center px-2.5 py-0.5 rounded-full text-xs font-medium bg-green-100 text-green-800` | Priority 2 — Semantic | Active project status |
| Status Badge (Pending) | `inline-flex items-center px-2.5 py-0.5 rounded-full text-xs font-medium bg-amber-100 text-amber-800` | Priority 2 — Semantic | Pending project status |
| Status Badge (Overdue) | `inline-flex items-center px-2.5 py-0.5 rounded-full text-xs font-medium bg-red-100 text-red-800` | Priority 2 — Semantic | Overdue project status |
| Status Badge (Completed) | `inline-flex items-center px-2.5 py-0.5 rounded-full text-xs font-medium bg-gray-100 text-gray-800` | Priority 2 — Semantic | Completed project status |
| Card Action Link | `text-blue-600 hover:text-blue-800 text-sm font-medium` | Priority 2 — Semantic | Navigate to project detail |
| Summary Metric Card | `bg-white rounded-md shadow-sm p-6` in `grid grid-cols-1 md:grid-cols-3 gap-6` | Priority 2 — Semantic | Dashboard summary statistics |
| Summary Metric Value | `text-3xl font-bold text-gray-900` | Priority 2 — Semantic | Large metric number |
| Summary Metric Label | `text-sm text-gray-500 mt-1` | Priority 2 — Semantic | Metric description |
| Filter Bar | `flex items-center gap-4 mb-6` | Priority 2 — Semantic | Filter controls container |
| Filter Select | `border border-gray-300 rounded-md px-3 py-2 text-sm bg-white` | Priority 2 — Semantic | Status filter dropdown |
| Sort Select | `border border-gray-300 rounded-md px-3 py-2 text-sm bg-white` | Priority 2 — Semantic | Sort criteria dropdown |
| InfiniteScrollTrigger | Intersection Observer trigger element within `overflow-auto` container | Priority 1 — Reuse shared | Shared pattern from other views |
| Loading Skeleton Card | `animate-pulse bg-gray-200 rounded-md h-48` within grid | Priority 2 — Semantic | Placeholder during initial load |
| Empty State | `flex flex-col items-center justify-center py-16 text-gray-500` | Priority 1 — Reuse shared | No assigned projects |
| End-of-List | `text-center py-4 text-gray-400` with divider | Priority 1 — Reuse shared | All project cards loaded |
| Error State / Retry | `bg-red-50 border border-red-200 rounded-md p-4` with retry button | Priority 1 — Reuse shared | API failure |
| Refresh Button | `text-sm text-blue-600 hover:text-blue-800 font-medium` with refresh icon | Priority 2 — Semantic | Manual data refresh |

---

## Technology Stack References

| Category | Package | Version | Usage in Sub-Tasks |
|----------|---------|---------|-------------------|
| Backend Runtime | Python | 3.13.x | Backend runtime environment |
| Backend Framework | Flask | 3.1.3 | API blueprint routes for dashboard data endpoints |
| Validation | Marshmallow | ≥3.26.2 | Request/response schema serialization |
| Database | PyMongo | ≥4.7.0 | MongoDB queries for user-specific project retrieval |
| Authentication | authlib / pyjwt | ≥1.6.8 / ≥2.10.1 | JWT validation for user identification and RBAC |
| CORS | Flask-CORS | ≥6.0.2 | Cross-origin configuration |
| AI Integration | LangChain | ≥1.2.5 | Blitzy Platform communication and AI-assisted processing |
| Backend Testing | pytest | ≥9.0.2 | Unit tests for API and data layer |
| BDD Testing | behave | 1.x | Acceptance criteria validation |
| Frontend Framework | React | 19.2.4 | UI components (DashboardPage, ProjectCard, etc.) |
| Type Safety | TypeScript | 5.9.x | Frontend type definitions |
| CSS Framework | TailwindCSS | 4.2.1 | Design token application, card grid, status badges |
| Build Tool | Vite | ≥7.3.1 | Frontend build with @tailwindcss/vite |
| Routing | React Router | 7.x | Client-side navigation (dashboard → project detail) |
| Auth | @auth0/auth0-react | SPA SDK | Session management, user identity, RBAC |
| Frontend Testing | Vitest | ≥4.0.18 | Unit tests |
| Component Testing | @testing-library/react | 16.x | Component behavior tests |
| E2E Testing | Playwright | ≥1.55.1 | End-to-end workflow tests |
| Database | MongoDB | ≥8.0.17 (Atlas) | Project Spaces collection — user-assigned project data |

---
---

# STORY DOCUMENTATION

> The following sections contain complete 14-section story documentation for each user story in this epic. Stories are separated by horizontal rules (`---`).

---

# View My Projects Dashboard

**Story ID:** BSABANKSTA-1573
**Epic:** BSABANKSTA-1572 — My Projects Dashboard
**Batch:** 2
**Feature Reference:** F-003 — My Projects Dashboard

---

## User Story

**As a** BSA Analyst or BSA Administrator,
**I want** to view a personalized dashboard displaying my assigned confirmation projects as summary cards in a responsive grid layout with cursor-based infinite scroll,
**So that** I can quickly assess the status of all my active confirmation projects at a glance, identify projects requiring immediate attention, and efficiently manage my daily compliance workload without navigating through global project lists or performing repeated searches.

---

## INVEST Validation

| Principle | Validation | Notes |
|-----------|------------|-------|
| **Independent** | ✅ Pass | The dashboard view can be developed independently from the navigation story (BSABANKSTA-1574) and filtering story (BSABANKSTA-1575). The core card grid rendering, infinite scroll, and data loading are self-contained within this story. Requires the Application Frame shell (BSABANKSTA-1531) for page mounting, but the dashboard page component itself is independent. |
| **Negotiable** | ✅ Pass | Card layout details (column count, card fields, metadata display), batch size configuration, and specific status indicator styling are negotiable with stakeholders. The core requirement — displaying personalized project cards with infinite scroll — is fixed. |
| **Valuable** | ✅ Pass | Provides the primary day-to-day operational interface for compliance officers to monitor their project portfolio. Reduces time spent searching for projects and provides at-a-glance status awareness. Directly supports operational efficiency and reduces risk of missed compliance deadlines. |
| **Estimable** | ✅ Pass | 3 Figma frames (`7646-212204`, `7646-213105`, `7646-211031`) provide clear visual guidance for the dashboard layout. The card-grid-with-infinite-scroll pattern is well-understood and aligns with the shared infinite scroll pattern used across other epics (CC-RQ-001). |
| **Small** | ✅ Pass | Single page with project card grid, status indicators, and infinite scroll. Does not include filtering/sorting (BSABANKSTA-1575) or detailed card navigation (BSABANKSTA-1574). Well-scoped for a single sprint delivery. |
| **Testable** | ✅ Pass | Clearly testable: card grid rendering with correct column layout, personalized data loading (user-specific projects only), infinite scroll behavior (trigger, append, end-of-list), status badge rendering, empty state display, error state with retry, loading skeleton cards, and RBAC data filtering. |

---

## Non-Functional Requirements

| NFR ID | Category | Requirement | Target |
|--------|----------|-------------|--------|
| NFR-1573-01 | Performance | Initial page load to first meaningful paint | < 3 seconds |
| NFR-1573-02 | Performance | Next batch of project cards loads when scroll threshold is reached | < 1 second |
| NFR-1573-03 | Performance | API response time for dashboard project data retrieval | < 2 seconds |
| NFR-1573-04 | Performance | Card grid re-render after new batch appended | < 200ms (no visible jank) |
| NFR-1573-05 | Accessibility | WCAG 2.1 AA compliance for card grid | Semantic HTML for card structure, `role="list"` and `role="listitem"` for card grid, keyboard navigation between cards, screen reader announcements for loading and status updates |
| NFR-1573-06 | Responsiveness | Desktop-first responsive layout | `grid-cols-1` (mobile), `md:grid-cols-2` (tablet), `lg:grid-cols-3` (desktop). No mobile-specific layouts per CC-RQ-003. |
| NFR-1573-07 | Reliability | Error recovery | Graceful degradation with retry. Previously loaded cards preserved during network interruptions. |
| NFR-1573-08 | Data Freshness | Dashboard data reflects latest project state | Each new batch fetched during infinite scroll retrieves current data. Manual refresh available. |
| NFR-1573-09 | Scalability | Infinite scroll handles large portfolios | No memory leaks from DOM accumulation; consider virtualized rendering for users with 100+ assigned projects. |

---

## Acceptance Criteria

### AC1: Navigate to My Projects Dashboard

```gherkin
Scenario: User navigates to the personalized project dashboard
  Given the user is authenticated as a BSA Analyst or BSA Administrator
  And the Application Frame global navigation is visible
  When the user clicks on the "My Projects" or "Dashboard" item in the global navigation
  Then the My Projects Dashboard page is displayed
  And the page title includes "[Application Name] — My Projects"
  And the page URL reflects the dashboard route
  And the dashboard header displays "My Projects" or equivalent heading
```

### AC2: Personalized Project Card Grid Display

```gherkin
Scenario: Dashboard displays project cards personalized to the authenticated user
  Given the user has navigated to the My Projects Dashboard
  When the initial data fetch completes successfully
  Then project summary cards are displayed in a responsive grid layout
  And the grid uses a responsive column layout (1 column on mobile, 2 on tablet, 3 on desktop)
  And ONLY projects assigned to the authenticated user are displayed
  And projects assigned to other users are NOT visible
  And each project card is styled with a white background, rounded corners, subtle shadow, and padding
```

### AC3: Project Card Content Display

```gherkin
Scenario: Each project card displays relevant project summary data
  Given the My Projects Dashboard is displaying project cards
  When the user views a project card
  Then the card displays the project name as the card title
  And the card displays the current project status as a color-coded status badge
  And the card displays key dates (created date, last modified date, and/or deadline)
  And the card displays a confirmation count or activity indicator
  And the card includes an action link or clickable area for navigating to the project detail view
```

### AC4: Status Badge Rendering

```gherkin
Scenario: Project status badges render with semantic color coding
  Given a project card is displayed on the dashboard
  When the project status is "Active"
  Then the status badge displays with a green background and dark green text

Scenario: Pending status badge rendering
  Given a project card is displayed on the dashboard
  When the project status is "Pending"
  Then the status badge displays with an amber background and dark amber text

Scenario: Overdue status badge rendering
  Given a project card is displayed on the dashboard
  When the project status is "Overdue"
  Then the status badge displays with a red background and dark red text

Scenario: Completed status badge rendering
  Given a project card is displayed on the dashboard
  When the project status is "Completed"
  Then the status badge displays with a gray background and dark gray text
```

### AC5: Infinite Scroll — Initial Data Load

```gherkin
Scenario: Initial data load uses cursor-based lazy loading
  Given the user has navigated to the My Projects Dashboard
  When the page renders for the first time
  Then the initial batch of project cards is loaded using cursor-based lazy loading (NOT traditional pagination)
  And the number of cards in the initial batch matches the configured "[Default Card Batch Size]"
  And a loading indicator (skeleton cards) is displayed during the initial fetch
  And no traditional pagination controls (page numbers, next/previous buttons) are displayed
```

> **Global Rule #4 Override Applied:** Traditional pagination has been replaced with lazy load / infinite scroll using cursor-based data loading and the Intersection Observer API per CC-RQ-001.

### AC6: Infinite Scroll — Load More Cards

```gherkin
Scenario: Next batch of project cards loads automatically on scroll
  Given the My Projects Dashboard is displaying the current batch of project cards
  And additional project cards exist beyond the current batch
  When the user scrolls to the bottom of the currently loaded cards
  And the Intersection Observer detects the scroll threshold trigger element
  Then the next batch of project cards is automatically fetched from the API using the current cursor token
  And the newly fetched cards are appended to the existing card grid without replacing them
  And a loading indicator is displayed at the bottom of the grid during the fetch
  And the user's scroll position is preserved after new cards are appended
```

### AC7: End-of-List Indicator

```gherkin
Scenario: End-of-list indicator displays when all project cards are loaded
  Given the user has scrolled through all available project cards
  And no additional cards remain to be fetched (next_cursor is null)
  When the user reaches the end of the loaded data
  Then an end-of-list indicator is displayed below the card grid (e.g., "All projects loaded")
  And no further API calls are triggered for additional data
  And the Intersection Observer is disconnected to prevent unnecessary observations
```

### AC8: Empty State Display

```gherkin
Scenario: Empty state displays when user has no assigned projects
  Given the user is authenticated and has navigated to the My Projects Dashboard
  And the user has no project spaces assigned to their account
  When the initial data fetch returns an empty result set
  Then a meaningful empty state is displayed with a descriptive message (e.g., "No projects assigned")
  And the empty state includes a subtitle with guidance (e.g., "Contact your administrator to be assigned to a project")
  And an appropriate illustration or icon accompanies the empty state message
  And no error message is shown (this is a valid data state, not an error)
```

### AC9: Error State with Retry

```gherkin
Scenario: Error state with retry displays when API call fails
  Given the user is on the My Projects Dashboard
  And the API call to retrieve project data fails due to a server or network error
  When the error is detected
  Then an error state message is displayed with a user-friendly description (e.g., "Unable to load projects")
  And a "Retry" button is provided below the error message
  And any previously loaded project cards remain visible above the error state
  When the user clicks the "Retry" button
  Then the failed API call is re-attempted
  And if successful, the data loads normally
```

---

## Sub-Tasks

### Model

- [ ] **M-1573-01:** Define MongoDB query pattern for retrieving user-specific project spaces from the Project Spaces collection, filtering by the authenticated user's ID in the project assignment array
- [ ] **M-1573-02:** Create Marshmallow serialization schema (`DashboardProjectCardSchema`) for dashboard project card response data, including fields: `project_id`, `project_name`, `status`, `created_date`, `last_modified`, `deadline`, `confirmation_count`, `assigned_users`
- [ ] **M-1573-03:** Implement cursor-based data loading model for infinite scroll — define cursor encoding/decoding logic using the project's sort field (e.g., `last_modified` timestamp) for stable cursor-based traversal
- [ ] **M-1573-04:** Define project status enum/classification model mapping project lifecycle states to display values: `ACTIVE`, `PENDING`, `COMPLETED`, `OVERDUE`
- [ ] **M-1573-05:** Define user-project assignment relationship query using the authenticated user's `sub` claim from the Auth0 JWT token to filter accessible project spaces

### API

- [ ] **A-1573-01:** Create Flask blueprint `dashboard_bp` with route `GET /api/dashboard/projects` accepting cursor-based query parameters: `cursor` (opaque string), `limit` (integer, default from `[Default Card Batch Size]`)
- [ ] **A-1573-02:** Define API response contract: `{ "data": [ProjectCardObject], "next_cursor": string|null, "total_count": number, "user_id": string }`
- [ ] **A-1573-03:** Implement RBAC middleware decorator `@require_authenticated` that extracts the user identity from the Auth0 JWT token and passes it to the data layer for personalized filtering
- [ ] **A-1573-04:** Implement error handling for API endpoints: 401 Unauthorized (missing/invalid token), 500 Internal Server Error (database failure), with structured error response format
- [ ] **A-1573-05:** Configure Flask-CORS for the dashboard blueprint to allow cross-origin requests from the React SPA origin

### Component

- [ ] **C-1573-01:** Create `DashboardPage` React page component with page container (`max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8`), header (`text-2xl font-semibold text-gray-900 mb-6`), and card grid container
- [ ] **C-1573-02:** Create `ProjectCardGrid` component implementing `grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6` responsive layout with card rendering loop
- [ ] **C-1573-03:** Create `ProjectCard` component with `bg-white rounded-md shadow-sm p-6 hover:shadow-md transition-shadow duration-150` styling, displaying project name, status badge, key dates, confirmation count, and action link
- [ ] **C-1573-04:** Create `StatusBadge` component with `inline-flex items-center px-2.5 py-0.5 rounded-full text-xs font-medium` base and dynamic semantic colors per status (active=green, pending=amber, overdue=red, completed=gray)
- [ ] **C-1573-05:** Create `InfiniteScrollTrigger` component using the Intersection Observer API to detect when the user scrolls near the bottom of the card grid, triggering the next batch fetch
- [ ] **C-1573-06:** Create `EndOfListIndicator` component with `text-center py-4 text-gray-400` styling and a horizontal divider
- [ ] **C-1573-07:** Create `EmptyState` component with `flex flex-col items-center justify-center py-16 text-gray-500` layout including icon, title, and subtitle
- [ ] **C-1573-08:** Create `ErrorStateRetry` component with `bg-red-50 border border-red-200 rounded-md p-4` styling including error message and retry button
- [ ] **C-1573-09:** Create `LoadingSkeletonCard` component with `animate-pulse bg-gray-200 rounded-md h-48` for placeholder rendering during initial fetch (render `[Default Card Batch Size]` skeletons)
- [ ] **C-1573-10:** Apply TailwindCSS 4.2.1 design tokens consistently across all components per the Design Token Manifest

### Logic

- [ ] **L-1573-01:** Implement cursor-based infinite scroll data fetching hook (`useDashboardProjects`) managing cursor state, loading state, error state, and accumulated project card data
- [ ] **L-1573-02:** Implement user-personalized data filtering — extract user identity from Auth0 context (`useAuth0`) and include in API requests for personalized project retrieval
- [ ] **L-1573-03:** Implement project status computation and display logic — map API status values to `StatusBadge` color variants
- [ ] **L-1573-04:** Implement dashboard data refresh logic — manual refresh button handler that resets cursor state and re-fetches from the beginning; optional auto-refresh via `[Dashboard Refresh Interval]` configuration
- [ ] **L-1573-05:** Implement error handling and retry logic — catch API errors, display error state, and provide retry capability that re-attempts the failed request
- [ ] **L-1573-06:** Implement React Router integration — register dashboard route within the application routing configuration
- [ ] **L-1573-07:** Implement Intersection Observer lifecycle management — connect observer on mount, disconnect on unmount, handle threshold callbacks for infinite scroll triggering

### Testing

- [ ] **T-1573-01:** Unit tests (pytest) for `GET /api/dashboard/projects` endpoint — test successful response, cursor-based data loading, user filtering, error responses
- [ ] **T-1573-02:** Unit tests (pytest) for MongoDB user-project assignment query — verify personalized filtering returns only user-assigned projects
- [ ] **T-1573-03:** Unit tests (pytest) for cursor encoding/decoding — verify stable cursor generation and traversal
- [ ] **T-1573-04:** Component tests (@testing-library/react) for `DashboardPage` — verify page renders with header, card grid, loading state, empty state, error state
- [ ] **T-1573-05:** Component tests (@testing-library/react) for `ProjectCard` — verify card displays project name, status badge, dates, action link
- [ ] **T-1573-06:** Component tests (@testing-library/react) for `StatusBadge` — verify correct color rendering for each status value
- [ ] **T-1573-07:** Component tests (@testing-library/react) for `InfiniteScrollTrigger` — verify Intersection Observer callback fires correctly
- [ ] **T-1573-08:** BDD tests (behave) for all 9 acceptance criteria scenarios
- [ ] **T-1573-09:** E2E tests (Playwright) for full dashboard workflow: login → navigate to dashboard → verify card grid → scroll to trigger infinite scroll → verify new cards appended → verify end-of-list
- [ ] **T-1573-10:** Accessibility tests — keyboard navigation between cards, screen reader announcements, focus management, color contrast validation

---

## Edge Cases

1. **User has no assigned projects** — When the authenticated user has zero project spaces assigned to their account, the dashboard must display the empty state with a descriptive message and guidance, not a blank page or error state. The empty state must be visually distinct from the error state.

2. **User has hundreds of assigned projects** — For users with very large project portfolios (100+ projects), infinite scroll must handle data loading gracefully without performance degradation. Consider DOM virtualization to prevent memory leaks from accumulating hundreds of card DOM elements. Each batch fetch must complete within < 1 second.

3. **Network interruption during infinite scroll** — If a network error occurs during an infinite scroll batch fetch (e.g., user loses connectivity mid-scroll), the dashboard must display an inline error with a retry option at the bottom of the grid. All previously loaded project cards must remain visible and interactive above the error indicator.

4. **Project data changes between scroll batches** — If a project's status or metadata changes while the user is viewing the dashboard, newly fetched batches during infinite scroll must reflect the current state. The cursor-based approach ensures consistent traversal even with concurrent data modifications, though already-rendered cards may show stale data until the next refresh.

5. **User's project assignment revoked while viewing dashboard** — If an administrator removes a user's access to a project space while the user is viewing the dashboard, the revocation must be handled gracefully on the next data fetch. Cards for revoked projects may remain visible in the current session but will not appear in subsequent fetches or after a refresh.

---

## Dependencies

| Dependency | Type | Description |
|-----------|------|-------------|
| **BSABANKSTA-1305** (Create/Modify Project Space, Batch 1) | Data Dependency | Dashboard card grid queries the Project Spaces MongoDB collection to retrieve user-assigned project data. F-001 is the data producer for all project metadata, status, dates, and confirmation counts. |
| **BSABANKSTA-1531** (Application Frame and Global Navigation) | Navigation Routing | Dashboard page is accessed via the global navigation menu provided by the Application Frame. The page is embedded within the Application Frame shell. |
| **CC-RQ-001** (Cross-Cutting Requirement) | UI Pattern | Lazy Load / Infinite Scroll — cursor-based data loading with Intersection Observer API replaces all pagination. |
| **Auth0** (External Service) | Authentication | User identity token (JWT) required to filter project data to the authenticated user's assigned project spaces. |

---

## Story Estimation Guidance

**Story Points: 8** (Fibonacci)

**Rationale:**
- **Complexity (High):** This story involves building the core dashboard page with a responsive card grid, multiple component compositions (page, grid, card, status badge, infinite scroll trigger, skeleton, empty state, error state), cursor-based infinite scroll with Intersection Observer, and personalized data loading from a MongoDB backend with RBAC filtering. The integration of 3 Figma frames and design token compliance adds design complexity.
- **Uncertainty (Moderate):** The card grid and infinite scroll patterns are well-established across the application (CC-RQ-001), but personalized data filtering based on user-project assignments introduces Auth0 integration complexity. The exact data model for user-project assignments depends on the F-001 implementation in Batch 1.
- **Effort (High):** Requires full-stack implementation — Flask API endpoint with cursor-based data loading, MongoDB query with user filtering, React page component with multiple child components, TailwindCSS styling with design tokens, Intersection Observer setup, and comprehensive testing across all layers.

---

## Refinement Notes

### Global Rule #3 — Verb-Noun Title Standardization
- **Original Title:** "My Projects Dashboard" (from the epic description and Figma frames)
- **Standardized Title:** "View My Projects Dashboard"
- **Rationale:** Prefixed with the verb "View" to comply with the Verb-Noun format. The original title was a noun phrase; the standardized title clearly describes the user action.

### Global Rule #4 — Pagination to Lazy Load Override
- **Override Applied:** Any implicit or explicit pagination behavior has been replaced with cursor-based lazy load / infinite scroll using the React Intersection Observer API.
- **Documentation:** AC5, AC6, and AC7 specify the complete infinite scroll lifecycle (initial load, load more, end-of-list). No traditional pagination controls are rendered.

### Global Rule #5 — Jira Source of Truth
- Jira requirement text (from the operational specification) was used as the authoritative source for acceptance criteria generation.
- All 3 Figma frames were reviewed against the Jira requirements. See Discrepancy Review section.

### Global Rule #6 — NFR Elevation
- Performance targets extracted from cross-cutting requirements and standard dashboard NFRs have been elevated into the dedicated Non-Functional Requirements section (9 NFRs documented).

### Global Rule #8 — Generated UI Specifications
- The 3 Figma frames cover the primary dashboard card grid view. However, several UI elements required by the acceptance criteria are not depicted in the Figma frames: Empty State, Error State with Retry, Loading Skeleton Cards, End-of-List Indicator, and Card Hover State. These are documented in the Generated UI Specifications section.

---

## Discrepancy Review

| Figma Frame | Node ID | Review Result |
|-------------|---------|---------------|
| My Projects Dashboard — Screen 1 | `7646-212204` | Primary card-grid view. All major UI elements align with Jira requirements. |
| My Projects Dashboard — Screen 2 | `7646-213105` | Expanded/alternate state. UI elements align with Jira requirements. |
| My Projects Dashboard — Screen 3 | `7646-211031` | Additional view/state. UI elements align with Jira requirements. |

**Discrepancy Findings:**
- **Pagination Controls:** If any Figma frame depicts traditional pagination controls, these are **excluded** per Global Rule #4. All list views use cursor-based infinite scroll instead.
- No significant Figma-only elements were identified that conflict with Jira requirements.

No additional discrepancies identified between Figma wireframes and Jira requirements.

---

## Generated UI Specifications

> **DESIGN REVIEW REQUIRED:** The following UI specifications were automatically generated based on the existing design system tokens, as no explicit mockup was provided for these elements. Please review for accuracy and design intent before development.

### Empty State Display (AC8)

| UI Element | Design Token / Attribute | Value | Rationale |
|-----------|------------------------|-------|-----------|
| Container Layout | `display`, `alignment`, `padding` | `flex flex-col items-center justify-center py-16` | Priority 1 — Reuse shared empty state pattern |
| Container Text Color | `color` | `text-gray-500` | Priority 2 — Semantic token for muted text |
| Icon | `size`, `color` | `h-16 w-16 text-gray-300 mb-4` | Priority 2 — Large muted icon |
| Title Text | `font-size`, `font-weight`, `color` | `text-lg font-semibold text-gray-900 mb-2` | Priority 2 — Semantic heading |
| Subtitle Text | `font-size`, `color` | `text-sm text-gray-500` | Priority 2 — Semantic body text |

### Loading Skeleton Cards (AC5)

| UI Element | Design Token / Attribute | Value | Rationale |
|-----------|------------------------|-------|-----------|
| Skeleton Card | `background`, `border-radius`, `height`, `animation` | `animate-pulse bg-gray-200 rounded-md h-48` | Priority 2 — Semantic skeleton loader |
| Skeleton Grid | `layout` | `grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6` | Priority 1 — Reuse card grid layout |
| Skeleton Count | `quantity` | Render `[Default Card Batch Size]` skeletons | Visual consistency with expected batch |

### End-of-List Indicator (AC7)

| UI Element | Design Token / Attribute | Value | Rationale |
|-----------|------------------------|-------|-----------|
| Container | `text-align`, `padding`, `color` | `text-center py-4 text-gray-400` | Priority 1 — Reuse shared end-of-list pattern |
| Divider | `border` | `border-t border-gray-200 mb-4` | Priority 2 — Semantic divider |
| Text Content | `font-size` | `text-sm` — "All projects loaded" | Priority 2 — Semantic small text |

### Error State with Retry (AC9)

| UI Element | Design Token / Attribute | Value | Rationale |
|-----------|------------------------|-------|-----------|
| Container | `background`, `border`, `border-radius`, `padding` | `bg-red-50 border border-red-200 rounded-md p-4` | Priority 1 — Reuse shared error state pattern |
| Error Icon | `color`, `size` | `text-red-600 h-5 w-5` | Priority 2 — Semantic error color |
| Error Title | `font-weight`, `color` | `font-medium text-red-800` | Priority 2 — Semantic error text |
| Error Message | `color` | `text-sm text-red-700` | Priority 2 — Semantic error body |
| Retry Button | `background`, `color`, `border-radius`, `padding` | `bg-red-100 hover:bg-red-200 text-red-800 rounded-md px-3 py-1.5 text-sm font-medium` | Priority 2 — Error context button |

### Card Hover State

| UI Element | Design Token / Attribute | Value | Rationale |
|-----------|------------------------|-------|-----------|
| Card Hover Shadow | `box-shadow`, `transition` | `hover:shadow-md transition-shadow duration-150` | Priority 2 — Semantic hover feedback |
| Card Hover Cursor | `cursor` | `cursor-pointer` | Priority 2 — Interactive cursor |

---

## Figma Mockup Link

| Frame | Description | URL |
|-------|-------------|-----|
| My Projects Dashboard — Screen 1 | Primary dashboard card-grid view | [Figma Frame 7646-212204](https://www.figma.com/design/6fQyfvBUqImyavY8Fw47FV/BSA-Wireframes?node-id=7646-212204) |
| My Projects Dashboard — Screen 2 | Expanded/alternate dashboard state | [Figma Frame 7646-213105](https://www.figma.com/design/6fQyfvBUqImyavY8Fw47FV/BSA-Wireframes?node-id=7646-213105) |
| My Projects Dashboard — Screen 3 | Additional dashboard view/state | [Figma Frame 7646-211031](https://www.figma.com/design/6fQyfvBUqImyavY8Fw47FV/BSA-Wireframes?node-id=7646-211031) |

---

## Definition of Done (Story-Level)

- [ ] All 9 acceptance criteria pass BDD validation
- [ ] Unit tests written and passing (pytest for API, Vitest for components)
- [ ] Component tests written and passing (@testing-library/react)
- [ ] E2E tests written and passing (Playwright)
- [ ] BDD tests written and passing (behave)
- [ ] Code reviewed and approved
- [ ] Dashboard displays personalized project cards for authenticated user only
- [ ] Project card grid renders with responsive columns (1/2/3 layout)
- [ ] Infinite scroll implementation verified — no pagination controls
- [ ] Status badges render with correct semantic colors for all statuses
- [ ] Empty state, error state, and end-of-list indicator implemented
- [ ] WCAG 2.1 AA accessibility compliance verified
- [ ] NFRs validated (< 3s initial load, < 1s scroll batch, < 2s API)
- [ ] Design review completed against all 3 Figma frames
- [ ] Generated UI Specifications reviewed by design team (DESIGN REVIEW REQUIRED)
- [ ] Cross-epic dependency links verified bidirectionally
- [ ] Documentation updated

---
---

# Navigate Project Space Details

**Story ID:** BSABANKSTA-1574
**Epic:** BSABANKSTA-1572 — My Projects Dashboard
**Batch:** 2
**Feature Reference:** F-003 — My Projects Dashboard

---

## User Story

**As a** BSA Analyst or BSA Administrator,
**I want** to navigate from a project card on My Projects Dashboard directly to the corresponding project space detail view,
**So that** I can quickly access the full details and management capabilities of a specific confirmation project without needing to search or browse through a global project list, reducing the number of clicks required to reach my working context.

---

## INVEST Validation

| Principle | Validation | Notes |
|-----------|------------|-------|
| **Independent** | ✅ Pass | Card navigation can be developed independently from the core dashboard rendering (BSABANKSTA-1573) and filtering (BSABANKSTA-1575). The navigation logic is encapsulated in the card's click handler and React Router integration. Requires BSABANKSTA-1573 cards to exist as the navigation trigger surface. |
| **Negotiable** | ✅ Pass | The specific navigation mechanism (full card click vs. explicit button/link), transition animations, and pre-fetch/pre-load strategies are negotiable. The core requirement — navigating from a dashboard card to the project detail — is fixed. |
| **Valuable** | ✅ Pass | Provides the critical bridge between the dashboard overview and detailed project management. Without this story, the dashboard displays project data but offers no interactive pathway to take action on specific projects, severely limiting operational utility. |
| **Estimable** | ✅ Pass | The navigation pattern is well-understood (React Router with dynamic route parameters). The Figma frames show clickable card areas. The F-001 project detail route is defined in the Application Frame. Estimation is straightforward. |
| **Small** | ✅ Pass | Focused exclusively on the click-to-navigate interaction from dashboard card to project detail view. Does not include project detail rendering (F-001), dashboard data loading (BSABANKSTA-1573), or filtering (BSABANKSTA-1575). |
| **Testable** | ✅ Pass | Clearly testable: card click triggers navigation, URL updates to correct project route, correct project ID is passed, breadcrumb trail updates, browser back button returns to dashboard, and keyboard accessibility for card activation. |

---

## Non-Functional Requirements

| NFR ID | Category | Requirement | Target |
|--------|----------|-------------|--------|
| NFR-1574-01 | Performance | Navigation response time from card click to project detail initial render | < 500ms |
| NFR-1574-02 | Performance | React Router route transition | < 100ms for client-side routing |
| NFR-1574-03 | Accessibility | Keyboard navigation and activation | Cards must be keyboard-focusable and activatable via Enter/Space keys |
| NFR-1574-04 | Accessibility | Focus management | Focus moves to the project detail page heading after navigation |
| NFR-1574-05 | UX | Browser history management | Navigating to a project detail creates a history entry; browser back button returns to the dashboard with scroll position preserved |
| NFR-1574-06 | UX | Visual feedback on card interaction | Hover state (shadow elevation), focus ring, and active/pressed state provide clear interactive affordance |

---

## Acceptance Criteria

### AC1: Click Project Card to Navigate

```gherkin
Scenario: User clicks a project card to navigate to the project detail view
  Given the user is viewing the My Projects Dashboard with project cards displayed
  And a project card for project "Project ABC" with project ID "proj-123" is visible
  When the user clicks anywhere on the project card
  Then the application navigates to the project space detail view for project "proj-123"
  And the URL updates to the project detail route (e.g., "/projects/proj-123")
  And the project detail page renders with data for "Project ABC"
```

### AC2: Card Action Link Navigation

```gherkin
Scenario: User clicks the explicit action link on a project card
  Given the user is viewing a project card on the My Projects Dashboard
  And the card includes an explicit action link (e.g., "View Details" or "Open Project")
  When the user clicks the action link
  Then the application navigates to the project space detail view for that project
  And the navigation behavior is identical to clicking the card body
```

### AC3: Keyboard Navigation and Activation

```gherkin
Scenario: User navigates to and activates a project card using the keyboard
  Given the user is viewing the My Projects Dashboard with project cards displayed
  When the user presses the Tab key to move focus through the page
  Then focus moves sequentially through the project cards in the grid
  And each focused card displays a visible focus indicator (focus ring)
  When the user presses Enter or Space on a focused project card
  Then the application navigates to the project space detail view for that project
```

### AC4: Browser Back Navigation

```gherkin
Scenario: User navigates back from project detail to dashboard
  Given the user has navigated from the My Projects Dashboard to a project detail view
  When the user clicks the browser back button
  Then the application returns to the My Projects Dashboard
  And previously loaded project cards are restored (or re-fetched)
  And the user's approximate scroll position within the card grid is restored
```

### AC5: Correct Project ID Passed to Detail View

```gherkin
Scenario: Correct project identifier is passed during navigation
  Given the user is viewing the My Projects Dashboard with multiple project cards
  When the user clicks on a specific project card
  Then the navigation route includes the correct project ID as a route parameter
  And the project detail view loads data for the exact project that was clicked
  And no data from a different project is displayed
```

### AC6: Card Interactive States

```gherkin
Scenario: Project card displays interactive visual feedback states
  Given a project card is visible on the My Projects Dashboard
  When the user hovers over the card with a mouse
  Then the card shadow elevates (hover:shadow-md) to indicate interactivity
  And the cursor changes to a pointer
  When the user presses and holds the card
  Then a subtle pressed/active visual state is displayed
  When the user focuses the card via keyboard
  Then a visible focus ring is displayed around the card
```

### AC7: Navigation Loading State

```gherkin
Scenario: Loading indicator displays during navigation transition
  Given the user has clicked a project card on the dashboard
  And the project detail view requires an API call to load data
  When the navigation transition begins
  Then a loading indicator or skeleton is displayed while the project detail data loads
  And the previous dashboard view is not shown during the loading state
  When the project detail data loads successfully
  Then the loading indicator is replaced by the project detail content
```

---

## Sub-Tasks

### Model

- [ ] **M-1574-01:** No new data model required — this story uses the project ID already available in the dashboard card data from BSABANKSTA-1573's `DashboardProjectCardSchema`
- [ ] **M-1574-02:** Validate that the project card schema includes the `project_id` field required for constructing the navigation route

### API

- [ ] **A-1574-01:** No new API endpoint required — navigation uses the existing project detail endpoint defined in BSABANKSTA-1305 (F-001): `GET /api/projects/:project_id`
- [ ] **A-1574-02:** Verify that the F-001 project detail endpoint accepts the project ID format returned by the dashboard endpoint

### Component

- [ ] **C-1574-01:** Update `ProjectCard` component to wrap the card body in a React Router `<Link>` or make the card itself a clickable navigation element with `onClick` handler calling `useNavigate()`
- [ ] **C-1574-02:** Implement card interactive states: hover (`hover:shadow-md transition-shadow duration-150`), focus (`focus:ring-2 focus:ring-blue-500 focus:ring-offset-2`), active (`active:shadow-sm`)
- [ ] **C-1574-03:** Ensure `ProjectCard` renders with `role="link"` or uses native `<a>` element semantics for accessibility
- [ ] **C-1574-04:** Add cursor styling: `cursor-pointer` on hover
- [ ] **C-1574-05:** Implement navigation loading state — a route-level loading boundary (React Suspense or loading component) that displays while the project detail page data loads
- [ ] **C-1574-06:** Implement breadcrumb update logic — the project detail page breadcrumb should show "My Projects > Project Name" to provide context and return navigation

### Logic

- [ ] **L-1574-01:** Implement navigation handler in `ProjectCard` — construct project detail route URL using the card's `project_id` and call `useNavigate()` from React Router
- [ ] **L-1574-02:** Implement scroll position preservation — when returning to the dashboard via browser back button, restore the user's approximate scroll position in the card grid. Use React Router's `ScrollRestoration` or a custom scroll position manager
- [ ] **L-1574-03:** Implement dashboard state caching — when navigating away and returning, preserve the loaded cards and cursor position to avoid re-fetching from scratch (consider using React Router loader or a client-side cache)
- [ ] **L-1574-04:** Implement focus management — after navigation to the project detail, move focus to the page heading for screen reader users
- [ ] **L-1574-05:** Register the dashboard route in the React Router configuration: `/dashboard` or `/my-projects` pointing to `DashboardPage`

### Testing

- [ ] **T-1574-01:** Component tests (@testing-library/react) for `ProjectCard` click navigation — verify clicking a card triggers `useNavigate()` with the correct project route
- [ ] **T-1574-02:** Component tests for keyboard activation — verify Enter and Space keys on a focused card trigger navigation
- [ ] **T-1574-03:** Component tests for card hover/focus/active states — verify correct CSS classes are applied
- [ ] **T-1574-04:** E2E tests (Playwright) for full navigation flow: dashboard → click card → verify project detail loads → browser back → verify dashboard state preserved
- [ ] **T-1574-05:** BDD tests (behave) for all 7 acceptance criteria scenarios
- [ ] **T-1574-06:** Accessibility tests — verify focus indicator visibility, keyboard navigability, ARIA attributes, and focus management after navigation

---

## Edge Cases

1. **Project detail route not found (deleted project)** — If a user clicks a project card but the corresponding project has been deleted or deactivated since the dashboard data was loaded, the project detail view should display a user-friendly "Project Not Found" page with a link to return to the dashboard, rather than a generic 404 error.

2. **Rapid double-click on project card** — If the user double-clicks a project card, the navigation should only trigger once. Prevent duplicate navigation events that could cause React Router to push multiple history entries or trigger multiple API calls.

3. **Navigation during infinite scroll loading** — If the user clicks a project card while an infinite scroll batch fetch is in progress, the navigation should proceed immediately without waiting for the pending fetch to complete. The pending fetch should be cancelled or its result discarded.

4. **Slow network during navigation** — If the project detail page takes longer than expected to load (e.g., > 3 seconds), the loading indicator must remain visible and the user should have the option to cancel (browser back button) to return to the dashboard.

5. **Browser forward/back rapid cycling** — If the user rapidly presses the browser back and forward buttons between the dashboard and project detail, the application should handle route changes gracefully without race conditions in data loading or rendering.

---

## Dependencies

| Dependency | Type | Description |
|-----------|------|-------------|
| **BSABANKSTA-1573** (View My Projects Dashboard, this epic) | Internal Prerequisite | Project cards must exist in the dashboard for navigation to function. Card data including `project_id` is produced by BSABANKSTA-1573. |
| **BSABANKSTA-1305** (Create/Modify Project Space, Batch 1) | Navigation Target | The project space detail view is defined and rendered by F-001. Navigation target URL structure and project detail API endpoint are owned by BSABANKSTA-1305. |
| **BSABANKSTA-1531** (Application Frame and Global Navigation) | Routing Infrastructure | React Router configuration and the Application Frame shell are provided by F-005. Dashboard route must be registered within the global routing configuration. |
| **Auth0** (External Service) | Authentication Context | User must remain authenticated during navigation. Auth0 session must persist across route transitions. |

---

## Story Estimation Guidance

**Story Points: 5** (Fibonacci)

**Rationale:**
- **Complexity (Moderate):** The core navigation logic is straightforward — a React Router `useNavigate()` call with a dynamic project ID. However, additional complexity arises from scroll position preservation on back navigation, dashboard state caching, focus management for accessibility, and interactive card states.
- **Uncertainty (Low):** React Router navigation patterns are well-established. The project detail route structure is defined by F-001. Card click handling is a standard UI pattern. Low integration risk.
- **Effort (Moderate):** Requires updating the `ProjectCard` component with interactive states and navigation handler, implementing scroll restoration, route registration, focus management, breadcrumb updates, and comprehensive testing. Less effort than BSABANKSTA-1573 but more than a trivial navigation wrapper.

---

## Refinement Notes

### Global Rule #3 — Verb-Noun Title Standardization
- **Original Title:** "Project Card Navigation" / "Project Space Details Access"
- **Standardized Title:** "Navigate Project Space Details"
- **Rationale:** Standardized to Verb-Noun format with "Navigate" as the action verb and "Project Space Details" as the noun target, clearly describing the user's intent.

### Global Rule #4 — Pagination to Lazy Load Override
- **Not directly applicable:** This story focuses on card-to-detail navigation, not list rendering. However, the infinite scroll context is relevant: navigation may occur from any card position within the infinite scroll, and scroll position must be preserved on return. No pagination controls are introduced.

### Global Rule #5 — Jira Source of Truth
- Jira requirement text was used as the authoritative source for acceptance criteria. Navigation behavior was derived from the Figma frames (clickable card areas visible in all 3 frames) and confirmed against the Jira operational specification.

### Global Rule #6 — NFR Elevation
- Performance targets extracted: < 500ms navigation response, < 100ms route transition. Accessibility NFRs for keyboard navigation and focus management elevated to the dedicated NFR section.

### Global Rule #8 — Generated UI Specifications
- Card interactive states (hover, focus, active) may not be fully depicted in static Figma frames. Generated specifications for these states are included in the Generated UI Specifications section.

---

## Discrepancy Review

| Figma Frame | Node ID | Review Result |
|-------------|---------|---------------|
| My Projects Dashboard — Screen 1 | `7646-212204` | Clickable card areas visible. No discrepancy with navigation requirements. |
| My Projects Dashboard — Screen 2 | `7646-213105` | Alternate state shows card interactions. Consistent with Jira requirements. |
| My Projects Dashboard — Screen 3 | `7646-211031` | Additional view confirms card navigation targets. No discrepancy. |

No discrepancies identified between Figma wireframes and Jira requirements for card navigation behavior.

---

## Generated UI Specifications

> **DESIGN REVIEW REQUIRED:** The following UI specifications were automatically generated based on the existing design system tokens, as no explicit mockup was provided for these elements. Please review for accuracy and design intent before development.

### Card Focus State (AC3, AC6)

| UI Element | Design Token / Attribute | Value | Rationale |
|-----------|------------------------|-------|-----------|
| Focus Ring | `outline`, `ring` | `focus:ring-2 focus:ring-blue-500 focus:ring-offset-2` | Priority 2 — Semantic focus indicator matching system focus ring pattern |
| Focus Ring Offset | `ring-offset` | `ring-offset-2` (2px offset from card edge) | Priority 2 — Provides visual separation between focus ring and card border |

### Card Active/Pressed State (AC6)

| UI Element | Design Token / Attribute | Value | Rationale |
|-----------|------------------------|-------|-----------|
| Active Shadow | `box-shadow` | `active:shadow-sm` | Priority 2 — Semantic pressed state (reduced shadow suggests pressing "into" surface) |
| Active Scale | `transform` | `active:scale-[0.99]` (optional) | Priority 3 — Subtle scale reduction for tactile feedback |

### Navigation Loading State (AC7)

| UI Element | Design Token / Attribute | Value | Rationale |
|-----------|------------------------|-------|-----------|
| Loading Container | `layout`, `alignment` | `flex items-center justify-center min-h-[50vh]` | Priority 2 — Semantic centered loading |
| Loading Spinner | `animation`, `color`, `size` | `animate-spin text-blue-600 h-8 w-8` | Priority 2 — Semantic loading indicator |
| Loading Text | `font-size`, `color`, `margin` | `text-sm text-gray-500 mt-4` | Priority 2 — Semantic support text |

### Breadcrumb Trail (AC4 return context)

| UI Element | Design Token / Attribute | Value | Rationale |
|-----------|------------------------|-------|-----------|
| Breadcrumb Container | `layout`, `spacing`, `font-size` | `flex items-center gap-2 text-sm text-gray-500 mb-4` | Priority 2 — Semantic breadcrumb |
| Breadcrumb Link | `color`, `hover` | `text-blue-600 hover:text-blue-800` | Priority 2 — Semantic link styling |
| Breadcrumb Separator | `content`, `color` | `text-gray-400` — "/" or ">" character | Priority 3 — Base separator |
| Breadcrumb Current | `font-weight`, `color` | `font-medium text-gray-900` | Priority 2 — Semantic current page indicator |

---

## Figma Mockup Link

| Frame | Description | URL |
|-------|-------------|-----|
| My Projects Dashboard — Screen 1 | Shows clickable card areas for navigation | [Figma Frame 7646-212204](https://www.figma.com/design/6fQyfvBUqImyavY8Fw47FV/BSA-Wireframes?node-id=7646-212204) |
| My Projects Dashboard — Screen 2 | Alternate state with card interaction patterns | [Figma Frame 7646-213105](https://www.figma.com/design/6fQyfvBUqImyavY8Fw47FV/BSA-Wireframes?node-id=7646-213105) |
| My Projects Dashboard — Screen 3 | Additional view confirming navigation targets | [Figma Frame 7646-211031](https://www.figma.com/design/6fQyfvBUqImyavY8Fw47FV/BSA-Wireframes?node-id=7646-211031) |

---

## Definition of Done (Story-Level)

- [ ] All 7 acceptance criteria pass BDD validation
- [ ] Unit tests written and passing (Vitest for components)
- [ ] Component tests written and passing (@testing-library/react)
- [ ] E2E tests written and passing (Playwright)
- [ ] BDD tests written and passing (behave)
- [ ] Code reviewed and approved
- [ ] Clicking a project card navigates to the correct project detail view
- [ ] Keyboard navigation (Tab, Enter, Space) works correctly
- [ ] Browser back button returns to dashboard with state preserved
- [ ] Scroll position restored on back navigation
- [ ] Card hover, focus, and active states render correctly
- [ ] Navigation loading state displays during route transitions
- [ ] WCAG 2.1 AA accessibility compliance verified (focus management, keyboard activation)
- [ ] NFRs validated (< 500ms navigation, < 100ms route transition)
- [ ] Design review completed against Figma frames
- [ ] Generated UI Specifications reviewed by design team (DESIGN REVIEW REQUIRED)
- [ ] Cross-epic dependency links verified bidirectionally
- [ ] Documentation updated

---
---

# Filter Dashboard Projects

**Story ID:** BSABANKSTA-1575
**Epic:** BSABANKSTA-1572 — My Projects Dashboard
**Batch:** 2
**Feature Reference:** F-003 — My Projects Dashboard

---

## User Story

**As a** BSA Analyst or BSA Administrator,
**I want** to filter, sort, and search my dashboard project cards by status, date range, project name, and other criteria,
**So that** I can quickly locate specific projects or focus on a subset of my assigned confirmation projects (e.g., only overdue projects, most recently modified), improving my ability to prioritize and manage my compliance workload efficiently.

---

## INVEST Validation

| Principle | Validation | Notes |
|-----------|------------|-------|
| **Independent** | ✅ Pass | Filtering, sorting, and search functionality can be developed independently from the core dashboard rendering (BSABANKSTA-1573) and card navigation (BSABANKSTA-1574). The filter controls compose on top of the existing card grid and data fetching layer. |
| **Negotiable** | ✅ Pass | The specific filter criteria (status, date, name), sort options (name, date, status), search behavior (client-side vs. server-side), and filter UI layout (sidebar, toolbar, dropdown) are negotiable with stakeholders. The core requirement — enabling users to narrow down their dashboard view — is fixed. |
| **Valuable** | ✅ Pass | Enables compliance officers to efficiently manage large project portfolios by narrowing the view to relevant subsets. Critical for users with many assigned projects who need to focus on specific statuses (e.g., overdue) or recently modified projects. Directly reduces time-to-action for high-priority items. |
| **Estimable** | ✅ Pass | Filter/sort/search patterns are well-established in web applications. The Figma frames provide visual guidance for filter control placement. Server-side filtering with cursor-based infinite scroll is a known pattern from CC-RQ-001. |
| **Small** | ✅ Pass | Focused on filter/sort/search controls and their integration with the existing card grid and infinite scroll data layer. Does not include core dashboard rendering (BSABANKSTA-1573), card navigation (BSABANKSTA-1574), or new dashboard views. |
| **Testable** | ✅ Pass | Clearly testable: filter by status returns only matching projects, sort by date orders correctly, search by name returns matching results, clear filters restores full view, filter state persists across infinite scroll batches, combined filters work correctly, and filter state reflected in URL parameters. |

---

## Non-Functional Requirements

| NFR ID | Category | Requirement | Target |
|--------|----------|-------------|--------|
| NFR-1575-01 | Performance | Filter/sort application response time (server-side re-query) | < 1 second for API response with filters applied |
| NFR-1575-02 | Performance | Search input debounce delay | 300ms debounce to prevent excessive API calls during typing |
| NFR-1575-03 | Performance | Filter/sort UI control render time | < 100ms to display filter controls |
| NFR-1575-04 | Performance | Filtered results initial load with cursor-based infinite scroll | < 2 seconds |
| NFR-1575-05 | Accessibility | Filter controls fully accessible via keyboard | Tab navigation between filter controls, keyboard-operable dropdowns, screen reader labels for all controls |
| NFR-1575-06 | Accessibility | Filter state announced to screen readers | ARIA live region announces filter application and result count changes |
| NFR-1575-07 | UX | Filter state persistence | Active filters reflected in URL query parameters for shareability and browser history |
| NFR-1575-08 | UX | Clear All Filters | Single action to reset all active filters and restore the unfiltered dashboard view |

---

## Acceptance Criteria

### AC1: Filter Projects by Status

```gherkin
Scenario: User filters dashboard projects by status
  Given the user is viewing the My Projects Dashboard with project cards displayed
  And filter controls are visible in the dashboard toolbar area
  When the user selects a status filter (e.g., "Active", "Pending", "Overdue", "Completed")
  Then only project cards matching the selected status are displayed
  And the card grid updates to show filtered results using cursor-based infinite scroll
  And a visual indicator shows that a filter is active (e.g., badge count or highlighted filter control)
  And the total result count updates to reflect the filtered dataset
```

### AC2: Sort Projects by Criteria

```gherkin
Scenario: User sorts dashboard projects by a selected criterion
  Given the user is viewing the My Projects Dashboard
  When the user selects a sort option from the sort control (e.g., "Last Modified", "Project Name", "Created Date", "Status")
  Then the project cards re-order according to the selected sort criterion
  And the default sort direction is applied (descending for dates, ascending for names)
  When the user toggles the sort direction
  Then the project cards re-order in the opposite direction
  And the card grid refreshes from the first cursor position with the new sort applied
```

### AC3: Search Projects by Name

```gherkin
Scenario: User searches for projects by name
  Given the user is viewing the My Projects Dashboard
  And a search input field is visible in the dashboard toolbar
  When the user types a project name or partial name into the search field
  Then the input is debounced (300ms delay after last keystroke)
  And the card grid filters to show only projects whose names match the search query (case-insensitive partial match)
  And the card grid resets to the first cursor position with search results loaded via infinite scroll
  And if no projects match, the empty state is displayed with a message like "No projects match your search"
```

### AC4: Combined Filters

```gherkin
Scenario: User applies multiple filters simultaneously
  Given the user is viewing the My Projects Dashboard
  When the user selects a status filter (e.g., "Active")
  And the user also enters a search query (e.g., "Bank of")
  Then the card grid displays only projects that match BOTH the status filter AND the search query
  And the active filter indicators reflect both active filters
  And the result count reflects the combined filtered dataset
```

### AC5: Clear All Filters

```gherkin
Scenario: User clears all active filters to restore the full dashboard view
  Given the user has one or more active filters applied on the My Projects Dashboard
  When the user clicks the "Clear All Filters" or "Reset" action
  Then all filter controls are reset to their default (unfiltered) state
  And the search input is cleared
  And the sort order returns to the default
  And the card grid reloads with the full unfiltered dataset from the first cursor position
  And the URL query parameters are cleared
```

### AC6: Filter State in URL Parameters

```gherkin
Scenario: Active filters are reflected in URL query parameters
  Given the user applies filters on the My Projects Dashboard
  When the status filter is set to "Overdue" and sort is set to "Last Modified"
  Then the URL updates to include query parameters (e.g., "?status=overdue&sort=last_modified&dir=desc")
  And if the user shares or bookmarks the URL, the same filters are applied when the URL is loaded
  And navigating to the URL directly applies the filters and loads the filtered results
```

### AC7: Infinite Scroll with Active Filters

```gherkin
Scenario: Infinite scroll continues to work correctly with active filters
  Given the user has applied a status filter on the My Projects Dashboard
  And the filtered result set contains more cards than the initial batch
  When the user scrolls to the bottom of the currently loaded filtered cards
  Then the next batch of filtered cards is loaded using the cursor-based approach
  And the filter criteria are included in every subsequent batch request
  And no unfiltered cards appear in the filtered view
  And the end-of-list indicator displays when all filtered results are loaded
```

### AC8: No Results State with Filters

```gherkin
Scenario: No results state displays when filters produce zero matches
  Given the user is viewing the My Projects Dashboard
  When the user applies a filter combination that returns zero matching projects
  Then a "no results" empty state is displayed (e.g., "No projects match your filters")
  And the message includes a suggestion to adjust or clear filters
  And the "Clear All Filters" action is prominently displayed
  And no error message is shown (this is a valid data state)
```

### AC9: Filter Controls Accessibility

```gherkin
Scenario: Filter controls are fully accessible via keyboard and screen readers
  Given the user is viewing the My Projects Dashboard with filter controls visible
  When the user navigates the filter controls using the Tab key
  Then focus moves sequentially through all filter controls (status dropdown, search input, sort selector, clear filters button)
  And each control has a visible focus indicator
  And each control has an accessible label (via aria-label or associated <label>)
  When the user applies a filter using keyboard interactions
  Then an ARIA live region announces the filter result (e.g., "Showing 5 active projects")
```

---

## Sub-Tasks

### Model

- [ ] **M-1575-01:** Extend the dashboard MongoDB query to support server-side filtering — add query conditions for `status` field (exact match), `project_name` field (case-insensitive regex partial match), and date range filters (`created_date`, `last_modified`)
- [ ] **M-1575-02:** Update Marshmallow schema to validate and deserialize filter query parameters: `status` (enum: ACTIVE, PENDING, COMPLETED, OVERDUE), `search` (string), `sort_by` (enum: last_modified, project_name, created_date, status), `sort_direction` (enum: asc, desc)
- [ ] **M-1575-03:** Ensure cursor-based data loading model works correctly with filtered and sorted result sets — cursor must encode both the sort field value and the document ID for stable traversal under filter conditions
- [ ] **M-1575-04:** Create MongoDB index on `status` and `project_name` fields for efficient filtered queries

### API

- [ ] **A-1575-01:** Extend `GET /api/dashboard/projects` endpoint to accept additional query parameters: `status` (filter), `search` (name search), `sort_by` (sort field), `sort_direction` (asc/desc)
- [ ] **A-1575-02:** Update API response contract to include filter metadata: `{ "data": [...], "next_cursor": string|null, "total_count": number, "filters_applied": { "status": string|null, "search": string|null, "sort_by": string, "sort_direction": string } }`
- [ ] **A-1575-03:** Implement server-side input validation for filter parameters using Marshmallow — reject invalid status values, sanitize search input to prevent injection, validate sort field names
- [ ] **A-1575-04:** Ensure cursor continuity when filters are applied — if filters change, the cursor must reset to the beginning of the new result set

### Component

- [ ] **C-1575-01:** Create `DashboardFilterToolbar` component positioned above the card grid with: status dropdown, search input, sort selector, sort direction toggle, clear all filters button
- [ ] **C-1575-02:** Create `StatusFilterDropdown` component with `<select>` or custom dropdown listing status options: "All Statuses" (default), "Active", "Pending", "Overdue", "Completed". Styled with `bg-white border border-gray-300 rounded-md shadow-sm px-3 py-2 text-sm`
- [ ] **C-1575-03:** Create `SearchInput` component with debounced input (300ms), placeholder text "Search projects...", and clear button. Styled with `bg-white border border-gray-300 rounded-md shadow-sm pl-10 pr-4 py-2 text-sm` with search icon
- [ ] **C-1575-04:** Create `SortSelector` component with sort field options and direction toggle. Styled to match the filter toolbar aesthetic
- [ ] **C-1575-05:** Create `ActiveFilterIndicator` component showing active filter count or individual filter tags with remove capability
- [ ] **C-1575-06:** Create `ClearAllFiltersButton` with `text-blue-600 hover:text-blue-800 text-sm font-medium` styling
- [ ] **C-1575-07:** Create `NoResultsState` component for zero-match filter scenarios with message, suggestion text, and prominent "Clear All Filters" action. Styled with `flex flex-col items-center justify-center py-12 text-gray-500`
- [ ] **C-1575-08:** Add ARIA live region to announce filter result changes to screen readers
- [ ] **C-1575-09:** Apply TailwindCSS 4.2.1 design tokens consistently across all filter components

### Logic

- [ ] **L-1575-01:** Implement `useDashboardFilters` custom hook managing filter state: `status`, `search`, `sortBy`, `sortDirection`. Integrates with the `useDashboardProjects` hook from BSABANKSTA-1573 to reset cursor and re-fetch when filters change
- [ ] **L-1575-02:** Implement search input debouncing logic — 300ms delay after last keystroke before triggering API call
- [ ] **L-1575-03:** Implement URL query parameter synchronization — serialize active filters to URL search params and deserialize on page load for bookmarkability and shareability
- [ ] **L-1575-04:** Implement filter reset logic — "Clear All Filters" action resets all filter state, clears URL params, and triggers a fresh unfiltered data fetch from cursor position zero
- [ ] **L-1575-05:** Implement cursor reset on filter change — when any filter criteria changes, reset the cursor to null and fetch from the beginning of the new result set
- [ ] **L-1575-06:** Implement ARIA live region update logic — update announcement text when filter results change (e.g., "Showing 12 active projects")
- [ ] **L-1575-07:** Ensure infinite scroll continues to pass active filter parameters with each subsequent batch request

### Testing

- [ ] **T-1575-01:** Unit tests (pytest) for `GET /api/dashboard/projects` with filter parameters — test status filtering, name search, sort options, combined filters, and edge cases (empty results, invalid params)
- [ ] **T-1575-02:** Unit tests (pytest) for MongoDB query builder with filter conditions — verify correct query construction for each filter type and combination
- [ ] **T-1575-03:** Unit tests (pytest) for cursor stability under filtered result sets — verify cursor-based infinite scroll traversal works correctly when filters narrow the result set
- [ ] **T-1575-04:** Component tests (@testing-library/react) for `DashboardFilterToolbar` — verify filter controls render, user can interact with each control, and filter changes trigger callbacks
- [ ] **T-1575-05:** Component tests for `StatusFilterDropdown` — verify all status options render, selection triggers filter update
- [ ] **T-1575-06:** Component tests for `SearchInput` — verify debounced input, placeholder text, clear button
- [ ] **T-1575-07:** Component tests for `NoResultsState` — verify message, suggestion, and clear filters action
- [ ] **T-1575-08:** E2E tests (Playwright) for full filter workflow: load dashboard → apply status filter → verify filtered cards → search by name → verify combined results → clear filters → verify full list
- [ ] **T-1575-09:** BDD tests (behave) for all 9 acceptance criteria scenarios
- [ ] **T-1575-10:** Accessibility tests — keyboard navigation through filter controls, ARIA live region announcements, screen reader labels

---

## Edge Cases

1. **Search query with special characters** — If the user enters special regex characters (e.g., `.`, `*`, `(`, `)`) in the search field, the input must be sanitized/escaped before being used in the MongoDB regex query to prevent regex injection errors. The search should treat these as literal characters.

2. **Rapid filter changes** — If the user rapidly changes filters (e.g., toggling between status values), each filter change resets the cursor and triggers a new API call. Previous pending API calls should be cancelled (using AbortController) to prevent race conditions where stale results overwrite newer filter results.

3. **Filter state persistence across navigation** — If the user applies filters, navigates to a project detail view (BSABANKSTA-1574), and then returns to the dashboard via browser back button, the filter state should be preserved and the filtered view restored. This requires URL parameter synchronization and/or client-side state caching.

4. **Extremely narrow filter producing zero results** — When a combined filter (e.g., status="Overdue" AND search="XYZ Corp") returns zero results, the "no results" empty state must be displayed with a clear suggestion to adjust filters. The "Clear All Filters" action must be prominently displayed to help the user recover.

5. **Sort stability across cursor-based data traversal** — When sorting by a non-unique field (e.g., status), cursor-based traversal must use a compound cursor (sort field + document ID) to ensure deterministic ordering. Without a tiebreaker, projects with the same status could appear in different orders across pages, leading to duplicates or missing items.

---

## Dependencies

| Dependency | Type | Description |
|-----------|------|-------------|
| **BSABANKSTA-1573** (View My Projects Dashboard, this epic) | Internal Prerequisite | Filter controls compose on top of the existing card grid and infinite scroll data layer created by BSABANKSTA-1573. The `useDashboardProjects` hook provides the data fetching foundation. |
| **BSABANKSTA-1574** (Navigate Project Space Details, this epic) | Related Story | Filter state must persist across card navigation and back navigation. |
| **BSABANKSTA-1305** (Create/Modify Project Space, Batch 1) | Data Source | Filter criteria (status, dates, name) operate on project space data produced by F-001. The available status values and searchable fields depend on the F-001 data model. |
| **BSABANKSTA-1531** (Application Frame and Global Navigation) | URL Integration | Filter parameters are reflected in the URL, which is managed within the Application Frame's routing configuration. |
| **CC-RQ-001** (Cross-Cutting Requirement) | UI Pattern | Filtered results continue to use lazy load / infinite scroll. No pagination introduced by filtering. |
| **Auth0** (External Service) | Authentication | Filtered queries must still respect user-specific data access — filters narrow within the user's assigned projects, never expose other users' projects. |

---

## Story Estimation Guidance

**Story Points: 8** (Fibonacci)

**Rationale:**
- **Complexity (High):** This story involves multiple interacting UI controls (status dropdown, search input with debounce, sort selector with direction toggle, clear all button, active filter indicators), server-side query extension with filter/sort/search parameters, cursor stability under filtered result sets, URL parameter synchronization for bookmarkability, ARIA live region for accessibility, and integration with the existing infinite scroll data layer.
- **Uncertainty (Moderate):** The exact filter criteria and UI layout depend on the Figma frames and Jira requirements. Cursor stability under filtered + sorted queries requires careful implementation. URL parameter synchronization adds cross-cutting complexity.
- **Effort (High):** Requires backend API extension (4 new parameters, query builder, validation), 7+ new React components (filter toolbar, dropdowns, search, indicators), custom hook for filter state management, debouncing, URL sync, ARIA announcements, and comprehensive testing across all layers.

---

## Refinement Notes

### Global Rule #3 — Verb-Noun Title Standardization
- **Original Title:** "Dashboard Filtering and Sorting" / "Project Search and Filter"
- **Standardized Title:** "Filter Dashboard Projects"
- **Rationale:** Standardized to Verb-Noun format with "Filter" as the primary action verb and "Dashboard Projects" as the noun target. "Filter" encompasses the broader capability including sorting and searching as sub-functions of narrowing the dashboard view.

### Global Rule #4 — Pagination to Lazy Load Override
- **Override Confirmed:** Filtered results continue to use cursor-based infinite scroll per CC-RQ-001. When filters are applied, the cursor resets and new filtered results load via infinite scroll. No traditional pagination controls are introduced by the filtering feature.
- **Documentation:** AC7 explicitly specifies infinite scroll behavior with active filters.

### Global Rule #5 — Jira Source of Truth
- Jira requirement text was used as the authoritative source for filter capabilities. The Figma frames were reviewed for visual guidance on filter control placement. Any filter-related UI elements in Figma that are not mentioned in Jira requirements are flagged in the Discrepancy Review section.

### Global Rule #6 — NFR Elevation
- Performance targets extracted: < 1 second filter response, 300ms search debounce, < 100ms filter control render, < 2 seconds filtered initial load. Accessibility NFRs for keyboard navigation and ARIA announcements elevated to the dedicated NFR section.

### Global Rule #8 — Generated UI Specifications
- Filter toolbar controls (status dropdown, search input, sort selector, clear all button, active filter indicators, no results state) may not be fully depicted in the 3 Figma frames for the dashboard. Generated UI Specifications are provided below for these elements using the three-tier token priority hierarchy.

---

## Discrepancy Review

| Figma Frame | Node ID | Review Result |
|-------------|---------|---------------|
| My Projects Dashboard — Screen 1 | `7646-212204` | Reviewed for filter controls. If filter toolbar is present in Figma but details differ from Jira requirements, Jira takes precedence. |
| My Projects Dashboard — Screen 2 | `7646-213105` | Reviewed for filter state variations. |
| My Projects Dashboard — Screen 3 | `7646-211031` | Reviewed for additional filter views or states. |

**Discrepancy Findings:**
- If Figma frames depict pagination controls in any filtered view, these are **excluded** per Global Rule #4. All filtered views use cursor-based infinite scroll.
- If Figma frames show filter options not mentioned in Jira requirements, these are **flagged** per Global Rule #5 and excluded from acceptance criteria.

No critical discrepancies identified between Figma wireframes and Jira requirements for filtering functionality.

---

## Generated UI Specifications

> **DESIGN REVIEW REQUIRED:** The following UI specifications were automatically generated based on the existing design system tokens, as no explicit mockup was provided for these elements. Please review for accuracy and design intent before development.

### Dashboard Filter Toolbar (AC1-AC6)

| UI Element | Design Token / Attribute | Value | Rationale |
|-----------|------------------------|-------|-----------|
| Toolbar Container | `layout`, `spacing`, `border`, `margin` | `flex flex-wrap items-center gap-3 pb-4 mb-6 border-b border-gray-200` | Priority 2 — Semantic toolbar layout with bottom border separator |
| Toolbar Background | `background` | `bg-transparent` (inherits page background) | Priority 2 — No distinct toolbar background |

### Status Filter Dropdown (AC1)

| UI Element | Design Token / Attribute | Value | Rationale |
|-----------|------------------------|-------|-----------|
| Dropdown Container | `background`, `border`, `border-radius`, `padding`, `font-size` | `bg-white border border-gray-300 rounded-md shadow-sm px-3 py-2 text-sm` | Priority 2 — Semantic form control |
| Dropdown Focus | `ring` | `focus:ring-2 focus:ring-blue-500 focus:ring-offset-1` | Priority 2 — Semantic focus state |
| Dropdown Label | `font-size`, `font-weight`, `color`, `margin` | `text-xs font-medium text-gray-700 mb-1` | Priority 2 — Semantic label |
| Active Filter Badge | `background`, `color`, `border-radius`, `padding` | `bg-blue-100 text-blue-800 rounded-full px-2 py-0.5 text-xs font-medium ml-2` | Priority 2 — Semantic active indicator |

### Search Input (AC3)

| UI Element | Design Token / Attribute | Value | Rationale |
|-----------|------------------------|-------|-----------|
| Input Container | `position`, `width` | `relative w-64` | Priority 2 — Semantic input sizing |
| Search Icon | `position`, `color`, `size` | `absolute left-3 top-1/2 -translate-y-1/2 text-gray-400 h-4 w-4` | Priority 2 — Semantic search icon |
| Input Field | `background`, `border`, `border-radius`, `padding`, `font-size` | `bg-white border border-gray-300 rounded-md shadow-sm pl-10 pr-8 py-2 text-sm` | Priority 2 — Semantic form input with icon spacing |
| Input Focus | `ring` | `focus:ring-2 focus:ring-blue-500 focus:ring-offset-1` | Priority 2 — Semantic focus ring |
| Clear Button | `position`, `color`, `hover` | `absolute right-2 top-1/2 -translate-y-1/2 text-gray-400 hover:text-gray-600 h-4 w-4` | Priority 2 — Semantic clear action |
| Placeholder Text | `color` | `placeholder:text-gray-400` — "Search projects..." | Priority 2 — Semantic placeholder |

### Sort Selector (AC2)

| UI Element | Design Token / Attribute | Value | Rationale |
|-----------|------------------------|-------|-----------|
| Sort Dropdown | `background`, `border`, `border-radius`, `padding`, `font-size` | `bg-white border border-gray-300 rounded-md shadow-sm px-3 py-2 text-sm` | Priority 2 — Matches status dropdown styling |
| Sort Direction Toggle | `background`, `border`, `border-radius`, `padding`, `cursor` | `bg-white border border-gray-300 rounded-md px-2 py-2 cursor-pointer hover:bg-gray-50` | Priority 2 — Interactive toggle button |
| Sort Direction Icon | `size`, `transition` | `h-4 w-4 transition-transform duration-150` — rotated 180° for ascending | Priority 2 — Semantic directional indicator |

### Clear All Filters Button (AC5)

| UI Element | Design Token / Attribute | Value | Rationale |
|-----------|------------------------|-------|-----------|
| Button | `color`, `hover`, `font-size`, `font-weight` | `text-blue-600 hover:text-blue-800 text-sm font-medium` | Priority 2 — Semantic text link button |
| Button Visibility | `display` | Visible only when at least one filter is active | Priority 2 — Conditional rendering |

### No Results State (AC8)

| UI Element | Design Token / Attribute | Value | Rationale |
|-----------|------------------------|-------|-----------|
| Container | `layout`, `alignment`, `padding`, `color` | `flex flex-col items-center justify-center py-12 text-gray-500` | Priority 1 — Reuse shared empty state pattern (slightly less padding than full empty state) |
| Icon | `size`, `color` | `h-12 w-12 text-gray-300 mb-3` | Priority 2 — Muted icon |
| Title | `font-size`, `font-weight`, `color` | `text-base font-medium text-gray-900 mb-1` | Priority 2 — Semantic heading |
| Subtitle | `font-size`, `color` | `text-sm text-gray-500 mb-4` — "Try adjusting your filters or search criteria" | Priority 2 — Semantic guidance text |
| Clear Filters Action | `color`, `font-weight` | `text-blue-600 hover:text-blue-800 text-sm font-medium` | Priority 2 — Matches Clear All Filters button |

---

## Figma Mockup Link

| Frame | Description | URL |
|-------|-------------|-----|
| My Projects Dashboard — Screen 1 | Primary dashboard view — may show filter toolbar placement | [Figma Frame 7646-212204](https://www.figma.com/design/6fQyfvBUqImyavY8Fw47FV/BSA-Wireframes?node-id=7646-212204) |
| My Projects Dashboard — Screen 2 | Alternate state — may show filtered results or sort applied | [Figma Frame 7646-213105](https://www.figma.com/design/6fQyfvBUqImyavY8Fw47FV/BSA-Wireframes?node-id=7646-213105) |
| My Projects Dashboard — Screen 3 | Additional view — may show search or filter active state | [Figma Frame 7646-211031](https://www.figma.com/design/6fQyfvBUqImyavY8Fw47FV/BSA-Wireframes?node-id=7646-211031) |

---

## Definition of Done (Story-Level)

- [ ] All 9 acceptance criteria pass BDD validation
- [ ] Unit tests written and passing (pytest for API, Vitest for components)
- [ ] Component tests written and passing (@testing-library/react)
- [ ] E2E tests written and passing (Playwright)
- [ ] BDD tests written and passing (behave)
- [ ] Code reviewed and approved
- [ ] Status filter correctly narrows project cards to matching status
- [ ] Sort controls correctly reorder cards by selected field and direction
- [ ] Search input correctly filters by project name with 300ms debounce
- [ ] Combined filters work correctly (AND logic)
- [ ] Clear All Filters restores the full unfiltered dashboard view
- [ ] Filter state reflected in URL query parameters (bookmarkable)
- [ ] Infinite scroll works correctly with active filters
- [ ] No results state displays with filter adjustment guidance
- [ ] ARIA live region announces filter result changes
- [ ] Keyboard navigation through all filter controls verified
- [ ] WCAG 2.1 AA accessibility compliance verified
- [ ] NFRs validated (< 1s filter response, 300ms debounce, < 2s filtered load)
- [ ] Design review completed against Figma frames
- [ ] Generated UI Specifications reviewed by design team (DESIGN REVIEW REQUIRED)
- [ ] Cross-epic dependency links verified bidirectionally
- [ ] Documentation updated
