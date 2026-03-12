# View All Confirmations

**Story ID:** BSABANKSTA-1536
**Epic:** BSABANKSTA-1531 — Application Frame and Global Navigation
**Batch:** 2

---

## User Story

**As a** BSA Analyst or BSA Administrator,
**I want** to view a unified, cross-project All Confirmations page with sortable columns and infinite scroll,
**So that** I can monitor and review all confirmation records across all projects in a single aggregated view, improving oversight efficiency and compliance monitoring.

---

## INVEST Validation

| Principle | Validation | Notes |
|-----------|------------|-------|
| **Independent** | ✅ Pass | The navigation shell and page container can be developed independently from the F-004 data aggregation layer. Routing and page rendering are decoupled from backend reporting logic. |
| **Negotiable** | ✅ Pass | Column configuration, sort options, and batch size for infinite scroll are negotiable implementation details. The core requirement is cross-project confirmation visibility in a single unified view. |
| **Valuable** | ✅ Pass | Provides organization-wide confirmation oversight critical for BSA/AML compliance monitoring. Enables analysts and administrators to review all confirmation records without switching between individual project spaces. |
| **Estimable** | ✅ Pass | 1 Figma frame provides clear visual direction for the table layout. The table-with-infinite-scroll pattern is well-understood, making estimation feasible. |
| **Small** | ✅ Pass | Single table-view page with sortable columns and infinite scroll — well-scoped to a single page with clearly defined data loading, sorting, and scroll behaviors. |
| **Testable** | ✅ Pass | Can verify data loading from multiple project spaces, column sorting (ascending/descending), infinite scroll triggering and appending, empty state, error state with retry, and role-based access filtering. |

---

## Non-Functional Requirements

| NFR ID | Category | Requirement | Target |
|--------|----------|-------------|--------|
| NFR-1536-01 | Performance | Initial page load to first meaningful paint | < 3 seconds |
| NFR-1536-02 | Performance | Next batch of records loads when scroll threshold is reached | < 1 second |
| NFR-1536-03 | Performance | Client-side sort operation on currently loaded data | < 500 milliseconds |
| NFR-1536-04 | Performance | API response time for confirmation data retrieval | < 2 seconds |
| NFR-1536-05 | Accessibility | WCAG 2.1 AA compliance for data tables | Proper `<th>`/`<td>` semantics, `aria-sort` attributes on sortable headers, screen reader announcements for sort changes and data loading |
| NFR-1536-06 | Responsiveness | Desktop-first responsive layout | No mobile-specific layouts per CC-RQ-003; responsive breakpoints for tablet and desktop |
| NFR-1536-07 | Data Freshness | Confirmation data reflects the latest state from all project spaces | Each new batch fetched during infinite scroll retrieves current data; stale data in previously loaded batches is acceptable |
| NFR-1536-08 | Scalability | Infinite scroll must handle large datasets without degrading performance | No memory leaks from accumulating DOM elements; consider virtualized rendering for very large datasets |

---

## Acceptance Criteria

### AC1: Navigate to All Confirmations Page

```gherkin
Scenario: User navigates to the All Confirmations page
  Given the user is authenticated and has access to the application
  And the global navigation or reporting menu is visible
  When the user clicks on the "All Confirmations" item in the global navigation or reporting menu
  Then the All Confirmations page is displayed
  And the confirmation data table is rendered with column headers
  And the page URL reflects the All Confirmations route
```

### AC2: Cross-Project Data Loading with Infinite Scroll

```gherkin
Scenario: Initial data load uses cursor-based lazy loading
  Given the user has navigated to the All Confirmations page
  When the page renders for the first time
  Then the initial batch of confirmation records from ALL accessible project spaces is displayed in the table
  And data is loaded using cursor-based lazy loading (NOT traditional pagination)
  And a loading indicator is shown while the initial batch is being fetched
  And the number of records in the initial batch matches the configured batch size
```

> **Global Rule #4 Override Applied:** Traditional pagination has been replaced with cursor-based lazy loading / infinite scroll per CC-RQ-001. See [Refinement Notes](#refinement-notes) for details.

