# Manage Profile Dropdown

**Story ID:** BSABANKSTA-1532
**Epic:** BSABANKSTA-1531 — Application Frame and Global Navigation
**Batch:** 2

---

## User Story

**As a** BSA Analyst or BSA Administrator (any authenticated user),
**I want** to access a profile dropdown menu in the Application Header that displays my profile summary, provides navigation to profile settings, and allows me to securely log out,
**So that** I can efficiently manage my account, access profile settings, and securely end my session from any page in the application without navigating away from my current context.

---

## INVEST Validation

| Principle | Validation | Notes |
|-----------|------------|-------|
| **Independent** | ✅ Pass | Can be developed independently of other dropdown/header components; only requires the Application Header shell for mounting. The Profile Dropdown is a self-contained UI component with its own state management. |
| **Negotiable** | ✅ Pass | Menu items, layout, and visual styling are negotiable; the core non-negotiable requirement is profile summary display, settings navigation access, and secure logout functionality. |
| **Valuable** | ✅ Pass | Provides essential user account management and secure session termination — critical for security compliance in BSA/AML banking applications where session integrity and audit-grade access logging are mandatory. |
| **Estimable** | ✅ Pass | 2 Figma frames (`8238-118805`, `8304-123177`) provide clear visual direction for both closed and expanded dropdown states, making estimation feasible with low visual uncertainty. |
| **Small** | ✅ Pass | Single dropdown component with a limited set of menu items (Profile Summary, Profile Settings, Logout) — well-scoped for a single sprint delivery. |
| **Testable** | ✅ Pass | Can verify dropdown toggle open/close, menu item rendering, profile data display, logout action, keyboard navigation, ARIA attributes, and role-conditional header layout through unit, component, and E2E tests. |

---

## Non-Functional Requirements

| Category | Requirement | Target | Rationale |
|----------|-------------|--------|-----------|
| **Performance — Toggle** | Dropdown open/close response time | < 100ms | Dropdown rendering is purely client-side with no network call; must feel instantaneous to the user. |
| **Performance — Profile Data** | Profile summary population from Auth0 session | < 1 second | Profile data (name, email, avatar) is retrieved from the cached Auth0 session; initial fetch on login, subsequent reads from local cache. |
| **Performance — Logout** | Auth0 logout completion and redirect to login page | < 2 seconds | Includes Auth0 session termination, local storage/cookie clearance, and redirect to the login page. |
| **Accessibility** | WCAG 2.1 AA compliance | Full compliance | Keyboard navigable dropdown (Escape to close, Arrow keys to navigate items, Enter/Space to select), proper `aria-expanded`, `aria-haspopup`, `aria-label` attributes, focus trap within dropdown when open, and focus return to trigger on close. |
| **Security** | Session invalidation on logout | Complete token invalidation | Logout must invalidate Auth0 session token, clear local storage, clear session cookies, and ensure no residual authentication state remains in the browser. |
| **Responsiveness** | Desktop-first design | Desktop and responsive web | No mobile-specific layouts per CC-RQ-003. The Profile Dropdown must function correctly on standard desktop viewport widths (≥ 1024px) and degrade gracefully on smaller screens. |
| **Reliability** | Dropdown state consistency | Zero stuck states | Rapid toggle, click-outside, and keyboard interactions must never leave the dropdown in an inconsistent open/closed state. |

---

## Acceptance Criteria

### AC1: Profile Dropdown Toggle — Open

```gherkin
Scenario: User opens the Profile Dropdown menu
  Given the user is authenticated and on any page within the application
  And the Application Header is displayed with the user's profile avatar or initials
  When the user clicks on the profile avatar/initials or profile trigger button in the Application Header
  Then the Profile Dropdown menu is displayed below the trigger button
  And the dropdown is positioned absolute with a white background, rounded corners, and elevated shadow styling
  And the aria-expanded attribute is set to "true" on the trigger element
  And the aria-haspopup attribute is set to "true" on the trigger element
```

### AC2: Profile Dropdown Toggle — Close

```gherkin
Scenario: User closes the Profile Dropdown menu
  Given the Profile Dropdown menu is currently open
  When the user clicks outside the dropdown area OR presses the Escape key
  Then the dropdown menu is closed and hidden from the DOM visibility
  And the aria-expanded attribute is set to "false" on the trigger element
  And focus returns to the profile trigger button
```

