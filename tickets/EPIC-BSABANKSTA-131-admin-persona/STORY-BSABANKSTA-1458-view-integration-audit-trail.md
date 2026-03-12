# View Integration Audit Trail

**Story ID:** BSABANKSTA-1458
**Epic:** BSABANKSTA-131 — BSA Admin Persona
**Batch:** 2
**Original Jira Title:** Admin | Reporting | Integration Audit Trail

---

## 1. User Story

**As a** BSA Administrator,
**I want** to view a comprehensive integration audit trail that displays chronological event logs with filtering capabilities and event detail expansion,
**So that** I can monitor, investigate, and verify integration activities across the BSA Banking Confirmations system, ensuring compliance with audit requirements and enabling rapid investigation of integration issues or anomalies.

---

## 2. INVEST Validation

| Principle | Status | Validation Notes |
|-----------|--------|-----------------|
| **Independent** | ✅ Pass | Can be developed independently. This is a read-only log display requiring only the audit data source (append-only collection) and the admin navigation shell provided by the Application Frame (BSABANKSTA-1531). No write operations or cross-story state mutations. |
| **Negotiable** | ✅ Pass | Log display format, specific filter criteria, detail field layout, and column ordering are negotiable. The core non-negotiable requirement is chronological event visibility, filtering capability, and event detail expansion for compliance investigation. |
| **Valuable** | ✅ Pass | Provides audit-grade traceability for BSA/AML compliance — essential for regulatory reporting, integration monitoring, and rapid investigation of anomalies. Directly supports FinCEN and OFAC compliance obligations. |
| **Estimable** | ✅ Pass | 3 Figma frames provide clear visual direction for main audit log view, event detail expansion, and filtered/additional states. The data model (append-only read) and UI pattern (data table with infinite scroll) are well-understood. |
| **Small** | ✅ Pass | Single reporting page scoped to read-only event log display with filtering and detail expansion. No CRUD operations on audit data. Well-contained within a single admin reporting view. |
| **Testable** | ✅ Pass | All acceptance criteria are testable: event display correctness, chronological ordering, filter application, event detail expansion, infinite scroll behavior, RBAC enforcement, empty states, and error handling. |

---

## 3. Non-Functional Requirements

Performance targets extracted per Global Rule #6 (NFR Elevation):

| Category | Requirement | Target | Measurement |
|----------|-------------|--------|-------------|
| **Initial Load** | Audit log first meaningful paint | < 3 seconds | Time from navigation to first rendered audit events |
| **Infinite Scroll** | Next batch load time | < 1 second | Time from scroll threshold trigger to new events rendered |
| **Filter Response** | Filtered result display | < 1 second | Time from filter application to filtered results rendered |
| **Detail Expansion** | Event detail panel render | < 500ms | Time from row click/expand to full detail panel rendered |
| **API Response** | Audit data retrieval endpoint | < 2 seconds | Server response time for GET `/api/admin/audit-trail` |
| **Data Integrity** | Immutable audit records | Append-only | No modification or deletion of audit records permitted; storage is append-only |
| **Accessibility** | WCAG 2.1 AA compliance | Full compliance | Data tables, expandable detail panels, filter controls, keyboard navigation |
| **Security** | RBAC enforcement | BSA Administrator only | All page access and API endpoints validate BSA Administrator role; non-admin users receive 403 |
| **Responsiveness** | Desktop-first responsive layout | Desktop optimized | No mobile-specific layouts required per CC-RQ-003; responsive down to 1024px minimum |
| **Audit Compliance** | Timestamp precision | Timezone-aware | All event timestamps must include timezone information (ISO 8601 with offset); all events traceable to originating system and user |
| **Scalability** | Large dataset handling | Millions of records | Cursor-based data loading must maintain consistent performance regardless of total audit volume |

---

## 4. Acceptance Criteria

### AC1: Navigate to Integration Audit Trail

```gherkin
Scenario: BSA Administrator navigates to the Integration Audit Trail page
  Given the user is authenticated as a BSA Administrator
  And the user is within the Application Frame (BSABANKSTA-1531)
  And the user navigates to the Admin Reporting section
  When the user selects "Integration Audit Trail" from the reporting menu
  Then the Integration Audit Trail page is displayed
  And the page title indicates "Integration Audit Trail"
  And the chronological event log is rendered with the most recent events loaded first
  And the filter panel is visible with all available filter controls
```

### AC2: Chronological Event Display with Infinite Scroll

```gherkin
Scenario: Audit events are displayed in reverse chronological order with lazy loading
  Given the Integration Audit Trail page has loaded successfully
  When the initial data fetch completes
  Then audit events are displayed in reverse chronological order (newest first)
  And each event row displays a summary including timestamp, event type, source system, action, and target entity
  And data is loaded using cursor-based lazy loading (NOT pagination — Global Rule #4 override)
  And the event log container uses an overflow-auto scrollable area with an Intersection Observer trigger row at the bottom
```

### AC3: Infinite Scroll — Load More Events

```gherkin
Scenario: Additional audit events load automatically as user scrolls
  Given the user is viewing the Integration Audit Trail
  And there are more audit events beyond the currently loaded batch
  When the user scrolls to the bottom of the currently loaded audit events
  And the Intersection Observer detects the scroll threshold element
  Then the next batch of audit events is automatically fetched using the cursor from the previous response
  And a loading indicator is displayed at the bottom of the log during the fetch
  And newly fetched events are appended below the existing events without disrupting scroll position
```

### AC4: End-of-List Indicator

