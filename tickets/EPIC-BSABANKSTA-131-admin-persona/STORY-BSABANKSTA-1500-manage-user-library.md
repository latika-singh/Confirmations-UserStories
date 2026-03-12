# Manage User Library

**Story ID:** BSABANKSTA-1500
**Epic:** BSABANKSTA-131 — BSA Admin Persona
**Batch:** 2
**Feature Reference:** F-002 — BSA Admin Persona

---

## User Story

**As a** BSA Administrator,
**I want** to view, search, and manage application users through a table-based user management library accessible via Admin Settings in the Application Header,
**So that** I can efficiently administer user accounts, manage access permissions, and maintain the user directory for BSA compliance operations, ensuring proper role assignments and account governance across the organization.

---

## INVEST Validation

| Principle | Validation | Notes |
|-----------|------------|-------|
| **Independent** | ✅ Pass | Can be developed independently of other admin stories. Requires the Application Header shell (BSABANKSTA-1531) for mounting the Admin Settings entry point, but the user management page itself is self-contained with its own route, data source, and API layer. |
| **Negotiable** | ✅ Pass | Table columns, search criteria, and specific management actions (e.g., role change options, activation toggle) are negotiable with stakeholders. The core requirement — providing administrators visibility into and control over the user directory — is fixed. |
| **Valuable** | ✅ Pass | Provides critical administrative capability for managing user access in BSA/AML compliance operations. Essential for role-based security governance, ensuring only authorized personnel hold appropriate system privileges. Directly supports regulatory audit requirements for access control documentation. |
| **Estimable** | ✅ Pass | 1 Figma frame (`7178-133386`) provides clear table layout and interaction patterns. The data table with infinite scroll is a well-understood UI pattern, and Auth0 Management API is a documented external service, reducing estimation uncertainty. |
| **Small** | ✅ Pass | Single page with user table, search bar, sortable columns, and management actions. Well-scoped for a single sprint delivery. Does not include user creation (Auth0-managed) or self-service profile editing (covered by BSABANKSTA-1532). |
| **Testable** | ✅ Pass | Clearly testable: user list rendering, search filtering, infinite scroll behavior, sortable columns, user management action outcomes (role change, activation/deactivation), RBAC enforcement (admin-only access), empty state, and error state can all be verified through automated BDD, unit, component, and E2E tests. |

---

## Non-Functional Requirements

| Category | Requirement | Target | Rationale |
|----------|-------------|--------|-----------|
| **Performance — Initial Load** | User list first meaningful paint | < 3 seconds | Ensures administrators experience responsive page loads when accessing user management |
| **Performance — Search** | Search query results display | < 1 second | Debounced search must return filtered results quickly for efficient user lookup |
| **Performance — Infinite Scroll** | Next batch load on scroll threshold | < 1 second | Seamless infinite scroll experience requires rapid data appending when the Intersection Observer triggers |
| **Performance — Sort/Filter** | Client-side sort and filter on loaded data | < 500ms | Sorting and filtering already-loaded table data should be near-instantaneous |
| **Performance — API Response** | User data retrieval API response | < 2 seconds | Backend response time for cursor-based user queries must be performant even with large user directories |
| **Accessibility** | WCAG 2.1 AA compliance for data tables | Full compliance | Proper `<th>`/`<td>` semantics, `aria-sort` attributes on sortable columns, screen reader announcements for data updates, keyboard-navigable table rows and action buttons |
| **Security** | RBAC enforcement | BSA Administrator only | All user management API endpoints must validate the BSA Administrator role via Auth0 JWT claims. Non-admin users must receive HTTP 403 responses. The Admin Settings navigation entry must be conditionally rendered — invisible to BSA Analyst users. |
| **Responsiveness** | Desktop-first responsive layout | No mobile-specific layouts | Per CC-RQ-003, the application targets desktop/responsive web only. User management table must be usable at standard desktop viewport widths (≥ 1024px). |
| **Reliability** | Error recovery | Graceful degradation with retry | All API failures must present user-friendly error messages with retry capability. Already-loaded data must be preserved during network interruptions. |

---

## Acceptance Criteria

### AC1: Navigate to User Management Library

```gherkin
Given the user is authenticated as a BSA Administrator
  And the Application Header displays the "Admin Settings" option
When the user clicks on "Admin Settings" and selects "User Management"
Then the User Management Library page is displayed
  And the page contains a data table for user records
  And a search bar is visible above the table
  And the page URL reflects the admin user management route
```

### AC2: User List Display with Infinite Scroll

```gherkin
Given the User Management Library page is loaded
When the initial data fetch completes successfully
Then the user list is displayed in a table format with columns including name, email, role, status, and last login
  And data is loaded using cursor-based lazy loading with an initial batch size
  And the table container uses an overflow-auto scrollable area with an Intersection Observer trigger row at the bottom
  And no traditional pagination controls (page numbers, next/previous buttons) are displayed
```

> **Global Rule #4 Override Applied:** Traditional pagination has been replaced with lazy load / infinite scroll using cursor-based data loading and the Intersection Observer API per CC-RQ-001.

### AC3: Infinite Scroll — Load More Users

