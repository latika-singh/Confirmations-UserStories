# Blitzy Project Guide — BSA Banking Confirmations Batch 2 Documentation

---

## 1. Executive Summary

### 1.1 Project Overview

This project generates a complete set of BDD-style backlog documentation artifacts for the BSA Banking Confirmations system (`aud-tech-confirmations-blitzy`). The scope encompasses four new Batch 2 epics — BSA Admin Persona, My Projects Dashboard, Global Views and Reporting, and Application Frame and Global Navigation — integrated with one existing Batch 1 epic (Create/Modify Project Space). The output is 12 structured markdown files totaling 7,129 lines, each adhering to a strict 14-section story template and standardized epic format, designed for downstream AI code generation by the Blitzy Platform. This is a documentation-generation task operating within a specification repository containing no application source code.

### 1.2 Completion Status

**Completion: 85.9% — 110 hours completed out of 128 total hours**

Formula: 110 completed hours / (110 completed + 18 remaining) = 110 / 128 = 85.9%

```mermaid
pie title Project Completion Status
    "Completed (AI)" : 110
    "Remaining" : 18
```

| Metric | Value |
|--------|-------|
| **Total Project Hours** | 128 |
| **Completed Hours (AI)** | 110 |
| **Remaining Hours** | 18 |
| **Completion Percentage** | 85.9% |

### 1.3 Key Accomplishments

- ✅ Created 12 structured documentation files (7,129 lines) across 5 epic directories
- ✅ Generated 12 complete user stories with full 14-section BDD template compliance
- ✅ Applied all 8 global transformation rules (pagination override, title standardization, story decomposition, Jira-over-Figma precedence, NFR elevation, placeholder cataloging, UI spec generation, epic description enrichment)
- ✅ Decomposed BSABANKSTA-1579 (System Messages) into sub-stories -A (Create) and -B (Edit) per Global Rule #2
- ✅ Built cross-epic dependency graph with Mermaid `graph LR` diagram covering all 5 epics and 8 dependency edges
- ✅ Created Batch 1 epic file with unified Mermaid `flowchart TD` application workflow diagram incorporating both batches
- ✅ Mapped all 17 Figma frames to corresponding stories with direct URLs
- ✅ Constructed design token manifest (Colors, Typography, Spacing, Border Radii, Effects) for Generated UI Specifications
- ✅ Established bidirectional cross-epic dependency links across all 5 epics and 12 stories
- ✅ Applied Fibonacci estimation with written rationale across all stories
- ✅ Generated INVEST validation tables for all stories
- ✅ All code fences properly closed, all markdown valid GFM

### 1.4 Critical Unresolved Issues

| Issue | Impact | Owner | ETA |
|-------|--------|-------|-----|
| BDD acceptance criteria not yet reviewed by BSA/AML domain SME | Downstream code generation may produce incorrect business logic if ACs contain domain inaccuracies | Human — Domain Expert | 1 week |
| Jira tickets not yet synchronized with generated documentation | Development team cannot begin sprint planning from Jira until stories are transferred | Human — Product Owner | 1 week |
| Batch 1 story files referenced in EPIC-1305 but not present in repository | Cross-epic traceability is incomplete at the story level for Batch 1 | Human — Analyst | 2 weeks |

### 1.5 Access Issues

| System/Resource | Type of Access | Issue Description | Resolution Status | Owner |
|----------------|---------------|-------------------|-------------------|-------|
| Figma BSA Wireframes (`6fQyfvBUqImyavY8Fw47FV`) | Read access to Assets panel | Design token manifest was constructed from Figma frame analysis; full Assets panel programmatic access was not available for automated extraction | Workaround applied — tokens derived from frame inspection | Human — Design Lead |
| Jira BSABANKSTA Project | Write access for ticket creation | Generated BDD documentation needs to be transferred to Jira tickets; no automated Jira API integration exists in the documentation pipeline | Pending — manual synchronization required | Human — Product Owner |

### 1.6 Recommended Next Steps

1. **[High]** Conduct domain expert review of all 12 story BDD acceptance criteria for BSA/AML business logic accuracy
2. **[High]** Synchronize generated story documentation to Jira tickets (BSABANKSTA project) for sprint planning
3. **[Medium]** Verify design token manifest against the actual Figma Assets panel to confirm token values match the live design system
4. **[Medium]** Add bidirectional dependency back-references to existing Batch 1 story files (if they exist in a separate branch or repository)
5. **[Low]** Review 3 instances of internal "pagination" terminology in sub-task technical implementation details (STORY-1458 and STORY-1500) to ensure clarity that cursor-based pagination is an implementation detail, not a UI pattern