```gherkin
Scenario: End-of-list indicator displays when all events are loaded
  Given the user is viewing the Integration Audit Trail
  And all available audit events matching the current filter criteria have been loaded
  When the user scrolls to the end of the event list
  Then an end-of-list indicator is displayed (e.g., "All audit events loaded" with a visual divider)
  And no further API calls are made for additional events
  And the Intersection Observer trigger is deactivated
```

### AC5: Event Detail Expansion

```gherkin
Scenario: User expands an audit event row to view full details
  Given the audit event log is displayed with event rows
  When the user clicks on or expands an audit event row
  Then the detailed information panel for that event is displayed inline below the row
  And the detail panel includes: full event payload, metadata, precise timestamp with timezone, source system identifier, user ID, action performed, target entity, entity type, and correlation ID
  And the detail panel renders within 500 milliseconds
  And the expanded row is visually distinguished from collapsed rows
  And clicking the same row again collapses the detail panel
```

### AC6: Filtering by Date Range

```gherkin
Scenario: BSA Administrator filters audit events by date range
  Given the Integration Audit Trail page is displayed
  And the filter panel is visible
  When the BSA Administrator selects a start date and an end date in the date range filter
  Then the audit log is filtered to display only events with timestamps within the specified date range (inclusive)
  And the data reloads from the API with the date range parameters applied
  And the cursor resets to fetch the first batch of filtered results
  And the applied filter is visually indicated in the filter panel
```

### AC7: Filtering by Event Type

```gherkin
Scenario: BSA Administrator filters audit events by event type
  Given the Integration Audit Trail page is displayed
  And the filter panel includes a dropdown or selector for event types
  When the BSA Administrator selects an event type filter (e.g., "Project Created", "User Updated", "Integration Sync")
  Then the audit log is filtered to show only events matching the selected event type
  And the data reloads from the API with the event type parameter applied
  And the applied filter is visually indicated in the filter panel
```

### AC8: Filtering by Source System/Integration

```gherkin
Scenario: BSA Administrator filters audit events by source system
  Given the Integration Audit Trail page is displayed
  And the filter panel includes a selector for source systems/integrations
  When the BSA Administrator selects a source system/integration filter
  Then the audit log displays only events originating from the selected source system
  And the data reloads from the API with the source system parameter applied
  And the applied filter is visually indicated in the filter panel
```

### AC9: Combined Filters

```gherkin
Scenario: BSA Administrator applies multiple filters simultaneously
  Given the BSA Administrator is viewing the Integration Audit Trail
  When the BSA Administrator applies multiple filters (e.g., date range AND event type AND source system)
  Then the audit log displays only events matching ALL applied filter criteria simultaneously
  And filters are composable — each filter can be applied or removed independently without affecting others
  And the cursor resets when any filter changes to fetch fresh filtered results
  And all active filters are visually displayed in the filter panel
```

### AC10: Clear All Filters

```gherkin
Scenario: BSA Administrator clears all applied filters
  Given one or more filters are currently applied to the audit trail
  And the filtered result set is displayed
  When the BSA Administrator clicks the "Clear Filters" or "Reset" button
  Then all applied filters are removed
  And the filter controls return to their default unselected states
  And the full unfiltered audit log is reloaded from the beginning (cursor reset)
  And the audit events display in reverse chronological order without any filter constraints
```

### AC11: Role-Based Access Control — Admin Only

```gherkin
Scenario: Non-admin user is denied access to Integration Audit Trail
  Given a user is authenticated with the BSA Analyst role (non-administrator)
  When the user attempts to navigate to the Integration Audit Trail page
  Then access to the page is denied
  And the user is shown an appropriate "Unauthorized" or "Access Denied" message
  And the API endpoint returns a 403 Forbidden response for any audit trail data requests from the non-admin user
```

### AC12: Empty State — No Matching Events

```gherkin
Scenario: No audit events match the current filter criteria
  Given the BSA Administrator has applied one or more filters to the audit trail
  And no audit events exist that match the combined filter criteria
  When the filtered results render
  Then a meaningful empty state is displayed in the event log area
  And the empty state message indicates no events were found for the current filters (e.g., "No audit events found matching the selected criteria")
  And the empty state follows the shared Empty State display pattern
  And the filter panel remains accessible so the user can modify or clear filters
```

### AC13: Error State with Retry

```gherkin
Scenario: API failure triggers error state with retry capability
  Given the BSA Administrator is viewing or loading the Integration Audit Trail
  When the API call to retrieve audit data fails (network error, server error, timeout)
  Then an error message is displayed in the event log area indicating the failure
  And a "Retry" button is presented to the user
  And the error state follows the shared Error State / Retry display pattern
  And clicking "Retry" re-initiates the failed API request
  And previously loaded events (if any) are preserved above the error indicator
```

---

## 5. Sub-Tasks

### 5.1 Model