### AC3: Infinite Scroll Trigger

```gherkin
Scenario: Next batch loads automatically when user scrolls to bottom
  Given the All Confirmations table is displaying the current batch of records
  And additional confirmation records exist beyond the current batch
  When the user scrolls to the bottom of the currently loaded records
  And the Intersection Observer detects the scroll threshold element
  Then the next batch of confirmation records is automatically fetched from the API
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
Scenario: User sorts table data by clicking a column header
  Given the All Confirmations table is displaying records
  And the table has sortable column headers with sort indicators
  When the user clicks on a sortable column header
  Then the table data is sorted by that column in ascending order
  And the active sort column header displays an ascending sort indicator
  And the aria-sort attribute on the active column is set to "ascending"

Scenario: User toggles sort direction by clicking the same column header again
  Given the All Confirmations table is sorted by a column in ascending order
  When the user clicks on the same column header again
  Then the sort direction toggles to descending order
  And the sort indicator updates to reflect descending order
  And the aria-sort attribute updates to "descending"

Scenario: User sorts by a different column
  Given the All Confirmations table is sorted by Column A
  When the user clicks on a different sortable column header (Column B)
  Then the table data is re-sorted by Column B in ascending order
  And Column A's sort indicator is cleared
  And Column B displays the active ascending sort indicator
  And the data is reloaded from the first batch with the new sort parameters
```

### AC6: Cross-Project Confirmation Aggregation

```gherkin
Scenario: Confirmation records are aggregated across all accessible project spaces
  Given the user has access to multiple project spaces
  When the All Confirmations page loads
  Then confirmation records are aggregated across ALL accessible project spaces
  And each confirmation record displays its originating project space name for context
  And records from different project spaces are interleaved based on the active sort order
```

### AC7: Empty State

```gherkin
Scenario: Empty state displays when no confirmation records exist
  Given the user has no project spaces assigned or no confirmation records exist across any accessible project space
  When the All Confirmations page loads
  Then a meaningful empty state message is displayed (e.g., "No confirmations found")
  And the empty state includes a descriptive subtitle and an appropriate icon
  And the empty state follows the shared Empty State component pattern
  And no error message is shown (this is a valid data state, not an error)
```

### AC8: Error State with Retry

```gherkin
Scenario: Error state with retry displays when API call fails
  Given the user is on the All Confirmations page
  And the API call to retrieve confirmation data fails due to a server or network error
  When the error is detected
  Then an error message is displayed describing the issue (e.g., "Unable to load confirmations")
  And a "Retry" button is displayed below the error message
  And the error state follows the shared Error State / Retry component pattern
  When the user clicks the "Retry" button
  Then the API call is retried
  And a loading indicator is shown during the retry attempt
```

### AC9: Role-Based Access Filtering

```gherkin
Scenario: BSA Analyst sees only confirmations for accessible project spaces
  Given a user with the BSA Analyst role is authenticated
  And the analyst has access to a subset of project spaces
  When the analyst navigates to the All Confirmations page
  Then only confirmation records from project spaces the analyst has access to are displayed
  And no records from restricted project spaces are visible

Scenario: BSA Administrator sees confirmations across all project spaces
  Given a user with the BSA Administrator role is authenticated
  When the administrator navigates to the All Confirmations page
  Then confirmation records from ALL project spaces in the system are displayed
```

---

## Sub-Tasks

### Model

| Sub-Task ID | Description | Technical Details |
|-------------|-------------|-------------------|
| ST-1536-M1 | MongoDB aggregation pipeline for cross-project confirmation data retrieval | Design a MongoDB aggregation pipeline that queries across project spaces, applying RBAC filters based on the authenticated user's accessible project space IDs. Include `$lookup` for project space name resolution. |
| ST-1536-M2 | Cursor-based pagination model for infinite scroll | Implement cursor token generation and parsing (e.g., base64-encoded compound key of sort field + `_id`). Define configurable batch size (default: 25 records). Support forward-only cursor traversal. |
| ST-1536-M3 | Marshmallow schema for confirmation list response | Define `ConfirmationListResponseSchema` with fields: `data` (list of confirmation objects including project space origin), `next_cursor` (string or null), `total_count` (integer). Include nested `ConfirmationItemSchema` with all table column fields. |
| ST-1536-M4 | Sort parameter model | Define `SortParameterSchema` with fields: `sort_by` (enum of sortable column names), `sort_direction` (enum: `asc`, `desc`). Validate against allowed sortable columns. |