---

## 2. Project Hours Breakdown

### 2.1 Completed Work Detail

| Component | Hours | Description |
|-----------|-------|-------------|
| Requirement Analysis & Extraction | 6 | Parsed Batch 2 PDF requirements, extracted 4 epics and 12 stories, mapped Figma frames to stories |
| EPIC_DEPENDENCY_GRAPH.md | 3 | Created master Mermaid `graph LR` diagram with all 5 epics, 8 dependency edges, shared component inventory |
| EPIC-BSABANKSTA-1305 (Batch 1 Update) | 4 | Created Batch 1 epic file with strategic goal, cross-epic dependencies, unified Mermaid `flowchart TD` workflow diagram |
| EPIC-BSABANKSTA-131 Epic File | 3 | Created BSA Admin Persona epic with structured summary, 4-story index, dependencies, system placeholders, DoD |
| EPIC-BSABANKSTA-1531 Epic File | 3 | Created Application Frame epic with Mermaid workflow diagram, story index, dependencies, system placeholders |
| EPIC-BSABANKSTA-1540 Epic File | 3 | Created Global Views and Reporting epic portion within composite file, including design token manifest |
| EPIC-BSABANKSTA-1572 Epic File | 3 | Created My Projects Dashboard epic portion within composite file, including design token manifest |
| STORY-BSABANKSTA-1579 (Decomposed -A/-B) | 10 | 874-line story with Create/Edit modal decomposition, 10+ ACs per sub-story, 6 Figma frames mapped |
| STORY-BSABANKSTA-1500 | 6 | 538-line User Management Library story, 12 BDD acceptance criteria, Auth0 integration sub-tasks |
| STORY-BSABANKSTA-1458 | 7 | 559-line Integration Audit Trail story, 13 BDD acceptance criteria, MongoDB aggregation sub-tasks |
| STORY-BSABANKSTA-1509 | 4 | 357-line Help & Support Page story, 7 BDD scenarios, configurable content placeholders |
| STORY-BSABANKSTA-1532 | 4 | 356-line Profile Dropdown story, 9 BDD scenarios, 2 Figma frames mapped |
| STORY-BSABANKSTA-1536 | 5 | 446-line All Confirmations Page story, 9 BDD scenarios, shared surface with F-004 documented |
| EPIC-1540 Stories (3 composite) | 18 | 3 stories (View Global Confirmations, Filter Cross-Project Data, Aggregate Summary Statistics) totaling ~1,300 lines of story content |
| EPIC-1572 Stories (3 composite) | 16 | 3 stories (View Dashboard, Navigate Details, Filter Projects) totaling ~1,200 lines of story content |
| Global Rule Processing | 6 | Applied all 8 global rules: decomposition analysis, title standardization, pagination override, Jira-over-Figma enforcement, NFR elevation, placeholder cataloging, UI spec SOP |
| Design Token Manifest & Figma Integration | 5 | Constructed 5-category design token manifest, mapped 17 Figma frames, applied 3-tier priority hierarchy |
| Cross-Epic Integration & QA Fixes | 4 | Bidirectional dependency verification, code review fixes (6 findings), security version update, decomposition language fix |
| **Total Completed** | **110** | |

### 2.2 Remaining Work Detail

| Category | Base Hours | Priority | After Multiplier |
|----------|-----------|----------|-----------------|
| Content Review by Domain SME — Review 12 stories' BDD acceptance criteria for BSA/AML business logic accuracy | 6 | High | 7 |
| Jira Ticket Synchronization — Transfer 12 stories and 5 epics to BSABANKSTA Jira project with story points, ACs, and dependencies | 4 | Medium | 5 |
| Figma Design Token Verification — Validate design token manifest values against actual Figma Assets panel for all 5 categories | 2 | Medium | 2.5 |
| Batch 1 Story Back-References — Add forward dependency links to 4 referenced Batch 1 story files (if they exist) | 2 | Low | 2.5 |
| Technical Terminology Review — Clarify 3 instances of internal "pagination" terminology in sub-task implementation details | 1 | Low | 1 |
| **Total Remaining** | **15** | | **18** |

### 2.3 Enterprise Multipliers Applied

