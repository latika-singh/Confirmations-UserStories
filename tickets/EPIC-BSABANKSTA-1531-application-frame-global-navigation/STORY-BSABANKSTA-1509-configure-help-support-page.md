# Configure Help & Support Page

**Story ID:** BSABANKSTA-1509
**Epic:** BSABANKSTA-1531 — Application Frame and Global Navigation
**Batch:** 2

---

## User Story

**As a** BSA Analyst or BSA Administrator (any authenticated user),
**I want** to access a configurable help and support page with contextual help resources and navigation categories,
**So that** I can find application-specific guidance and support resources without leaving the application, reducing onboarding time and support requests.

---

## INVEST Validation

| Principle | Validation | Notes |
|-----------|------------|-------|
| **Independent** | ✅ Pass | Can be developed independently of other stories; only requires the Global Navigation Framework (F-005-RQ-002) for routing. The Help & Support page is a self-contained content page with its own API endpoint and configurable data source. |
| **Negotiable** | ✅ Pass | Content structure, layout, and category organization are negotiable; the core non-negotiable requirement is configurable help content that resolves deployment-specific placeholders and is accessible via the global navigation. |
| **Valuable** | ✅ Pass | Provides user self-service support directly within the application, reducing manual support overhead, shortening user onboarding time, and minimizing context switching away from compliance workflows. |
| **Estimable** | ✅ Pass | Single Figma frame (`7542-109669`) and clear content-page layout make estimation straightforward with low visual uncertainty. Technology decisions (configurable key-value content, placeholder resolution) are well-understood patterns. |
| **Small** | ✅ Pass | Single content page with configurable blocks and placeholder resolution — well-scoped for a single sprint delivery. No complex data aggregation, no multi-step workflows, no cross-project data dependencies. |
| **Testable** | ✅ Pass | Can verify content rendering, navigation access, placeholder resolution, category display, accessibility compliance, error/empty states, and configurable deployment variable behavior through unit, component, BDD, and E2E tests. |

---

## Non-Functional Requirements

| Category | Requirement | Target | Rationale |
|----------|-------------|--------|-----------|
| **Performance — Page Load** | Initial content render time | < 3 seconds | Help content is fetched from a configuration API endpoint; the page must render first meaningful paint within 3 seconds including content retrieval and placeholder resolution. |
| **Performance — Content Fetch** | API response time for help content retrieval | < 1 second | The `/api/help-support/content` endpoint serves pre-configured content from MongoDB; response should be fast with optional caching. |
| **Accessibility** | WCAG 2.1 AA compliance | Full compliance | All content must be structured with semantic HTML (`<main>`, `<section>`, `<article>`, `<nav>`), proper heading hierarchy (`h1`–`h3`), ARIA landmark roles, and sufficient color contrast (≥ 4.5:1 for normal text). |
| **Content Format** | Configurable content support | Markdown or rich text | Help content must support structured formatting (headings, lists, links, emphasis) through markdown or rich text rendering in the content blocks. |
| **Responsiveness** | Desktop-first responsive layout | Desktop and responsive web | No mobile-specific layouts per CC-RQ-003. Page must render correctly on standard desktop viewport widths (≥ 1024px) and degrade gracefully on smaller screens. |
| **Configuration** | Deployment-variable-driven content | Zero hard-coded content | All help content, including `[Application Name]`, `[Support Email]`, and environment-specific URLs, must be deployable via configurable environment variables — not hard-coded in the application source. |
| **Reliability** | Graceful degradation on content failure | Fallback message display | If the help content API is unavailable or returns empty, the page must display a meaningful fallback message rather than a blank page or unhandled error. |

---

## Acceptance Criteria

### AC1: Help Page Navigation Access

```gherkin
Scenario: User navigates to the Help & Support page via global navigation
  Given the user is authenticated and on any page within the application
  And the Global Navigation Framework is rendered with available navigation items
  When the user clicks on the "Help & Support" navigation item in the global navigation
  Then the application navigates to the Help & Support page route
  And the Help & Support page is displayed with configurable content
  And the page title reflects "Help & Support" or the configured equivalent
```

### AC2: Configurable Content Rendering