| Sub-Task ID | Description | Technical Details |
|-------------|-------------|-------------------|
| M-1458-01 | Define MongoDB `audit_trail` collection schema | Append-only collection with fields: `event_id` (unique identifier), `event_type` (string enum), `source_system` (string), `timestamp` (ISODate with timezone), `user_id` (string), `action` (string), `target_entity` (string — entity identifier), `entity_type` (string — e.g., "project_space", "user", "system_message"), `payload` (object — full event data), `metadata` (object — contextual metadata), `correlation_id` (string — for request tracing) |
| M-1458-02 | Create Marshmallow serialization schema | `AuditEventSchema` for response serialization with fields matching collection schema; `AuditEventSummarySchema` for list view (excludes full payload); `AuditEventDetailSchema` for expanded detail view (includes full payload and metadata) |
| M-1458-03 | Define cursor-based pagination model | Cursor model using compound key of `timestamp` + `event_id` for deterministic ordering; `CursorSchema` with fields: `cursor` (encoded string), `limit` (integer, default 50, max 100); response includes `next_cursor` (string or null when end of data) |
| M-1458-04 | Define filter parameter model | `AuditFilterSchema` with fields: `date_start` (ISODate, optional), `date_end` (ISODate, optional), `event_type` (string, optional), `source_system` (string, optional); validation rules for date range consistency (start ≤ end) |
| M-1458-05 | Create compound indexes for query performance | Indexes: `{ timestamp: -1, event_id: -1 }` (primary cursor index), `{ event_type: 1, timestamp: -1 }` (filtered queries), `{ source_system: 1, timestamp: -1 }` (source filter queries), `{ timestamp: -1 }` (date range queries) |

### 5.2 API

| Sub-Task ID | Description | Technical Details |
|-------------|-------------|-------------------|
| A-1458-01 | Implement `GET /api/admin/audit-trail` endpoint | Flask blueprint route accepting cursor-based query params: `cursor` (string, optional), `limit` (int, default 50), `date_start` (ISO date, optional), `date_end` (ISO date, optional), `event_type` (string, optional), `source_system` (string, optional). Returns: `{ data: [AuditEventSummary], next_cursor: string|null, total_count: number, filters_applied: { date_start, date_end, event_type, source_system } }` |
| A-1458-02 | Implement `GET /api/admin/audit-trail/<event_id>` endpoint | Flask blueprint route returning full audit event detail by `event_id`. Returns: `{ data: AuditEventDetail }` with complete payload, metadata, and all audit fields. Returns 404 if event not found. |
| A-1458-03 | Implement `GET /api/admin/audit-trail/filters` endpoint | Flask blueprint route returning available filter options. Returns: `{ event_types: [string], source_systems: [string] }` — dynamically populated from distinct values in the `audit_trail` collection. |
| A-1458-04 | Implement RBAC middleware for admin-only access | Decorator or middleware enforcing BSA Administrator role for ALL `/api/admin/audit-trail/*` endpoints. Validates JWT token, extracts user role, returns 403 Forbidden for non-admin users. Integrates with Auth0 role claims. |
| A-1458-05 | Implement request validation and error handling | Input validation via Marshmallow schemas; standardized error responses (400 for invalid params, 403 for unauthorized, 404 for not found, 500 for server errors); request logging for audit compliance. |

### 5.3 Component

| Sub-Task ID | Description | Technical Details |
|-------------|-------------|-------------------|
| C-1458-01 | Implement `AuditTrailPage` page component | React page component serving as the container for the audit trail view. Integrates with React Router for `/admin/reporting/audit-trail` route. Composes `AuditFilterPanel`, `AuditEventLog`, and state/error handling components. TailwindCSS layout: `max-w-7xl mx-auto px-4 py-6`. |
| C-1458-02 | Implement `AuditEventLog` component | Scrollable event log container. TailwindCSS: `overflow-auto` container with defined max-height. Renders list of `AuditEventRow` components in reverse chronological order. Integrates `InfiniteScrollTrigger` at bottom. |
| C-1458-03 | Implement `AuditEventRow` component | Collapsible row displaying event summary: timestamp, event type badge, source system, action, target entity. Click/expand toggles `AuditEventDetail`. TailwindCSS: `border-b border-gray-200 hover:bg-gray-50 cursor-pointer transition-colors`. ARIA: `role="row"`, `aria-expanded` for expansion state. |
| C-1458-04 | Implement `AuditEventDetail` component | Expanded detail panel rendered inline below the parent row. Displays full event payload, metadata, correlation ID, precise timestamp with timezone, source system details, and user information. TailwindCSS: `bg-gray-50 p-4 border-l-4 border-blue-500`. Handles large payloads with collapsible JSON viewer. |
| C-1458-05 | Implement `AuditFilterPanel` component | Filter controls section: date range picker (start/end), event type dropdown, source system dropdown, "Clear Filters" button. TailwindCSS: `flex flex-wrap gap-4 p-4 bg-white rounded-lg shadow-sm border border-gray-200 mb-4`. Communicates filter changes to parent via callback props. |
| C-1458-06 | Implement `InfiniteScrollTrigger` component | Intersection Observer-based trigger element placed at the bottom of the event log. Uses `useRef` and `IntersectionObserver` API. When visible, calls `onLoadMore` callback. TailwindCSS: `h-1` (minimal height sentinel element). |
| C-1458-07 | Implement `EndOfListIndicator` component | Displayed when all events are loaded. TailwindCSS: `text-center py-4 text-gray-400` with a horizontal divider (`border-t border-gray-200`). Text: "All audit events loaded". |
| C-1458-08 | Implement `EmptyState` component (shared pattern) | Displayed when no events match filters. TailwindCSS: `flex flex-col items-center justify-center py-16 text-gray-500`. Includes icon, descriptive message, and suggestion to modify filters. |
| C-1458-09 | Implement `ErrorStateRetry` component (shared pattern) | Displayed on API failure. TailwindCSS: `bg-red-50 border border-red-200 rounded-md p-4`. Includes error message text and a "Retry" button styled with `bg-red-600 hover:bg-red-700 text-white px-4 py-2 rounded-md`. |
| C-1458-10 | Implement loading skeleton/indicator | Loading indicator shown during data fetch and infinite scroll loading. Skeleton rows for initial load; spinner/progress indicator for subsequent scroll loads. TailwindCSS: `animate-pulse bg-gray-200 rounded`. |