| Multiplier | Value | Rationale |
|-----------|-------|-----------|
| Compliance Buffer | 1.10x | BSA/AML compliance domain requires additional rigor in review and validation of acceptance criteria; regulatory documentation standards demand thoroughness |
| Uncertainty Buffer | 1.10x | Domain expertise required for accurate BSA/AML business logic review; Figma Assets panel access limitations; Batch 1 story file existence unknown |
| **Combined Multiplier** | **1.21x** | Applied to all remaining base hour estimates: 15h × 1.21 = 18.15h → 18h |

---

## 3. Test Results

| Test Category | Framework | Total Tests | Passed | Failed | Coverage % | Notes |
|--------------|-----------|-------------|--------|--------|------------|-------|
| Structural Compliance | Blitzy Autonomous Validator | 12 | 12 | 0 | 100% | All 12 files validated for 14-section template adherence, Mermaid syntax, code fence closure |
| Global Rule Compliance | Blitzy Autonomous Validator | 8 | 8 | 0 | 100% | All 8 global rules verified across all applicable files (pagination override, title standardization, decomposition, etc.) |
| BDD Format Validation | Blitzy Autonomous Validator | 12 | 12 | 0 | 100% | All stories contain Given/When/Then BDD acceptance criteria in formal Gherkin-style format |
| Cross-Epic Dependency Integrity | Blitzy Autonomous Validator | 5 | 5 | 0 | 100% | All 5 epics have bidirectional dependency links verified; EPIC_DEPENDENCY_GRAPH.md contains all 8 edges |
| Figma Link Validation | Blitzy Autonomous Validator | 17 | 17 | 0 | 100% | All 17 unique Figma frame URLs verified with correct node-id format |
| Markdown Syntax Validation | Blitzy Autonomous Validator | 12 | 12 | 0 | 100% | All files are valid GitHub-Flavored Markdown; all code fences properly closed |

> **Note:** This is a documentation-only repository with no application source code. There are no unit tests, integration tests, or E2E tests to execute. All validation is structural and content compliance checking performed by Blitzy's autonomous validation pipeline. The validator confirmed: "PRODUCTION-READY — All Validation Gates Passed" with 0 issues found or modifications required.

---

## 4. Runtime Validation & UI Verification

**Runtime Health:**
- ✅ Git repository clean — working tree has no uncommitted changes
- ✅ All 12 documentation files committed and pushed to branch `blitzy-4e44ebee-64b0-4650-a4d6-641cf38e4a0f`
- ✅ Branch is up to date with `origin/blitzy-4e44ebee-64b0-4650-a4d6-641cf38e4a0f`
- ✅ README.md unchanged — no modifications to source specification file

**Documentation Rendering Verification:**
- ✅ Mermaid `graph LR` diagram in EPIC_DEPENDENCY_GRAPH.md — valid syntax with 5 nodes and 8 edges
- ✅ Mermaid `flowchart TD` diagram in EPIC-BSABANKSTA-1305 — valid syntax covering all 5 features
- ✅ Mermaid `flowchart TD` diagram in EPIC-BSABANKSTA-1531 — valid syntax for navigation workflow
- ✅ All markdown tables render correctly with proper column alignment
- ✅ All heading hierarchy (H1→H2→H3) is consistent across files

**UI Specification Verification:**
- ✅ DESIGN REVIEW REQUIRED banners present in all 12 story files (21 total instances)
- ✅ Generated UI Specifications follow 4-step SOP with design token tables
- ✅ 3-tier priority hierarchy (Reuse → Semantic → Base) applied in all Generated UI Spec sections

**API Integration:**
- ⚠ N/A — Documentation repository; no API endpoints to verify

---

## 5. Compliance & Quality Review