```gherkin
Scenario: Help & Support page renders deployment-specific configurable content
  Given the Help & Support page is loaded
  And the system has configurable help content stored in the configuration data store
  When the page renders
  Then all configurable content blocks display deployment-specific content
  And the `[Application Name]` placeholder is resolved to the configured application name value
  And all environment-specific URLs and references are resolved to their configured values
  And the content is displayed within a readable, centered layout (max-w-prose mx-auto pattern)
```

### AC3: Navigation Categories Display

```gherkin
Scenario: Help topics are organized into navigable categories
  Given the Help & Support page is loaded
  And help content is organized into multiple categories
  When the user views the page content
  Then categorized help topics are displayed with distinct section headings
  And each category is navigable via anchor links or a sidebar category navigation
  And the category structure follows a logical hierarchy from general to specific topics
```

### AC4: Contextual Help Resources

```gherkin
Scenario: User selects a specific help category to view relevant content
  Given the user is on the Help & Support page
  And categorized help topics are displayed
  When the user selects a help category by clicking on a category heading or navigation link
  Then the relevant help content for that category is displayed or scrolled into view
  And the selected category is visually highlighted as the active selection
  And the browser URL hash or scroll position updates to reflect the selected category
```

### AC5: Help Page Accessibility

```gherkin
Scenario: Help & Support page meets accessibility requirements
  Given the Help & Support page is loaded with configurable content
  When the page is accessed by assistive technology (screen reader, keyboard-only navigation)
  Then all content is structured with semantic HTML elements (main, section, article, nav)
  And heading hierarchy is properly nested (h1 for page title, h2 for categories, h3 for sub-topics)
  And all interactive elements (links, navigation items) are keyboard accessible
  And ARIA landmark roles are applied to content regions
  And color contrast meets WCAG 2.1 AA minimum ratio (≥ 4.5:1 for normal text, ≥ 3:1 for large text)
```

### AC6: Placeholder Resolution

```gherkin
Scenario: All configurable placeholders are resolved to deployment-specific values
  Given the system has configurable deployment variables set for the current environment
  And the Help & Support page content contains placeholder tokens (e.g., [Application Name], [Support Email])
  When the Help & Support page renders
  Then all `[Application Name]` placeholders are resolved to the configured application name
  And all `[Support Email]` placeholders are resolved to the configured support email address
  And all environment-specific URL placeholders are resolved to the configured URLs
  And no raw placeholder tokens (e.g., "[Application Name]") are visible to the user
```

### AC7: Content Loading State

```gherkin
Scenario: Loading indicator is displayed while help content is being fetched
  Given the user has navigated to the Help & Support page
  When the help content API request is in progress
  Then a loading indicator or skeleton content is displayed to the user
  And the loading state does not block interaction with the Application Header or global navigation
  And upon successful content retrieval, the loading indicator is replaced with the rendered content
```

---

## Sub-Tasks

### Model

| Sub-Task ID | Description | Technical Details |
|-------------|-------------|-------------------|
| M-1509-01 | Define MongoDB collection schema for help content configuration | Create a `help_content` collection in MongoDB with fields: `category_id` (string, unique), `category_title` (string), `display_order` (integer), `content_blocks` (array of objects with `block_type`, `heading`, `body`, `links`), `is_active` (boolean), `created_at` (datetime), `updated_at` (datetime). Index on `is_active` and `display_order` for efficient retrieval. |
| M-1509-02 | Create Marshmallow schema for help content validation | Define `HelpContentSchema` and `ContentBlockSchema` Marshmallow schemas for input validation and serialization. Validate required fields (`category_title`, `content_blocks`), enforce string length limits, and validate `block_type` enum values (text, link_list, faq, contact). |
| M-1509-03 | Define data model for system placeholders | Create a `system_config` collection (or shared configuration collection) with key-value pairs for deployment variables: `application_name`, `support_email`, `help_content_url`, and environment-specific URLs. Used by the placeholder resolution service. |

### API