### 5.4 Logic

| Sub-Task ID | Description | Technical Details |
|-------------|-------------|-------------------|
| L-1458-01 | Implement RBAC enforcement logic | Client-side route guard checking BSA Administrator role from Auth0 token claims. Redirect non-admin users to unauthorized page. Server-side: Flask decorator validating role from JWT before processing any audit trail request. |
| L-1458-02 | Implement filter state management | React state management for composable filter criteria: `dateRange` (start, end), `eventType` (string or null), `sourceSystem` (string or null). State updates trigger data refetch with cursor reset. Filters are independently removable. Use `useReducer` or state management pattern for complex filter state. |
| L-1458-03 | Implement cursor-based infinite scroll data fetching | Custom React hook `useAuditTrailData` managing: current cursor, loading state, has-more flag, event data array, error state. Fetches data from API with current cursor and filter params. Appends new events to existing array. Sets `hasMore = false` when `next_cursor` is null. Cancels in-flight requests on filter change via `AbortController`. |
| L-1458-04 | Implement event detail expansion toggle | React state management for expanded row IDs. Toggle logic: click on row adds/removes row ID from expanded set. Only one row expanded at a time (accordion behavior) or multiple (based on UX decision from Figma). |
| L-1458-05 | Implement MongoDB aggregation pipeline | Server-side aggregation pipeline: `$match` stage for filter criteria (date range, event type, source system) + cursor position, `$sort` stage for `{ timestamp: -1, event_id: -1 }`, `$limit` stage for batch size + 1 (to detect hasMore). Efficiently handles compound cursor for deterministic pagination over large datasets. |
| L-1458-06 | Implement timestamp formatting with timezone | Utility function converting ISO 8601 timestamps to human-readable format with timezone offset display. Format: `YYYY-MM-DD HH:mm:ss TZ` (e.g., "2025-01-15 14:30:22 EST"). Uses `Intl.DateTimeFormat` for locale-aware formatting on the client. |
| L-1458-07 | Implement React Router integration | Route definition: `/admin/reporting/audit-trail` within the admin reporting section. Breadcrumb integration: Admin > Reporting > Integration Audit Trail. Navigation guard ensuring admin role before route activation. |
| L-1458-08 | Implement error handling and retry logic | Centralized error handling: API errors caught and displayed via `ErrorStateRetry` component. Retry logic re-invokes the last failed API call with the same parameters. `AbortController` cancels pending requests when filters change or component unmounts. Network timeout handling with configurable timeout duration. |

### 5.5 Testing

| Sub-Task ID | Description | Technical Details |
|-------------|-------------|-------------------|
| T-1458-01 | Unit tests — Audit trail API endpoints (pytest) | Test `GET /api/admin/audit-trail` with various cursor and filter combinations. Test `GET /api/admin/audit-trail/<event_id>` for existing and non-existing events. Test `GET /api/admin/audit-trail/filters` for dynamic filter options. Validate response structure, status codes, cursor behavior. |
| T-1458-02 | Unit tests — RBAC middleware (pytest) | Test admin user receives 200 for all endpoints. Test non-admin user (BSA Analyst) receives 403 for all endpoints. Test unauthenticated user receives 401. Test expired/invalid tokens. |
| T-1458-03 | Unit tests — MongoDB query builder (pytest) | Test query construction with individual filters (date range, event type, source system). Test combined filter queries. Test cursor-based pagination with compound cursor. Test sort order (reverse chronological). Test edge cases: empty filters, invalid date ranges, non-existent event types. |
| T-1458-04 | Component tests — AuditTrailPage (@testing-library/react) | Test page renders with event log and filter panel. Test loading state displays skeleton. Test error state displays retry component. Test empty state displays when no events. |
| T-1458-05 | Component tests — AuditEventLog and AuditEventRow (@testing-library/react) | Test event rows render with correct summary data. Test row expansion toggles detail panel. Test detail panel displays full event information. Test ARIA attributes for accessibility. Test keyboard navigation (Enter/Space to expand). |
| T-1458-06 | Component tests — AuditFilterPanel (@testing-library/react) | Test date range picker interaction. Test event type dropdown selection. Test source system filter selection. Test "Clear Filters" resets all filter states. Test filter change callbacks fire correctly. |
| T-1458-07 | BDD tests — Acceptance criteria validation (behave) | Implement Given/When/Then step definitions for all 13 acceptance criteria. Validate complete user flows: navigation, viewing, scrolling, filtering, clearing, expanding, error handling, RBAC. |
| T-1458-08 | E2E tests — Full audit trail flow (Playwright) | Test end-to-end: login as admin → navigate to audit trail → verify event display → scroll for more → expand event detail → apply date filter → apply event type filter → combine filters → clear filters → verify empty state → verify error retry. |
| T-1458-09 | Accessibility tests (Playwright + axe) | Test expandable rows have correct `aria-expanded` attributes. Test keyboard navigation through event rows. Test screen reader compatibility for data table. Test focus management on row expansion/collapse. Test color contrast for all text elements. |
| T-1458-10 | Performance test — Initial load validation | Measure time to first meaningful paint for audit trail page. Validate < 3 second target. Test with simulated large datasets (10,000+ events). Test infinite scroll batch load time < 1 second. |

---

## 6. Edge Cases

### Edge Case 1: Very Large Audit Volume

**Scenario:** The system has accumulated millions of audit events over extended operation.