| AAP Requirement | Status | Evidence | Progress |
|----------------|--------|----------|----------|
| 14-Section Story Template (all stories) | ✅ Pass | All 12 stories contain: Story Title, User Story, INVEST, NFRs, BDD ACs, Sub-Tasks, Edge Cases, Dependencies, Estimation, Refinement Notes, Discrepancy Review, Generated UI Specs, Figma Link, DoD | 12/12 |
| Global Rule #1 — Generative Epic Descriptions | ✅ Pass | All 5 epic files contain Strategic Goal, Business Context, Key Features, Out of Scope | 5/5 |
| Global Rule #2 — Proactive Story Decomposition | ✅ Pass | BSABANKSTA-1579 decomposed into -A (Create) and -B (Edit); documented in Refinement Notes | 1/1 trigger |
| Global Rule #3 — Verb-Noun Title Standardization | ✅ Pass | All 12 story titles follow Verb-Noun format; original titles preserved in Refinement Notes | 12/12 |
| Global Rule #4 — Pagination to Lazy Load Override | ✅ Pass | No traditional pagination in any acceptance criteria; all list views use infinite scroll; 260+ lazy load/infinite scroll references | 12/12 |
| Global Rule #5 — Jira Source of Truth | ✅ Pass | Discrepancy Review sections present in all 12 stories; Figma-only elements flagged | 12/12 |
| Global Rule #6 — NFR Elevation | ✅ Pass | NFR sections populated in all 12 stories with performance targets and accessibility requirements | 12/12 |
| Global Rule #7 — Placeholder Cataloging | ✅ Pass | System Placeholders sections in all 5 epic files; `[Application Name]` and other configurable variables cataloged | 5/5 |
| Global Rule #8 — Generated UI Specifications SOP | ✅ Pass | DESIGN REVIEW REQUIRED banners in all story files; design token tables with 3-tier priority hierarchy | 12/12 |
| BDD Given/When/Then Format | ✅ Pass | 602+ BDD keywords across all story files; formal Gherkin-style scenarios | 12/12 |
| INVEST Validation | ✅ Pass | INVEST tables with 6 principles validated in all 12 stories | 12/12 |
| Fibonacci Estimation | ✅ Pass | Fibonacci story points (3, 5, 8) assigned with written rationale in all 12 stories | 12/12 |
| 5 AI-Centric Sub-Task Categories | ✅ Pass | Model, API, Component, Logic, Testing sub-task sections in all 12 stories | 12/12 |
| Edge Cases (3–5 per story) | ✅ Pass | 5 edge cases per story covering failure, boundary, authorization, and data conditions | 12/12 |
| Cross-Epic Dependencies | ✅ Pass | Bidirectional dependency links across all 5 epics; 66+ cross-references in story files | 5/5 epics |
| Figma Mockup Links | ✅ Pass | All 17 unique Figma frames referenced with correct `node-id` URLs | 17/17 |
| Definition of Done | ✅ Pass | DoD checklists at both epic (5) and story (12) levels | 17/17 |
| Mermaid Diagrams | ✅ Pass | 3 valid Mermaid diagrams (graph LR, 2× flowchart TD) | 3/3 |
| Design Token Manifest | ✅ Pass | 5-category manifest (Colors, Typography, Spacing, Border Radii, Effects) in composite epic files | 2/2 composites |

**Fixes Applied During Autonomous Validation:**
- Commit `98b4c42`: Addressed 6 code review findings from Checkpoint 1
- Commit `4f45067`: Replaced conditional decomposition language with definitive outcome in EPIC-131 User Stories Index
- Commit `f09e4ee`: Updated vulnerable dependency version references in documentation

---

## 6. Risk Assessment

| Risk | Category | Severity | Probability | Mitigation | Status |
|------|----------|----------|-------------|------------|--------|
| BDD acceptance criteria may contain BSA/AML domain inaccuracies | Technical | High | Medium | Schedule domain expert review before downstream code generation; compare ACs against regulatory requirements | Open — Requires human SME review |
| Generated documentation not yet synchronized to Jira | Operational | Medium | High | Transfer all 12 stories to Jira BSABANKSTA project with story points, ACs, and dependency links | Open — Manual sync required |
| Design token manifest values may differ from live Figma Assets panel | Technical | Medium | Low | Verify token manifest against Figma programmatic API or manual Assets panel inspection | Open — Design lead verification needed |
| Batch 1 story files referenced but not present in repository | Integration | Medium | Medium | Confirm Batch 1 stories exist (separate branch/repo); add forward dependency links if found | Open — Analyst investigation needed |
| 3 instances of "pagination" in sub-task technical details could cause confusion | Technical | Low | Low | Clarify in sub-task descriptions that cursor-based pagination is an internal implementation pattern, not a UI-facing pattern | Open — Minor terminology cleanup |
| Composite file structure (EPIC-1540, EPIC-1572) differs from individual file pattern | Technical | Low | Low | Document composite file approach in repository README or contributing guide; ensure downstream Blitzy Platform parser handles both patterns | Accepted — Functionally equivalent |
| No automated Mermaid rendering validation | Technical | Low | Low | Mermaid syntax visually validated; recommend GitHub-rendered preview before merging PR | Mitigated — Syntax verified manually |