| Sub-Task ID | Description | Technical Details |
|-------------|-------------|-------------------|
| A-1509-01 | Create Flask blueprint for help support routes | Define `help_support_bp` Flask blueprint with base URL prefix `/api/help-support`. Register in the application factory. |
| A-1509-02 | Implement GET `/api/help-support/content` endpoint | Returns all active help content categories with resolved placeholders. Query `help_content` collection where `is_active=true`, ordered by `display_order`. Apply placeholder resolution before response. Response contract: `{ "categories": [{ "category_id": string, "category_title": string, "content_blocks": [...] }], "placeholders_resolved": boolean }`. |
| A-1509-03 | Implement request/response error handling | Return `200 OK` with content on success, `500 Internal Server Error` with structured error message on database failure, `401 Unauthorized` if user is not authenticated (JWT validation via Auth0). Include CORS headers per Flask-CORS configuration. |

### Component

| Sub-Task ID | Description | Technical Details |
|-------------|-------------|-------------------|
| C-1509-01 | Create `HelpSupportPage` React page component | Top-level page component using TailwindCSS layout: `max-w-prose mx-auto px-4 py-8`. Fetches help content on mount, renders categories, handles loading/error/empty states. Registered as a route in React Router at `/help-support`. |
| C-1509-02 | Create `HelpCategoryNav` sidebar navigation component | Sticky sidebar or in-page navigation listing all help categories as anchor links. Active category highlighted with visual indicator. Uses semantic `<nav>` element with `aria-label="Help categories"`. |
| C-1509-03 | Create `HelpContentBlock` reusable content rendering component | Renders individual content blocks based on `block_type`: text (paragraph/rich text), link_list (ordered list of resource links), faq (collapsible question/answer pairs), contact (support contact information with resolved `[Support Email]` placeholder). |
| C-1509-04 | Create `HelpPageSkeleton` loading state component | Skeleton loader displayed during content fetch. Uses TailwindCSS `animate-pulse bg-gray-200 rounded` patterns for content placeholder shapes matching the expected content layout. |
| C-1509-05 | Integrate navigation breadcrumb | Breadcrumb component showing: Home > Help & Support. Uses semantic `<nav aria-label="Breadcrumb">` with `<ol>` list structure. Positioned above the main content area. |
| C-1509-06 | Apply design tokens from Figma BSA Wireframes | Reference Figma file `6fQyfvBUqImyavY8Fw47FV`, frame `7542-109669` for layout dimensions, typography scale, color palette, spacing values, and border radii. Map Figma tokens to TailwindCSS utility classes. |

### Logic

| Sub-Task ID | Description | Technical Details |
|-------------|-------------|-------------------|
| L-1509-01 | Implement placeholder resolution service | Client-side utility service that scans content strings for placeholder tokens (`[Application Name]`, `[Support Email]`, `[Help Content URL]`) and replaces them with values from environment configuration. Falls back to displaying raw placeholder text with a console warning if a variable is not set. |
| L-1509-02 | Implement content fetching and caching logic | Custom React hook `useHelpContent()` that fetches content from `/api/help-support/content`, caches the response for the session duration (sessionStorage), and provides `data`, `isLoading`, `error`, and `refetch` states. Handles retry logic on failure. |
| L-1509-03 | Integrate navigation routing with React Router | Register `/help-support` route in the application's React Router configuration. Ensure the route is accessible from the Global Navigation Framework and redirects unauthenticated users to the Auth0 login flow. |
| L-1509-04 | Implement error handling for missing content | If the help content API returns an empty response or fails, display a graceful fallback message: "Help content is currently unavailable. Please contact support at [Support Email]." Log the error to the browser console and application monitoring. |

### Testing

| Sub-Task ID | Description | Technical Details |
|-------------|-------------|-------------------|
| T-1509-01 | Unit tests for help content API endpoint (pytest) | Test `GET /api/help-support/content` returns 200 with valid content, returns 401 for unauthenticated requests, returns 500 on database error. Test placeholder resolution in response data. Test empty content collection handling. |
| T-1509-02 | Component tests for HelpSupportPage (@testing-library/react) | Test component renders loading state on mount, displays content after fetch, renders all categories, renders content blocks by type, handles error state with retry button, handles empty state with fallback message. |
| T-1509-03 | Component tests for HelpContentBlock (@testing-library/react) | Test rendering for each `block_type` (text, link_list, faq, contact). Test placeholder resolution within content blocks. Test accessibility attributes on rendered elements. |
| T-1509-04 | BDD acceptance tests (behave) | Implement Given/When/Then scenarios for all 7 acceptance criteria. Test navigation access, content rendering, category display, contextual help selection, accessibility, placeholder resolution, and loading state. |
| T-1509-05 | E2E tests for navigation-to-help-page flow (Playwright) | Test full user flow: authenticate → navigate to Help & Support via global navigation → verify page loads with content → interact with categories → verify placeholder resolution → navigate away and back. |
| T-1509-06 | Placeholder resolution tests (Vitest) | Test placeholder resolution service with: all variables set (full resolution), partial variables set (partial resolution with warnings), no variables set (raw placeholders displayed with warnings), empty content string, content with no placeholders. |