### AC3: Profile Summary Display

```gherkin
Scenario: User views their profile summary in the dropdown
  Given the Profile Dropdown menu is open
  When the user views the dropdown content
  Then the user's full name (sourced from Auth0 session) is displayed at the top of the dropdown
  And the user's email address (sourced from Auth0 session) is displayed below the name
  And the user's profile avatar or generated initials are displayed alongside the name and email
```

### AC4: Profile Settings Navigation

```gherkin
Scenario: User navigates to Profile Settings from the dropdown
  Given the Profile Dropdown menu is open
  And the user can see the "Profile Settings" or "My Profile" menu item
  When the user clicks on the "Profile Settings" menu item
  Then the user is navigated to the profile management page via client-side routing
  And the Profile Dropdown menu closes automatically
```

### AC5: Logout Action

```gherkin
Scenario: User logs out via the Profile Dropdown
  Given the Profile Dropdown menu is open
  And the user can see the "Logout" or "Sign Out" menu item
  When the user clicks on the "Logout" menu item
  Then the Auth0 session is terminated and the access token is invalidated
  And all local storage and session cookies related to authentication are cleared
  And the user is redirected to the login page
```

### AC6: Keyboard Navigation

```gherkin
Scenario: User navigates the Profile Dropdown using keyboard
  Given the Profile Dropdown menu is open
  When the user presses the Tab key or Arrow Down key
  Then focus moves to the next menu item sequentially
  When the user presses the Arrow Up key
  Then focus moves to the previous menu item
  When the user presses Enter or Space on a focused menu item
  Then the focused menu item action is activated
  When the user presses the Escape key
  Then the dropdown menu closes and focus returns to the trigger button
```

### AC7: Role-Conditional Header Layout — Administrator

```gherkin
Scenario: BSA Administrator sees full header with Admin Settings and Profile Dropdown
  Given a user with the BSA Administrator role is authenticated
  When the Application Header renders on any page
  Then both the Profile Dropdown trigger AND the Admin Settings option are visible in the Application Header
  And the Admin Settings option is positioned as a sibling element to the Profile Dropdown trigger within the header flex layout
```

### AC8: Role-Conditional Header Layout — Analyst

```gherkin
Scenario: BSA Analyst sees header with Profile Dropdown only
  Given a user with the BSA Analyst role (non-admin) is authenticated
  When the Application Header renders on any page
  Then only the Profile Dropdown trigger is visible in the Application Header
  And the Admin Settings option is NOT displayed in the header
```

### AC9: Profile Dropdown Persistent Availability

```gherkin
Scenario: Profile Dropdown is always accessible from the sticky Application Header
  Given the user is authenticated
  When the user navigates to any page within the application
  Then the Application Header remains visible at the top of the viewport (sticky positioning)
  And the Profile Dropdown trigger button is always accessible within the header
```

---

## Sub-Tasks

### Model

| Sub-Task ID | Description | Technical Details |
|-------------|-------------|-------------------|
| ST-1532-M1 | Define user profile data model from Auth0 session | Create a data model/type for user profile data: `name` (string), `email` (string), `avatar_url` (string, optional), `role` (enum: BSA Administrator, BSA Analyst). Profile data is sourced from the Auth0 identity provider — no dedicated MongoDB collection required. |
| ST-1532-M2 | Define role enumeration model | Create an enumeration for user roles: `BSA_ADMINISTRATOR`, `BSA_ANALYST`. This model drives role-conditional rendering logic across the Application Header. |
| ST-1532-M3 | Define profile dropdown menu item model | Create a data model for dropdown menu items: `label` (string), `icon` (optional), `action` (navigation route or function), `divider_after` (boolean). Used to render the menu items dynamically. |

### API

