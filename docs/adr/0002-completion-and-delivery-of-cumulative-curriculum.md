---
status: proposed
date: 2026-09-09
authors:
  - Igors Dubanevics
  - Antigravity
supersedes: docs/adr/0001-restructure-gecs-as-a-cumulative-research-project.md
---

# ADR 0002: Completion, Polish, and Delivery of the Cumulative Research-Project Curriculum

## Context and Problem Statement

[ADR 0001](0001-restructure-gecs-as-a-cumulative-research-project.md) established the foundational architecture for restructuring GECS into a four-session cumulative research-project course based on the Project Gutenberg `bookstats` project. 

The primary structural milestones in the codebase have now been drafted:
- The canonical reference project `bookstats` is implemented, tagged, and tested across all 9 milestones.
- The course directory has been renamed to `sessions/`.
- All four core sessions (`version-control.qmd`, `virtual-environments.qmd`, `testing.qmd`, `automation.qmd`) have been written around the cumulative project model.
- Progressive Git diagrams with blue (`main`) and red (feature branch) styling have been added, and original course quotes have been restored.

However, to finish the implementation and deliver a production-ready, publishable course website, several inconsistencies, orphaned files, slide mismatches, and validation gaps must be systematically resolved. This ADR defines the scope, decisions, and completion criteria for the final implementation phase.

## Decision Drivers

1. **Pedagogical Alignment**: Every session document, slide deck, and reference page must tell the exact same cumulative story: starting from an empty repository to a published interactive analysis.
2. **Visual Ergonomics**: No visual overlap or crowded diagram text; all graphics must be legible, color-consistent, and directly synchronized with the adjacent instructional text.
3. **No Legacy Orphans**: Eliminate or repurpose legacy course remnants (e.g. `sessions/project-structure.qmd`, outdated `gecs-make` slide references, outdated dates/schedules) that contradict the 4-session architecture.
4. **Autonomous Participant Experience**: Reinforce that participants can analyze any books from Project Gutenberg (Frankenstein and Dracula are non-mandatory worked examples).
5. **Verified Automation & Deployment**: The published reference repository on GitHub must build and deploy its GitHub Pages artifact without manual intervention or email privacy blocks.

---

## Decisions

### 1. Information Architecture & Legacy File Deprecation

1. **Retire `sessions/project-structure.qmd`**:
   - In the legacy 5-lesson curriculum, "Project Structure" was a standalone session. In the 4-session architecture (ADR 0001), project structure (separating `data/raw/`, `data/intermediate/`, `src/bookstats/`, `output/`) is taught organically as part of **Session 2: Reproducible Projects**.
   - `sessions/project-structure.qmd` will be moved to `docs/archive/` or converted into an optional reference appendix, removing it completely from the active Quarto website navigation.
2. **Update Course Landing Page (`index.qmd`)**:
   - Update the course schedule table on `index.qmd` to list the four sessions:
     1. Collaboration with Git and GitHub
     2. Reproducible Projects
     3. Code Quality and Testing
     4. Automation and Publication
   - Retain the one-time "Setting Up" pre-course clinic announcement.
   - Synchronize date and semester metadata.

### 2. Slide Decks Modernization

1. **Align `slides/automation.qmd`**:
   - Update `slides/automation.qmd` so that paths, script names, and diagrams reflect `bookstats` conventions (`data/raw/`, `data/intermediate/`, `data/processed/`, `src/bookstats/`, `Makefile`, Marimo) rather than the legacy `gecs-make` / `scripts/count_words.py` / `counts/` conventions.
2. **Author Complementary Slide Decks**:
   - Provide minimal, clean revealjs slide decks for Sessions 1–3 (`slides/version-control.qmd`, `slides/virtual-environments.qmd`, `slides/testing.qmd`) matching the design system of `slides/automation.qmd`.

### 3. Progressive Visual System and Diagram Standards

1. **Mermaid Graph Styling**:
   - All Git branching diagrams must strictly use:
     - `main` branch: Royal Blue (`#2563eb`)
     - Feature branches: Crimson Red (`#dc2626`)
     - Commit badges: Light slate (`#f1f5f9`) with dark text (`#1e293b`)
   - Commit labels must be concise (max 3–4 words) to prevent label clipping and visual collision on narrow viewports.
2. **TDD Flowcharts**:
   - Standardize the Red-Green-Refactor diagrams across Session 3 and slides with clear, pastel-tinted cards (`#fee2e2` for Red, `#dcfce7` for Green, `#e0e7ff` for Refactor).
3. **DAG Representations**:
   - Standardize node colors for file DAGs:
     - Raw data: Violet/Indigo (`#e0e7ff`)
     - Intermediate/Processed data: Blue (`#dbeafe`)
     - Output reports & figures: Green (`#dcfce7`)
     - Code & Python prerequisites: Amber/Gold dashed (`#fef3c7`)

### 4. Canonical Project Verification & Remote CI

1. **Verify Push to Remote**:
   - Push the clean git history of `/Users/igorsdubanevics/GitHub/bookstats` to `https://github.com/igorsdub/bookstats.git` with privacy-protected noreply email.
2. **Verify Live GitHub Pages**:
   - Trigger the GitHub Actions workflow (`.github/workflows/publish.yml`) and confirm that the live Marimo application renders and interactive sliders/dropdowns function on GitHub Pages.

---

## Consequences

### Positive
- The course website will be 100% internally consistent: every session, reference page, slide deck, and diagram will reinforce the single cumulative Gutenberg project.
- No confusion for learners caused by outdated filenames (`scripts/count_words.py` vs `src/bookstats/counts.py`) or legacy session numbers.
- Ready-to-deliver teaching materials for instructors with both interactive web documentation and high-resolution presentation slides.

### Risks & Mitigations
- *Risk*: Slide creation could expand scope significantly.
  *Mitigation*: Keep slides concise (15–20 high-impact visual slides per session) focusing on high-level mental models (the dial, the bus factor, the TDD loop, the DAG) while delegating step-by-step code execution to the website sessions.

---

## Implementation Checklist

- [ ] **Archive Legacy Session**: Move `sessions/project-structure.qmd` out of the website build.
- [ ] **Update Schedule (`index.qmd`)**: Reflect the 4-session sequence and confirm room/time placeholders.
- [ ] **Revise Automation Slides (`slides/automation.qmd`)**: Update to `bookstats` package and Makefile targets.
- [ ] **Create Slide Decks for Sessions 1–3**:
  - [ ] `slides/version-control.qmd`
  - [ ] `slides/virtual-environments.qmd`
  - [ ] `slides/testing.qmd`
- [ ] **Verify Remote Deployment**: Push `bookstats` to remote and verify GitHub Pages publication.
- [ ] **Full Quarto Build**: Ensure `quarto render` compiles cleanly with zero warnings or broken references.