---

## Edge Cases

| # | Edge Case | Expected Behavior | Category |
|---|-----------|-------------------|----------|
| 1 | **Missing Configuration** — Help content configuration collection is empty or all categories are marked inactive | The system displays a graceful fallback message: "Help content is currently being configured. Please contact your administrator for assistance." The page does not render a blank screen or throw an unhandled error. A warning is logged to the application monitoring system. | Empty State |
| 2 | **Unresolved Placeholder** — The `[Application Name]` or `[Support Email]` deployment variable is not set in the environment configuration | The system displays the raw placeholder text (e.g., `[Application Name]`) or a sensible default value (e.g., "BSA Banking Confirmations"). A console warning is logged: "Unresolved placeholder: [Application Name] — deployment variable not configured." The page remains functional and all other content renders correctly. | Configuration Failure |
| 3 | **Extremely Long Content** — Help content for a category exceeds expected length (e.g., > 10,000 characters per content block) | The page handles content overflow gracefully within the `max-w-prose` layout container. Long text wraps naturally, long lists scroll within the page, and the overall page remains scrollable. No horizontal overflow or layout breakage occurs. Content does not bleed into the Application Header or navigation areas. | Boundary Condition |
| 4 | **Network Error on Content Fetch** — The API call to `GET /api/help-support/content` fails due to network timeout, server error, or connectivity loss | The system displays an error state with the shared Error State / Retry pattern: a red-tinted container (`bg-red-50 border border-red-200 rounded-md p-4`) with an error message ("Unable to load help content. Please try again.") and a "Retry" button that re-triggers the content fetch. Previously cached content (if available in sessionStorage) may be displayed as a fallback. | Network Failure |
| 5 | **Unauthorized Access Attempt** — An unauthenticated user attempts to access the Help & Support page directly via URL (`/help-support`) without a valid Auth0 session | The application's authentication guard detects the missing or expired Auth0 session and redirects the user to the Auth0 login page. After successful authentication, the user is redirected back to the Help & Support page (return URL preserved). No help content is exposed to unauthenticated users. | Authorization Failure |

---

## Dependencies

### Cross-Epic Dependencies

| Dependency | Epic / Feature | Type | Description |
|------------|---------------|------|-------------|
| **Global Navigation Framework** | BSABANKSTA-1531 (this epic) / F-005-RQ-002 | Navigation Routing | The Help & Support page is accessed via the Global Navigation Framework. The navigation route (`/help-support`) must be registered in the application's React Router configuration and the navigation item must be visible to all authenticated users. |
| **Application Header** | BSABANKSTA-1531 (this epic) / F-005 | UI Container | The Help & Support page renders within the Application Frame shell — the persistent Application Header and global navigation remain visible while the help content occupies the main content area. |
| **Project Space Management** | BSABANKSTA-1305 (Batch 1) / F-001 | Navigation Context | The Help & Support page shares the application navigation context with Project Space views. Users can navigate between Project Spaces and Help & Support via the Global Navigation Framework. |
| **System Placeholders** | Cross-cutting (CC-RQ-002) | Configuration Dependency | The Help & Support page uses the `[Application Name]` configurable placeholder and potentially `[Support Email]` and environment-specific URLs. These must be implemented as configurable deployment variables resolved at runtime. |
| **Lazy Load / Infinite Scroll** | Cross-cutting (CC-RQ-001) | UI Pattern | If help content contains list views (e.g., FAQ lists, resource lists), the lazy load / infinite scroll pattern applies per Global Rule #4. For this story, standard page scrolling is expected since help content is not a data-driven list — the dependency is conditional. |
| **Auth0 Authentication** | External Service | Authentication | The Help & Support page requires a valid Auth0 session. Unauthenticated access attempts are redirected to the Auth0 login flow. User session data (name, role) may influence help content personalization in future iterations. |