**Expected Behavior:** Infinite scroll with cursor-based data loading must handle this gracefully with consistent performance. Cursor-based pagination prevents the offset-based performance degradation that occurs with traditional SKIP/LIMIT patterns on large collections. MongoDB compound indexes on `{ timestamp: -1, event_id: -1 }` ensure O(log n) seek time regardless of total audit volume. The UI should never attempt to load or count the full dataset — only the current visible batch plus the next cursor.

**Mitigation:** Document index optimization requirements in the Model sub-tasks. Implement data retention policies as a future consideration (out of scope for this story but noted for operational planning).

### Edge Case 2: Date Range Filter with No Events

**Scenario:** The BSA Administrator selects a date range where no audit events occurred (e.g., a future date range, or a historical gap in audit activity).

**Expected Behavior:** The empty state component displays with filter-specific messaging: "No audit events found for the selected date range." The filter panel remains accessible so the user can modify the date range or clear filters. No error is thrown — an empty result set is a valid state.

**Mitigation:** The API returns an empty `data` array with `next_cursor: null` and `total_count: 0`. The frontend renders the `EmptyState` component with contextual messaging based on applied filters.

### Edge Case 3: Network Interruption During Scroll

**Scenario:** The API call for the next batch of audit events fails mid-scroll due to a network interruption, server timeout, or transient error.

**Expected Behavior:** An inline error indicator with a "Retry" button is displayed at the bottom of the already-loaded events. All previously loaded events are preserved and remain visible above the error indicator. The user can click "Retry" to re-attempt fetching the failed batch using the same cursor position. Alternatively, the user can continue to interact with already-loaded events (expand details, etc.).

**Mitigation:** The `useAuditTrailData` hook maintains a separate error state for scroll loading vs. initial loading. The error is displayed inline rather than replacing the entire view. Retry re-uses the last successful cursor.

### Edge Case 4: Event Detail with Large Payload

**Scenario:** An audit event has an extremely large payload — for example, a bulk operation log containing hundreds of changed entities, or a full request/response body from a complex integration sync.

**Expected Behavior:** The `AuditEventDetail` component handles payload overflow gracefully. Large JSON payloads are rendered in a collapsible JSON viewer with syntax highlighting. Payloads exceeding a threshold (e.g., 10KB displayed) show a truncated preview with a "Show Full Payload" toggle. The detail panel does not cause layout overflow or horizontal scrolling of the parent container.

**Mitigation:** Implement a maximum render size for inline payload display with a virtualized or lazy-rendered JSON tree for very large payloads. Consider server-side payload truncation with a "fetch full payload" API call for extreme cases.

### Edge Case 5: Rapid Filter Changes

**Scenario:** The BSA Administrator rapidly changes filter criteria — for example, selecting different event types or adjusting date ranges in quick succession while previous API requests are still in flight.

**Expected Behavior:** Each filter change cancels any pending in-flight API request using `AbortController`. The data resets and reloads with the latest filter parameters. No stale data from a previous filter request is displayed. The cursor resets to the beginning on each filter change. Only the most recent filter state is reflected in the displayed results.

**Mitigation:** Implement request cancellation via `AbortController` in the `useAuditTrailData` hook. Debounce rapid filter changes (e.g., 300ms debounce on date range input) to minimize unnecessary API calls. The loading indicator displays during the transition between filter states.

---

## 7. Dependencies

### Cross-Epic Dependencies

| Dependency | Epic | Type | Description |
|-----------|------|------|-------------|
| **BSABANKSTA-1305** (Batch 1 — Create/Modify Project Space) | F-001 | **Data Dependency (Producer → Consumer)** | Audit trail logs project space lifecycle events generated by F-001. Audit records reference project space entities created, modified, and managed by F-001. F-001 produces entity lifecycle events → the audit trail consumes and displays them. This is the primary data source for project-related audit entries. |
| **BSABANKSTA-131** (BSA Admin Persona) | F-002 | **Parent Epic** | This story (BSABANKSTA-1458) is a child of the BSA Admin Persona epic. The Integration Audit Trail is an admin-only reporting capability within the admin persona's feature set. |
| **BSABANKSTA-1531** (Application Frame and Global Navigation) | F-005 | **Navigation Dependency** | The Audit Trail page is accessed via admin navigation routing provided by the Application Frame. The admin reporting section within the Application Frame hosts the navigation entry point to this story. |
| **BSABANKSTA-1540** (Global Views and Reporting) | F-004 | **Related Reporting Capability** | Global Views and Reporting is a related reporting epic. While audit trail and global reporting serve different purposes (operational audit vs. business reporting), they share reporting UI patterns, navigation patterns within the admin/reporting area, and may surface complementary data. |
| **BSABANKSTA-1579** (Manage Global System Messages) | F-002 | **Audit Event Producer** | System message create/edit/delete actions generate audit events that appear in the audit trail. |
| **BSABANKSTA-1500** (Manage User Library) | F-002 | **Audit Event Producer** | User management actions (create, update, deactivate) generate audit events that appear in the audit trail. |

### Cross-Cutting Dependencies

| Dependency | Reference | Description |
|-----------|-----------|-------------|
| **Lazy Load / Infinite Scroll** | CC-RQ-001 | Audit event log implements the shared infinite scroll pattern using cursor-based data loading and Intersection Observer API. This is a mandatory Global Rule #4 override. |
| **Shared Empty State Pattern** | Cross-feature | Empty state display reuses the shared component pattern (`flex flex-col items-center justify-center py-16 text-gray-500`) defined across all list-view features. |
| **Shared Error State / Retry Pattern** | Cross-feature | Error state with retry button reuses the shared component pattern (`bg-red-50 border border-red-200 rounded-md p-4`) defined across all features. |
| **Shared End-of-List Indicator** | Cross-feature | End-of-list indicator reuses the shared pattern (`text-center py-4 text-gray-400` with divider) across all infinite scroll views. |
| **Auth0 Authentication** | Infrastructure | RBAC enforcement depends on Auth0 JWT tokens with role claims for BSA Administrator role validation. |