### API

| Sub-Task ID | Description | Technical Details |
|-------------|-------------|-------------------|
| ST-1536-A1 | Flask blueprint route for `GET /api/confirmations` | Implement endpoint with query parameters: `cursor` (optional string), `limit` (optional integer, default 25, max 100), `sort_by` (optional string), `sort_direction` (optional string, default `desc`). Require authentication via JWT. |
| ST-1536-A2 | Response contract definition | Response JSON structure: `{ "data": [...], "next_cursor": "string|null", "total_count": number }`. Each item in `data` includes confirmation fields plus `project_space_id` and `project_space_name`. |
| ST-1536-A3 | Cross-project data aggregation endpoint | Implement server-side aggregation logic that merges confirmation records across all project spaces accessible to the authenticated user. Apply sort parameters to the aggregated dataset before cursor slicing. |
| ST-1536-A4 | RBAC middleware for endpoint | Apply role-based access control: BSA Administrator sees all project spaces; BSA Analyst sees only assigned project spaces. Inject accessible project space IDs into the aggregation query filter. |

### Component

| Sub-Task ID | Description | Technical Details |
|-------------|-------------|-------------------|
| ST-1536-C1 | React `AllConfirmationsPage` page component | Top-level page component with React Router route registration. Manages page-level state (loading, error, data). TailwindCSS layout: full-width container with header and table area. |
| ST-1536-C2 | `ConfirmationsTable` component | Data table with TailwindCSS: `table-auto w-full`. Sortable column headers with click handlers and `aria-sort` attributes. Column header sort indicators (ascending/descending icons). Renders confirmation rows with project space origin column. |
| ST-1536-C3 | `InfiniteScrollTrigger` component | Invisible sentinel element rendered after the last table row. Uses Intersection Observer API to detect when the element enters the viewport. Triggers data fetch callback. Disconnects observer when `next_cursor` is null. |
| ST-1536-C4 | `EndOfListIndicator` component | Rendered when all data is loaded (`next_cursor` is null and data is non-empty). TailwindCSS: `text-center py-4 text-gray-400` with a horizontal divider above. Displays "All confirmations loaded" message. |
| ST-1536-C5 | `EmptyState` component (shared) | Rendered when no confirmation records exist. TailwindCSS: `flex flex-col items-center justify-center py-16 text-gray-500`. Includes icon, heading, and subtitle. |
| ST-1536-C6 | `ErrorStateRetry` component (shared) | Rendered when API call fails. TailwindCSS: `bg-red-50 border border-red-200 rounded-md p-4`. Includes error message text and a "Retry" button styled with primary button tokens. |
| ST-1536-C7 | Column header sort indicators | Visual sort direction indicators (chevron up/down icons) within `<th>` elements. Applies `aria-sort="ascending"`, `aria-sort="descending"`, or `aria-sort="none"` attributes. |
| ST-1536-C8 | Loading skeleton/indicator | Loading indicator displayed during initial data fetch and during infinite scroll batch fetches. TailwindCSS: skeleton rows or spinner. Positioned inline within the table area. |

### Logic