| Sub-Task ID | Description | Technical Details |
|-------------|-------------|-------------------|
| ST-1532-A1 | Implement GET `/api/auth/profile` endpoint | Flask blueprint route that returns the current user's profile data from the Auth0 session (name, email, avatar, role). Validates JWT token and returns 401 if session is invalid or expired. |
| ST-1532-A2 | Implement POST `/api/auth/logout` endpoint | Flask blueprint route that handles server-side session cleanup: invalidate server-side session references, clear any cached user data, and return a success response with Auth0 logout redirect URL. |
| ST-1532-A3 | Implement role validation endpoint | Flask endpoint or middleware that validates the current user's role from the Auth0 JWT token claims for header conditional rendering. Returns role information as part of the profile response or as a separate lightweight endpoint. |

### Component

| Sub-Task ID | Description | Technical Details |
|-------------|-------------|-------------------|
| ST-1532-C1 | Implement `ProfileDropdown` React component | Absolute-positioned dropdown container using TailwindCSS: `bg-white rounded-md shadow-lg` with `divide-y` for section separation. Manages open/close state, click-outside detection, and keyboard event handlers. |
| ST-1532-C2 | Implement profile trigger element in `ApplicationHeader` | Profile avatar/initials trigger button within the `sticky top-0 bg-white border-b shadow-sm` flex layout header. Includes `aria-expanded` and `aria-haspopup` attributes. |
| ST-1532-C3 | Implement `ProfileSummary` sub-component | Displays user's full name, email address, and avatar/initials at the top of the dropdown. Handles missing profile fields gracefully with fallback displays. |
| ST-1532-C4 | Implement `DropdownMenuItem` component | Reusable menu item component for "Profile Settings" and "Logout" actions. Supports icon, label, keyboard focus styling, and divider rendering. |
| ST-1532-C5 | Implement keyboard navigation handler | Keyboard event handlers for Escape (close), Arrow Up/Down (navigate items), Enter/Space (activate), and Tab (sequential navigation). Implements focus trap within the open dropdown. |
| ST-1532-C6 | Implement click-outside detection | Custom hook or event listener to detect clicks outside the dropdown boundary and close the menu. Must handle portal-rendered elements and nested click events correctly. |
| ST-1532-C7 | Apply design tokens from Figma BSA Wireframes | Apply design tokens from Figma file `6fQyfvBUqImyavY8Fw47FV` for colors, typography, spacing, border radii, and shadow effects to all dropdown components. |

### Logic

| Sub-Task ID | Description | Technical Details |
|-------------|-------------|-------------------|
| ST-1532-L1 | Integrate Auth0 SPA SDK for session data and logout | Use `@auth0/auth0-react` SDK hooks (`useAuth0`) to retrieve user profile data from the Auth0 session and invoke the `logout()` method for session termination. |
| ST-1532-L2 | Implement role-based conditional rendering logic | Logic to show/hide the Admin Settings header element based on the user's role. BSA Administrator sees Admin Settings + Profile Dropdown; BSA Analyst sees only Profile Dropdown. |
| ST-1532-L3 | Implement dropdown state management | React state management (open/close boolean) with proper handling for debounce on rapid toggles, mutual exclusion with other header dropdowns (Admin Settings), and animation timing. |
| ST-1532-L4 | Implement focus management and focus trap | Focus management utilities: trap focus within the dropdown when open, restore focus to the trigger button on close, and manage focus ring visibility for keyboard navigation. |
| ST-1532-L5 | Integrate React Router for profile settings navigation | Use React Router's `useNavigate` hook to programmatically navigate to the profile settings page when the "Profile Settings" menu item is activated. |
| ST-1532-L6 | Implement session invalidation and redirect logic | On logout: clear Auth0 tokens, clear localStorage/sessionStorage, clear authentication cookies, and redirect to the login page. Handle logout failures with retry and forced cleanup fallback. |

### Testing

