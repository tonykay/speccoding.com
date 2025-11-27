# Feature Specification: Migrate from Commento to Disqus Comments

**Feature Branch**: `003-i-d-like`
**Created**: 2025-11-27
**Status**: Draft
**Input**: User description: "I'd like to disable commento and using Disqus instead for my comments, my id is @disqus_YWibv6kNhd"

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Reader Uses Disqus Comments (Priority: P1)

Blog readers can leave comments, reply to other comments, and engage in discussions using the Disqus commenting platform instead of Commento.

**Why this priority**: This is the core migration requirement - switching the comment system from Commento to Disqus. Without this, existing comment functionality would be lost.

**Independent Test**: Navigate to any published blog post on https://specoding.com/, scroll to the comments section, and verify the Disqus widget loads with the correct site identifier.

**Acceptance Scenarios**:

1. **Given** a reader views a blog post, **When** they scroll to the bottom, **Then** they see the Disqus comment widget (not Commento)
2. **Given** Disqus is loaded, **When** a reader posts a comment, **Then** the comment appears in the Disqus thread
3. **Given** existing functionality, **When** the migration is complete, **Then** all Disqus features work (replies, reactions, sorting)

---

### User Story 2 - Clean Removal of Commento (Priority: P2)

The site no longer loads Commento JavaScript or CSS resources, reducing page weight and preventing conflicts between comment systems.

**Why this priority**: Essential for clean migration - prevents confusion and resource waste from loading two comment systems simultaneously.

**Independent Test**: View page source and network tab, verify no requests to cdn.commento.io or loading of commento-custom.css.

**Acceptance Scenarios**:

1. **Given** a blog post page loads, **When** inspecting network requests, **Then** no Commento CDN requests are made
2. **Given** the page HTML, **When** searching for "commento", **Then** no Commento-related code appears
3. **Given** the static files, **When** reviewing CSS files, **Then** commento-custom.css is removed

---

### Edge Cases

- What happens if Disqus service is temporarily unavailable?
- How does the page render if JavaScript is disabled?
- Are there any existing Commento comments that need migration (likely not applicable for public CDN)?
- Does dark mode work correctly with Disqus theme?

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: Blog posts MUST display Disqus comments at the end of post content
- **FR-002**: Disqus MUST be configured with shortname `disqus_YWibv6kNhd` (extracted from provided ID)
- **FR-003**: Commento configuration MUST be disabled in site parameters
- **FR-004**: Commento partial template code MUST be removed or commented out
- **FR-005**: Custom Commento CSS file MUST be removed from static assets
- **FR-006**: Disqus widget MUST load asynchronously without blocking page render
- **FR-007**: Comments MUST be associated with correct page URLs for thread identification
- **FR-008**: Disqus MUST support threaded replies (default Disqus behavior)
- **FR-009**: The comment section MUST remain visually consistent with blog design
- **FR-010**: Site build process MUST complete without errors after migration

### Assumptions

- Hugo-clarity theme already includes Disqus support via `_internal/disqus.html` template
- The Disqus shortname is `disqus_YWibv6kNhd` (format suggests this is the account identifier)
- No comment data needs to be migrated (Commento was using public CDN, likely no persistent comments)
- Disqus's default styling is acceptable or can be customized via Disqus admin panel
- Site uses Hugo's standard Disqus integration pattern
- Readers are familiar with Disqus (widely used commenting platform)

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: Disqus widget loads and displays correctly on all blog post pages
- **SC-002**: No Commento-related network requests appear in browser DevTools
- **SC-003**: Page load time does not increase significantly (Disqus async loading)
- **SC-004**: Readers can successfully post comments via Disqus without errors
- **SC-005**: Comment threading and reply functionality works correctly
- **SC-006**: Site builds successfully with `hugo --gc --minify` after changes
- **SC-007**: The migration completes without breaking existing site functionality