| Sub-Task ID | Description | Technical Details |
|-------------|-------------|-------------------|
| ST-1536-L1 | Infinite scroll data fetching with cursor management | Custom React hook (`useInfiniteConfirmations`) managing cursor state, data accumulation, loading state, and error state. Handles fetch, append, and reset operations. Uses `AbortController` for fetch cancellation on sort change. |
| ST-1536-L2 | Client-side sort state management | Sort state (column + direction) managed via React state or URL search parameters. Sort change triggers: (1) cancel any pending fetch, (2) reset accumulated data, (3) refetch from first batch with new sort parameters. |
| ST-1536-L3 | Cross-project data merging and display logic | Frontend receives pre-merged data from API. Display logic maps each confirmation record to its project space name. Handles potential data shape variations across project spaces. |
| ST-1536-L4 | RBAC enforcement — client-side route guard | React Router route guard ensures only authenticated users access the All Confirmations page. Role-based filtering is enforced server-side; client renders whatever data the API returns. |
| ST-1536-L5 | Error handling and retry logic | Centralized error handling for API failures. Retry logic resets error state and re-invokes the last failed API call. Handles both initial load errors and infinite scroll batch errors differently (full-page error vs. inline error). |
| ST-1536-L6 | React Router integration | Register `/confirmations` or `/reporting/all-confirmations` route in the application router. Integrate with global navigation menu for route highlighting. |

### Testing

| Sub-Task ID | Description | Technical Details |
|-------------|-------------|-------------------|
| ST-1536-T1 | Unit tests (pytest) for confirmations API endpoint | Test cursor-based pagination logic, sort parameter validation, RBAC filtering (admin vs. analyst), empty result set, error responses, and boundary conditions (first page, last page, invalid cursor). |
| ST-1536-T2 | Unit tests (pytest) for MongoDB aggregation pipeline | Test cross-project aggregation, cursor generation/parsing, sort application, and RBAC filter injection. Mock MongoDB collections. |
| ST-1536-T3 | Component tests (@testing-library/react) for `AllConfirmationsPage` | Test initial data loading, table rendering, sort interaction, empty state rendering, error state rendering with retry, and loading state transitions. |
| ST-1536-T4 | Component tests (@testing-library/react) for `ConfirmationsTable` | Test column header rendering, sort click handling, `aria-sort` attribute management, and row rendering with project space origin. |
| ST-1536-T5 | Component tests (@testing-library/react) for `InfiniteScrollTrigger` | Test Intersection Observer setup, callback invocation on intersection, and observer disconnection when no more data. |
| ST-1536-T6 | BDD tests (behave) for acceptance criteria validation | Implement feature files mapping to all 9 acceptance criteria scenarios. Validate end-to-end behavior using Given/When/Then steps. |
| ST-1536-T7 | E2E tests (Playwright) for full page flow | Test complete user flow: navigation to page, initial data load, scroll-triggered batch loading, column sorting, empty state, error state with retry. Verify infinite scroll replaces pagination. |
| ST-1536-T8 | Performance test for initial load NFR | Verify < 3 second initial page load to first meaningful paint under representative data volume. Measure time-to-interactive and largest contentful paint. |

---

## Edge Cases

| # | Edge Case | Expected Behavior | Category |
|---|-----------|-------------------|----------|
| 1 | **Massive Dataset** — User has access to thousands of confirmation records across many project spaces | Infinite scroll handles gracefully with consistent performance. No memory leaks from accumulating DOM elements. Consider virtualized row rendering if DOM node count exceeds threshold. Loading indicators remain responsive. | Performance / Scalability |
| 2 | **Network Interruption During Scroll** — API call for next batch fails mid-scroll due to network loss | Display inline error message below the last loaded row with a retry option. Preserve all previously loaded records in the table. Do not clear or reset existing data. Resume loading from the same cursor position on retry. | Error Handling |
| 3 | **Concurrent Data Changes** — New confirmations are created or modified in another tab while user is viewing the page | Newly loaded batches via infinite scroll reflect the current database state. Previously loaded records maintain their position and content (eventual consistency model). No automatic refresh of already-rendered rows. | Data Consistency |
| 4 | **Zero Accessible Projects** — Authenticated user has no project space access permissions assigned | Display the shared Empty State component with an appropriate message (e.g., "No project spaces assigned. Contact your administrator for access."). Do not show an error state — this is a valid authorization state. | Authorization / Empty State |
| 5 | **Sort During Lazy Load** — User clicks a sort column header while a lazy load fetch is in progress | Cancel the pending fetch request via `AbortController`. Reset accumulated data. Reload from the first batch using the new sort parameters and cursor starting position. Display loading indicator during the fresh fetch. | User Interaction / Race Condition |

---

## Dependencies

### Cross-Epic Dependencies