| Sub-Task ID | Description | Technical Details |
|-------------|-------------|-------------------|
| ST-1532-T1 | Unit tests for profile API endpoint (pytest) | Test GET `/api/auth/profile` for: valid token returns profile data, expired token returns 401, missing fields handled gracefully. Test POST `/api/auth/logout` for: successful session cleanup, failure handling. |
| ST-1532-T2 | Component tests for ProfileDropdown (@testing-library/react) | Test: dropdown opens on trigger click, closes on outside click, closes on Escape key, menu items render correctly, profile summary displays name and email, keyboard navigation moves focus between items. |
| ST-1532-T3 | BDD acceptance tests (behave) | Implement Given/When/Then scenarios matching all 9 acceptance criteria. Validate dropdown toggle, profile display, navigation, logout, keyboard nav, role-conditional rendering, and persistent availability. |
| ST-1532-T4 | E2E tests for full dropdown interaction flow (Playwright) | End-to-end tests covering: navigate to app → click profile trigger → verify dropdown opens → verify profile data → click Profile Settings → verify navigation → return → click Logout → verify redirect to login. |
| ST-1532-T5 | Accessibility tests for ARIA attributes and keyboard navigation | Automated accessibility tests verifying: `aria-expanded` toggles correctly, `aria-haspopup` is present, focus trap works within dropdown, keyboard navigation sequence is correct, focus returns to trigger on close. |
| ST-1532-T6 | Role-conditional rendering tests | Test that BSA Administrator role renders both Profile Dropdown trigger and Admin Settings in the header. Test that BSA Analyst role renders only Profile Dropdown trigger without Admin Settings. |

---

## Edge Cases

| # | Edge Case | Scenario | Expected Behavior | Category |
|---|-----------|----------|-------------------|----------|
| 1 | **Expired Auth0 Session** | User clicks the profile dropdown trigger, but the Auth0 session token has expired since the page was loaded. | System detects token expiry during profile data access, closes any open dropdown, displays a session expiration notification, and redirects the user to the login page with the return URL preserved for post-login redirect. | Authentication Failure |
| 2 | **Logout Failure** | Auth0 logout API call fails due to network error or Auth0 service unavailability. | Display an error notification to the user with a retry option. If the retry also fails, force-clear all local authentication tokens (localStorage, sessionStorage, cookies) and redirect to the login page to ensure the user is not left in an ambiguous authentication state. | Network / Service Failure |
| 3 | **Missing Profile Data** | Auth0 session returns incomplete profile data — for example, no avatar URL, no email, or no display name. | Display graceful fallbacks: generated initials (first letter of first and last name) for missing avatar, "Email not available" placeholder text for missing email, and "User" as default display name. Never display blank or broken UI elements. | Data Incompleteness |
| 4 | **Rapid Toggle** | User rapidly clicks the profile trigger button multiple times in quick succession (e.g., double-click or stress testing). | Dropdown state management handles toggle debounce correctly. The dropdown does not enter a double-open, stuck-open, or flickering state. Each click toggles the state once, and rapid sequences resolve to a stable final state. | User Interaction |
| 5 | **Simultaneous Header Dropdowns** | On an admin user's Application Header, both the Profile Dropdown and the Admin Settings dropdown are triggered in close succession (e.g., user clicks Admin Settings then immediately clicks Profile). | Implement mutual exclusion pattern: only one header dropdown can be open at any time. Opening one dropdown automatically closes any other open dropdown. Managed through a shared header dropdown state context. | Cross-Component Interaction |

---

## Dependencies

### Cross-Epic Dependencies

| Dependency | Epic | Type | Description |
|------------|------|------|-------------|
| **BSABANKSTA-1531** (Application Frame) | This epic | Parent Epic | Profile Dropdown is embedded within the Application Header component, which is part of the Application Frame and Global Navigation epic. |
| **BSABANKSTA-131** (BSA Admin Persona) | BSABANKSTA-131 | Shared Application Header (Mutual) | **CRITICAL**: Admin Settings (BSABANKSTA-1500) and Profile Dropdown (this story) coexist in the same Application Header bar. Both components must coordinate UI space, interaction patterns (only one dropdown open at a time), and share a common header layout context. This is a bidirectional dependency. |
| **BSABANKSTA-1500** (User Management Library) | BSABANKSTA-131 | Sibling Component | Admin Settings and Profile Dropdown are sibling components within the Application Header flex layout. They share the header dropdown state context for mutual exclusion behavior. |
| **BSABANKSTA-1305** (Create/Modify Project Space) | BSABANKSTA-1305 (Batch 1) | Navigation Context | Profile Dropdown is part of the persistent Application Header visible across all project space views. The header provides consistent navigation context while users interact with project space features. |
| **Auth0** (External Service) | External | Service Dependency | Login/logout flow and profile data (name, email, avatar, role) are sourced from the Auth0 identity provider via the `@auth0/auth0-react` SPA SDK. |
| **Global Navigation Framework** (F-005-RQ-002) | BSABANKSTA-1531 | Architecture Dependency | The Application Header containing the Profile Dropdown is a core structural element of the Global Navigation shell that persists across all application views. |
| **Lazy Load / Infinite Scroll** (CC-RQ-001) | Cross-cutting | UI Pattern | Not directly applicable to this dropdown component, but the Profile Dropdown exists within the same Application Header that governs pages containing infinite scroll list views. |