### Bidirectional Dependency Notes

- **BSABANKSTA-1509 → BSABANKSTA-1531**: This story depends on the Global Navigation Framework and Application Frame shell from its own epic.
- **BSABANKSTA-1531 → BSABANKSTA-1305**: Navigation routing shares context with Batch 1 Project Space Management.
- **BSABANKSTA-1509 → CC-RQ-002**: Configurable placeholder system is a cross-cutting dependency used by this story and other stories across multiple epics.

---

## Story Estimation Guidance

| Attribute | Value |
|-----------|-------|
| **Story Points** | **3** (Fibonacci) |
| **Complexity** | Low — Single content page with configurable blocks. No complex data aggregation, no multi-step workflows, no cross-project data joins. |
| **Uncertainty** | Low — 1 Figma frame (`7542-109669`) provides clear layout direction. Content-page patterns are well-established and the configurable content infrastructure uses standard key-value patterns. |
| **Effort** | Moderate — Requires building the configurable content infrastructure: MongoDB collection schema, API endpoint, placeholder resolution service, React page component with category navigation, and comprehensive testing. While each piece is straightforward, the end-to-end content pipeline requires careful implementation. |
| **Risk** | Low — Minimal cross-epic dependencies beyond the navigation framework. No shared surfaces with other epics (unlike BSABANKSTA-1536). Self-contained feature boundary. |
| **Rationale** | 3 story points reflects a well-scoped, low-risk feature with clear requirements and a single Figma reference. The moderate effort for the configurable content pipeline (API + placeholder resolution + caching) justifies 3 points over a simpler 2-point static page, but the scope does not warrant 5 points since there is no complex data logic or cross-epic surface coordination. |

---

## Refinement Notes

### Global Rule #2 — Proactive Story Decomposition
- **Status:** No decomposition required.
- **Rationale:** This story contains 7 acceptance criteria (AC1–AC7) with a single user workflow (navigating to and interacting with a configurable content page). The AC count is well below the >10 threshold defined by Global Rule #2, and the story follows a single linear flow (navigate → view content → browse categories → resolve placeholders) without distinct multi-workflow patterns (e.g., separate create/edit/delete flows). The story remains a well-scoped, single-page content display feature.

### Global Rule #3 — Verb-Noun Title Standardization
- **Original Jira Title:** "Configurable Help & Support page BSABANKSTA-1509"
- **Standardized Title:** "Configure Help & Support Page"
- **Rationale:** Applied Verb-Noun format per Global Rule #3. The adjective "Configurable" was converted to the imperative verb "Configure" to match the standard action-oriented naming convention. The story ID suffix "BSABANKSTA-1509" was moved to metadata fields. Title case applied to all words.

### Global Rule #4 — Pagination Override
- **Assessment:** No pagination references detected in the source requirements for this story. The Help & Support page is a content page, not a data-driven list or table view.
- **Action:** No pagination-to-lazy-load override required. Standard page scrolling is appropriate for this content page. If future requirements introduce paginated content lists within the help page, the lazy load / infinite scroll pattern (CC-RQ-001) must be applied.

### Global Rule #5 — Jira Source of Truth
- **Assessment:** Jira requirement text was used as the authoritative source for all acceptance criteria. The Figma frame (`7542-109669`) was used for visual layout reference only.
- **Action:** Any UI elements present in Figma but absent from Jira requirements are flagged in the Discrepancy Review section below.

### Global Rule #6 — NFR Elevation
- **Assessment:** Performance targets (< 3s page load, < 1s API response), accessibility requirements (WCAG 2.1 AA), and configuration requirements (deployment-variable-driven content) were extracted from the raw requirements.
- **Action:** All NFRs elevated to the dedicated Non-Functional Requirements section above.

### Global Rule #7 — Placeholder Management
- **Assessment:** The Help & Support page uses the following configurable placeholders:
  - `[Application Name]` — Application title displayed in content headings and references
  - `[Support Email]` — Support contact email displayed in contact sections
  - `[Help Content URL]` — Optional external help resource URL
  - Environment-specific API base URLs