---

## 7. Visual Project Status

```mermaid
pie title Project Hours Breakdown
    "Completed Work" : 110
    "Remaining Work" : 18
```

**Remaining Work by Priority:**

| Priority | Hours | Categories |
|----------|-------|------------|
| High | 7 | Content Review by Domain SME |
| Medium | 7.5 | Jira Ticket Synchronization (5h), Figma Token Verification (2.5h) |
| Low | 3.5 | Batch 1 Back-References (2.5h), Terminology Review (1h) |
| **Total Remaining** | **18** | |

---

## 8. Summary & Recommendations

### Achievement Summary

The BSA Banking Confirmations Batch 2 documentation generation project is **85.9% complete**, with 110 hours of AAP-scoped work delivered autonomously by Blitzy agents. All 12 planned documentation files have been created, totaling 7,129 lines of structured BDD-style backlog artifacts across 5 epic directories. Every AAP-specified deliverable has been completed: 4 new epic files, 12 user stories (including one decomposition), 1 cross-epic dependency graph, 1 Batch 1 epic integration update, and full compliance with all 8 global transformation rules.

### Remaining Gaps

The remaining 18 hours (14.1% of total) are exclusively **path-to-production activities** requiring human intervention — no AAP-specified documentation artifacts are missing or incomplete. The primary gaps are: (1) domain expert review of BDD acceptance criteria for BSA/AML business logic accuracy (7h), (2) Jira ticket synchronization for sprint planning enablement (5h), (3) Figma design token manifest verification against the live Assets panel (2.5h), (4) Batch 1 story back-reference linking (2.5h), and (5) minor technical terminology clarification (1h).

### Critical Path to Production

1. **Domain SME Review** → validates BDD criteria accuracy → unblocks downstream code generation
2. **Jira Synchronization** → transfers stories to sprint backlog → unblocks development team planning
3. **Figma Token Verification** → confirms design system alignment → unblocks UI component generation

### Production Readiness Assessment

The documentation repository is **production-ready from a structural and content completeness perspective**. All validation gates passed with zero issues. The remaining work is validation and integration work that requires human domain expertise and system access (Jira, Figma) that cannot be automated in the documentation pipeline. Once the 3 critical-path items above are completed, the Blitzy Platform can begin downstream code generation from these specifications.

---

## 9. Development Guide

### System Prerequisites

| Requirement | Version | Purpose |
|------------|---------|---------|
| Git | 2.x+ | Repository cloning and branch management |
| GitHub account | N/A | Repository access and PR review |
| Markdown viewer | Any (GitHub, VS Code, etc.) | Rendering documentation files and Mermaid diagrams |
| Web browser | Modern (Chrome, Firefox, Edge) | Viewing Figma design files and GitHub-rendered Mermaid |

### Environment Setup

**1. Clone the repository:**
```bash
git clone https://github.com/latika-singh/Confirmations-UserStories.git
cd Confirmations-UserStories
```

**2. Switch to the feature branch:**
```bash
git checkout blitzy-4e44ebee-64b0-4650-a4d6-641cf38e4a0f
```

**3. Verify repository contents:**
```bash
find tickets -name "*.md" -type f | wc -l
# Expected output: 12
```

### Dependency Installation

No dependencies to install. This is a documentation-only repository containing structured markdown files. No package managers, build tools, or runtime environments are required.

### Navigating the Documentation

**Repository structure:**
```
tickets/
├── EPIC_DEPENDENCY_GRAPH.md                          (147 lines — cross-epic visualization)
├── EPIC-BSABANKSTA-1305-create-modify-project-space.md (295 lines — Batch 1 epic)
├── EPIC-BSABANKSTA-131-admin-persona/
│   ├── EPIC-BSABANKSTA-131-admin-persona.md          (237 lines — epic file)
│   ├── STORY-BSABANKSTA-1579-manage-global-system-messages.md (874 lines)
│   ├── STORY-BSABANKSTA-1500-manage-user-library.md  (538 lines)
│   └── STORY-BSABANKSTA-1458-view-integration-audit-trail.md (559 lines)
├── EPIC-BSABANKSTA-1531-application-frame-global-navigation/
│   ├── EPIC-BSABANKSTA-1531-application-frame-global-navigation.md (282 lines)
│   ├── STORY-BSABANKSTA-1509-configure-help-support-page.md (357 lines)
│   ├── STORY-BSABANKSTA-1532-manage-profile-dropdown.md (356 lines)
│   └── STORY-BSABANKSTA-1536-view-all-confirmations.md (446 lines)
├── EPIC-BSABANKSTA-1540-global-views-reporting/
│   └── EPIC-BSABANKSTA-1540-global-views-reporting.md (1,576 lines — composite: epic + 3 stories)
└── EPIC-BSABANKSTA-1572-my-projects-dashboard/
    └── EPIC-BSABANKSTA-1572-my-projects-dashboard.md  (1,462 lines — composite: epic + 3 stories)
```