| Dependency | Epic | Type | Description |
|-----------|------|------|-------------|
| **BSABANKSTA-1531** | Application Frame and Global Navigation (this epic) | Navigation Container | This page is accessed via global navigation routing provided by the Application Frame. The All Confirmations route is registered within the global navigation menu. |
| **BSABANKSTA-1540** | Global Views and Reporting | **Shared Surface (Critical)** | F-004 provides the data aggregation logic and reporting backend. This story (BSABANKSTA-1536) provides the navigation container and page shell. The backend confirmation aggregation API is conceptually owned by the Global Views and Reporting epic. |
| **BSABANKSTA-1305** | Create/Modify Project Space (Batch 1) | **Data Dependency** | All confirmation records originate from project spaces created and managed by F-001. The confirmation data model, project space entity structure, and RBAC project space assignments are established by this epic. |
| **BSABANKSTA-1572** | My Projects Dashboard | Related View | Users may navigate between the personalized My Projects Dashboard and the All Confirmations page for different data perspectives (project-specific vs. cross-project). |
| **BSABANKSTA-131** | BSA Admin Persona | Indirect | Administrator role definitions from this epic determine the RBAC rules that govern whether a user sees all confirmations (admin) or a filtered subset (analyst). |

### Cross-Cutting Dependencies

| Dependency | Reference | Description |
|-----------|-----------|-------------|
| Lazy Load / Infinite Scroll | CC-RQ-001 | Implements the shared infinite scroll pattern mandated across all list views in the application. Uses cursor-based data loading with Intersection Observer API. |
| Global Navigation Framework | F-005-RQ-002 | The All Confirmations page is a primary navigation destination within the global navigation framework. |
| Shared Empty State Component | Shared UI Component | Reuses the application-wide Empty State display pattern. |
| Shared Error State / Retry Component | Shared UI Component | Reuses the application-wide Error State with retry button pattern. |
| Shared End-of-List Indicator | Shared UI Component | Reuses the application-wide End-of-List indicator pattern for infinite scroll views. |
| Auth0 Authentication | External Service | Requires authenticated user session for RBAC enforcement and project space access determination. |

### Bidirectional Dependency Links

- **This story → BSABANKSTA-1540 stories**: This story's page shell depends on F-004 data aggregation logic
- **BSABANKSTA-1540 stories → This story**: F-004 reporting stories reference this page as the navigation surface for cross-project confirmation data
- **This story → BSABANKSTA-1305**: Data dependency on project space entities and confirmation records
- **BSABANKSTA-1305 → This story**: Batch 1 epic updated to reference this story's aggregation view in its workflow diagram

---

## Story Estimation Guidance

| Attribute | Value |
|-----------|-------|
| **Story Points** | **8** (Fibonacci) |
| **Complexity** | High |
| **Uncertainty** | Moderate |
| **Effort** | Significant |

### Rationale

- **High complexity**: Cross-project data aggregation via MongoDB pipeline, cursor-based infinite scroll implementation with Intersection Observer API, multi-column sortable table with `aria-sort` attributes, and three distinct UI states (empty, error, end-of-list) in addition to the primary data table.
- **Moderate uncertainty**: This is a shared surface between two epics (F-005 provides navigation and page shell; F-004 provides data aggregation logic). The interface contract between the page component and the reporting backend requires careful coordination. The exact data model for cross-project confirmation aggregation must be aligned with F-001 project space entity structures.
- **Significant effort**: Full table component with sortable headers, infinite scroll trigger with Intersection Observer, cursor management, error handling with retry logic, empty state, end-of-list indicator, loading skeletons, and RBAC enforcement. Backend requires MongoDB aggregation pipeline with cursor-based pagination and cross-project data merging.
- **Integration risk**: Data dependency on F-001 project space entities (BSABANKSTA-1305) and F-004 reporting data logic (BSABANKSTA-1540). Changes to project space data model or reporting aggregation logic directly impact this story.
- **8 points** reflects the combination of complex UI (infinite scroll table with sort), backend aggregation pipeline, cross-epic coordination across three epics, and comprehensive state management (loading, error, empty, end-of-list).

