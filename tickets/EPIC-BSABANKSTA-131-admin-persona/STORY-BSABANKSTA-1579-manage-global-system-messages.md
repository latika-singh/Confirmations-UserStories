# Manage Global System Messages

**Story ID:** BSABANKSTA-1579
**Epic:** BSABANKSTA-131 — BSA Admin Persona
**Batch:** 2

> **Decomposition Notice (Global Rule #2):** This story has been decomposed into two sub-stories based on analysis revealing 11 distinct acceptance criteria (exceeding the 10-AC threshold) and two clearly distinct user workflows — Create and Edit. Each sub-story below contains the full 14-section template. See Refinement Notes (Section 10 of each sub-story below) for detailed rationale.

---

# STORY-BSABANKSTA-1579-A: Create Global System Message

## 1. Story Title

**Create Global System Message**

---

## 2. User Story

**As a** BSA Administrator,
**I want** to create new global system messages through a modal dialog interface with visibility configuration,
**So that** I can communicate important system-wide announcements, alerts, and notifications to all application users, ensuring timely awareness of compliance updates, system changes, or operational notices.

---

## 3. INVEST Validation

| Principle | Validation | Notes |
|-----------|------------|-------|
| **Independent** | ✅ Pass | Can be developed independently; requires only the Application Header admin navigation shell (BSABANKSTA-1531) for modal access and the system messages data model. No dependency on Edit flow for core create functionality. |
| **Negotiable** | ✅ Pass | Message fields (title, body, priority), visibility options (date range, audience), and modal layout are negotiable. Core capability is message creation with visibility configuration. |
| **Valuable** | ✅ Pass | Provides critical administrative communication capability for BSA compliance operations — enables system-wide messaging without code deployments. Directly supports compliance update dissemination and operational notices. |
| **Estimable** | ✅ Pass | 6 Figma frames provide comprehensive visual direction covering all modal states including create flow. Technology stack (React modal, Flask API, MongoDB) is well-understood. |
| **Small** | ✅ Pass | Scoped to create-only flow after decomposition from combined create/edit story. Includes modal open, form entry, validation, visibility config, save, and cancel — a well-bounded feature. |
| **Testable** | ✅ Pass | Can verify modal open/close, form field rendering, field validation (required fields, character limits), visibility configuration, save with API persistence, cancel/dismiss, success confirmation, RBAC enforcement. |

---

## 4. Non-Functional Requirements

| Category | Requirement | Target |
|----------|------------|--------|
| **Performance — Modal** | Modal open/close latency from trigger click to fully rendered modal | < 200ms |
| **Performance — Save** | Create message API round-trip (POST request to success confirmation) | < 2 seconds |
| **Performance — List Load** | System messages list initial load with lazy load / infinite scroll | < 3 seconds |
| **Accessibility** | WCAG 2.1 AA compliance — focus trap within modal, Escape key to close, `aria-modal="true"`, `aria-labelledby` for modal title, `aria-describedby` for modal content | Full compliance |
| **Security** | Role-Based Access Control — only BSA Administrator role can access create functionality | Enforced at both UI and API layers |
| **Data Validation** | All required fields validated client-side before API submission; server-side validation as fallback | 100% coverage |
| **Responsiveness** | Desktop-first responsive layout; no mobile-specific layouts | Per CC-RQ-003 |
| **Reliability** | Form data preserved on validation failure; no data loss on non-destructive errors | Zero data loss |

---

## 5. Acceptance Criteria

### AC-1579-A-01: Open Create System Message Modal

**Given** the user is authenticated as a BSA Administrator
**And** the user is on the System Messages management page within the Admin area
**When** the user clicks the "Create Message" button
**Then** a modal dialog is displayed with empty form fields for message creation
**And** the modal renders as a fixed overlay with a `bg-white rounded-lg shadow-xl` centered container
**And** focus is automatically moved to the first interactive element within the modal
**And** focus is trapped within the modal (`aria-modal="true"`)
**And** the modal has a descriptive title via `aria-labelledby`

### AC-1579-A-02: Create System Message — Required Field Validation

**Given** the Create System Message modal is open with empty form fields
**When** the user clicks the "Save" or "Create" button without completing all required fields
**Then** inline validation errors are displayed adjacent to each missing or invalid required field
**And** each error message clearly describes the validation requirement (e.g., "Title is required", "Message body cannot be empty")
**And** the form is NOT submitted to the API
**And** focus is moved to the first field with a validation error

### AC-1579-A-03: Create System Message — Character Limit Validation

**Given** the Create System Message modal is open
**When** the user enters message content exceeding the maximum character limit for any field
**Then** an inline validation error is displayed indicating the character limit (e.g., "Message body must not exceed [Max System Message Length] characters")
**And** a character counter displays the current count relative to the maximum
**And** the form is NOT submitted

### AC-1579-A-04: Save New System Message Successfully

**Given** the Create System Message modal is open
**And** all required fields are populated with valid data
**And** visibility settings have been configured
**When** the user clicks the "Save" or "Create" button
**Then** the system message is persisted to the database via POST `/api/admin/system-messages`
**And** a success confirmation is displayed to the user
**And** the modal closes automatically
**And** the system messages list is refreshed to include the newly created message
**And** focus returns to the "Create Message" trigger button

### AC-1579-A-05: Cancel Create Modal via Button

**Given** the Create System Message modal is open
**And** the user may or may not have entered data in the form fields
**When** the user clicks the "Cancel" button
**Then** the modal closes without saving any data
**And** any entered data is discarded
**And** focus returns to the "Create Message" trigger button

### AC-1579-A-06: Dismiss Create Modal via Escape Key

**Given** the Create System Message modal is open
**When** the user presses the Escape key
**Then** the modal closes without saving any data
**And** any entered data is discarded
**And** focus returns to the "Create Message" trigger button

### AC-1579-A-07: Configure Message Visibility During Creation

**Given** the Create System Message modal is open
**When** the user configures visibility settings including:
- Active/Inactive status toggle
- Start date and end date for visibility window
- Target audience selection (e.g., All Users, BSA Administrators only, BSA Analysts only)
**Then** the visibility configuration is captured as part of the message data
**And** the configured visibility rules will be applied when the message is saved
**And** date fields validate that the start date is before the end date

### AC-1579-A-08: System Messages List Display with Infinite Scroll

**Given** the BSA Administrator navigates to the System Messages management page
**When** the page loads
**Then** the system messages list is displayed using lazy load / infinite scroll
**And** messages are loaded in batches using cursor-based data loading via GET `/api/admin/system-messages`
**And** a loading indicator is displayed while the initial batch is fetched
**And** no pagination controls (page numbers, next/previous buttons) are present

### AC-1579-A-09: End-of-List Indicator for Messages List

**Given** all system messages have been loaded via infinite scroll
**When** the user scrolls past the last loaded message
**Then** an end-of-list indicator is displayed (centered text with divider)
**And** no further API calls are made for additional messages

### AC-1579-A-10: Role-Based Access Control — Admin Only

**Given** a user authenticated with the BSA Analyst role (non-admin)
**When** the user attempts to navigate to the System Messages management page
**Then** access is denied
**And** the user is redirected to an appropriate page or shown an unauthorized access message
**And** the POST `/api/admin/system-messages` endpoint returns HTTP 403 Forbidden for non-admin requests

---

## 6. Sub-Tasks

### 6.1 Model

- [ ] Define MongoDB collection schema for `system_messages`:
  - `_id` (ObjectId) — primary key
  - `message_id` (string, UUID) — public identifier
  - `title` (string, required) — message title
  - `body` (string, required) — message body content
  - `priority` (string, enum: "info", "warning", "critical") — message priority level
  - `visibility_config` (sub-document):
    - `status` (string, enum: "active", "inactive") — active/inactive toggle
    - `start_date` (datetime, optional) — visibility window start
    - `end_date` (datetime, optional) — visibility window end
    - `target_audience` (string, enum: "all", "administrators", "analysts") — audience targeting
  - `created_by` (string) — Auth0 user ID of the creating administrator
  - `created_at` (datetime) — creation timestamp with timezone
  - `updated_at` (datetime) — last update timestamp with timezone
  - `version` (integer) — optimistic locking version field
- [ ] Create Marshmallow schema `SystemMessageCreateSchema` for create request validation:
  - Required fields: `title`, `body`
  - Optional fields: `priority` (default: "info"), `visibility_config`
  - Validation: character length limits, enum constraints, date range logic
- [ ] Create Marshmallow schema `SystemMessageResponseSchema` for API response serialization
- [ ] Define cursor-based pagination model for infinite scroll:
  - Cursor field: `created_at` + `_id` composite for stable ordering
  - Batch size: configurable (default 20)
  - Response includes `next_cursor` token and `has_more` flag

### 6.2 API

- [ ] Create Flask blueprint `admin_system_messages_bp` under `/api/admin/system-messages`
- [ ] Implement POST `/api/admin/system-messages` endpoint:
  - Request body: `SystemMessageCreateSchema` validated input
  - Response: 201 Created with `SystemMessageResponseSchema` serialized message
  - RBAC: BSA Administrator role required (401/403 on failure)
- [ ] Implement GET `/api/admin/system-messages` endpoint:
  - Query params: `cursor` (string, optional), `limit` (integer, default 20)
  - Response: `{ data: [...], next_cursor: string|null, has_more: boolean, total_count: number }`
  - Ordering: reverse chronological (`created_at` DESC)
  - RBAC: BSA Administrator role required
- [ ] Implement GET `/api/admin/system-messages/<message_id>` endpoint:
  - Response: single `SystemMessageResponseSchema` serialized message
  - RBAC: BSA Administrator role required
- [ ] Apply RBAC middleware decorator to all admin endpoints — validate BSA Administrator role from Auth0 JWT claims
- [ ] Configure CORS for admin endpoints via Flask-CORS

### 6.3 Component

- [ ] Create React `SystemMessageModal` wrapper component:
  - Fixed overlay (`fixed inset-0 bg-black/50 flex items-center justify-center z-50`)
  - Centered container (`bg-white rounded-lg shadow-xl max-w-2xl w-full mx-4`)
  - Focus trap implementation (Tab / Shift+Tab cycle within modal)
  - Escape key listener to close modal
  - `aria-modal="true"`, `aria-labelledby`, `role="dialog"` attributes
- [ ] Create `CreateMessageForm` component:
  - Form fields: Title (text input), Body (textarea with character counter), Priority (select/radio)
  - `VisibilityConfigPanel` sub-component: Status toggle, Date range pickers, Audience selector
  - Inline validation error display (`text-red-600 text-sm mt-1`)
  - Submit and Cancel buttons in modal footer
- [ ] Create `SystemMessagesList` component:
  - Infinite scroll container using `overflow-auto`
  - `IntersectionObserver`-based trigger element for loading next batch
  - Message cards/rows with title, status, created date, visibility info
  - Loading skeleton during initial fetch
  - "Create Message" action button
- [ ] Create `EndOfListIndicator` component: `text-center py-4 text-gray-400` with horizontal divider
- [ ] Apply TailwindCSS 4.2.1 design tokens throughout per Figma BSA Wireframes

### 6.4 Logic

- [ ] Implement RBAC enforcement in React:
  - Check Auth0 user roles before rendering admin routes/components
  - Redirect non-admin users attempting to access System Messages page
- [ ] Implement form validation logic:
  - Required field checks (title, body)
  - Character limit enforcement with real-time counter
  - Date range validation (start < end)
  - Audience selection validation
- [ ] Implement modal state management:
  - Open/close state with React state or context
  - Form data state management for create flow
  - Loading state during API submission
  - Success/error state after submission
- [ ] Implement visibility rule processing:
  - Date range validation logic
  - Audience targeting preparation for API submission
- [ ] Implement infinite scroll data fetching:
  - Cursor management for messages list
  - Automatic batch loading on scroll threshold
  - Loading indicator toggle during fetch
  - End-of-list detection and indicator display
- [ ] Implement Auth0 session validation for admin API calls

### 6.5 Testing

- [ ] **Unit Tests (pytest)**:
  - POST `/api/admin/system-messages` — valid creation, validation errors (missing title, body), character limit exceeded, invalid visibility config
  - GET `/api/admin/system-messages` — list retrieval, cursor-based pagination, empty list
  - RBAC middleware — admin access granted, analyst access denied (403), unauthenticated access denied (401)
  - Marshmallow schema validation — valid data, missing required fields, invalid enums, date range conflicts
- [ ] **Component Tests (@testing-library/react)**:
  - `SystemMessageModal` — renders overlay, focus trap works, Escape closes modal, aria attributes present
  - `CreateMessageForm` — renders all fields, validation errors display on submit with empty fields, character counter updates, successful submission
  - `VisibilityConfigPanel` — status toggle works, date range renders and validates, audience selector works
  - `SystemMessagesList` — renders messages, infinite scroll triggers load, end-of-list indicator appears
- [ ] **BDD Tests (behave)**: Acceptance criteria AC-1579-A-01 through AC-1579-A-10 as Gherkin scenarios
- [ ] **E2E Tests (Playwright)**: Full create flow — navigate to admin > open modal > fill form > configure visibility > save > verify list update > verify modal closed
- [ ] **Accessibility Tests**: Focus trap verification, keyboard navigation (Tab, Shift+Tab, Escape), screen reader announcements, ARIA attribute validation

---

## 7. Edge Cases

| # | Edge Case | Expected Behavior | Category |
|---|-----------|-------------------|----------|
| 1 | **Session Expiry During Create** | Admin's Auth0 session expires while the create modal is open and they are entering data. On save attempt, the system detects the expired session, displays a re-authentication prompt, and preserves the entered form data in local component state so it is not lost upon re-login. | Authentication |
| 2 | **Extremely Long Message Content** | Admin enters a message body exceeding the configurable `[Max System Message Length]` character limit. An inline validation error is displayed with the maximum character count (e.g., "Message body must not exceed 5000 characters — currently 5234"). The character counter turns red. Form submission is prevented. | Boundary |
| 3 | **Visibility Date Range Conflict** | Admin configures a start date that is after the end date in the visibility settings. An inline validation error is displayed on the date range fields: "Start date must be before end date." The form cannot be submitted until corrected. | Validation |
| 4 | **Empty System Messages List** | No system messages exist in the database. The messages list page displays the shared empty state component (`flex flex-col items-center justify-center py-16 text-gray-500`) with a contextual prompt: "No system messages yet. Click 'Create Message' to get started." and a call-to-action button to open the create modal. | Empty State |
| 5 | **API Failure During Create Save** | The POST API call to create a system message fails due to a server error or network issue. The modal remains open, the entered data is preserved, an error notification is displayed (e.g., "Failed to create message. Please try again."), and a retry option is available. The user does not lose their entered data. | Error Handling |

---

## 8. Dependencies

| Dependency | Epic / Source | Type | Description |
|------------|--------------|------|-------------|
| **BSABANKSTA-131** | BSA Admin Persona (this epic) | Parent Epic | System messages management is an admin-only capability within the BSA Admin Persona suite |
| **BSABANKSTA-1531** | Application Frame and Global Navigation | UI Container | System message management is accessed from the Admin area, hosted within the Application Header component. The Admin Settings entry point in the header provides navigation to the System Messages page. |
| **BSABANKSTA-1532** | Application Frame (BSABANKSTA-1531) | Shared Header | Admin Settings and Profile Dropdown coexist as sibling components in the Application Header bar — mutual coordination required |
| **BSABANKSTA-1305** | Batch 1 — Project Space Management | Data Context | System messages may reference project space contexts or be triggered by project space lifecycle events; messages about project-related compliance updates depend on F-001 entity awareness |
| **BSABANKSTA-1500** | BSA Admin Persona (this epic) | Shared Navigation | User Management Library shares the admin navigation context within the Application Header's Admin Settings area |
| **BSABANKSTA-1579-B** | This story (Edit sub-story) | Sibling Dependency | Create and Edit sub-stories share the SystemMessageModal component, SystemMessagesList, and the system_messages data model |
| **CC-RQ-001** | Cross-Cutting Requirement | UI Pattern | Messages list implements lazy load / infinite scroll pattern (no pagination) |
| **Auth0** | External Service | Authentication | Admin role validation sourced from Auth0 identity provider; JWT claims contain role information for RBAC enforcement |

---

## 9. Story Estimation Guidance

**Story Points: 5 (Fibonacci)**

**Rationale:**
- **Moderate-High Complexity**: Create modal with form validation, visibility configuration panel, and RBAC enforcement. The modal component requires accessibility features (focus trap, keyboard navigation, ARIA attributes).
- **Low-Moderate Uncertainty**: 3 of the 6 Figma frames apply to the create flow, providing clear visual direction. The MongoDB data model and Flask API patterns are well-established.
- **Moderate Effort**: Backend API (POST endpoint + list GET with cursor pagination), React modal component with form validation, infinite scroll for messages list, TailwindCSS styling, and comprehensive testing.
- **5 points** reflects a well-scoped create flow that, while involving a modal with multiple states, benefits from clear Figma direction and standard CRUD patterns. The decomposition from the original 8-point combined story reduces scope to a focused create-only workflow.

---

## 10. Refinement Notes

### Global Rule #2 — Proactive Story Decomposition
**DECOMPOSITION APPLIED.** The original story BSABANKSTA-1579 "Manage Global System Messages" was decomposed into two sub-stories:
- **STORY-BSABANKSTA-1579-A**: Create Global System Message (this sub-story)
- **STORY-BSABANKSTA-1579-B**: Edit Global System Message

**Rationale:** Analysis of the requirements revealed:
1. **11 distinct acceptance criteria** across create and edit flows (exceeding the >10 AC threshold)
2. **6 Figma frames** representing distinct modal states for both create and edit workflows
3. **Two clearly separate user workflows**: creating a new message (empty form → validation → save) and editing an existing message (pre-populated form → modify → update) — each with its own entry point, data flow, and API endpoint
4. The combined story violated the **Small** principle of INVEST — decomposition produces two focused, independently deliverable stories

### Global Rule #3 — Verb-Noun Title Standardization
**Original Jira Title:** "Admin | Manage Global System Messages | Create/Edit Modal BSABANKSTA-1579"
**Standardized Title (this sub-story):** "Create Global System Message"
**Transformation:** The organizational qualifier "Admin |" and UI specificity "| Create/Edit Modal" were removed. The combined "Manage" verb was split into specific verbs per sub-story: "Create" for -A and "Edit" for -B. Verb-Noun format applied.

### Global Rule #4 — Pagination to Lazy Load Override
**MANDATORY OVERRIDE APPLIED.** Any reference to pagination in the system messages list has been replaced with lazy load / infinite scroll using cursor-based data loading. The `SystemMessagesList` component uses `IntersectionObserver` API to detect scroll threshold and automatically load the next batch of messages. Documented per CC-RQ-001.

### Global Rule #5 — Jira Source of Truth
Jira requirement text was used as the authoritative source for all acceptance criteria. Figma frames were used for visual reference only. Any UI elements found in Figma but absent from Jira requirements are flagged in the Discrepancy Review section below.

### Global Rule #6 — NFR Elevation
Performance targets were extracted from acceptance criteria into the dedicated Non-Functional Requirements section:
- Modal open/close: < 200ms
- Message save API: < 2 seconds
- Messages list load: < 3 seconds
- WCAG 2.1 AA accessibility compliance

### Global Rule #7 — Placeholder Management
System messages may contain configurable placeholders such as `[Application Name]` in message templates. These are cataloged in the parent epic file (`EPIC-BSABANKSTA-131-admin-persona.md`) under System Placeholders. The `[Max System Message Length]` placeholder governs the character limit for message body content.

---

## 11. Discrepancy Review

**Figma frames analyzed for Create flow:** `8304-120310`, `8304-120342`, `8304-120318`

The following analysis compares UI elements present in the Figma wireframes against the Jira requirement text for BSABANKSTA-1579:

| Element | Figma Frame | Finding |
|---------|------------|---------|
| Create modal layout | `8304-120310` | Consistent with Jira requirements — modal with form fields for message creation |
| Form fields (title, body) | `8304-120342` | Consistent with Jira requirements — standard form input fields |
| Visibility configuration panel | `8304-120318` | Consistent with Jira requirements — visibility settings for message targeting |

No discrepancies identified between Figma wireframes and Jira requirements for the Create flow. All UI elements depicted in Figma frames `8304-120310`, `8304-120342`, and `8304-120318` are accounted for in the Jira requirement acceptance criteria.

> **Note:** Per Global Rule #5, if any decorative elements or additional UI chrome is present in Figma but not specified in Jira, they are excluded from acceptance criteria. The Figma frames serve as visual guidance only.

---

## 12. Generated UI Specifications

> **DESIGN REVIEW REQUIRED:** The following UI specifications were automatically generated based on the existing design system tokens, as no explicit mockup was provided for these elements. Please review for accuracy and design intent before development.

While the Create flow benefits from 3 dedicated Figma frames covering the primary modal states, the following supplementary UI elements are required by the acceptance criteria but are not explicitly depicted in any Figma wireframe:

### 12.1 Empty State — System Messages List

| UI Element | Design Token / Attribute | Value | Rationale |
|-----------|------------------------|-------|-----------|
| Container | `display`, `flex-direction`, `align-items`, `justify-content` | `flex flex-col items-center justify-center` | Reuse shared Empty State pattern — centered content layout (Priority 1: Reuse existing component) |
| Container | `padding-y` | `py-16` (64px top/bottom) | Consistent vertical spacing with shared Empty State across all features |
| Icon/Illustration | `color` | `text-gray-400` | Muted color for empty state visual — semantic neutral token |
| Primary Text | `color`, `font-size`, `font-weight` | `text-gray-500 text-lg font-medium` | Descriptive message: "No system messages yet" — readable but subdued |
| Secondary Text | `color`, `font-size` | `text-gray-400 text-sm` | Helper text: "Click 'Create Message' to get started" |
| CTA Button | `background`, `color`, `padding`, `border-radius` | `bg-blue-600 text-white px-4 py-2 rounded-md hover:bg-blue-700` | Action button to open create modal — primary action styling |

### 12.2 Error State — API Failure

| UI Element | Design Token / Attribute | Value | Rationale |
|-----------|------------------------|-------|-----------|
| Container | `background`, `border`, `border-radius`, `padding` | `bg-red-50 border border-red-200 rounded-md p-4` | Reuse shared Error State / Retry pattern (Priority 1: Reuse existing component) |
| Error Icon | `color` | `text-red-500` | Semantic error color from design system |
| Error Text | `color`, `font-size` | `text-red-700 text-sm` | Error message: "Failed to create message. Please try again." |
| Retry Button | `background`, `color`, `padding`, `border-radius` | `bg-red-100 text-red-700 px-3 py-1.5 rounded-md hover:bg-red-200 text-sm font-medium` | Retry action with error-context styling |

### 12.3 End-of-List Indicator — Messages List

| UI Element | Design Token / Attribute | Value | Rationale |
|-----------|------------------------|-------|-----------|
| Container | `text-align`, `padding-y` | `text-center py-4` | Reuse shared End-of-List pattern (Priority 1: Reuse existing component) |
| Divider | `border-top`, `margin` | `border-t border-gray-200 mx-8 mb-2` | Subtle horizontal divider above text |
| Text | `color`, `font-size` | `text-gray-400 text-sm` | Muted indicator text: "All messages loaded" |

### 12.4 Inline Validation Error Styling

| UI Element | Design Token / Attribute | Value | Rationale |
|-----------|------------------------|-------|-----------|
| Error Text | `color`, `font-size`, `margin-top` | `text-red-600 text-sm mt-1` | Semantic error token applied below input fields (Priority 2: Apply semantic tokens) |
| Input Border (Error State) | `border-color` | `border-red-500 focus:ring-red-500` | Visual error indicator on the input field itself |
| Character Counter (Over Limit) | `color` | `text-red-600` | Counter turns red when limit exceeded |
| Character Counter (Under Limit) | `color` | `text-gray-400` | Default muted counter color |

### 12.5 Success Confirmation Toast

| UI Element | Design Token / Attribute | Value | Rationale |
|-----------|------------------------|-------|-----------|
| Container | `background`, `border`, `border-radius`, `padding`, `shadow` | `bg-green-50 border border-green-200 rounded-md p-4 shadow-md` | Success feedback pattern — semantic success tokens (Priority 2: Apply semantic tokens) |
| Icon | `color` | `text-green-500` | Semantic success color |
| Text | `color`, `font-size` | `text-green-700 text-sm` | Success message: "System message created successfully" |
| Auto-dismiss | `duration` | `5000ms` | Toast auto-dismisses after 5 seconds |

---

## 13. Figma Mockup Link

The following Figma frames from the BSA Wireframes file (`6fQyfvBUqImyavY8Fw47FV`) are associated with the Create flow of this story:

| Frame | Description | URL |
|-------|------------|-----|
| **System Messages Modal — State 1** | System Messages management page with message list and "Create Message" action button | [Figma Frame 8304-120310](https://www.figma.com/design/6fQyfvBUqImyavY8Fw47FV/BSA-Wireframes?node-id=8304-120310) |
| **System Messages Modal — State 2** | Create System Message modal — empty form fields displayed for new message entry | [Figma Frame 8304-120342](https://www.figma.com/design/6fQyfvBUqImyavY8Fw47FV/BSA-Wireframes?node-id=8304-120342) |
| **System Messages Modal — State 3** | Create modal with visibility configuration panel — active/inactive, date range, audience | [Figma Frame 8304-120318](https://www.figma.com/design/6fQyfvBUqImyavY8Fw47FV/BSA-Wireframes?node-id=8304-120318) |

> **Note:** Frames 4–6 (`8304-123107`, `8304-123120`, `8304-123149`) are associated with the Edit flow and are documented in STORY-BSABANKSTA-1579-B below.

---

## 14. Definition of Done

- [ ] All acceptance criteria (AC-1579-A-01 through AC-1579-A-10) pass BDD validation
- [ ] Unit tests written and passing — pytest for Flask API endpoints (POST create, GET list, GET detail)
- [ ] Unit tests written and passing — pytest for RBAC middleware (admin access granted, analyst denied)
- [ ] Unit tests written and passing — pytest for Marshmallow schema validation (valid data, missing fields, invalid enums)
- [ ] Component tests written and passing — @testing-library/react for SystemMessageModal, CreateMessageForm, VisibilityConfigPanel
- [ ] Component tests written and passing — @testing-library/react for SystemMessagesList with infinite scroll
- [ ] E2E tests written and passing — Playwright for full create flow (open modal → fill → configure visibility → save → verify list)
- [ ] BDD tests written and passing — behave scenarios for all acceptance criteria
- [ ] Code reviewed and approved by at least one peer
- [ ] Modal create flow works correctly: open → fill fields → validate → save → close → list refreshed
- [ ] Visibility configuration is persisted correctly with the message (status, date range, audience)
- [ ] RBAC enforcement verified — only BSA Administrator can access create functionality (UI and API)
- [ ] System messages list uses lazy load / infinite scroll (no pagination controls present)
- [ ] Focus trap within modal verified (Tab, Shift+Tab cycle; Escape closes)
- [ ] WCAG 2.1 AA accessibility compliance verified (aria-modal, aria-labelledby, focus management)
- [ ] NFRs validated: < 200ms modal open, < 2s save response, < 3s list load
- [ ] Figma frames `8304-120310`, `8304-120342`, `8304-120318` reviewed against implementation
- [ ] Generated UI Specifications for undepicted elements reviewed by design team
- [ ] Cross-epic dependency links verified (BSABANKSTA-1531 shared Application Header)
- [ ] Decomposition documented in Refinement Notes
- [ ] All documentation complete and reviewed

---
---

# STORY-BSABANKSTA-1579-B: Edit Global System Message

## 1. Story Title

**Edit Global System Message**

---

## 2. User Story

**As a** BSA Administrator,
**I want** to edit existing global system messages through a pre-populated modal dialog interface with the ability to modify content and visibility configuration,
**So that** I can update, correct, or adjust system-wide announcements to ensure that users always see accurate and current compliance information, system updates, or operational notices.

---

## 3. INVEST Validation

| Principle | Validation | Notes |
|-----------|------------|-------|
| **Independent** | ✅ Pass | Can be developed independently once the system messages data model and SystemMessageModal component are available (shared with Create sub-story). Edit flow has its own API endpoint (PUT) and distinct entry point (edit action on existing message). |
| **Negotiable** | ✅ Pass | Editable fields, confirmation dialogs, and destructive action workflows are negotiable. Core capability is message content and visibility modification. |
| **Valuable** | ✅ Pass | Enables administrators to maintain accuracy of system-wide communications — critical for BSA compliance where outdated or incorrect messages could cause regulatory confusion or non-compliance. |
| **Estimable** | ✅ Pass | 3 Figma frames (edit modal states, confirmation dialog, success/visibility) provide clear visual direction. Edit follows similar modal patterns as Create with pre-population logic added. |
| **Small** | ✅ Pass | Scoped to edit-only flow after decomposition. Includes modal open with pre-populated data, field modification, validation, visibility config update, save, cancel, and destructive action confirmation — focused and deliverable. |
| **Testable** | ✅ Pass | Can verify modal opens with correct pre-populated data, field modifications persist, validation works on modified fields, confirmation dialogs appear for destructive actions, RBAC enforcement, success feedback. |

---

## 4. Non-Functional Requirements

| Category | Requirement | Target |
|----------|------------|--------|
| **Performance — Modal** | Edit modal open with pre-populated data (includes API fetch for message detail) | < 500ms |
| **Performance — Save** | Update message API round-trip (PUT request to success confirmation) | < 2 seconds |
| **Performance — List Refresh** | Messages list refresh after successful edit | < 1 second |
| **Accessibility** | WCAG 2.1 AA compliance — focus trap within modal, Escape to close, aria attributes, form label associations | Full compliance |
| **Security** | RBAC enforcement — only BSA Administrator can edit messages; API validates admin role | Enforced at UI and API |
| **Data Integrity** | Optimistic locking via version field to prevent concurrent edit conflicts | Version-based conflict detection |
| **Data Validation** | All required fields re-validated on edit; server-side validation as fallback | 100% coverage |
| **Responsiveness** | Desktop-first responsive layout; no mobile-specific layouts | Per CC-RQ-003 |

---

## 5. Acceptance Criteria

### AC-1579-B-01: Open Edit System Message Modal with Pre-Populated Data

**Given** the user is authenticated as a BSA Administrator
**And** the user is viewing the system messages list on the System Messages management page
**When** the user clicks the "Edit" action on an existing system message row
**Then** the modal dialog is displayed pre-populated with the selected message's current data (title, body, priority, visibility configuration)
**And** the modal renders as a fixed overlay with a `bg-white rounded-lg shadow-xl` centered container
**And** focus is trapped within the modal (`aria-modal="true"`)
**And** the modal title indicates edit mode (e.g., "Edit System Message")

### AC-1579-B-02: Edit System Message — Modify Fields

**Given** the Edit System Message modal is open with pre-populated data
**When** the BSA Administrator modifies one or more fields (title, body, priority, visibility settings)
**Then** the modified values are reflected in the form inputs
**And** modified fields are visually distinguishable or tracked for change detection

### AC-1579-B-03: Edit System Message — Field Validation

**Given** the Edit System Message modal is open
**When** the user clears a required field and attempts to save
**Then** inline validation errors are displayed for the invalid field(s)
**And** the form is NOT submitted to the API
**And** focus is moved to the first field with a validation error

### AC-1579-B-04: Save Edited System Message Successfully

**Given** the Edit System Message modal is open with modified data
**And** all required fields contain valid values
**When** the user clicks the "Save" or "Update" button
**Then** the updated message is persisted to the database via PUT `/api/admin/system-messages/<message_id>`
**And** a success confirmation is displayed to the user
**And** the modal closes automatically
**And** the system messages list reflects the updated message content
**And** focus returns to the edited message row or the trigger element

### AC-1579-B-05: Concurrent Edit Conflict Detection

**Given** the Edit System Message modal is open
**And** another administrator has modified the same message since it was loaded
**When** the current user attempts to save their changes
**Then** the system detects the version conflict (via optimistic locking)
**And** a conflict warning is displayed: "This message has been modified by another user. Please reload and try again."
**And** the user is prompted to reload the latest version before re-applying edits

### AC-1579-B-06: Cancel Edit Modal

**Given** the Edit System Message modal is open with pre-populated (and possibly modified) data
**When** the user clicks "Cancel" or presses the Escape key
**Then** the modal closes without saving any changes
**And** the original message data in the list is unchanged
**And** focus returns to the edited message row or trigger element

### AC-1579-B-07: Update Message Visibility Configuration

**Given** the Edit System Message modal is open
**When** the user modifies visibility settings including:
- Changing Active/Inactive status
- Adjusting the start date and/or end date of the visibility window
- Changing the target audience
**Then** the updated visibility configuration is captured
**And** date fields validate that the start date is before the end date
**And** the modified visibility rules are persisted when the message is saved

### AC-1579-B-08: Confirmation Dialog for Deactivating a Message

**Given** the BSA Administrator is editing a system message
**When** the administrator changes the message status from "Active" to "Inactive" and saves
**Then** a confirmation dialog is displayed: "Are you sure you want to deactivate this message? It will no longer be visible to users."
**And** the dialog requires explicit confirmation ("Confirm") or cancellation ("Cancel")
**And** only upon confirmation is the status change persisted

### AC-1579-B-09: Confirmation Dialog for Deleting a Message

**Given** the BSA Administrator is on the system messages list
**When** the administrator triggers a delete action on a system message
**Then** a confirmation dialog is displayed: "Are you sure you want to delete this message? This action cannot be undone."
**And** the dialog requires explicit confirmation before the message is permanently removed
**And** upon confirmation, the message is deleted via the API and removed from the list

### AC-1579-B-10: Role-Based Access Control — Admin Only

**Given** a user authenticated with the BSA Analyst role (non-admin)
**When** the user attempts to trigger an edit action on a system message or access the PUT API endpoint
**Then** the "Edit" action is not visible in the UI for non-admin users
**And** the PUT `/api/admin/system-messages/<message_id>` endpoint returns HTTP 403 Forbidden for non-admin requests

---

## 6. Sub-Tasks

### 6.1 Model

- [ ] Extend the `system_messages` MongoDB collection schema (shared with Create sub-story) to support edit operations:
  - `version` (integer) field for optimistic locking — incremented on each update
  - `updated_by` (string) — Auth0 user ID of the last editor
  - `history` (array, optional) — changelog entries for audit trail integration
- [ ] Create Marshmallow schema `SystemMessageUpdateSchema` for update request validation:
  - All fields optional (partial update support): `title`, `body`, `priority`, `visibility_config`
  - At least one field must be modified for valid submission
  - Same validation rules as create (character limits, enum constraints, date range)
  - Version field included for optimistic locking
- [ ] Define conflict detection model:
  - Compare submitted `version` against current document `version`
  - Return 409 Conflict if versions don't match

### 6.2 API

- [ ] Implement PUT `/api/admin/system-messages/<message_id>` endpoint:
  - Request body: `SystemMessageUpdateSchema` validated input + `version` field
  - Response: 200 OK with `SystemMessageResponseSchema` serialized updated message
  - Conflict response: 409 Conflict with `{ error: "version_conflict", message: "..." }`
  - RBAC: BSA Administrator role required (401/403 on failure)
- [ ] Implement DELETE `/api/admin/system-messages/<message_id>` endpoint:
  - Response: 204 No Content on successful deletion
  - RBAC: BSA Administrator role required
- [ ] Implement optimistic locking logic:
  - Read current document version
  - Compare with submitted version
  - Update only if versions match; return 409 if not
  - Increment version on successful update
- [ ] Apply RBAC middleware decorator to PUT and DELETE endpoints

### 6.3 Component

- [ ] Create `EditMessageForm` component (extends or parameterizes `CreateMessageForm`):
  - Pre-populates all fields with existing message data fetched from GET endpoint
  - Tracks field modifications for change detection
  - Shares `VisibilityConfigPanel` sub-component with Create form
  - Submit calls PUT endpoint instead of POST
  - Handles version conflict errors with reload prompt
- [ ] Create `ConfirmationDialog` component:
  - Reuse modal pattern: `bg-white rounded-lg shadow-xl` centered container (smaller size)
  - Configurable title, message body, confirm label, and cancel label
  - Destructive action styling: confirm button with `bg-red-600 text-white hover:bg-red-700`
  - Non-destructive styling: confirm button with `bg-blue-600 text-white`
  - Focus trap within confirmation dialog
  - Accessible: `role="alertdialog"`, `aria-labelledby`, `aria-describedby`
- [ ] Update `SystemMessagesList` component to include "Edit" and "Delete" action buttons per message row:
  - "Edit" button triggers modal open with message ID
  - "Delete" button triggers confirmation dialog
  - Actions only visible to BSA Administrator role (conditional rendering)
- [ ] Create `ConflictWarning` component for version mismatch display:
  - Warning alert: `bg-yellow-50 border border-yellow-200 rounded-md p-4`
  - Message text and "Reload" action button

### 6.4 Logic

- [ ] Implement modal state management for edit mode:
  - Fetch message detail via GET `/api/admin/system-messages/<message_id>` on modal open
  - Pre-populate form fields with fetched data
  - Loading state during detail fetch
  - Track field modifications for dirty state detection
- [ ] Implement optimistic locking client-side:
  - Store version field from fetched message data
  - Include version in PUT request body
  - Handle 409 Conflict response — display ConflictWarning and prompt reload
- [ ] Implement confirmation dialog logic:
  - Deactivation confirmation: triggered when status changes from active to inactive
  - Deletion confirmation: triggered on delete action
  - Dialog state management (open/close, pending action, confirmed action)
- [ ] Implement form validation logic for edit:
  - Same validation rules as create (shared validation functions)
  - Required field checks on modified data
  - Date range validation on visibility config changes
- [ ] Implement RBAC conditional rendering:
  - Hide "Edit" and "Delete" actions for non-admin users
  - Validate role before opening edit modal

### 6.5 Testing

- [ ] **Unit Tests (pytest)**:
  - PUT `/api/admin/system-messages/<message_id>` — valid update, partial update, validation errors, version conflict (409), message not found (404)
  - DELETE `/api/admin/system-messages/<message_id>` — successful deletion, message not found (404), non-admin access (403)
  - RBAC middleware — admin access granted for PUT/DELETE, analyst access denied
  - Optimistic locking — version match succeeds, version mismatch returns 409
- [ ] **Component Tests (@testing-library/react)**:
  - `EditMessageForm` — renders with pre-populated data, validation errors on cleared required fields, successful submission calls PUT
  - `ConfirmationDialog` — renders with correct message, confirm triggers action, cancel closes dialog, focus trap works
  - `ConflictWarning` — renders warning message, reload button triggers data refresh
  - `SystemMessagesList` — Edit and Delete buttons visible for admin, hidden for non-admin
- [ ] **BDD Tests (behave)**: Acceptance criteria AC-1579-B-01 through AC-1579-B-10 as Gherkin scenarios
- [ ] **E2E Tests (Playwright)**: Full edit flow — click edit on message → verify pre-populated fields → modify fields → save → verify list updated; Delete flow — click delete → confirm → verify removed
- [ ] **Accessibility Tests**: Focus trap in edit modal and confirmation dialog, keyboard navigation, screen reader announcements for edit mode and confirmation prompts

---

## 7. Edge Cases

| # | Edge Case | Expected Behavior | Category |
|---|-----------|-------------------|----------|
| 1 | **Concurrent Edit Conflict** | Two administrators attempt to edit the same system message simultaneously. The first save succeeds (version incremented). The second admin receives a 409 Conflict response with a `ConflictWarning` component displayed, prompting them to reload the latest version before re-applying their edits. Entered data is preserved in the form while the conflict is resolved. | Concurrency |
| 2 | **Session Expiry During Edit** | Admin's Auth0 session expires while the edit modal is open and they are modifying data. On save attempt, the system detects the expired token (401 response), displays a re-authentication prompt, and preserves the modified form data in local component state so edits are not lost upon re-login. | Authentication |
| 3 | **Editing a Recently Deactivated Message** | Admin opens an edit modal for a message that was deactivated by another admin between the list load and the edit action. The GET detail request returns the deactivated status. The edit modal displays the current (deactivated) state, and the admin can reactivate or further modify the message. | State Consistency |
| 4 | **Delete Last System Message** | Admin deletes the last remaining system message in the list. After deletion, the messages list transitions to the empty state component with the "No system messages yet" prompt and create CTA button. | Edge Transition |
| 5 | **Network Failure During Delete Confirmation** | Admin confirms deletion but the DELETE API call fails due to network error. The confirmation dialog closes, the message remains in the list, and an error notification is displayed: "Failed to delete message. Please try again." The message is not removed from the UI (no optimistic delete). | Error Handling |

---

## 8. Dependencies

| Dependency | Epic / Source | Type | Description |
|------------|--------------|------|-------------|
| **BSABANKSTA-131** | BSA Admin Persona (this epic) | Parent Epic | System messages edit capability is an admin-only function within the BSA Admin Persona suite |
| **BSABANKSTA-1531** | Application Frame and Global Navigation | UI Container | System message edit is accessed from the Admin area within the Application Header; navigation routing via F-005 |
| **BSABANKSTA-1532** | Application Frame (BSABANKSTA-1531) | Shared Header | Admin Settings and Profile Dropdown coexist in the Application Header — mutual coordination required |
| **BSABANKSTA-1305** | Batch 1 — Project Space Management | Data Context | Edited system messages may reference project space contexts; compliance message updates may relate to F-001 project lifecycle changes |
| **BSABANKSTA-1500** | BSA Admin Persona (this epic) | Shared Navigation | User Management Library shares admin navigation context within Application Header's Admin Settings area |
| **BSABANKSTA-1579-A** | This story (Create sub-story) | Sibling Dependency | Edit sub-story shares SystemMessageModal component, SystemMessagesList, form validation logic, and the system_messages data model with Create sub-story. Create must establish the data model and base components. |
| **CC-RQ-001** | Cross-Cutting Requirement | UI Pattern | Messages list implements lazy load / infinite scroll (no pagination) |
| **Auth0** | External Service | Authentication | Admin role validation sourced from Auth0 identity provider; JWT claims contain role information for RBAC enforcement |

---

## 9. Story Estimation Guidance

**Story Points: 5 (Fibonacci)**

**Rationale:**
- **Moderate-High Complexity**: Edit modal with pre-populated data, optimistic locking for conflict detection, confirmation dialogs for destructive actions (deactivate, delete), and version-based conflict resolution add complexity beyond a simple edit form.
- **Low-Moderate Uncertainty**: 3 Figma frames apply to the edit flow (confirmation dialog, success state, visibility config edit). The edit pattern mirrors the create modal, reducing UI uncertainty, but optimistic locking and conflict resolution add backend complexity.
- **Moderate Effort**: PUT and DELETE API endpoints with optimistic locking, EditMessageForm with pre-population and change tracking, ConfirmationDialog component, conflict warning UI, and comprehensive testing including concurrency scenarios.
- **5 points** reflects a well-scoped edit flow that benefits from shared components with the Create sub-story but introduces unique complexity through optimistic locking, confirmation dialogs, and delete functionality. The decomposition from the original 8-point story reduces scope while acknowledging the added concurrency handling.

---

## 10. Refinement Notes

### Global Rule #2 — Proactive Story Decomposition
**DECOMPOSITION APPLIED.** This is the Edit sub-story (STORY-BSABANKSTA-1579-B) of the decomposed BSABANKSTA-1579. See STORY-BSABANKSTA-1579-A above for the Create sub-story and full decomposition rationale.

**Summary:** The original combined story exceeded the 10-AC threshold (11 ACs total) and contained two distinct workflows (create and edit). This sub-story covers the edit, deactivation, and deletion workflows specifically.

### Global Rule #3 — Verb-Noun Title Standardization
**Original Jira Title:** "Admin | Manage Global System Messages | Create/Edit Modal BSABANKSTA-1579"
**Standardized Title (this sub-story):** "Edit Global System Message"
**Transformation:** The combined "Manage" verb was split into the specific verb "Edit" for this sub-story. "Global System Message" (singular) reflects the edit-one-at-a-time nature of the workflow. Organizational qualifiers removed per Verb-Noun standardization.

### Global Rule #4 — Pagination to Lazy Load Override
**MANDATORY OVERRIDE APPLIED.** The system messages list (shared context with Create sub-story) uses lazy load / infinite scroll with cursor-based data loading. No pagination controls are present. This override applies to the messages list view from which the edit action is triggered. Documented per CC-RQ-001.

### Global Rule #5 — Jira Source of Truth
Jira requirement text was used as the authoritative source for all acceptance criteria. Figma frames were used for visual reference only. Any discrepancies are documented in the Discrepancy Review section.

### Global Rule #6 — NFR Elevation
Performance targets extracted to the dedicated NFR section:
- Edit modal open with pre-populated data: < 500ms
- Update API response: < 2 seconds
- List refresh after edit: < 1 second
- Version conflict detection is an NFR for data integrity

### Global Rule #7 — Placeholder Management
Edited system messages may contain configurable placeholders such as `[Application Name]` in their content. These are cataloged in the parent epic file (`EPIC-BSABANKSTA-131-admin-persona.md`). The edit flow must preserve placeholder syntax in message content.

---

## 11. Discrepancy Review

**Figma frames analyzed for Edit flow:** `8304-123107`, `8304-123120`, `8304-123149`

The following analysis compares UI elements present in the Figma wireframes against the Jira requirement text for BSABANKSTA-1579:

| Element | Figma Frame | Finding |
|---------|------------|---------|
| Edit modal with pre-populated fields | `8304-123107` | Consistent with Jira requirements — modal displaying existing message data for editing |
| Confirmation dialog for destructive action | `8304-123120` | Consistent with Jira requirements — confirmation prompt for deactivation/deletion |
| Success/visibility state after edit | `8304-123149` | Consistent with Jira requirements — success confirmation and updated visibility display |

No discrepancies identified between Figma wireframes and Jira requirements for the Edit flow. All UI elements depicted in Figma frames `8304-123107`, `8304-123120`, and `8304-123149` are accounted for in the Jira requirement acceptance criteria.

> **Note:** Per Global Rule #5, any decorative elements or additional UI chrome present in Figma but not specified in Jira are excluded from acceptance criteria. The Figma frames serve as visual guidance only.

---

## 12. Generated UI Specifications

> **DESIGN REVIEW REQUIRED:** The following UI specifications were automatically generated based on the existing design system tokens, as no explicit mockup was provided for these elements. Please review for accuracy and design intent before development.

While the Edit flow benefits from 3 dedicated Figma frames covering the primary edit modal states, the following supplementary UI elements are required by the acceptance criteria but are not explicitly depicted in any Figma wireframe:

### 12.1 Conflict Warning — Version Mismatch

| UI Element | Design Token / Attribute | Value | Rationale |
|-----------|------------------------|-------|-----------|
| Container | `background`, `border`, `border-radius`, `padding` | `bg-yellow-50 border border-yellow-200 rounded-md p-4` | Warning pattern for non-destructive alert — semantic warning tokens (Priority 2: Apply semantic tokens) |
| Warning Icon | `color` | `text-yellow-500` | Semantic warning color consistent with design system |
| Warning Text | `color`, `font-size` | `text-yellow-700 text-sm` | Message: "This message has been modified by another user. Please reload and try again." |
| Reload Button | `background`, `color`, `padding`, `border-radius` | `bg-yellow-100 text-yellow-700 px-3 py-1.5 rounded-md hover:bg-yellow-200 text-sm font-medium` | Action button styled within warning context |

### 12.2 Confirmation Dialog — Deactivation

| UI Element | Design Token / Attribute | Value | Rationale |
|-----------|------------------------|-------|-----------|
| Overlay | `background` | `fixed inset-0 bg-black/50 z-60` | Stacks above the edit modal overlay (z-60 > z-50) |
| Dialog Container | `background`, `border-radius`, `shadow`, `max-width`, `padding` | `bg-white rounded-lg shadow-xl max-w-md w-full mx-4 p-6` | Reuse modal pattern at smaller size (Priority 1: Reuse existing component) |
| Title | `font-size`, `font-weight`, `color` | `text-lg font-semibold text-gray-900` | Clear dialog title: "Deactivate Message?" |
| Body Text | `font-size`, `color`, `margin` | `text-sm text-gray-600 mt-2` | Descriptive text explaining the action consequences |
| Confirm Button | `background`, `color`, `padding`, `border-radius` | `bg-red-600 text-white px-4 py-2 rounded-md hover:bg-red-700 font-medium` | Destructive action styling for deactivation confirm |
| Cancel Button | `background`, `color`, `border`, `padding`, `border-radius` | `bg-white text-gray-700 border border-gray-300 px-4 py-2 rounded-md hover:bg-gray-50 font-medium` | Non-destructive cancel styling |
| Button Layout | `display`, `gap`, `justify-content` | `flex justify-end gap-3 mt-6` | Right-aligned button group with standard spacing |

### 12.3 Confirmation Dialog — Deletion

| UI Element | Design Token / Attribute | Value | Rationale |
|-----------|------------------------|-------|-----------|
| Dialog Container | Same as Deactivation dialog | Same values | Reuse same dialog component with different content |
| Title | `font-size`, `font-weight`, `color` | `text-lg font-semibold text-gray-900` | Title: "Delete Message?" |
| Body Text | `font-size`, `color` | `text-sm text-gray-600 mt-2` | Warning text: "This action cannot be undone." with `text-red-600 font-medium` for emphasis |
| Confirm Button | `background`, `color` | `bg-red-600 text-white px-4 py-2 rounded-md hover:bg-red-700 font-medium` | Destructive action styling — same pattern as deactivation |

### 12.4 Error State — Edit/Delete API Failure

| UI Element | Design Token / Attribute | Value | Rationale |
|-----------|------------------------|-------|-----------|
| Container | `background`, `border`, `border-radius`, `padding` | `bg-red-50 border border-red-200 rounded-md p-4` | Reuse shared Error State / Retry pattern (Priority 1: Reuse existing component) |
| Error Icon | `color` | `text-red-500` | Semantic error color |
| Error Text | `color`, `font-size` | `text-red-700 text-sm` | Context-specific error: "Failed to update message. Please try again." or "Failed to delete message." |
| Retry Button | `background`, `color`, `padding` | `bg-red-100 text-red-700 px-3 py-1.5 rounded-md hover:bg-red-200 text-sm font-medium` | Retry action within error context |

### 12.5 Inline Validation Error Styling (Edit Context)

| UI Element | Design Token / Attribute | Value | Rationale |
|-----------|------------------------|-------|-----------|
| Error Text | `color`, `font-size`, `margin-top` | `text-red-600 text-sm mt-1` | Same as Create flow — shared validation error pattern (Priority 2: Apply semantic tokens) |
| Input Border (Error State) | `border-color` | `border-red-500 focus:ring-red-500` | Visual error indicator on the input field |

---

## 13. Figma Mockup Link

The following Figma frames from the BSA Wireframes file (`6fQyfvBUqImyavY8Fw47FV`) are associated with the Edit flow of this story:

| Frame | Description | URL |
|-------|------------|-----|
| **System Messages Modal — State 4** | Edit System Message modal — pre-populated form fields with existing message data for editing | [Figma Frame 8304-123107](https://www.figma.com/design/6fQyfvBUqImyavY8Fw47FV/BSA-Wireframes?node-id=8304-123107) |
| **System Messages Modal — State 5** | Confirmation dialog — destructive action confirmation for deactivation or deletion | [Figma Frame 8304-123120](https://www.figma.com/design/6fQyfvBUqImyavY8Fw47FV/BSA-Wireframes?node-id=8304-123120) |
| **System Messages Modal — State 6** | Edit success / updated visibility configuration display | [Figma Frame 8304-123149](https://www.figma.com/design/6fQyfvBUqImyavY8Fw47FV/BSA-Wireframes?node-id=8304-123149) |

> **Note:** Frames 1–3 (`8304-120310`, `8304-120342`, `8304-120318`) are associated with the Create flow and are documented in STORY-BSABANKSTA-1579-A above.

### All 6 Figma Frames (Complete Reference)

| Frame | Node ID | Description | Sub-Story |
|-------|---------|------------|-----------|
| State 1 | `8304-120310` | System Messages management page / list view | -A (Create) |
| State 2 | `8304-120342` | Create System Message modal — empty form | -A (Create) |
| State 3 | `8304-120318` | Create modal — visibility configuration panel | -A (Create) |
| State 4 | `8304-123107` | Edit System Message modal — pre-populated form | -B (Edit) |
| State 5 | `8304-123120` | Confirmation dialog — destructive action | -B (Edit) |
| State 6 | `8304-123149` | Edit success / updated visibility display | -B (Edit) |

---

## 14. Definition of Done

- [ ] All acceptance criteria (AC-1579-B-01 through AC-1579-B-10) pass BDD validation
- [ ] Unit tests written and passing — pytest for Flask API endpoints (PUT update, DELETE, version conflict 409)
- [ ] Unit tests written and passing — pytest for RBAC middleware (admin access granted, analyst denied for PUT/DELETE)
- [ ] Unit tests written and passing — pytest for optimistic locking logic (version match, version mismatch)
- [ ] Component tests written and passing — @testing-library/react for EditMessageForm, ConfirmationDialog, ConflictWarning
- [ ] E2E tests written and passing — Playwright for full edit flow (click edit → verify pre-populated → modify → save → verify list updated)
- [ ] E2E tests written and passing — Playwright for delete flow (click delete → confirm → verify removed from list)
- [ ] BDD tests written and passing — behave scenarios for all acceptance criteria
- [ ] Code reviewed and approved by at least one peer
- [ ] Modal edit flow works correctly: open pre-populated → modify fields → validate → save → close → list refreshed
- [ ] Optimistic locking and version conflict detection verified (concurrent edit scenario)
- [ ] Confirmation dialogs work correctly for deactivation and deletion actions
- [ ] Visibility configuration updates are persisted correctly
- [ ] RBAC enforcement verified — only BSA Administrator can edit/delete messages (UI and API)
- [ ] "Edit" and "Delete" actions hidden for non-admin users in the messages list
- [ ] Focus trap within edit modal and confirmation dialogs verified
- [ ] WCAG 2.1 AA accessibility compliance verified (aria-modal, role="alertdialog", focus management)
- [ ] NFRs validated: < 500ms edit modal open, < 2s update response, < 1s list refresh
- [ ] Figma frames `8304-123107`, `8304-123120`, `8304-123149` reviewed against implementation
- [ ] Generated UI Specifications for undepicted elements (conflict warning, confirmation dialogs) reviewed by design team
- [ ] Cross-epic dependency links verified (BSABANKSTA-1531 shared Application Header, BSABANKSTA-1579-A shared components)
- [ ] Decomposition documented in Refinement Notes
- [ ] All documentation complete and reviewed