### Bidirectional Traceability

- **This story → BSABANKSTA-1305**: This story's audit event log displays lifecycle events produced by F-001 (Project Space Management). The dependency link must also be documented in BSABANKSTA-1305's Dependencies section (Batch 1 epic file).
- **This story → BSABANKSTA-1531**: Navigation routing is provided by F-005. The dependency link must also be documented in BSABANKSTA-1531's relevant navigation stories.

---

## 8. Story Estimation Guidance

**Estimated Story Points:** 8 (Fibonacci Scale)

| Factor | Assessment | Impact |
|--------|-----------|--------|
| **Complexity** | High | Multiple filter dimensions (date range, event type, source system) with composable and independently removable filters; event detail expansion with large payload handling; chronological reverse-order display with cursor-based infinite scroll; immutable append-only data model with compliance-grade integrity requirements |
| **Uncertainty** | Moderate | 3 Figma frames provide good visual direction for main view, detail expansion, and filtered state, reducing UI uncertainty. However, the audit data model complexity, MongoDB aggregation pipeline design for compound cursor pagination, and large-payload handling add technical uncertainty |
| **Effort** | Significant | Full-stack implementation spanning: MongoDB collection schema with compound indexes, 3 Flask API endpoints with RBAC middleware, 10+ React components including filter panel with date picker, infinite scroll with Intersection Observer, expandable detail rows with JSON viewer, plus comprehensive test coverage across all 5 testing categories |
| **Figma Coverage** | 3 frames | Adequate coverage for main audit log view, event detail expansion, and filtered/additional state — reduces design ambiguity |
| **Risk Factors** | Performance at scale | Audit trails can grow to millions of records; cursor-based pagination and index optimization are critical for meeting < 3s initial load and < 1s scroll targets |

**Rationale:** 8 points reflects the combination of complex multi-dimensional filtering UI, MongoDB aggregation pipeline design for efficient cursor-based pagination over potentially millions of append-only records, compliance-grade data handling requirements (immutable records, timezone-aware timestamps, full traceability), and the breadth of component implementation (10+ components with accessibility requirements for expandable data tables). The read-only nature of the data model (no CRUD operations on audit records) and 3 Figma frames providing clear visual direction prevent the estimate from reaching 13 points.

---

## 9. Refinement Notes

### Global Rule #3 — Verb-Noun Title Standardization

**Original Title:** "Admin | Reporting | Integration Audit Trail BSABANKSTA-1458"
**Standardized Title:** "View Integration Audit Trail"
**Rationale:** The Verb-Noun format was applied by selecting "View" as the primary action verb (this is a read-only reporting page) and "Integration Audit Trail" as the noun phrase. The organizational qualifiers "Admin | Reporting |" were removed from the title as they represent navigation hierarchy context metadata rather than the story's functional description. These qualifiers are preserved in the User Story and Dependencies sections.

### Global Rule #4 — Pagination to Lazy Load Override

**MANDATORY OVERRIDE APPLIED**

Any pagination pattern referenced in the source requirements for the audit event log has been replaced with lazy load / infinite scroll using cursor-based data loading and the Intersection Observer API. This override is critical for audit trails which can grow to millions of records — traditional page-number pagination becomes unusable at scale and provides a poor user experience for sequential log review.

**Implementation Pattern:**
- Cursor-based API: `GET /api/admin/audit-trail?cursor={encoded_cursor}&limit=50`
- Frontend: `InfiniteScrollTrigger` component using `IntersectionObserver`
- Data appending: New batches appended to existing event array without page transitions
- End detection: `next_cursor === null` triggers `EndOfListIndicator`

Documented per CC-RQ-001 (Cross-Cutting Requirement — Lazy Load / Infinite Scroll).

### Global Rule #5 — Jira Source of Truth

Jira requirement text ("Admin | Reporting | Integration Audit Trail") was used as the authoritative source of truth for acceptance criteria generation. All acceptance criteria were derived from Jira requirements. Any elements present in Figma wireframes but not supported by Jira requirement text are flagged in the Discrepancy Review section below.

### Global Rule #6 — NFR Elevation

Performance targets and non-functional requirements were extracted from acceptance criteria and elevated into the dedicated Non-Functional Requirements section (Section 3). Extracted NFRs include:
- < 3 second initial load time
- < 1 second infinite scroll batch load
- < 1 second filter application response
- < 500ms event detail expansion render
- < 2 second API response time
- Append-only data integrity
- WCAG 2.1 AA accessibility compliance
- RBAC security enforcement

### Global Rule #7 — Placeholder Management

No direct `[Application Name]` placeholder usage identified in the Integration Audit Trail story. However, audit event records may contain application name references within their event payload and metadata fields. The `[Application Name]` placeholder is cataloged at the epic level in `EPIC-BSABANKSTA-131-admin-persona.md` and applies to the Application Frame header visible on this page.

---

## 10. Discrepancy Review

### Figma Frame Analysis

The following 3 Figma frames were analyzed against Jira requirement text per Global Rule #5:

| Frame | Node ID | Figma Content | Jira Alignment |
|-------|---------|---------------|----------------|
| Integration Audit Trail — Screen 1 | `7408-96357` | Main audit log view with event rows, column headers, and filter area | ✅ Aligned — core audit log display matches Jira requirement for chronological event viewing |
| Integration Audit Trail — Screen 2 | `7437-87603` | Event detail expansion or filtered view showing expanded event information | ✅ Aligned — event detail expansion matches Jira requirement for drill-down investigation capability |
| Integration Audit Trail — Screen 3 | `7485-99556` | Additional audit state (potentially additional detail or alternate view) | ✅ Aligned — supplementary view supports Jira requirement scope |

### Pagination Control Check (Global Rule #4)

**IMPORTANT:** If any of the 3 Figma frames depict traditional pagination controls (page numbers, next/previous buttons, "Page X of Y" indicators), these UI elements are **excluded from acceptance criteria** and are overridden per Global Rule #4. All data loading is implemented as lazy load / infinite scroll with cursor-based fetching, regardless of what the wireframes show.

### Discrepancy Findings

No critical discrepancies identified between Figma wireframes and Jira requirements for the core audit trail functionality (event display, detail expansion, filtering). The following observations are noted:

- **Empty State Display**: Not explicitly depicted in any of the 3 Figma frames. Generated UI specification provided in Section 12.
- **End-of-List Indicator**: Not explicitly depicted in Figma. Generated UI specification provided in Section 12.
- **Error State / Retry**: Not explicitly depicted in Figma. Generated UI specification provided in Section 12.
- **Loading Indicator (Infinite Scroll)**: Not explicitly depicted in Figma. Generated UI specification provided in Section 12.