```gherkin
Given the user has scrolled to the bottom of the currently loaded user records
When the Intersection Observer detects the scroll threshold trigger element
Then the next batch of user records is automatically fetched from the API using the current cursor token
  And the new records are appended to the existing table rows without replacing them
  And a loading indicator is displayed at the bottom of the table during the fetch
  And the scroll position is preserved after the new records are appended
```

### AC4: End-of-List Indicator

```gherkin
Given all available user records have been loaded via infinite scroll
When the user scrolls to the end of the complete dataset
Then an end-of-list indicator is displayed below the last table row
  And no further API calls are initiated
  And the Intersection Observer trigger element is removed or disabled
```

### AC5: User Search Functionality

```gherkin
Given the User Management Library page is displayed
When the BSA Administrator enters a search query in the search field
Then the search input applies a 300ms debounce before executing the query
  And the user list is filtered to show only records matching the query across relevant fields (name, email)
  And the filtered results load using the same cursor-based lazy load pattern
  And the cursor is reset to the beginning for the new search context
  And the search query is preserved in the search field
```

### AC6: Sortable Table Columns

```gherkin
Given the User Management Library page is displaying user records
When the BSA Administrator clicks on a sortable column header (e.g., name, email, role, status)
Then the table data is sorted by the selected column
  And a visual sort direction indicator (ascending/descending arrow) is displayed on the active column header
  And clicking the same column header again toggles the sort direction
  And the aria-sort attribute is updated on the sorted column for accessibility
```

### AC7: View User Details

```gherkin
Given the User Management Library page is displayed with user records
When the BSA Administrator clicks on a user record row or the "View" action button for a specific user
Then the user's detailed information is displayed, including full profile data, assigned role, account status, and last login activity
  And the detail view provides context for administrative actions
```

### AC8: Manage User Role

```gherkin
Given the BSA Administrator is viewing the user list or user detail
When the administrator triggers a role change action on a specific user (e.g., promote to BSA Administrator or assign BSA Analyst role)
Then a confirmation dialog is presented before the role change is applied
  And upon confirmation, the role change is persisted via the API
  And a success notification message is displayed
  And the user list reflects the updated role immediately
```

### AC9: Manage User Account Status

```gherkin
Given the BSA Administrator is viewing the user list or user detail
When the administrator triggers an activate or deactivate action on a specific user account
Then a confirmation dialog is presented before the status change is applied
  And upon confirmation, the account status change is persisted via the API and Auth0
  And a success notification message is displayed
  And the user list reflects the updated account status immediately
```

### AC10: Role-Based Access Control — Admin Only

```gherkin
Given a user with the BSA Analyst role (non-admin) is authenticated
When the user attempts to access the User Management Library page via direct URL navigation
Then access is denied with an appropriate HTTP 403 unauthorized response
  And the user is redirected to an authorized page or shown an access denied message
  And the "Admin Settings" option is NOT visible in the Application Header for this user
```

### AC11: Empty State — No Users Matching Search

```gherkin
Given no users match the current search query or filter criteria
When the User Management Library page renders the filtered result set
Then a meaningful empty state message is displayed in the table area (e.g., "No users found matching your search criteria")
  And the empty state follows the shared Empty State display pattern
  And the search field remains active for the user to modify their query
```

### AC12: Error State with Retry

```gherkin
Given the API call to retrieve user data fails due to a network error or server error
When the error is detected
Then an error message is displayed to the BSA Administrator with a clear description of the issue
  And a "Retry" button is presented to re-attempt the failed API call
  And the error state follows the shared Error State / Retry display pattern
  And any previously loaded data is preserved if the error occurs during an infinite scroll fetch
```

---

## Sub-Tasks

### Model

| ID | Task | Details |
|----|------|---------|
| M-1 | User data model integration with Auth0 | Define the user data model that integrates with Auth0 identity provider fields: `user_id`, `name`, `email`, `role` (BSA Administrator / BSA Analyst), `status` (active / inactive), `last_login`, `created_at`, `picture`, `email_verified`. Map Auth0 user metadata to application-level user attributes. |
| M-2 | MongoDB user metadata collection | Define MongoDB collection schema for `user_profiles` to store extended user metadata beyond Auth0 (e.g., department, notes, custom attributes). Fields: `auth0_user_id` (indexed), `department`, `notes`, `preferences`, `updated_at`, `updated_by`. |
| M-3 | Marshmallow serialization schemas | Create `UserListSchema` for list response serialization (subset of fields for table display) and `UserDetailSchema` for full detail view serialization. Include `CursorPaginatedResponseSchema` wrapping the user list with `data`, `next_cursor`, and `total_count` fields. |
| M-4 | Cursor-based pagination model | Define the cursor pagination model: cursor token (encoded `user_id` or `created_at` composite), `limit` (default 25, max 100), `search` query string, `sort_by` column name, `sort_direction` (asc/desc). Cursor encodes the last item's sort key for stateless pagination. |
| M-5 | Sort parameter model | Define allowed sort columns (`name`, `email`, `role`, `status`, `last_login`) and validation for sort direction (`asc`, `desc`). Default sort: `name` ascending. |

### API