---

## Refinement Notes

### Global Rule #2 — Proactive Story Decomposition
- **Status:** No decomposition required.
- **Rationale:** This story contains 9 acceptance criteria (AC1–AC9) with a single user workflow (viewing cross-project confirmation data in a table with infinite scroll and sorting). The AC count is below the >10 threshold defined by Global Rule #2, and the story focuses on a single cohesive page interaction pattern (navigate → load → scroll → sort) without distinct multi-workflow branches (e.g., separate create/edit/delete flows). The story remains a well-scoped, single-page data viewing experience.

### Global Rule #3 — Verb-Noun Title Standardization
- **Original Jira Title:** "Reporting | All Confirmations Page BSABANKSTA-1536"
- **Standardized Title:** "View All Confirmations"
- **Rationale:** Applied Verb-Noun format. The prefix "Reporting |" is organizational metadata used for Jira categorization and has been removed from the story title. "All Confirmations Page" was transformed to "View All Confirmations" where "View" is the verb and "All Confirmations" is the noun phrase, accurately describing the user action.

### Global Rule #4 — Pagination to Lazy Load Override
- **Override Status:** ✅ **MANDATORY OVERRIDE APPLIED**
- **Details:** Any pagination pattern in the source requirements has been replaced with lazy load / infinite scroll using cursor-based data loading. The implementation specifies:
  - Cursor-based API pagination (cursor token + batch size) instead of page numbers
  - Intersection Observer API for detecting scroll threshold
  - Automatic next-batch fetching when the sentinel element enters the viewport
  - End-of-list indicator when `next_cursor` is null
- **Reference:** CC-RQ-001 — Global pagination-to-lazy-load override
- **Impact:** All acceptance criteria reference infinite scroll behavior. No "page number", "next page", "previous page", or "results per page" controls exist in this story.