- **Action:** All placeholders cataloged in the epic-level System Placeholders section of `EPIC-BSABANKSTA-1531-application-frame-global-navigation.md`. All must be implemented as configurable deployment variables, not hard-coded values.

### Global Rule #8 — Generated UI Specifications
- **Assessment:** The Help & Support page has 1 Figma frame covering the primary layout. However, error states, empty states, and loading states are not depicted in the wireframe.
- **Action:** Generated UI Specifications provided in the dedicated section below for undepicted UI elements.

---

## Discrepancy Review

No discrepancies identified between the Figma wireframe (frame `7542-109669`) and the Jira requirements for BSABANKSTA-1509.

**Review Summary:**
- The Figma frame depicts a content page layout with categorized help topics, which aligns with the Jira requirement for a "Configurable Help & Support page."
- No UI elements were identified in the Figma frame that are absent from the Jira requirement text.
- The Jira requirements specify configurable content with placeholder resolution, which is a backend/logic concern not directly visible in the wireframe — this is expected and does not constitute a discrepancy.

**Figma-Only Elements Check:** None identified. All visual elements in the Figma frame correspond to Jira-specified requirements.

---

## Generated UI Specifications

> **DESIGN REVIEW REQUIRED:** The following UI specifications were automatically generated based on the existing design system tokens, as no explicit mockup was provided for these elements. Please review for accuracy and design intent before development.

The Help & Support page has 1 Figma frame (`7542-109669`) covering the primary content layout. The following UI elements are mentioned in the requirements or acceptance criteria but are **not depicted** in the Figma wireframe and require generated specifications:

### Error State — Failed Content Load