### Story-Level Dependencies

| This Story | Related Story | Direction | Dependency Detail |
|------------|---------------|-----------|-------------------|
| BSABANKSTA-1532 (Profile Dropdown) | BSABANKSTA-1500 (Admin Settings, in BSABANKSTA-131) | Bidirectional | Shared Application Header — must coordinate dropdown mutual exclusion, header flex layout space allocation, and role-conditional visibility |
| BSABANKSTA-1532 (Profile Dropdown) | BSABANKSTA-1509 (Help & Support, in BSABANKSTA-1531) | Outbound | Profile Dropdown is accessible from the same header visible on the Help & Support page |
| BSABANKSTA-1532 (Profile Dropdown) | BSABANKSTA-1536 (All Confirmations, in BSABANKSTA-1531) | Outbound | Profile Dropdown is accessible from the same header visible on the All Confirmations page |

---

## Story Estimation Guidance

**Story Points: 5** (Fibonacci Scale)

| Factor | Assessment | Impact |
|--------|------------|--------|
| **Complexity** | Moderate | Dropdown UI with keyboard navigation, focus management, ARIA attributes, and role-conditional rendering requires careful implementation of accessibility patterns and state management. |
| **Uncertainty** | Moderate | Shared Application Header coordination with Admin Settings (BSABANKSTA-1500 in BSABANKSTA-131 epic) requires cross-team integration planning and mutual exclusion pattern design. |
| **Effort** | Moderate | Auth0 SPA SDK integration for session data and logout, role-based conditional rendering logic, comprehensive keyboard navigation, focus trap implementation, and click-outside detection. |
| **Visual Clarity** | Low Risk | 2 Figma frames (`8238-118805` for default header, `8304-123177` for expanded dropdown) reduce visual ambiguity significantly. |
| **Integration Risk** | Moderate | Cross-epic header coordination with BSABANKSTA-1500 adds integration complexity. Auth0 external service dependency introduces potential configuration and environment-specific challenges. |

**Rationale:** 5 story points reflects solid component work with non-trivial accessibility requirements (keyboard navigation, focus trap, ARIA attributes), Auth0 integration complexity, role-conditional rendering, and the shared Application Header coordination overhead. The 2 Figma frames mitigate visual uncertainty, but the cross-epic dependency on Admin Settings and the mutual dropdown exclusion pattern add integration complexity that elevates this beyond a simple 3-point dropdown implementation.

---

## Refinement Notes

### Global Rule #2 — Proactive Story Decomposition
- **Status:** No decomposition required.
- **Rationale:** This story contains 9 acceptance criteria (AC1–AC9) with a single user workflow (profile dropdown interaction — open, view, navigate, logout). The AC count is below the >10 threshold defined by Global Rule #2, and there are no distinct multi-workflow patterns (e.g., separate create/edit/delete flows) that would warrant decomposition into sub-stories. The story remains a cohesive unit focused on the Profile Dropdown component lifecycle.