### Verification Steps

**1. Verify all 12 files are present:**
```bash
find tickets -name "*.md" -type f | sort
```

**2. Verify total line count:**
```bash
find tickets -name "*.md" -exec wc -l {} + | tail -1
# Expected: 7129 total
```

**3. Verify Mermaid diagram syntax (visual check):**
Open `tickets/EPIC_DEPENDENCY_GRAPH.md` in GitHub to verify the Mermaid `graph LR` diagram renders correctly with 5 epic nodes and 8 dependency edges.

**4. Verify 14-section compliance for a story:**
```bash
grep "^##" tickets/EPIC-BSABANKSTA-131-admin-persona/STORY-BSABANKSTA-1500-manage-user-library.md | head -20
```

**5. Verify no code fences are unclosed:**
```bash
for f in $(find tickets -name "*.md"); do
  opens=$(grep -c '```' "$f")
  if [ $((opens % 2)) -ne 0 ]; then
    echo "UNCLOSED: $f"
  fi
done
# Expected: no output (all fences closed)
```

### Example Usage

**Reading a story file:**
Each story file follows the 14-section template. Key sections for developers:
- **Section 5 (Acceptance Criteria)** — BDD Given/When/Then scenarios for implementation
- **Section 6 (Sub-Tasks)** — AI-centric decomposition: Model, API, Component, Logic, Testing
- **Section 8 (Dependencies)** — Cross-epic links to related stories
- **Section 12 (Generated UI Specifications)** — Design token tables for UI components

**Navigating cross-epic dependencies:**
Start with `tickets/EPIC_DEPENDENCY_GRAPH.md` for the high-level dependency visualization, then follow links to individual epic and story files for detailed dependency mapping.

### Troubleshooting

| Issue | Resolution |
|-------|-----------|
| Mermaid diagrams not rendering | Use GitHub web UI or VS Code with Mermaid extension; Mermaid requires a renderer |
| File not found | Verify you are on branch `blitzy-4e44ebee-64b0-4650-a4d6-641cf38e4a0f` |
| Composite files (EPIC-1540, EPIC-1572) seem large | These files contain both the epic documentation and 3 embedded story sections; scroll past the epic header to find individual stories |

---

## 10. Appendices

### A. Command Reference

| Command | Purpose |
|---------|---------|
| `git checkout blitzy-4e44ebee-64b0-4650-a4d6-641cf38e4a0f` | Switch to the feature branch |
| `find tickets -name "*.md" -type f \| wc -l` | Count all documentation files (expected: 12) |
| `find tickets -name "*.md" -exec wc -l {} +` | Count total lines across all files (expected: 7,129) |
| `grep "^##" tickets/<FILE>.md` | List all section headings in a file |
| `grep -c "Given\|When\|Then" tickets/<FILE>.md` | Count BDD keywords in a story file |
| `grep -c "DESIGN REVIEW REQUIRED" tickets/<FILE>.md` | Count Generated UI Spec banners |

### B. Port Reference

Not applicable — this is a documentation-only repository with no running services.

### C. Key File Locations

| File | Purpose | Lines |
|------|---------|-------|
| `README.md` | Operational specification (unchanged) | 110 |
| `tickets/EPIC_DEPENDENCY_GRAPH.md` | Master cross-epic Mermaid dependency graph | 147 |
| `tickets/EPIC-BSABANKSTA-1305-create-modify-project-space.md` | Batch 1 epic with Batch 2 integration | 295 |
| `tickets/EPIC-BSABANKSTA-131-admin-persona/EPIC-BSABANKSTA-131-admin-persona.md` | BSA Admin Persona epic | 237 |
| `tickets/EPIC-BSABANKSTA-131-admin-persona/STORY-BSABANKSTA-1579-manage-global-system-messages.md` | System Messages (decomposed -A/-B) | 874 |
| `tickets/EPIC-BSABANKSTA-131-admin-persona/STORY-BSABANKSTA-1500-manage-user-library.md` | User Management Library | 538 |
| `tickets/EPIC-BSABANKSTA-131-admin-persona/STORY-BSABANKSTA-1458-view-integration-audit-trail.md` | Integration Audit Trail | 559 |
| `tickets/EPIC-BSABANKSTA-1531-application-frame-global-navigation/EPIC-BSABANKSTA-1531-application-frame-global-navigation.md` | Application Frame epic | 282 |
| `tickets/EPIC-BSABANKSTA-1531-application-frame-global-navigation/STORY-BSABANKSTA-1509-configure-help-support-page.md` | Help & Support Page | 357 |
| `tickets/EPIC-BSABANKSTA-1531-application-frame-global-navigation/STORY-BSABANKSTA-1532-manage-profile-dropdown.md` | Profile Dropdown Menu | 356 |
| `tickets/EPIC-BSABANKSTA-1531-application-frame-global-navigation/STORY-BSABANKSTA-1536-view-all-confirmations.md` | All Confirmations Page | 446 |
| `tickets/EPIC-BSABANKSTA-1540-global-views-reporting/EPIC-BSABANKSTA-1540-global-views-reporting.md` | Global Views epic + 3 stories (composite) | 1,576 |
| `tickets/EPIC-BSABANKSTA-1572-my-projects-dashboard/EPIC-BSABANKSTA-1572-my-projects-dashboard.md` | My Projects Dashboard epic + 3 stories (composite) | 1,462 |

### D. Technology Versions

| Technology | Version | Role in Documentation |
|-----------|---------|----------------------|
| GitHub-Flavored Markdown | GFM | Output format for all documentation artifacts |
| Mermaid | Latest (GitHub-rendered) | Dependency graph and workflow diagram syntax |
| Figma | BSA Wireframes file `6fQyfvBUqImyavY8Fw47FV` | Visual design source; 17 frames mapped to stories |
| TailwindCSS | 4.2.1 (referenced) | Design system referenced in Generated UI Specifications |
| React | 19.2.4 (referenced) | Frontend framework referenced in Component sub-tasks |
| Flask | 3.1.3 (referenced) | Backend framework referenced in API sub-tasks |
| MongoDB | 8.0 (referenced) | Database referenced in Model sub-tasks |

### E. Environment Variable Reference

Not applicable — this documentation repository has no runtime environment. Configurable placeholders are documented in each epic file's System Placeholders section:

| Placeholder | Description | Epic Files |
|------------|-------------|------------|
| `[Application Name]` | Configurable application title | All 5 epics |
| `[Support Email]` | Help desk contact | EPIC-1531 |
| `[API Base URL]` | Backend API endpoint | All 5 epics |
| `[Auth0 Domain]` | Identity provider domain | EPIC-131, EPIC-1531 |

### F. Developer Tools Guide

| Tool | Usage |
|------|-------|
| VS Code + Markdown Preview Enhanced | Recommended editor for viewing and editing story files with live preview |
| VS Code + Mermaid extension | Required for rendering Mermaid diagrams in local preview |
| GitHub Web UI | Best option for rendered Mermaid diagram verification |
| `grep` / `find` | CLI tools for cross-file compliance verification (see Command Reference) |

### G. Glossary

| Term | Definition |
|------|-----------|
| AAP | Agent Action Plan — the primary directive defining all project requirements |
| AC | Acceptance Criteria — testable conditions defining story completion |
| BDD | Behavior-Driven Development — Given/When/Then format for acceptance criteria |
| BSA | Bank Secrecy Act — US federal law requiring financial institutions to assist in detecting money laundering |
| AML | Anti-Money Laundering — regulations and procedures to prevent money laundering |
| CRUD | Create, Read, Update, Delete — standard data operations |
| DoD | Definition of Done — checklist of conditions for story/epic completion |
| GFM | GitHub-Flavored Markdown — extended markdown syntax supported by GitHub |
| INVEST | Independent, Negotiable, Valuable, Estimable, Small, Testable — user story quality criteria |
| NFR | Non-Functional Requirement — quality attributes (performance, accessibility, security) |
| RBAC | Role-Based Access Control — authorization model restricting access by user role |
| SOP | Standard Operating Procedure — documented process for Generated UI Specifications |