These are supplementary UI states required by the acceptance criteria that were not part of the Figma wireframe scope. They are handled via the Generated UI Specifications SOP (Global Rule #8) in Section 12.

---

## 11. Generated UI Specifications

> **DESIGN REVIEW REQUIRED:** The following UI specifications were automatically generated based on the existing design system tokens, as no explicit mockup was provided for these elements. Please review for accuracy and design intent before development.

The 3 Figma frames for this story cover the main audit log view, event detail expansion, and filtered/additional state. The following UI elements are required by the acceptance criteria but are not explicitly depicted in the wireframes. Specifications are generated using the three-tier token priority hierarchy.

### 11.1 Empty State Display (No Matching Events)

**Priority Level:** 1 — Reuse Similar Existing Component (shared Empty State pattern)

| UI Element | Design Token / Attribute | Value | Rationale |
|-----------|-------------------------|-------|-----------|
| Container | `display`, `flex-direction`, `align-items`, `justify-content` | `flex flex-col items-center justify-center` | Shared Empty State pattern — centered content layout consistent across all list views |
| Container padding | `padding-y` | `py-16` (4rem / 64px) | Standard vertical padding for empty state containers, provides adequate visual breathing room |
| Icon | `color`, `size` | `text-gray-400`, `w-12 h-12` (48px) | Muted icon color with sufficient size for visual prominence without overpowering |
| Heading text | `font-size`, `font-weight`, `color` | `text-lg font-semibold text-gray-500` | Legible heading for empty state message at appropriate emphasis level |
| Description text | `font-size`, `color` | `text-sm text-gray-400` | Secondary descriptive text: "No audit events found matching the selected criteria" |
| Action hint | `font-size`, `color` | `text-sm text-blue-600` | Optional link-styled text: "Try adjusting your filters" to guide user action |

### 11.2 End-of-List Indicator

**Priority Level:** 1 — Reuse Similar Existing Component (shared End-of-List pattern)

| UI Element | Design Token / Attribute | Value | Rationale |
|-----------|-------------------------|-------|-----------|
| Container | `text-align`, `padding-y` | `text-center py-4` (1rem / 16px) | Shared End-of-List pattern — centered indicator with compact vertical padding |
| Divider | `border-top` | `border-t border-gray-200` | Subtle visual separator indicating the boundary of loaded content |
| Text | `font-size`, `color` | `text-sm text-gray-400` | Muted text: "All audit events loaded" — low visual weight, informational only |
| Margin top | `margin-top` | `mt-2` (0.5rem / 8px) | Spacing between divider and text for visual clarity |

### 11.3 Error State with Retry

**Priority Level:** 1 — Reuse Similar Existing Component (shared Error State / Retry pattern)

| UI Element | Design Token / Attribute | Value | Rationale |
|-----------|-------------------------|-------|-----------|
| Container | `background`, `border`, `border-radius`, `padding` | `bg-red-50 border border-red-200 rounded-md p-4` | Shared Error State pattern — light red background with red border communicates error severity |
| Error icon | `color`, `size` | `text-red-500`, `w-5 h-5` (20px) | Semantic error color with compact icon size |
| Error heading | `font-weight`, `color` | `font-semibold text-red-800` | High-emphasis error heading for immediate visibility |
| Error message | `font-size`, `color` | `text-sm text-red-700` | Error description text: "Failed to load audit events. Please try again." |
| Retry button | `background`, `color`, `padding`, `border-radius`, `hover` | `bg-red-600 hover:bg-red-700 text-white px-4 py-2 rounded-md text-sm font-medium` | Semantic error-colored action button for retry. Hover state provides interactive feedback. |
| Button margin | `margin-top` | `mt-3` (0.75rem / 12px) | Spacing between error message and retry action |

### 11.4 Loading Indicator (Infinite Scroll Fetch)

**Priority Level:** 2 — Apply Semantic Tokens

| UI Element | Design Token / Attribute | Value | Rationale |
|-----------|-------------------------|-------|-----------|
| Container | `text-align`, `padding-y` | `text-center py-4` | Centered loading indicator at bottom of event log during fetch |
| Spinner | `animation`, `border`, `size` | `animate-spin rounded-full border-2 border-gray-300 border-t-blue-600 w-6 h-6` | Semantic loading spinner using primary brand color (blue-600) for the active segment and neutral gray for the track |
| Loading text | `font-size`, `color`, `margin-top` | `text-sm text-gray-500 mt-2` | Optional descriptive text: "Loading more events..." |
| Skeleton rows (initial load) | `background`, `animation`, `border-radius` | `bg-gray-200 animate-pulse rounded h-12 mb-2` | Pulsing skeleton rows matching event row height for initial page load state |
| Skeleton count | `quantity` | 8 rows | Sufficient skeleton rows to fill visible viewport during initial load |

### 11.5 Filter Clear / Reset Button

**Priority Level:** 2 — Apply Semantic Tokens

| UI Element | Design Token / Attribute | Value | Rationale |
|-----------|-------------------------|-------|-----------|
| Button | `background`, `color`, `border`, `padding`, `border-radius` | `bg-white text-gray-700 border border-gray-300 px-3 py-2 rounded-md text-sm font-medium` | Secondary/outline button style — visually distinct from primary actions, appropriate for a destructive-ish "reset" action |
| Hover state | `background`, `border-color` | `hover:bg-gray-50 hover:border-gray-400` | Subtle hover feedback |
| Icon (optional) | `size`, `color` | `w-4 h-4 text-gray-500` | Small "X" or reset icon preceding button text |
| Button text | Content | "Clear Filters" | Clear action label matching AC10 acceptance criteria |
| Disabled state | `opacity`, `cursor` | `opacity-50 cursor-not-allowed` | Disabled when no filters are applied |

---

## 12. Figma Mockup Link

The following Figma frames from the BSA Wireframes file (`6fQyfvBUqImyavY8Fw47FV`) correspond to this story:

| Frame | Description | Figma URL |
|-------|-------------|-----------|
| **Integration Audit Trail — Screen 1** | Main audit log view showing the chronological event log with column headers, event summary rows, and filter controls | [View in Figma](https://www.figma.com/design/6fQyfvBUqImyavY8Fw47FV/BSA-Wireframes?node-id=7408-96357) |
| **Integration Audit Trail — Screen 2** | Event detail expansion view showing an expanded audit event with full detail panel, or filtered view state | [View in Figma](https://www.figma.com/design/6fQyfvBUqImyavY8Fw47FV/BSA-Wireframes?node-id=7437-87603) |
| **Integration Audit Trail — Screen 3** | Additional audit detail state showing supplementary audit information or alternate view configuration | [View in Figma](https://www.figma.com/design/6fQyfvBUqImyavY8Fw47FV/BSA-Wireframes?node-id=7485-99556) |

---

## 13. Definition of Done

### Story-Level Completion Checklist

- [ ] All 13 acceptance criteria (AC1–AC13) pass BDD validation with automated tests
- [ ] Unit tests written and passing — pytest for API endpoints (`/api/admin/audit-trail`, `/<event_id>`, `/filters`)
- [ ] Unit tests written and passing — pytest for RBAC middleware (admin-only access enforcement)
- [ ] Unit tests written and passing — pytest for MongoDB query builder with all filter combinations
- [ ] Component tests written and passing — @testing-library/react for `AuditTrailPage`, `AuditEventLog`, `AuditEventRow`, `AuditEventDetail`, `AuditFilterPanel`
- [ ] E2E tests written and passing — Playwright for full audit trail flow (navigate, scroll, expand, filter, clear, error retry)
- [ ] BDD tests written and passing — behave step definitions for all Given/When/Then scenarios
- [ ] Accessibility tests passing — WCAG 2.1 AA compliance for data table, expandable rows, filter controls, keyboard navigation
- [ ] Code reviewed and approved by at least one peer reviewer
- [ ] Audit event log displays in reverse chronological order with infinite scroll (no pagination)
- [ ] Event detail expansion works correctly with < 500ms render time verified
- [ ] All filter types functional: date range, event type, source system — individually and in combination
- [ ] Filter clear/reset returns to full unfiltered view with cursor reset
- [ ] RBAC enforcement verified — only BSA Administrator role can access page and API endpoints
- [ ] Non-admin users (BSA Analyst) receive 403 Forbidden on API calls and redirect/block on frontend
- [ ] Empty state implemented and displays correctly when no events match filters
- [ ] Error state with retry implemented and functions correctly on API failure
- [ ] End-of-list indicator displays when all events are loaded
- [ ] Loading indicators display during initial load (skeleton) and infinite scroll (spinner)
- [ ] NFRs validated: < 3s initial load, < 1s scroll/filter, < 500ms detail expansion, < 2s API response
- [ ] Audit data integrity verified — records are append-only and immutable with timezone-aware timestamps
- [ ] Design review completed — implementation verified against all 3 Figma frames (node IDs: `7408-96357`, `7437-87603`, `7485-99556`)
- [ ] Cross-epic dependency links verified — BSABANKSTA-1305 data dependency documented and functional
- [ ] Cross-epic dependency links verified — BSABANKSTA-1531 navigation routing documented and functional
- [ ] Generated UI Specifications reviewed by design team (Empty State, End-of-List, Error State, Loading Indicator)
- [ ] All configurable placeholders resolved for deployment environment
- [ ] Documentation updated — story file, epic file, and dependency graph reflect final implementation
