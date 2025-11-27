---
description: "Task list for Commento Comment System Integration"
---

# Tasks: Commento Comment System Integration

**Input**: Design documents from `/specs/002-add-commento-support/`
**Prerequisites**: spec.md (user stories and requirements)

**Tests**: Tests are NOT explicitly requested in the specification, so this implementation focuses on deployment and manual verification.

**Organization**: Tasks are grouped by user story to enable independent implementation and testing of each story.

## Format: `[ID] [P?] [Story] Description`
- **[P]**: Can run in parallel (different files, no dependencies)
- **[Story]**: Which user story this task belongs to (e.g., US1, US2, US3)
- Include exact file paths in descriptions

## Path Conventions
- **Hugo project**: `layouts/`, `static/`, `config/_default/`, `themes/hugo-clarity/`
- Template overrides in `layouts/` to customize theme behavior
- Static assets in `static/` for custom CSS/JS

---

## Phase 1: Setup (Shared Infrastructure)

**Purpose**: Verify Hugo environment and understand theme structure

- [x] T001 Verify Hugo extended version installed and theme submodule initialized (`git submodule status`)
- [x] T002 [P] Review hugo-clarity theme post layout structure in `themes/hugo-clarity/layouts/_default/single.html`
- [x] T003 [P] Create backup of current site state (`hugo --gc --minify` and verify clean build)

---

## Phase 2: Foundational (Blocking Prerequisites)

**Purpose**: Set up Commento configuration foundation that all user stories depend on

**⚠️ CRITICAL**: No user story work can begin until this phase is complete

- [x] T004 Verify Commento service URL and credentials are available (check with user if not provided)
- [x] T005 Add Commento configuration to Hugo site parameters in `config/_default/params.toml` (commento.enabled, commento.url)
- [x] T006 Test local Hugo server runs without errors (`hugo server -D`)

**Checkpoint**: Foundation ready - user story implementation can now begin

---

## Phase 3: User Story 1 - Reader Leaves Comment (Priority: P1) 🎯 MVP

**Goal**: Enable basic commenting functionality where readers can view the comment section and post comments on blog posts

**Independent Test**: Navigate to any published blog post locally (`hugo server`), scroll to bottom, verify Commento widget loads and displays comment input

### Implementation for User Story 1

- [ ] T007 [US1] Create custom single post layout override at `layouts/_default/single.html` by copying from theme
- [ ] T008 [US1] Add Commento script and div HTML snippet to post layout after article content in `layouts/_default/single.html`:
  ```html
  {{ if .Site.Params.commento.enabled }}
  <div id="commento-section">
    <script defer src="{{ .Site.Params.commento.url }}/js/commento.js"></script>
    <div id="commento"></div>
  </div>
  {{ end }}
  ```
- [ ] T009 [US1] Configure Commento script to use page-specific identifier using `data-page-id` attribute set to `.Permalink` or `.RelPermalink`
- [ ] T010 [US1] Add async/defer script loading to ensure non-blocking page render (already in snippet, verify behavior)
- [ ] T011 [US1] Test locally with `hugo server -D` - verify comment widget appears on post pages
- [ ] T012 [US1] Test comment submission - post a test comment and verify it appears immediately
- [ ] T013 [US1] Verify comments persist on page refresh (reload post page and confirm comment still visible)

**Checkpoint**: At this point, User Story 1 (basic commenting) should be fully functional and testable independently

---

## Phase 4: User Story 2 - Reader Engages with Existing Comments (Priority: P2)

**Goal**: Enable threaded comment replies and comment interaction features

**Independent Test**: On a post with existing comments, click "Reply" and verify nested reply appears under parent comment

### Implementation for User Story 2

- [ ] T014 [US2] Verify Commento instance supports threaded replies (check Commento dashboard configuration)
- [ ] T015 [US2] Test threaded reply functionality - create parent comment, then reply to it
- [ ] T016 [US2] Verify reply threading displays correctly (indentation, visual hierarchy)
- [ ] T017 [US2] Test multiple comment threads on same post - verify organization is clear
- [ ] T018 [US2] Configure authentication methods in Commento dashboard (enable anonymous, email, Google, Twitter, GitHub per FR-007)
- [ ] T019 [US2] Configure post-moderation settings in Commento dashboard (comments appear immediately per FR-010)

**Checkpoint**: At this point, User Stories 1 AND 2 should both work independently

---

## Phase 5: User Story 3 - Site Appearance Consistency (Priority: P3)

**Goal**: Customize Commento styling to match blog design aesthetic

**Independent Test**: View comment section on multiple posts and verify visual consistency with blog theme

### Implementation for User Story 3

- [ ] T020 [US3] Create custom CSS file for Commento overrides at `static/css/commento-custom.css`
- [ ] T021 [US3] Extract blog brand colors and typography from hugo-clarity theme CSS
- [ ] T022 [US3] Write CSS overrides in `static/css/commento-custom.css` to match:
  - Button colors to blog's brand colors
  - Font family to match blog content font
  - Comment box borders and spacing to match blog card styling
  - Reply indentation and threading visual style