| ID | Task | Details |
|----|------|---------|
| A-1 | GET `/api/admin/users` | Flask blueprint route for listing users with cursor-based query parameters: `cursor`, `limit`, `search`, `sort_by`, `sort_direction`. Returns `{ data: [UserListSchema], next_cursor: string \| null, total_count: number }`. Integrates with Auth0 Management API for user retrieval. |
| A-2 | GET `/api/admin/users/<user_id>` | Flask blueprint route for retrieving a single user's complete detail. Returns `UserDetailSchema` with full profile data, role, status, activity history, and extended metadata from MongoDB. |
| A-3 | PUT `/api/admin/users/<user_id>/role` | Flask blueprint route for changing a user's role. Request body: `{ role: "bsa_administrator" \| "bsa_analyst" }`. Validates against last-admin protection rule. Updates Auth0 user app_metadata and returns updated user object. |
| A-4 | PUT `/api/admin/users/<user_id>/status` | Flask blueprint route for activating or deactivating a user account. Request body: `{ status: "active" \| "inactive" }`. Updates Auth0 user blocked status and returns updated user object. Triggers Auth0 session invalidation for deactivated users. |
| A-5 | RBAC middleware | Implement admin-only authorization middleware that validates the BSA Administrator role from Auth0 JWT access token claims. Apply to ALL `/api/admin/users/*` routes. Return HTTP 403 for non-admin callers. |
| A-6 | Auth0 Management API client | Implement a server-side Auth0 Management API client for user data retrieval (List Users, Get User, Update User). Handle token caching for Management API access tokens. Implement retry logic for Auth0 API rate limits. |
| A-7 | Response contract | Define standardized response envelope: `{ data: [...], next_cursor: string \| null, total_count: number }` for list endpoints. Error responses: `{ error: string, message: string, status_code: number }`. |

### Component

| ID | Task | Details |
|----|------|---------|
| C-1 | `UserManagementPage` | React page component serving as the container for the user management view. Integrates React Router for `/admin/users` route. Orchestrates `UserSearchBar`, `UserTable`, and state management. |
| C-2 | `UserTable` | Data table component with TailwindCSS styling: `overflow-auto` scrollable container, `table-auto w-full` table element with sortable column headers. Columns: Name, Email, Role, Status, Last Login, Actions. Each column header is clickable for sorting with `aria-sort` attribute. |
| C-3 | `InfiniteScrollTrigger` | Reusable component using the Intersection Observer API to detect when the user scrolls near the bottom of the table. Triggers the `onLoadMore` callback to fetch the next cursor page. Includes a sentinel row element. |
| C-4 | `UserSearchBar` | Search input component with 300ms debounce logic. Styled with TailwindCSS input utilities. Emits debounced search query to parent for API filtering. Includes clear button and search icon. |
| C-5 | `UserActions` | Per-row action component providing role change and activate/deactivate buttons. Conditionally renders action options based on user state. Triggers confirmation dialog before destructive actions. |
| C-6 | `UserDetailPanel` | Component for displaying a user's full profile detail (accessed via row click or "View" action). Displays name, email, role, status, last login, created date, department, and other extended metadata. |
| C-7 | `EndOfListIndicator` | Shared component: `text-center py-4 text-gray-400` container with a horizontal divider and "All users loaded" text. Displayed when `next_cursor` is null. |
| C-8 | `EmptyState` | Shared component: `flex flex-col items-center justify-center py-16 text-gray-500` container with icon, heading, and descriptive message for zero-result scenarios. |
| C-9 | `ErrorStateRetry` | Shared component: `bg-red-50 border border-red-200 rounded-md p-4` container with error message text and a "Retry" button styled with primary action tokens. |
| C-10 | `ConfirmationDialog` | Modal confirmation dialog reusing the pattern from BSABANKSTA-1579: `bg-white rounded-lg shadow-xl` centered overlay with action prompt, "Confirm" and "Cancel" buttons. Used before role changes and status changes. |
| C-11 | Loading indicator | Skeleton rows or spinner component displayed during initial data fetch and infinite scroll loading. Positioned at the bottom of the table during scroll fetches. |

### Logic

| ID | Task | Details |
|----|------|---------|
| L-1 | RBAC enforcement (frontend) | Validate BSA Administrator role from Auth0 user context before rendering the Admin Settings navigation entry and the UserManagementPage. Redirect non-admin users attempting direct URL access. |
| L-2 | RBAC enforcement (backend) | Validate BSA Administrator role from Auth0 JWT access token on every API request to `/api/admin/users/*`. Extract role from `app_metadata` or custom claims. Return HTTP 403 for unauthorized requests. |
| L-3 | Auth0 Management API client logic | Implement the server-side client for Auth0 Management API operations: list users with search and pagination, get user by ID, update user roles (app_metadata), update user blocked status. Handle Management API token lifecycle (obtain, cache, refresh). |
| L-4 | Search debounce logic | Implement 300ms debounce on the search input using a `useDebounce` hook or equivalent. On debounced value change, reset the cursor, clear existing results, and fetch a fresh first page of filtered users. |
| L-5 | Cursor-based infinite scroll state | Manage cursor state: `currentCursor`, `hasMore`, `isLoading`, `users[]`. On `InfiniteScrollTrigger` callback, fetch the next page using `currentCursor`, append results to `users[]`, and update `currentCursor` to `next_cursor` from the response. Set `hasMore = false` when `next_cursor` is null. |
| L-6 | Client-side sort state | Manage sort state: `sortColumn`, `sortDirection`. On column header click, update sort state and re-fetch from the API with new sort parameters, resetting the cursor. |
| L-7 | User management actions | Implement role change and status change workflows: trigger confirmation dialog → on confirm, call PUT API → handle success (update local state optimistically, show success notification) → handle error (revert optimistic update, show error). |
| L-8 | Last-admin protection | Implement server-side validation: before processing a role downgrade from BSA Administrator, query the total count of active administrators. If the target user is the last remaining administrator, reject the request with a descriptive error. |
| L-9 | React Router integration | Configure admin routing: `/admin/users` maps to `UserManagementPage`. Implement route guards that check BSA Administrator role before rendering. |
| L-10 | Error handling and retry | Implement centralized error handling: catch API errors, display error state with retry button. On retry, re-execute the failed API call. Preserve already-loaded data during infinite scroll errors. |