### Global Rule #3 — Verb-Noun Title Standardization
- **Original Jira Title:** "Application Header | Profile Dropdown Menu BSABANKSTA-1532"
- **Standardized Title:** "Manage Profile Dropdown"
- **Transformation Rationale:** Applied Verb-Noun format. The organizational qualifier "Application Header |" was removed as it serves as context metadata (the parent epic and Application Header context are documented in the story's epic reference and dependencies). The verb "Manage" was selected to reflect the full scope of user interactions: viewing profile summary, navigating to settings, and executing logout.

### Global Rule #4 — Pagination to Lazy Load Override
- **Status:** No override required.
- **Rationale:** This story describes a profile dropdown component, not a list/table view. No pagination patterns exist in the source requirements for this story. No pagination references were detected, and no lazy load / infinite scroll replacement is applicable.

### Global Rule #5 — Jira Source of Truth
- **Status:** Applied.
- **Action:** Jira requirement text was used as the authoritative source of truth for all acceptance criteria. Figma frames (`8238-118805` and `8304-123177`) were analyzed for visual guidance only. Any Figma-only elements not reflected in the Jira requirements are documented in the Discrepancy Review section below.

### Global Rule #6 — NFR Elevation
- **Status:** Applied.
- **Action:** Performance targets were extracted from the requirements and elevated into the dedicated Non-Functional Requirements section: < 100ms dropdown toggle, < 1 second profile data fetch, < 2 seconds logout flow completion. Accessibility (WCAG 2.1 AA) and security (session invalidation) requirements were also elevated to NFRs.

### Global Rule #7 — Placeholder Management
- **Status:** Not directly applicable to this component.
- **Rationale:** The Profile Dropdown does not directly use the `[Application Name]` configurable placeholder. However, the Application Header parent component that hosts the Profile Dropdown may display the application title, which would reference the `[Application Name]` placeholder. This placeholder is cataloged at the epic level in the EPIC-BSABANKSTA-1531 System Placeholders section.

### Global Rule #8 — Generated UI Specifications
- **Status:** Partially applied.
- **Action:** While the 2 Figma frames cover the primary dropdown states (closed and open), three undepicted supplementary UI elements were identified and documented in the Generated UI Specifications section: focus ring indicator, dropdown transition animation, and logout error notification state.

---

## Discrepancy Review

No discrepancies identified between Figma wireframes and Jira requirements.

The two Figma frames (`8238-118805` — default header state; `8304-123177` — expanded profile dropdown) were analyzed against the Jira requirement text for BSABANKSTA-1532. All UI elements depicted in the Figma wireframes (profile trigger area, dropdown container, profile summary section, menu items, dropdown positioning) are consistent with the Jira requirement descriptions.

**Review Notes:**
- Both Figma frames align with the Jira-specified scope: profile summary display, settings navigation, and logout functionality within the Application Header dropdown.
- No additional UI elements were found in the Figma frames that are absent from the Jira requirements.
- Per Global Rule #5, Jira requirement text remains the authoritative source of truth. If future Figma revisions introduce elements not present in the Jira requirements, those elements should be flagged here for review rather than included in acceptance criteria.

---

## Generated UI Specifications

> **DESIGN REVIEW REQUIRED:** The following UI specifications were automatically generated based on the existing design system tokens, as no explicit mockup was provided for these elements. Please review for accuracy and design intent before development.

The Profile Dropdown's primary states (closed header and expanded dropdown) are covered by the 2 Figma frames. However, the following supplementary UI elements are required by the acceptance criteria and edge cases but are not depicted in the static Figma wireframes:

### Undepicted UI Elements

| UI Element | Design Token / Attribute | Value | Rationale |
|------------|--------------------------|-------|-----------|
| **Focus Ring (Keyboard Navigation)** | `ring-width`, `ring-color`, `ring-offset` | `ring-2 ring-blue-500 ring-offset-2` | Apply semantic focus token (Priority 2). Standard focus indicator for keyboard navigation accessibility per WCAG 2.1 AA. Blue focus ring provides sufficient contrast on white dropdown background. Applied to the profile trigger button and each menu item when focused via keyboard. |
| **Dropdown Open/Close Transition** | `transition`, `duration`, `easing` | `transition-all duration-150 ease-in-out` with `opacity` and `transform: scale` | Apply semantic animation token (Priority 2). Smooth 150ms transition for dropdown appearing/disappearing provides visual feedback without introducing perceptible delay. Uses opacity fade (0 → 1) combined with subtle scale transform (95% → 100%) for a polished open effect. |
| **Logout Error Notification** | `background`, `border`, `border-radius`, `padding`, `text-color` | `bg-red-50 border border-red-200 rounded-md p-4 text-red-700` | Reuse shared Error State pattern (Priority 1). Consistent with the application-wide error notification pattern defined in the design system. Includes a retry button styled with `text-red-600 hover:text-red-800 font-medium underline`. Displayed inline below the dropdown or as a toast notification when logout fails. |
| **Menu Item Hover State** | `background-color` | `hover:bg-gray-50` | Apply semantic hover token (Priority 2). Subtle background color change on hover provides visual feedback for interactive menu items. Consistent with standard dropdown menu interaction patterns. |
| **Menu Item Active/Pressed State** | `background-color` | `active:bg-gray-100` | Apply semantic active token (Priority 2). Slightly darker background on press/click confirms the user's action before navigation or logout executes. |
| **Profile Avatar Fallback (Initials)** | `background`, `text-color`, `border-radius`, `font-size`, `font-weight` | `bg-blue-100 text-blue-700 rounded-full text-sm font-semibold` with centered flex layout | Apply semantic token (Priority 2). Circular container with user initials serves as fallback when no avatar URL is available from Auth0. Blue color palette aligns with the primary brand color from the design token manifest. |

**Note:** All primary Profile Dropdown UI elements (container styling, profile summary area, menu items, dropdown positioning relative to the trigger) are defined in the Figma frames and should be implemented directly from those visual specifications. The elements listed above are supplementary interaction states and fallback patterns that augment the Figma-defined design.

---

## Figma Mockup Link

| Frame | Description | URL |
|-------|-------------|-----|
| **Profile Dropdown — Screen 1** | Default Application Header state showing the profile trigger area (avatar/initials) in its closed/resting state within the sticky header bar. | [Figma Frame 8238-118805](https://www.figma.com/design/6fQyfvBUqImyavY8Fw47FV/BSA-Wireframes?node-id=8238-118805) |
| **Profile Dropdown — Screen 2** | Expanded Profile Dropdown state showing the open dropdown menu with profile summary (name, email), menu items (Profile Settings, Logout), and dropdown positioning relative to the Application Header. | [Figma Frame 8304-123177](https://www.figma.com/design/6fQyfvBUqImyavY8Fw47FV/BSA-Wireframes?node-id=8304-123177) |

---

## Definition of Done

- [ ] All 9 acceptance criteria (AC1–AC9) pass BDD validation
- [ ] Unit tests written and passing — pytest for API endpoints (`/api/auth/profile`, `/api/auth/logout`)
- [ ] Unit tests written and passing — Vitest for React component logic
- [ ] Component tests written and passing — @testing-library/react for `ProfileDropdown`, `ProfileSummary`, `DropdownMenuItem`
- [ ] E2E tests written and passing — Playwright for full dropdown interaction flow (open, navigate, logout)
- [ ] BDD tests written and passing — behave for all acceptance criteria scenarios
- [ ] Code reviewed and approved by at least one peer reviewer
- [ ] Profile dropdown toggle works correctly — opens on click, closes on outside click and Escape key
- [ ] Auth0 profile data displays correctly — user's name and email rendered from Auth0 session
- [ ] Profile avatar displays correctly — avatar image when available, generated initials as fallback
- [ ] Logout action successfully terminates Auth0 session, clears local storage/cookies, and redirects to login
- [ ] Keyboard navigation fully functional — Tab, Arrow Up/Down, Escape, Enter/Space all behave per AC6
- [ ] ARIA attributes correctly implemented — `aria-expanded`, `aria-haspopup`, `aria-label` on trigger and menu elements
- [ ] Role-conditional rendering verified — BSA Administrator sees Admin Settings + Profile Dropdown; BSA Analyst sees only Profile Dropdown
- [ ] Only one header dropdown open at a time — mutual exclusion with Admin Settings (BSABANKSTA-1500) verified
- [ ] WCAG 2.1 AA accessibility compliance verified via automated and manual testing
- [ ] NFRs validated — < 100ms toggle response, < 1s profile data fetch, < 2s logout completion
- [ ] Design review completed — implementation verified against Figma frames `8238-118805` and `8304-123177`
- [ ] Shared Application Header coordination verified with BSABANKSTA-1500 (Admin Settings in BSABANKSTA-131 epic)
- [ ] Cross-epic dependency links verified bidirectionally with BSABANKSTA-131, BSABANKSTA-1305, BSABANKSTA-1500
- [ ] All edge cases handled — expired session, logout failure, missing profile data, rapid toggle, simultaneous dropdowns
- [ ] Documentation updated — story file complete with all 14 sections