| UI Element | Design Token / Attribute | Value | Rationale |
|-----------|--------------------------|-------|-----------|
| Error Container | `background-color` | `bg-red-50` (#FEF2F2) | Reuse shared Error State pattern (Priority 1) — consistent with application-wide error display pattern defined in shared UI components. |
| Error Container | `border` | `border border-red-200` (#FECACA) | Reuse shared Error State pattern (Priority 1) — subtle red border provides visual error context. |
| Error Container | `border-radius` | `rounded-md` (6px) | Reuse shared Error State pattern (Priority 1) — matches card/container radius token. |
| Error Container | `padding` | `p-4` (16px) | Reuse shared Error State pattern (Priority 1) — consistent spacing. |
| Error Message Text | `color` | `text-red-700` (#B91C1C) | Apply semantic error color token (Priority 2) — high contrast on red-50 background. |
| Error Message Text | `font-size` | `text-sm` (14px) | Apply semantic body text token (Priority 2) — standard body text size. |
| Error Icon | `color` | `text-red-400` (#F87171) | Apply semantic error accent token (Priority 2) — slightly muted icon color. |
| Retry Button | `background-color` | `bg-red-600 hover:bg-red-700` | Apply semantic error action token (Priority 2) — primary action within error context. |
| Retry Button | `color` | `text-white` | Apply semantic button text token (Priority 2) — white text on colored button. |
| Retry Button | `padding` | `px-4 py-2` | Apply semantic button spacing token (Priority 2) — standard button padding. |
| Retry Button | `border-radius` | `rounded-md` (6px) | Apply semantic button radius token (Priority 2) — matches container radius. |

### Empty State — No Content Configured

| UI Element | Design Token / Attribute | Value | Rationale |
|-----------|--------------------------|-------|-----------|
| Empty State Container | `display` / `alignment` | `flex flex-col items-center justify-center` | Reuse shared Empty State pattern (Priority 1) — centered flex layout for empty state messaging. |
| Empty State Container | `padding` | `py-16` (64px vertical) | Reuse shared Empty State pattern (Priority 1) — generous vertical padding for visual breathing room. |
| Empty State Icon | `color` | `text-gray-400` (#9CA3AF) | Apply semantic muted icon token (Priority 2) — low-emphasis icon for non-critical state. |
| Empty State Icon | `size` | `w-16 h-16` (64px) | Apply semantic icon size token (Priority 2) — large icon for empty state visual anchor. |
| Empty State Heading | `color` | `text-gray-500` (#6B7280) | Reuse shared Empty State pattern (Priority 1) — muted heading color. |
| Empty State Heading | `font-size` | `text-lg` (18px) | Apply semantic heading token (Priority 2) — sub-heading scale. |
| Empty State Heading | `font-weight` | `font-medium` (500) | Apply semantic heading weight token (Priority 2). |
| Empty State Description | `color` | `text-gray-400` (#9CA3AF) | Apply semantic description text token (Priority 2) — lower-emphasis secondary text. |
| Empty State Description | `font-size` | `text-sm` (14px) | Apply semantic body text token (Priority 2). |

### Loading Skeleton State

| UI Element | Design Token / Attribute | Value | Rationale |
|-----------|--------------------------|-------|-----------|
| Skeleton Block | `background-color` | `bg-gray-200` (#E5E7EB) | Apply semantic skeleton token (Priority 2) — standard loading placeholder color. |
| Skeleton Block | `animation` | `animate-pulse` | Apply semantic loading animation token (Priority 2) — Tailwind pulse animation for loading indication. |
| Skeleton Block | `border-radius` | `rounded` (4px) | Apply semantic element radius token (Priority 2) — slight rounding for placeholder blocks. |
| Skeleton Title | `dimensions` | `h-6 w-3/4` | Apply proportional sizing (Priority 3) — simulates heading text block. |
| Skeleton Paragraph | `dimensions` | `h-4 w-full` (repeated 3–4 lines) | Apply proportional sizing (Priority 3) — simulates body text block. |
| Skeleton Category | `dimensions` | `h-8 w-1/2` | Apply proportional sizing (Priority 3) — simulates category heading. |

---

## Figma Mockup Link

| Frame | Description | URL |
|-------|-------------|-----|
| **Help & Support Page** | Primary layout frame depicting the configurable Help & Support content page with categorized help topics, navigation structure, and content blocks within the Application Frame shell. Shows the intended page layout with `max-w-prose mx-auto` centered content pattern, category headings, and content sections. | [Figma Frame 7542-109669](https://www.figma.com/design/6fQyfvBUqImyavY8Fw47FV/BSA-Wireframes?node-id=7542-109669) |

---

## Definition of Done

- [ ] All 7 acceptance criteria (AC1–AC7) pass BDD validation
- [ ] Unit tests written and passing — pytest for `GET /api/help-support/content` API endpoint (T-1509-01)
- [ ] Component tests written and passing — @testing-library/react for `HelpSupportPage`, `HelpContentBlock`, `HelpCategoryNav` components (T-1509-02, T-1509-03)
- [ ] BDD acceptance tests written and passing — behave scenarios for all 7 acceptance criteria (T-1509-04)
- [ ] E2E tests written and passing — Playwright for full navigation-to-help-page-to-interaction flow (T-1509-05)
- [ ] Placeholder resolution tests written and passing — Vitest for all variable configurations (T-1509-06)
- [ ] Code reviewed and approved by at least one peer engineer
- [ ] Configurable content infrastructure is functional — MongoDB collection, API endpoint, and placeholder resolution service operational
- [ ] `[Application Name]` placeholder resolves correctly in all deployment environments (dev, staging, production)
- [ ] `[Support Email]` placeholder resolves correctly in all deployment environments
- [ ] Help & Support page is accessible via the Global Navigation Framework ("Help & Support" navigation item routes to `/help-support`)
- [ ] WCAG 2.1 AA accessibility compliance verified — semantic HTML, heading hierarchy, ARIA landmarks, keyboard navigation, color contrast (≥ 4.5:1)
- [ ] Design review completed — implementation verified against Figma frame `7542-109669`
- [ ] Generated UI Specifications for error state, empty state, and loading skeleton reviewed by design team
- [ ] NFRs validated — page loads in < 3 seconds, API responds in < 1 second, responsive layout functional
- [ ] Cross-epic dependency links verified — Global Navigation Framework routing, Application Frame integration, System Placeholder resolution
- [ ] No raw placeholder tokens visible to end users in any deployment environment
- [ ] Documentation updated — story file, epic file references, and cross-epic dependency links confirmed