### Testing

| ID | Task | Details |
|----|------|---------|
| T-1 | Unit tests — User API endpoints (pytest) | Test GET `/api/admin/users` with various cursor, search, and sort parameters. Test GET `/api/admin/users/<id>`. Test PUT role change and status change endpoints. Verify response contracts and status codes. |
| T-2 | Unit tests — RBAC middleware (pytest) | Test that all admin endpoints return HTTP 403 for non-admin JWT tokens. Test that admin tokens are accepted. Test missing/invalid token handling. |
| T-3 | Unit tests — Auth0 Management API client (pytest) | Test Auth0 API client methods: list users, get user, update user. Mock Auth0 responses. Test token caching and refresh logic. Test error handling for Auth0 API failures and rate limits. |
| T-4 | Unit tests — Last-admin protection (pytest) | Test that role downgrade of the last administrator is rejected. Test that role downgrade is allowed when multiple administrators exist. |
| T-5 | Component tests — UserManagementPage (@testing-library/react) | Test page renders with user table. Test search input triggers debounced API call. Test infinite scroll loads more data. Test empty state renders when no users. Test error state renders on API failure. |
| T-6 | Component tests — UserTable (@testing-library/react) | Test table renders correct columns and rows. Test sortable column headers update sort state. Test `aria-sort` attributes are correctly applied. |
| T-7 | Component tests — UserSearchBar (@testing-library/react) | Test debounce behavior (300ms delay). Test clear button resets search. Test search input accessibility. |
| T-8 | Component tests — InfiniteScrollTrigger (@testing-library/react) | Test Intersection Observer callback fires when trigger element is visible. Test loading indicator displays during fetch. |
| T-9 | BDD tests (behave) | Implement Gherkin scenarios for all 12 acceptance criteria. Feature file: `user_management.feature`. Step definitions mapping Given/When/Then to test actions. |
| T-10 | E2E tests (Playwright) | Full flow: authenticate as admin → navigate to Admin Settings → User Management → verify table displays → search for a user → scroll to load more → view user detail → change user role → verify list update. |
| T-11 | Accessibility tests | Verify table semantics (`<th>`, `<td>`, `<caption>`). Verify `aria-sort` on sorted columns. Verify keyboard navigation through table rows and action buttons. Verify screen reader announcements on data load and action completion. |
| T-12 | Performance tests | Verify initial page load < 3 seconds. Verify search response < 1 second. Verify infinite scroll fetch < 1 second. Measure and assert against NFR targets. |

---

## Edge Cases

### 1. Last Administrator Role Change Prevention

| Attribute | Detail |
|-----------|--------|
| **Scenario** | The BSA Administrator attempts to downgrade the role of the last remaining administrator user to BSA Analyst. |
| **Expected Behavior** | The system prevents the action at the API level. The server queries the total count of active BSA Administrator users. If the target user is the only remaining administrator, the API returns a 409 Conflict response with the message: "Cannot remove the last administrator. At least one BSA Administrator must exist." The UI displays this error to the administrator. The role change is not applied. |
| **Category** | Authorization / Business Rule |

### 2. Search Query with No Matching Results

| Attribute | Detail |
|-----------|--------|
| **Scenario** | The BSA Administrator enters a search query (e.g., "xyznonexistent") that matches no users in the directory. |
| **Expected Behavior** | The debounced search executes, the API returns an empty data array with `total_count: 0` and `next_cursor: null`. The UI displays the shared Empty State component with a context-specific message: "No users found matching 'xyznonexistent'." The search field remains active and editable so the administrator can refine the query. |
| **Category** | Empty State / Boundary Condition |

### 3. Network Interruption During Infinite Scroll

| Attribute | Detail |
|-----------|--------|
| **Scenario** | The API call to fetch the next batch of user records fails due to a network interruption while the administrator is scrolling through the user list. |
| **Expected Behavior** | All previously loaded user records remain visible and intact in the table. An inline error indicator appears at the bottom of the table (in the area where the next batch would load) with the message: "Failed to load more users." A "Retry" button is displayed. Clicking "Retry" re-attempts the fetch with the same cursor position. The Intersection Observer does not trigger additional requests until the retry succeeds or the user explicitly retries. |
| **Category** | Network Failure / Error Recovery |

### 4. Concurrent User Modification by Two Administrators

