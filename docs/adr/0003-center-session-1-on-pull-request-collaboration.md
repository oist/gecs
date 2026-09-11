---
status: accepted
date: 2026-09-11
supersedes: "Session 1 design in ADR 0001"
---

# Center Session 1 on pull-request collaboration

Session 1 teaches collaboration rather than Git command coverage. Participants
complete two branch-and-pull-request cycles: first in their own course
repository, where they learn to make work inspectable, and then reciprocally in
a collaborator's repository, where issues, independent review, and the
fork-based contribution workflow gain their social meaning. This deliberate
repetition replaces the earlier sequence in ADR 0001 because introducing the
first book directly on `main` left branches and pull requests disconnected from
the participant's own work.

## Decision

Each participant starts by committing a minimal README on `main` and publishing
the course repository. They then add their first book and its README provenance
entry together on a short-lived branch, inspect the change in a self-authored
pull request, merge with a merge commit, synchronize `main`, and delete the
completed branch. The self pull request has no issue and cannot be self-approved.

Participants then work in pairs identified as Collaborator A and Collaborator
B. The active roles are repository owner, contributor, author, and reviewer.
The collaborators agree directly on the proposed book (GitHub Issues are covered
conceptually in an optional tip rather than as a required classroom blocker).
Collaborator B forks the owner's repository under a distinct name
(`gutenberg-analysis-<collaborator-a>`) to avoid personal namespace collisions,
clones the fork, and configures `origin` as the fork and `upstream` as the original
repository. One different book and its README provenance entry form one focused
commit. The pull request proposes the change to Collaborator A's `main`; the owner
inspects the diff, leaves a meaningful comment, approves, and creates a merge
commit in the base repository's `main`. Both people synchronize and clean up before
swapping roles and returning to the start of the collaborator workflow.

VS Code is the supported local path. Pull requests and reviews use GitHub's
website; Bash and Zsh equivalents and the VS Code pull-request extension remain
optional tips. Terminology is introduced at first use. Required actions and
expected results stay visible, while troubleshooting and deeper explanations
may be collapsed. The expected classroom path does not manufacture requested
changes or merge conflicts; unexpected conflicts are an instructor-help case.

Session 1 ends with **Your Turn**, which asks participants to start using Git
for one of their own projects: create a repository, make an initial commit, push
it to GitHub, and use short-lived branches for later changes. There is no formal
completion check for this application; experiences and obstacles feed into the
next Project Clinic.

## Consequences

- Collaboration is the learner-facing outcome and the completion checklist is
  behavioral.
- A focused commit may contain multiple files when they express one complete
  change.
- Required location checks are explained fully on first use and shortened when
  the collaborators swap roles.
- Recovery snapshots do not substitute for authoring and reviewing pull
  requests, so Session 1 does not present one as a completion path.
- The wider implementation proposed by ADR 0002 remains paused; this ADR changes
  only Session 1.

## Implementation contract

### Scope and source files

Implement this decision primarily in `sessions/version-control.qmd`. Make only
the smallest supporting changes needed in `CONTEXT.md`, `_quarto.yml`,
`sessions/setting-up.qmd`, and the landing page. Do not resume the Session 2–4,
slide, reference-project, or deployment work proposed by ADR 0002 as part of
this change.

Use **session**, never **lesson**, in new or revised learner-facing copy. Use
**collaborator**, never **partner**, for another participant. Collaborator A and
Collaborator B are stable people; repository owner, contributor, author, and
reviewer are temporary roles that reverse when the collaborators swap.

### Required page sequence

The Session 1 page must use this order:

1. opening collaboration framing;
2. visible readiness callout linking to `sessions/setting-up.qmd`;
3. behavioral learning outcomes;
4. brief course-project and data-suitability explanation;
5. **Collaborate with your future self**;
6. **Collaborate with another person**;
7. **Completion checklist**; and
8. **Your turn**.

The “future self” phrase is a light narrative device, not a glossary term. Do
not propagate it into `CONTEXT.md` or force every instruction to repeat it.

### Readiness seam

Session 1 assumes, but does not reteach, that:

- the participant has a GitHub account with MFA;
- Git has the participant's name and email;
- VS Code can access Git; and
- GitHub authentication succeeds.

Keep `sessions/setting-up.qmd` as the linked recovery path. Do not repeat
installation, MFA, authentication, or editor-configuration procedures inside
Session 1.

### Self pull-request cycle

Implement the first cycle exactly as follows:

1. create and open the local `gutenberg-analysis` directory;
2. initialize Git with `main` as the default branch;
3. create a minimal README containing project purpose, author, an empty book
   provenance table, the filename convention, repository contents, and visible
   placeholders for setup, running, and reproduction;
4. commit the README directly on `main` as
   `docs: start project README`;
5. publish `<participant-handle>/gutenberg-analysis` as a public GitHub
   repository, creating `origin`;
6. create and switch to `add-frankenstein`;
7. add `00084_frankenstein.txt` and its title, author, Gutenberg ID, and source
   URL to the README;
8. commit both files together as `data: add Frankenstein`;
9. push `add-frankenstein` to `origin`;
10. open a pull request whose base is the participant's `main` and whose compare
    branch is the participant's `add-frankenstein`;
11. inspect **Files changed** and leave a general comment confirming that the
    filename and source metadata agree;
12. explain that a pull-request author cannot approve their own pull request;
13. merge using **Create a merge commit**;
14. delete the remote branch using GitHub's post-merge control;
15. switch the local checkout to `main`, pull `origin/main`, verify the book,
    and delete the local branch.

Do not open an issue for the self pull request. A participant may substitute a
different Gutenberg book, but *Frankenstein* is the verified worked path and
must remain available as the low-friction fallback.

### Reciprocal collaborator cycle

Both participants must complete both roles. Implement the first pass in full,
then instruct them to swap roles and return to the issue step; do not duplicate
the instructions as a separately authored second round.

For the first pass, Collaborator A is repository owner and reviewer;
Collaborator B is contributor and author:

1. Collaborators agree directly on the proposed Project Gutenberg book and
   provenance (issues are covered in an optional tip box).
2. Collaborator B forks Collaborator A's repository, naming the fork
   `gutenberg-analysis-<collaborator-a>` to avoid account namespace collisions.
3. Collaborator B clones their fork into a clearly distinguishable local parent
   directory named for Collaborator A's GitHub handle, then opens it in a new VS
   Code window.
4. The clone-created `origin` must point to Collaborator B's fork.
5. Add `upstream` pointing to Collaborator A's original repository and verify
   both remotes before editing.
6. Create a short-lived branch named for the book, such as `add-dracula`.
7. Add one book file and its README provenance entry in one commit, such as
   `data: add Dracula`.
8. Push the contribution branch to `origin`, never directly to `upstream`.
9. Open the pull request with Collaborator A's `main` as the base and
   Collaborator B's contribution branch as the compare branch.
10. Describe the book addition and README provenance in the pull-request description
    (with issue-closing keywords like `Closes #N` explained in an optional tip).
11. Collaborator A inspects the diff against the agreed book, filename convention,
    provenance, commit meaning, and intended scope; leaves a meaningful comment;
    and approves.
12. Collaborator A selects **Create a merge commit**. Explain that the merge
    commit is created on Collaborator A's base repository `main`, not in
    Collaborator B's fork.
13. Collaborator A pulls `origin/main` into their own local repository.
14. Collaborator B uses **Sync fork > Update branch** on GitHub, switches their
    collaborator checkout to `main`, pulls `origin/main`, verifies the merge,
    and deletes the completed local branch.
15. Only after both copies are synchronized and clean do the collaborators swap
    roles and return to the fork step.

Do not put `Closes #N` in a fork commit message. Do not put a Gutenberg ID in a
commit message with a leading `#`, because GitHub interprets that syntax as an
issue or pull-request reference.

### Interface adapters

VS Code is the primary narrated interface for local repository work. GitHub's
website is the primary interface for issues, pull requests, reviews, fork
synchronization, and merges. Mention the GitHub Pull Requests VS Code extension
only in an optional tip.