- [ ] T023 [US3] Add `data-css-override` attribute to Commento script pointing to custom CSS file
- [ ] T024 [US3] Optionally disable Commento's Source Sans Pro font loading using `data-no-fonts="true"` if blog uses different fonts
- [ ] T025 [US3] Test visual consistency across multiple posts and different comment states (no comments, few comments, many nested replies)

**Checkpoint**: All user stories should now be independently functional with consistent branding

---

## Phase 6: Polish & Cross-Cutting Concerns

**Purpose**: Production readiness, testing, and documentation

- [ ] T026 [P] Test comment section on all major browsers (Chrome, Firefox, Safari, Edge)
- [ ] T027 [P] Test mobile responsive behavior (comment input, threading visibility on small screens)
- [ ] T028 [P] Verify comment section doesn't block page rendering (check PageSpeed Insights or Lighthouse)
- [ ] T029 [P] Test edge cases:
  - Empty comment submission (should be blocked)
  - Very long comment (10,000+ characters)
  - Commento service temporarily unavailable (graceful degradation)
  - JavaScript disabled (fallback message or hidden section)
- [ ] T030 Verify comments correctly associate with specific posts (no cross-post bleeding) - test with multiple posts
- [ ] T031 Build production site with `hugo --gc --minify` and verify no errors
- [ ] T032 Deploy to GitHub Pages and verify comment section loads correctly on production site
- [ ] T033 Manually verify deployed site at https://specoding.com/ - test comment submission on live site
- [ ] T034 [P] Update CLAUDE.md documentation to note Commento integration in architecture section
- [ ] T035 [P] Document Commento configuration parameters added to `config/_default/params.toml`

---

## Dependencies & Execution Order

### Phase Dependencies

- **Setup (Phase 1)**: No dependencies - can start immediately
- **Foundational (Phase 2)**: Depends on Setup completion - BLOCKS all user stories
- **User Stories (Phase 3-5)**: All depend on Foundational phase completion
  - User stories can proceed in parallel (if working with multiple developers)
  - Or sequentially in priority order (P1 → P2 → P3) for single developer
- **Polish (Phase 6)**: Depends on all desired user stories being complete

### User Story Dependencies

- **User Story 1 (P1)**: Can start after Foundational (Phase 2) - No dependencies on other stories
- **User Story 2 (P2)**: Can start after Foundational (Phase 2) - Builds on US1 HTML structure but independently testable
- **User Story 3 (P3)**: Can start after Foundational (Phase 2) - Adds styling to US1/US2 implementation

### Within Each User Story

- Layout file creation before HTML modifications
- Configuration before testing
- Local testing before deployment
- Basic functionality before advanced features

### Parallel Opportunities

- All Setup tasks can run in parallel (T002, T003 marked [P])
- US2 configuration tasks (T018, T019) can run in parallel once Commento dashboard accessible
- US3 CSS development tasks (T020, T021, T022) can overlap
- All Polish tasks marked [P] (T026, T027, T028, T029, T034, T035) can run in parallel

---

## Parallel Example: User Story 1

```bash
# These tasks can run together:
Task T011: "Test locally with hugo server -D"
Task T012: "Test comment submission"
Task T013: "Verify comments persist on page refresh"
# (All testing tasks after implementation is complete)
```

---

## Implementation Strategy

### MVP First (User Story 1 Only)

1. Complete Phase 1: Setup
2. Complete Phase 2: Foundational (CRITICAL - blocks all stories)
3. Complete Phase 3: User Story 1 (tasks T007-T013)
4. **STOP and VALIDATE**: Test User Story 1 independently with `hugo server`
5. If working, proceed to deployment testing or move to US2

### Incremental Delivery

1. Complete Setup + Foundational → Foundation ready
2. Add User Story 1 (T007-T013) → Test independently → Deploy to preview (MVP!)
3. Add User Story 2 (T014-T019) → Test independently → Deploy update
4. Add User Story 3 (T020-T025) → Test independently → Deploy final version
5. Each story adds value without breaking previous stories

### Single Developer Sequential Strategy

1. Complete Setup (Phase 1)
2. Complete Foundational (Phase 2)
3. Implement US1 → Test → Document
4. Implement US2 → Test → Document
5. Implement US3 → Test → Document
6. Complete Polish phase for production readiness
7. Deploy and verify on production

---

## Notes

- [P] tasks = different files, no dependencies, can run in parallel
- [Story] label (US1, US2, US3) maps task to specific user story for traceability
- Each user story should be independently completable and testable
- Commento is a hosted service - no backend code needed, only frontend integration
- Hugo template overrides allow customization without modifying theme files directly
- Commit after each user story phase completion
- Stop at any checkpoint to validate story independently
- **Constitution Compliance**:
  - Follows Hugo Best Practices (Principle II): Template overrides, local testing required
  - Follows Deployment Rigor (Principle III): Local build testing, production verification required
  - Test before commit, verify after deployment
