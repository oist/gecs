---
title: "GECS course-structure architecture review"
date: 2026-09-11
status: exploration
---

# Scope

This review pauses the broader implementation described by proposed ADR 0002
and examines only the course spine, preparation material, and Session 1. It
records deepening opportunities, not accepted decisions. Any decision that
survives design discussion should be incorporated into the accepted curriculum
ADR or recorded in a focused successor ADR when the trade-off warrants one.

# Candidates

## Make the issue-to-merge journey the Session 1 module

**Recommendation: Strong — top recommendation**

Files: `sessions/version-control.qmd`, `sessions/setting-up.qmd`, and
`sessions/vs-code.qmd`.

The complete issue-to-merge lifecycle should be the Session 1 module's sole
learner-facing interface. The current implementation has a coherent spine, but
the seam leaks in two places: preparation duplicates part of the publish and
push workflow, while the session later uses `git pull upstream main` without
first configuring an `upstream` remote.

Deepening would let one journey own repository creation, issue creation, fork
and clone semantics, remote naming, a short-lived branch, focused commits, a
pull request, review, merge, synchronization, branch cleanup, recovery, and the
completion check. VS Code and Bash/Zsh procedures remain adapters to the same
journey rather than separate teaching paths.

Benefits:

- locality: one collaboration story;
- leverage: one tested lifecycle;
- the interface becomes the test surface; and
- preparation can test readiness without pre-teaching Session 1.

This aligns with accepted ADR 0001.

## Deepen “Getting ready” around readiness

**Recommendation: Strong**

Files: `sessions/setting-up.qmd`, `sessions/vs-code.qmd`,
`sessions/terminal.qmd`, and `CONTEXT.md`.

The 408-line setup page is shallow: its interface exposes nearly all of its
implementation before a participant can answer the practical question, “Am I
ready for Session 1?” Concentrate the required outcomes into a short checklist
and place platform-specific work behind macOS and Windows/WSL adapters.

Linux remains explicitly supported by accepted ADR 0001 and should share the
Bash implementation unless that decision is deliberately reopened.

Benefits:

- locality: platform changes stay in one adapter;
- leverage: every participant crosses one readiness seam; and
- verification follows the checklist rather than the prose sequence.

## Deepen the four-session course spine

**Recommendation: Strong**

Files: `_quarto.yml`, `index.qmd`, `CONTEXT.md`, and the curriculum ADRs.

The live sidebar presents four sessions, the landing page still presents five
topics, and accepted ADR 0001 mixes “lesson” and “session.” Concentrate one
four-session taxonomy and use “session” consistently. Proposed ADR 0002 should
remain paused and must not be treated as superseding accepted ADR 0001 while
this narrower design work is underway.

Benefits:

- locality: one course vocabulary;
- leverage: navigation and narrative agree; and
- competing shallow taxonomies disappear.

## Narrow reference material to optional adapters

**Recommendation: Worth exploring**

Files: `sessions/vs-code.qmd`, `sessions/terminal.qmd`, and
`sessions/version-control.qmd`.

The VS Code and terminal references repeat instructional sequences and retain
legacy choices. Keep core actions local to Session 1, with adjacent Bash/Zsh
equivalents, and narrow the reference module's interface to optional lookup.

Benefits:

- locality: each concept remains in its session;
- depth: references answer lookup questions; and
- adapters remain optional without becoming a second course path.

# Recommended exploration order

Start with the Session 1 issue-to-merge module. It matches the immediate course
deliverable and contains the clearest correctness defect. Settle its interface
and lifecycle first; then reshape “Getting ready” to establish exactly the
preconditions that Session 1 needs.
