<!--
SYNC IMPACT REPORT
===================
Version Change: Template → 1.0.0
Type: MINOR (Initial constitution creation)
Date: 2025-11-27

Modified Principles:
- All principles newly created (first version)

Added Sections:
- Core Principles (5 principles for Hugo blog)
- Content Quality Standards
- Development Workflow
- Governance

Removed Sections:
- None (template baseline)

Templates Requiring Updates:
- ✅ spec-template.md: Aligned (user scenarios fit blog content planning)
- ✅ plan-template.md: Aligned (constitution check section ready)
- ✅ tasks-template.md: Aligned (phases match blog workflow)

Follow-up TODOs:
- None (all placeholders filled)

Rationale:
- MINOR version bump: First constitution establishes principles from scratch
- Blog-specific principles: Content quality, Hugo workflow, deployment rigor
- Governance includes amendment process and compliance verification
===================
-->

# Spec Coding Blog Constitution

## Core Principles

### I. Content Quality & Accuracy

Content published on SpecCoding.com MUST meet rigorous editorial standards:

- All technical claims MUST be verifiable through code examples, documentation links, or reproducible demonstrations
- Blog posts MUST include working code samples when demonstrating concepts
- External references MUST be cited with proper attribution and live links
- Draft posts MUST be reviewed for technical accuracy before publication
- Screenshots and images MUST be current and match described functionality

**Rationale**: As a blog teaching specification-driven development and AI-assisted coding, we have a responsibility to provide accurate, tested information. Readers trust us to guide their development practices.

### II. Hugo Best Practices

All site configuration and content MUST follow Hugo conventions and the hugo-clarity theme requirements:

- Configuration changes MUST be tested with `hugo server` locally before committing
- Production builds MUST use `hugo --gc --minify` to ensure optimal output
- Page bundles MUST be used for posts with multiple images or co-located assets
- Front matter MUST include required fields: `title`, `date`, `draft`, `tags`, `categories`
- Theme submodule updates MUST be tested for breaking changes before merging
- The `public/` directory MUST be committed for GitHub Pages deployment

**Rationale**: Hugo has established patterns that ensure build reliability, deployment consistency, and maintainability. Following these patterns prevents runtime errors and deployment failures.

### III. Deployment Rigor (NON-NEGOTIABLE)

All changes to the main branch MUST pass through the complete build and deployment pipeline:

- Local builds with `hugo --gc --minify` MUST complete without errors before pushing
- Git submodules MUST be initialized and updated (`git submodule update --init --recursive`)
- GitHub Actions workflow MUST pass all build steps
- Deployed site MUST be manually verified at https://specoding.com/ after deployment
- Breaking configuration changes MUST be tested in a branch first

**Rationale**: GitHub Pages deployment is unforgiving. A broken build means downtime for readers. The multi-step Hugo build process (Dart Sass compilation, minification, asset processing) has many failure points that must be caught before production.

### IV. Content Planning & Spec Alignment

Blog content development MUST follow specification-driven principles we teach:

- Content ideas MUST be captured with clear user value propositions before writing
- Blog posts MUST define their target audience and learning objectives upfront
- Complex tutorials MUST include step-by-step acceptance criteria (reader can verify their progress)
- Series content MUST be planned with dependency ordering (which posts prerequisite knowledge)
- Content specifications MUST use the `/speckit.specify` workflow for planning major posts or features

**Rationale**: We practice what we preach. Using spec-driven development for our own content creation demonstrates the methodology and ensures content quality through structured planning.

### V. Performance & User Experience

The site MUST remain fast, accessible, and user-friendly:

- Page weight MUST be minimized through image optimization and lazy loading
- Code blocks MUST be syntax-highlighted and easily readable (7 lines max before scrollable)
- Site search MUST be functional (JSON index generated via Hugo outputs)
- Navigation MUST be clear with properly configured menus in `menus/menu.en.toml`
- Mobile experience MUST be tested for all new layouts or theme customizations

**Rationale**: Technical readers value performance and accessibility. A slow or poorly-structured blog undermines our credibility as developers teaching best practices.

## Content Quality Standards

### Editorial Review Process

Before publishing (`draft = false`):

1. Technical accuracy verified through testing/validation
2. Code examples run successfully in described environment
3. Links checked for validity (no 404s)
4. Grammar and spelling reviewed
5. Images optimized (WebP where supported, reasonable dimensions)
6. Front matter complete and correct
7. SEO metadata present (description, OG image)

### Content Categorization

All posts MUST use taxonomies consistently:

- **Categories**: Broad topics (Spec Coding, AI Tools, Hugo, Development Practices)
- **Tags**: Specific technologies or concepts (Claude Code, TDD, Static Sites, Workflows)
- **Series**: Multi-part content grouped logically

### Code Sample Standards

Code samples MUST:

- Include language identifier for syntax highlighting
- Provide sufficient context to understand purpose
- Use realistic examples (avoid foo/bar unless teaching abstractions)
- Include comments where logic isn't self-evident
- Be tested before publication

## Development Workflow

### Local Development

Standard workflow:

1. Start server: `hugo server -D` (includes drafts)
2. Create content: `hugo new content/post/post-name.md`
3. Write and iterate with live reload
4. Test production build: `hugo --gc --minify`
5. Verify output in `public/` directory

### Git Workflow

1. Branch naming: `content/post-slug` for new posts, `feature/description` for site changes
2. Commit messages: Clear, descriptive (follow conventional commits)
3. Pull requests: Include screenshot/preview for visual changes
4. Merge to `main` triggers GitHub Actions deployment

### Theme Customization

When overriding theme templates:

1. Document reason for override in CLAUDE.md
2. Test across different content types
3. Ensure mobile compatibility
4. Consider theme update compatibility

## Governance

### Amendment Process

Constitution changes require:

1. Documented rationale for change
2. Version bump per semantic versioning (MAJOR.MINOR.PATCH)
3. Update to dependent templates (spec, plan, tasks)
4. Git commit with clear amendment description
5. Sync impact report embedded in constitution file

### Compliance Verification

All feature specifications and implementation plans MUST verify:

- Adherence to Hugo best practices (Principle II)
- Deployment testing requirements (Principle III)
- Content planning if applicable (Principle IV)

### Constitution Precedence

When conflicts arise:

1. Constitution principles override convenience
2. Non-negotiable principles (III) cannot be bypassed
3. Technical constraints may necessitate documented exceptions in implementation plans
4. Exceptions MUST justify why simpler compliant approaches were insufficient

### Living Document

This constitution:

- Evolves as the project matures
- Incorporates learnings from production issues
- Adapts to Hugo ecosystem changes
- Remains aligned with spec-driven development principles

**Version**: 1.0.0 | **Ratified**: 2025-11-27 | **Last Amended**: 2025-11-27