| Attribute | Detail |
|-----------|--------|
| **Scenario** | Two BSA Administrators attempt to change the same user's role simultaneously (e.g., Admin A changes User X to Analyst while Admin B changes User X to Administrator). |
| **Expected Behavior** | The system implements optimistic concurrency control using a version field or `updated_at` timestamp on the user record. The first request succeeds. The second request detects a version mismatch and returns a 409 Conflict response with the message: "This user's information has been modified by another administrator. Please refresh and try again." The second administrator's UI displays the conflict notification and refreshes the user data to show the current state. |
| **Category** | Concurrency / Data Integrity |

### 5. Deactivated User with Active Session

| Attribute | Detail |
|-----------|--------|
| **Scenario** | A BSA Administrator deactivates a user account while that user is currently logged in and actively using the application. |
| **Expected Behavior** | The deactivation API call sets the user's Auth0 `blocked` status to `true`. The deactivated user's current session continues until the next token refresh cycle (controlled by Auth0 token expiration settings). On the next token refresh attempt, Auth0 rejects the refresh because the user is blocked. The application detects the failed token refresh and logs the user out, redirecting them to the login page with a message indicating their account has been deactivated. The administrator sees a success confirmation immediately after the deactivation action. |
| **Category** | Session Management / Security |

---

## Dependencies

### Cross-Epic Dependencies

| Dependency | Epic | Type | Description |
|------------|------|------|-------------|
| **BSABANKSTA-1531** | Application Frame and Global Navigation | UI Container | The User Management Library is accessed via Admin Settings in the Application Header. The Application Header component, navigation routing, and admin menu structure are provided by BSABANKSTA-1531. |
| **BSABANKSTA-1532** | Application Frame and Global Navigation (BSABANKSTA-1531) | Shared Component — Mutual Dependency | The Admin Settings dropdown (containing User Management) and the Profile Dropdown (BSABANKSTA-1532) are sibling components within the shared Application Header bar. Dropdown mutual exclusion behavior must be coordinated — opening Admin Settings should close Profile Dropdown and vice versa. |
| **BSABANKSTA-1305** | Create/Modify Project Space (Batch 1) | Data Dependency | User management may affect user access to project spaces managed by BSABANKSTA-1305. Role changes and account deactivations impact what project spaces a user can access. User entities referenced across project space permissions originate from the user directory managed here. |
| **BSABANKSTA-1579** | Manage Global System Messages (Same Epic — BSABANKSTA-131) | Shared Navigation Context | Both User Management and System Messages share the admin navigation context within the Application Header's Admin Settings. Navigation between these admin features must be coordinated. |
| **BSABANKSTA-1458** | View Integration Audit Trail (Same Epic — BSABANKSTA-131) | Audit Logging | User management actions (role changes, account activation/deactivation) generate audit trail events that are displayed in the Integration Audit Trail. Audit event schema must accommodate user management action types. |

### External Dependencies

| Dependency | Type | Description |
|------------|------|-------------|
| **Auth0 Identity Provider** | External Service | User data is sourced from Auth0. The Auth0 Management API is used for listing users, retrieving user details, updating user roles (app_metadata), and blocking/unblocking user accounts. Auth0 token lifecycle management is required for Management API access. |
| **Auth0 Management API** | External API | Specific Auth0 Management API endpoints: `GET /api/v2/users`, `GET /api/v2/users/{id}`, `PATCH /api/v2/users/{id}`. Rate limits and quota management must be implemented. |

### Shared Pattern Dependencies

| Dependency | Reference | Description |
|------------|-----------|-------------|
| **Lazy Load / Infinite Scroll** | CC-RQ-001 | User list implements the shared infinite scroll pattern using cursor-based data loading and the Intersection Observer API. This pattern is shared across all list views in the application. |
| **Empty State Display** | Shared Component | Reuses the application-wide empty state pattern for zero-result scenarios. |
| **Error State / Retry** | Shared Component | Reuses the application-wide error state pattern with retry button for API failures. |
| **End-of-List Indicator** | Shared Component | Reuses the application-wide end-of-list indicator for completed infinite scroll datasets. |
| **Confirmation Dialog** | Shared Component (BSABANKSTA-1579 pattern) | Reuses the modal confirmation dialog pattern from the System Messages story for destructive user management actions. |

---

## Story Estimation Guidance

**Story Points:** 8 (Fibonacci Scale)

### Estimation Rationale

| Factor | Assessment | Impact |
|--------|------------|--------|
| **Complexity** | High | Data table with infinite scroll, debounced search, sortable columns with visual indicators, user management actions (role change, activate/deactivate) with confirmation dialogs, and Auth0 Management API integration for user data retrieval and mutation. Multiple interactive UI patterns in a single page. |
| **Uncertainty** | Moderate | Auth0 Management API integration introduces external system dependency with rate limits, token management, and potential API contract changes. Shared Application Header coordination with BSABANKSTA-1532 (Profile Dropdown) requires cross-team alignment on dropdown mutual exclusion behavior. |
| **Effort** | Significant | Full-stack implementation: Flask API with 4 endpoints + RBAC middleware, React page with 11+ components, Auth0 Management API client, cursor-based pagination logic, search debounce, sort state management, optimistic UI updates, confirmation dialogs, and comprehensive test coverage (unit, component, BDD, E2E, accessibility). |
| **Visual Clarity** | Moderate | 1 Figma frame reduces visual uncertainty for the primary table view, but undepicted states (empty, error, loading, detail view, confirmation dialog) require Generated UI Specifications. |
| **Risk** | Moderate | Auth0 Management API rate limits could affect user listing performance at scale. Last-admin protection rule adds business logic complexity. Concurrent modification handling requires optimistic locking. |