### Global Rule #5 — Jira Source of Truth
- **Applied:** Jira requirement text was used as the authoritative source of truth for all acceptance criteria. Figma frame `8241-118805` was reviewed for visual reference only. Any UI elements present in Figma but absent from the Jira requirement are flagged in the [Discrepancy Review](#discrepancy-review) section and excluded from acceptance criteria.

### Global Rule #6 — NFR Elevation
- **Applied:** Performance targets have been extracted from inline requirement text into the dedicated [Non-Functional Requirements](#non-functional-requirements) section with specific measurable thresholds:
  - < 3s initial page load
  - < 1s infinite scroll batch load
  - < 500ms client-side sort operation
  - < 2s API response time
  - WCAG 2.1 AA accessibility compliance

### Global Rule #7 — Placeholder Management
- **Status:** No `[Application Name]` placeholder is directly used within this story's content or UI elements. However, the All Confirmations page header area may reference the application name via the persistent Application Header component (managed by the parent Application Frame epic BSABANKSTA-1531). Placeholder resolution is handled at the Application Header level, not within this story's scope.

### Global Rule #8 — Generated UI Specifications
- **Status:** Applied.
- **Assessment:** The All Confirmations Page has 1 Figma frame (`8241-118805`) covering the primary table layout. However, 4 undepicted UI element groups were identified during story processing that require Generated UI Specifications per the 4-step SOP:
  1. **Empty State Display** — No Figma frame exists for the zero-data state; specification generated using the shared Empty State pattern (Priority 1: Reuse Similar Existing Component).
  2. **End-of-List Indicator** — No Figma frame exists for the all-data-loaded state; specification generated using the shared End-of-List Indicator pattern (Priority 1: Reuse Similar Existing Component).
  3. **Error State with Retry** — No Figma frame exists for API failure states; specification generated using the shared Error State / Retry pattern (Priority 1: Reuse Similar Existing Component).
  4. **Loading Indicator (Infinite Scroll Batch Fetch)** — No Figma frame exists for the in-progress data loading state; specification generated using semantic loading tokens (Priority 2: Semantic Tokens).
- **Action:** All 4 undepicted element groups are documented in the [Generated UI Specifications](#generated-ui-specifications) section with the mandatory `DESIGN REVIEW REQUIRED` banner, three-tier token priority hierarchy, and design token tables.

---

## Discrepancy Review

### Figma vs. Jira Comparison for Frame `8241-118805`

The Figma wireframe at node `8241-118805` (All Confirmations Page) was reviewed against the Jira requirement text for BSABANKSTA-1536.

| Element | Present in Figma | Present in Jira | Status | Action |
|---------|-----------------|-----------------|--------|--------|
| Confirmation data table with columns | ✅ Yes | ✅ Yes | Aligned | Included in acceptance criteria |
| Sortable column headers | ✅ Yes | ✅ Yes | Aligned | Included in acceptance criteria |
| Cross-project data display | ✅ Yes | ✅ Yes | Aligned | Included in acceptance criteria |
| Traditional pagination controls (page numbers, next/prev) | ⚠️ May be present | ❌ Overridden | **Global Rule #4 Override** | **Excluded from acceptance criteria.** Any traditional pagination controls shown in the Figma wireframe are superseded by the mandatory lazy load / infinite scroll override per CC-RQ-001. |
| Empty state display | ❌ Not depicted | ✅ Required | Gap — Generated UI Spec | See [Generated UI Specifications](#generated-ui-specifications) |
| Error state with retry | ❌ Not depicted | ✅ Required | Gap — Generated UI Spec | See [Generated UI Specifications](#generated-ui-specifications) |
| End-of-list indicator | ❌ Not depicted | ✅ Required | Gap — Generated UI Spec | See [Generated UI Specifications](#generated-ui-specifications) |
| Loading indicator (infinite scroll) | ❌ Not depicted | ✅ Required | Gap — Generated UI Spec | See [Generated UI Specifications](#generated-ui-specifications) |

**Summary:** No critical Figma-only elements were identified that conflict with Jira requirements. The primary discrepancy is the potential presence of traditional pagination controls in the Figma wireframe, which are explicitly overridden per Global Rule #4. Four required UI states (empty, error, end-of-list, loading) are not depicted in the Figma frame and have Generated UI Specifications provided below.

---

## Generated UI Specifications

> **DESIGN REVIEW REQUIRED:** The following UI specifications were automatically generated based on the existing design system tokens, as no explicit mockup was provided for these elements. Please review for accuracy and design intent before development.

### Empty State Display

| UI Element | Design Token / Attribute | Value | Rationale |
|-----------|------------------------|-------|-----------|
| Container layout | `display`, `flex-direction`, `align-items`, `justify-content` | `flex flex-col items-center justify-center` | Centered column layout for empty state messaging — reuses shared Empty State pattern (Priority 1: Reuse Similar Existing Component) |
| Container padding | `padding-y` | `py-16` (64px vertical padding) | Provides generous whitespace to visually distinguish the empty state from a loading state |
| Text color | `color` | `text-gray-500` (#6B7280) | Subdued text color consistent with informational (non-error) messaging across the design system |
| Icon | Component | Placeholder icon (e.g., document outline or search icon) | Provides visual context for the empty state; specific icon to be confirmed during design review |
| Heading text | `font-size`, `font-weight` | `text-lg font-semibold` | Clear heading for the empty state message (e.g., "No confirmations found") |
| Subtitle text | `font-size`, `color` | `text-sm text-gray-400` | Secondary descriptive text (e.g., "There are no confirmation records in your accessible project spaces") |

### End-of-List Indicator

| UI Element | Design Token / Attribute | Value | Rationale |
|-----------|------------------------|-------|-----------|
| Container layout | `text-align`, `padding-y` | `text-center py-4` (16px vertical padding) | Centered text below the table — reuses shared End-of-List Indicator pattern (Priority 1: Reuse Similar Existing Component) |
| Divider | `border-top` | `border-t border-gray-200` | Subtle horizontal divider separating the indicator from table content |
| Text color | `color` | `text-gray-400` (#9CA3AF) | Muted text color indicating passive informational content |
| Text content | `font-size` | `text-sm` | Small text: "All confirmations loaded" or "You've reached the end" |

### Error State with Retry

| UI Element | Design Token / Attribute | Value | Rationale |
|-----------|------------------------|-------|-----------|
| Container background | `background-color` | `bg-red-50` (#FEF2F2) | Light red background signaling error state — reuses shared Error State / Retry pattern (Priority 1: Reuse Similar Existing Component) |
| Container border | `border`, `border-color` | `border border-red-200` (#FECACA) | Subtle red border reinforcing error state |
| Container shape | `border-radius`, `padding` | `rounded-md p-4` | Medium radius (6px) with 16px padding for consistent card-like appearance |
| Error icon | Component | Alert/warning triangle icon in `text-red-500` | Visual error indicator |
| Error message text | `color`, `font-weight` | `text-red-700 font-medium` | Clear error message (e.g., "Unable to load confirmations. Please try again.") |
| Retry button | Component | Primary button: `bg-red-600 hover:bg-red-700 text-white rounded-md px-4 py-2 text-sm font-medium` | Actionable retry mechanism following error; red variant for error context |

### Loading Indicator (Infinite Scroll Batch Fetch)

| UI Element | Design Token / Attribute | Value | Rationale |
|-----------|------------------------|-------|-----------|
| Container layout | `display`, `justify-content`, `padding-y` | `flex justify-center py-4` | Centered loading indicator below the table during batch fetch — applies semantic loading tokens (Priority 2: Semantic Tokens) |
| Spinner/skeleton | Component | Animated spinner or skeleton table rows | Visual feedback for in-progress data fetch; spinner for subsequent batches, skeleton rows for initial load |
| Spinner color | `color` | `text-blue-500` (#3B82F6) | Primary brand color for loading indicators consistent with interactive element styling |
| Spinner size | `width`, `height` | `w-6 h-6` (24px × 24px) | Appropriate size for inline loading context without dominating the table area |
| Loading text (optional) | `color`, `font-size` | `text-gray-400 text-sm` | Optional "Loading more confirmations..." text below spinner |

---

## Figma Mockup Link

| Frame | Description | URL |
|-------|-------------|-----|
| **All Confirmations Page** | Cross-project confirmation data table with sortable column headers, displaying aggregated confirmation records from multiple project spaces | [Figma Frame 8241-118805](https://www.figma.com/design/6fQyfvBUqImyavY8Fw47FV/BSA-Wireframes?node-id=8241-118805) |

---

## Definition of Done

- [ ] All acceptance criteria (AC1–AC9) pass BDD validation
- [ ] Unit tests written and passing (pytest for API endpoints, Vitest for React components)
- [ ] Component tests written and passing (@testing-library/react for AllConfirmationsPage, ConfirmationsTable, InfiniteScrollTrigger)
- [ ] E2E tests written and passing (Playwright for full page flow)
- [ ] BDD tests written and passing (behave for acceptance criteria scenarios)
- [ ] Code reviewed and approved by at least one peer
- [ ] Infinite scroll implementation verified — no traditional pagination controls exist (Global Rule #4)
- [ ] Cross-project data aggregation verified across multiple project spaces with correct RBAC filtering
- [ ] Sortable columns functional with visual sort indicators and correct `aria-sort` attributes
- [ ] Empty state component implemented and renders correctly when no data exists
- [ ] Error state with retry component implemented and handles API failures gracefully
- [ ] End-of-list indicator implemented and displays when all records are loaded
- [ ] WCAG 2.1 AA accessibility compliance verified for data table (semantic `<th>`/`<td>`, `aria-sort`, screen reader announcements)
- [ ] NFRs validated: < 3s initial load, < 1s scroll batch load, < 500ms client-side sort, < 2s API response
- [ ] Design review completed — implementation verified against Figma frame `8241-118805`
- [ ] Generated UI Specifications for empty state, error state, end-of-list indicator, and loading indicator reviewed by design team
- [ ] Cross-epic integration verified: F-004 data aggregation (BSABANKSTA-1540) + F-005 navigation (BSABANKSTA-1531)
- [ ] Cross-epic dependency links verified bidirectionally with BSABANKSTA-1540, BSABANKSTA-1305, and BSABANKSTA-1572
- [ ] Documentation updated in epic file's User Stories Index