Place a collapsed Bash/Zsh equivalent immediately after each required VS Code
operation where a concise equivalent is useful. Terminal equivalents must
preserve the same repository, branch, and remote semantics. The contributor
synchronization equivalent must use fetch and merge, not rebase:

```bash
git fetch upstream
git switch main
git merge upstream/main
git push origin main
```

### Callout rules

Use Quarto callouts consistently:

- `callout-note`: define a concept at first use;
- `callout-tip`: optional Bash/Zsh or VS Code-extension adapter;
- `callout-important`: first-use destination check;
- `callout-warning`: genuinely risky cleanup ordering; and
- collapsed callout: optional troubleshooting or deeper explanation only.

Keep required GUI actions, expected observations, and destination checks
visible. A learner who ignores every collapsed callout must still be able to
complete the session.

Explain the full location check only before the first operation of each kind:

- first commit: open folder and current branch;
- first push: destination remote and branch;
- first pull request: base and compare repositories and branches;
- first merge: repository and branch receiving the merge commit.

When roles swap, replace the repeated explanations with one short instruction
to check the open folder, current branch, and intended destination.

### Visual contract

Use no more than four substantial instructional diagrams:

1. local `main` plus the first-book branch;
2. upstream repository, contributor fork (`origin`), and local clone;
3. contribution branch flowing toward the base repository's `main`; and
4. synchronized history after self and collaborator merge commits.

Use royal blue `#2563eb` for `main`, crimson `#dc2626` for short-lived branches,
and concise commit labels. Small destination callouts do not count as
substantial diagrams. Do not add a Git graph after every commit.

### Smooth-path constraint

Design the classroom exercise to merge cleanly. Do not seed errors, require a
requested-changes cycle, teach conflict resolution, introduce rebase, or compare
merge strategies in the main flow. A collapsed instructor-help note may say
that an unexpected conflict should be paused and resolved with an instructor.

### Completion and follow-up

The completion checklist must verify behavior, not terminology recall. Every
participant must have:

- published their own public course repository;
- merged their first book through a self pull request;
- authored an issue-linked pull request from a fork;
- reviewed and approved a collaborator's pull request;
- created a merge commit as repository owner;
- completed both the contributor and repository-owner roles;
- synchronized the relevant local repositories and fork;
- removed completed branches; and
- identified where `origin`, `upstream`, `main`, and the contribution branch
  live.

The closing **Your turn** section must be short and ungraded: ask participants
to create a Git repository for one of their own projects, make an initial
commit, push it to GitHub, and use short-lived branches for later changes.
Invite questions, obstacles, and discoveries into the next Project Clinic. Do
not mention OIST, organization-specific policy, minimum/better levels, or a
formal completion check.

### Exclusions

Remove or omit all of the following from the Session 1 learner path:

- the `git-collaboration` recovery snapshot as a substitute for participation;
- an issue for the self pull request;
- separate commits for a book file and its README provenance;
- `Closes #N` in commit messages;
- a duplicated reciprocal-round walkthrough;
- mandatory requested changes;
- merge-conflict resolution;
- rebasing and detailed Git internals;
- exhaustive Git command coverage; and
- OIST-specific instructions in **Your turn**.

### Validation

Before considering the implementation complete:

1. run `git diff --check`;
2. render `sessions/version-control.qmd` with the repository's Quarto tooling;
3. inspect the rendered page for heading hierarchy, expanded versus collapsed
   callouts, diagram legibility, image paths, and internal links;
4. verify that every visible GUI path has the intended destination and every
   terminal adapter has equivalent semantics;
5. search Session 1 for rejected terminology and obsolete instructions,
   including `Partner`, `Transfer Task`, `Closes #` in commit messages,
   `git pull upstream main`, and the recovery checkpoint; and
6. rehearse or time the complete reciprocal workflow before delivery. The
   target allocation is 10 minutes readiness and framing, 30 minutes self pull
   request, 5 minutes pairing, 30 minutes first collaborator pass, 30 minutes
   swapped pass, 10 minutes synchronization and cleanup, and 5 minutes closing.