**Conclusion:** 8 story points reflects the combination of a complex interactive UI (infinite scroll table with search, sort, and management actions), significant backend work (Auth0 Management API integration, RBAC middleware, 4 API endpoints), and cross-component coordination (shared Application Header with BSABANKSTA-1532). The familiar data table pattern prevents escalation to 13 points.

---

## Refinement Notes

### Global Rule #3 — Verb-Noun Title Standardization

- **Original Jira Title:** "Application Header | Admin Settings | User Management Library BSABANKSTA-1500"
- **Standardized Title:** "Manage User Library"
- **Transformation Rationale:** Applied Verb-Noun format per Global Rule #3. The organizational qualifiers "Application Header | Admin Settings |" describe the navigation hierarchy and access path, not the story's functional capability. These qualifiers are preserved as context in the User Story and Dependencies sections. "User Management Library" was condensed to "User Library" to follow the Verb-Noun pattern, with "Manage" as the action verb capturing the full scope of view, search, and administrative management actions.

### Global Rule #4 — Pagination to Lazy Load Override

- **Override Applied:** ✅ MANDATORY OVERRIDE
- **Original Pattern:** Any references to traditional pagination (page numbers, next/previous controls) in the source requirements or Figma wireframe have been replaced.
- **Replacement Pattern:** Lazy load / infinite scroll using cursor-based data loading and the Intersection Observer API.
- **Technical Implementation:** The user list API returns a `next_cursor` token with each response batch. The frontend `InfiniteScrollTrigger` component uses the Intersection Observer API to detect scroll proximity to the sentinel row, triggering the next fetch with the cursor token. When `next_cursor` is null, the `EndOfListIndicator` is displayed and no further API calls are made.
- **Reference:** CC-RQ-001

### Global Rule #5 — Jira Source of Truth

- **Applied:** ✅ Jira requirement text is the authoritative source of truth for acceptance criteria.
- **Action:** Any UI elements present in the Figma wireframe (node `7178-133386`) but absent from the Jira requirement text have been excluded from acceptance criteria and flagged in the Discrepancy Review section below.

### Global Rule #6 — NFR Elevation

- **Applied:** ✅ Performance targets extracted to dedicated Non-Functional Requirements section.
- **Elevated Targets:** < 3s initial load, < 1s search response, < 1s infinite scroll load, < 500ms client-side sort/filter, < 2s API response.
- **Additional NFRs:** WCAG 2.1 AA accessibility, RBAC security enforcement, desktop-first responsiveness.

### Global Rule #7 — Placeholder Management

- **Assessment:** No direct `[Application Name]` placeholder usage exists within the User Management Library page content itself. However, the Application Header parent component (provided by BSABANKSTA-1531) may display `[Application Name]` in its branding area. Placeholder management for the admin context is documented at the epic level in `EPIC-BSABANKSTA-131-admin-persona.md`.
- **Auth0 Configuration Placeholders:** The Auth0 domain (`[Auth0 Domain]`), client ID (`[Auth0 Client ID]`), and Management API audience (`[Auth0 Management API Audience]`) are deployment-specific configuration variables required by this story's Auth0 integration. These are cataloged in the epic-level System Placeholders section.

---

## Discrepancy Review

### Figma Frame Analyzed

- **Frame:** User Management Library — Node ID `7178-133386`
- **URL:** [https://www.figma.com/design/6fQyfvBUqImyavY8Fw47FV/BSA-Wireframes?node-id=7178-133386](https://www.figma.com/design/6fQyfvBUqImyavY8Fw47FV/BSA-Wireframes?node-id=7178-133386)

### Discrepancy Findings

| # | Element | Figma Location | Status | Action |
|---|---------|---------------|--------|--------|
| 1 | **Traditional Pagination Controls** | Bottom of user table (if present in wireframe) | **Overridden per Global Rule #4** | Any pagination controls (page numbers, next/previous buttons) depicted in the Figma wireframe are excluded from acceptance criteria and replaced with lazy load / infinite scroll per CC-RQ-001. This is a mandatory global override, not a Figma-Jira discrepancy. |

> **Note:** Jira requirement text was used as the source of truth for all acceptance criteria per Global Rule #5. The Figma frame was analyzed for supplementary visual guidance only. No additional UI elements were found in the Figma wireframe that are absent from the Jira requirements beyond the pagination override noted above.

If upon detailed Figma frame inspection additional elements are discovered that exist in Figma but are not referenced in the Jira requirements, they should be flagged here following this format:

> _"[Element Name] — Present in Figma frame `7178-133386` but not referenced in Jira requirements for BSABANKSTA-1500. Excluded from acceptance criteria per Global Rule #5. Flagged for design review."_

---

## Generated UI Specifications

> **⚠️ DESIGN REVIEW REQUIRED:** The following UI specifications were automatically generated based on the existing design system tokens, as no explicit mockup was provided for these elements. Please review for accuracy and design intent before development.

The Figma frame `7178-133386` covers the primary user table view. The following UI elements are required by acceptance criteria but are **not depicted** in the available Figma wireframe. Their design specifications have been generated using the three-tier token priority hierarchy.

### Empty State Display (AC11)

**Priority Level:** 1 — Reuse Existing Shared Component Pattern

| UI Element | Design Token / Attribute | Value | Rationale |
|-----------|--------------------------|-------|-----------|
| Container | Layout | `flex flex-col items-center justify-center py-16` | Reuses the shared Empty State pattern consistent across all list views in the application (CC-RQ-001 infinite scroll views) |
| Container | Text Color | `text-gray-500` | Semantic neutral color for informational empty states — consistent with design system's secondary text token |
| Icon | Size | `w-16 h-16` | Standard empty state illustration size from shared component library |
| Icon | Color | `text-gray-300` | Lighter neutral for decorative icon — establishes visual hierarchy below the message text |
| Heading | Typography | `text-lg font-semibold` | Section heading weight — provides clear visual anchor for the empty state message |
| Message | Typography | `text-sm text-gray-400` | Body text style for supplementary information — slightly lighter than the heading |
| Message | Content | "No users found matching '[query]'" | Dynamic message incorporating the search query for user context |

### End-of-List Indicator (AC4)

**Priority Level:** 1 — Reuse Existing Shared Component Pattern

| UI Element | Design Token / Attribute | Value | Rationale |
|-----------|--------------------------|-------|-----------|
| Container | Layout | `text-center py-4` | Reuses the shared End-of-List pattern for all infinite scroll views |
| Container | Text Color | `text-gray-400` | Subtle neutral color — indicator should not compete with table data for visual attention |
| Divider | Border | `border-t border-gray-200 mx-8` | Horizontal rule separating the last data row from the end indicator — consistent with table row borders |
| Text | Typography | `text-sm` | Small text for the "All users loaded" indicator |
| Text | Content | "All users loaded" | Clear, concise end-of-list message |

### Error State with Retry (AC12)

**Priority Level:** 1 — Reuse Existing Shared Component Pattern

| UI Element | Design Token / Attribute | Value | Rationale |
|-----------|--------------------------|-------|-----------|
| Container | Layout | `bg-red-50 border border-red-200 rounded-md p-4` | Reuses the shared Error State pattern — red semantic color system communicates error severity |
| Container | Margin | `mx-4 my-4` | Standard spacing from table edges |
| Error Icon | Color | `text-red-400` | Semantic error icon color — softer than text to avoid overwhelming the message |
| Error Icon | Size | `w-5 h-5` | Standard inline icon size |
| Error Message | Typography | `text-sm text-red-700` | Semantic error text color — high contrast against `bg-red-50` for readability |
| Error Message | Content | "Failed to load user data. Please try again." | User-friendly error message |
| Retry Button | Style | `bg-red-100 hover:bg-red-200 text-red-700 text-sm font-medium px-4 py-2 rounded-md` | Secondary-style button using the error color palette — visually grouped with the error context |
| Retry Button | Content | "Retry" | Clear action label |

### Loading Indicator (Infinite Scroll Fetch)

**Priority Level:** 2 — Apply Semantic Tokens

| UI Element | Design Token / Attribute | Value | Rationale |
|-----------|--------------------------|-------|-----------|
| Container | Layout | `flex items-center justify-center py-4` | Centered within the table's scroll area, positioned where the next batch of rows will appear |
| Spinner | Size | `w-6 h-6` | Standard spinner size for inline loading contexts — not so large as to suggest a full-page load |
| Spinner | Color | `text-blue-500` | Primary brand color for active loading state — consistent with interactive element tokens |
| Spinner | Animation | `animate-spin` | TailwindCSS spin animation for continuous rotation |
| Text | Typography | `text-sm text-gray-400 ml-2` | Optional loading text next to spinner |
| Text | Content | "Loading more users..." | Descriptive loading message |

### Confirmation Dialog (AC8, AC9)

**Priority Level:** 1 — Reuse Modal Pattern from BSABANKSTA-1579

| UI Element | Design Token / Attribute | Value | Rationale |
|-----------|--------------------------|-------|-----------|
| Overlay | Background | `fixed inset-0 bg-black bg-opacity-50` | Standard modal overlay pattern matching System Messages modal (BSABANKSTA-1579) |
| Dialog Container | Layout | `bg-white rounded-lg shadow-xl max-w-md mx-auto p-6` | Reuses the modal container pattern from BSABANKSTA-1579 — `bg-white rounded-lg shadow-xl` centered |
| Dialog Container | Position | `fixed top-1/2 left-1/2 transform -translate-x-1/2 -translate-y-1/2` | Centered positioning consistent with all application modals |
| Title | Typography | `text-lg font-semibold text-gray-900` | Modal heading style — semantic importance for the confirmation prompt |
| Message | Typography | `text-sm text-gray-600 mt-2` | Body text explaining the action to be confirmed |
| Confirm Button | Style | `bg-red-600 hover:bg-red-700 text-white font-medium px-4 py-2 rounded-md` | Destructive action button color — red for role changes and deactivation that have significant impact |
| Cancel Button | Style | `bg-white hover:bg-gray-50 text-gray-700 font-medium px-4 py-2 rounded-md border border-gray-300` | Neutral secondary button — clear visual differentiation from the destructive confirm action |
| Button Container | Layout | `flex justify-end gap-3 mt-6` | Right-aligned button group with standard gap spacing |
| Focus Trap | Behavior | `aria-modal="true"`, Tab/Shift+Tab constrained to dialog | Accessibility requirement — focus must not escape the dialog while open |

### User Detail Panel (AC7)

**Priority Level:** 2 — Apply Semantic Tokens

| UI Element | Design Token / Attribute | Value | Rationale |
|-----------|--------------------------|-------|-----------|
| Container | Layout | `bg-white rounded-lg shadow-md p-6 border border-gray-200` | Card-style detail panel — consistent with dashboard card patterns used in BSABANKSTA-1572 |
| Header | Typography | `text-xl font-semibold text-gray-900` | User name as the panel heading — prominent but not overscaled |
| Field Label | Typography | `text-xs font-medium text-gray-500 uppercase tracking-wide` | Label styling following form field conventions — uppercase for scanability |
| Field Value | Typography | `text-sm text-gray-900` | Standard body text for field values |
| Status Badge — Active | Style | `bg-green-100 text-green-800 text-xs font-medium px-2.5 py-0.5 rounded-full` | Semantic green for active status — consistent with success token family |
| Status Badge — Inactive | Style | `bg-red-100 text-red-800 text-xs font-medium px-2.5 py-0.5 rounded-full` | Semantic red for inactive/blocked status — consistent with error token family |
| Role Badge | Style | `bg-blue-100 text-blue-800 text-xs font-medium px-2.5 py-0.5 rounded-full` | Informational blue for role display — neutral semantic |
| Back/Close Action | Style | `text-gray-400 hover:text-gray-600` | Subtle close action — consistent with modal close patterns |

---

## Figma Mockup Link

### User Management Library

- **Frame:** User Management Library
- **Node ID:** `7178-133386`
- **URL:** [https://www.figma.com/design/6fQyfvBUqImyavY8Fw47FV/BSA-Wireframes?node-id=7178-133386](https://www.figma.com/design/6fQyfvBUqImyavY8Fw47FV/BSA-Wireframes?node-id=7178-133386)
- **Description:** Table-based user list with search bar, sortable column headers, user data rows displaying name, email, role, status, and last login fields. Management action controls are available per row. The frame depicts the primary populated state of the User Management Library as accessed from Admin Settings in the Application Header.

---

## Definition of Done

- [ ] All 12 acceptance criteria (AC1–AC12) pass BDD validation with automated Given/When/Then test scenarios
- [ ] Unit tests written and passing for all API endpoints (pytest) — list users, user detail, role change, status change
- [ ] Unit tests written and passing for RBAC middleware (pytest) — admin-only enforcement on all endpoints
- [ ] Unit tests written and passing for Auth0 Management API client (pytest) — user retrieval, role update, status update
- [ ] Unit tests written and passing for last-admin protection logic (pytest)
- [ ] Component tests written and passing (@testing-library/react) — UserManagementPage, UserTable, UserSearchBar, InfiniteScrollTrigger
- [ ] E2E tests written and passing (Playwright) — full user management flow from navigation through search, scroll, detail view, and management actions
- [ ] BDD tests written and passing (behave) — all 12 acceptance criteria scenarios
- [ ] Code reviewed and approved by at least one peer
- [ ] User list displays correctly with infinite scroll — no traditional pagination controls present
- [ ] Search functionality works with 300ms debounced queries and cursor reset
- [ ] Sortable columns function correctly with visual `aria-sort` direction indicators
- [ ] User management actions (role change, activate/deactivate) work correctly with confirmation dialogs
- [ ] Last-admin protection rule prevents removal of the sole remaining administrator
- [ ] RBAC enforcement verified — only BSA Administrator role can access the User Management Library page and API endpoints
- [ ] BSA Analyst users confirmed to NOT see "Admin Settings" in the Application Header
- [ ] Empty state component renders correctly when search returns no results
- [ ] Error state with retry button renders correctly on API failures
- [ ] End-of-list indicator renders correctly when all users are loaded
- [ ] WCAG 2.1 AA accessibility compliance verified for data table (`aria-sort`, `<th>`/`<td>` semantics, keyboard navigation, screen reader announcements)
- [ ] NFRs validated: < 3s initial load, < 1s search response, < 1s infinite scroll load, < 500ms client-side sort
- [ ] Auth0 Management API integration verified — user listing, detail retrieval, role updates, and status updates function correctly
- [ ] Auth0 Management API token caching and rate limit handling verified
- [ ] Design review completed against Figma frame `7178-133386`
- [ ] Generated UI Specifications for undepicted elements reviewed and approved by design team
- [ ] Shared Application Header coordination verified with BSABANKSTA-1532 (Profile Dropdown) — dropdown mutual exclusion behavior
- [ ] Cross-epic dependency links verified bidirectionally with BSABANKSTA-1531, BSABANKSTA-1532, BSABANKSTA-1305, BSABANKSTA-1579, and BSABANKSTA-1458
- [ ] All configurable placeholders (Auth0 domain, client ID, Management API audience) resolve correctly per deployment environment
- [ ] Documentation updated and complete
