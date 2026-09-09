---
status: accepted
---

# Restructure GECS as a cumulative research-project course

GECS will become a four-session, cumulative course in which each participant
starts a small research-style project from an empty repository and develops it
into a tested, automated, published analysis. The course will teach practices
through a discipline-neutral Project Gutenberg word-analysis project, use VS
Code as the supported interface, provide Bash and Zsh equivalents alongside
the GUI workflow, and require participants to transfer a small practice to
their current OIST project between sessions.

This ADR is the implementation contract for restructuring the website,
lessons, exercises, examples, and supporting materials. Treat its decisions as
accepted. Where it names a validation step, validate before writing learner
instructions rather than guessing about current tool behavior.

## Outcomes

By the end of the course, each participant must have a public course repository
that demonstrates all of the following:

1. a meaningful Git history beginning with an empty repository;
2. a minimal README that evolves with the project;
3. issue-driven collaboration through a fork, feature branch, pull request,
   review, merge commit, and branch deletion;
4. a Python 3.12 environment declared and locked with uv;
5. raw data, generated data, reusable source code, tests, notebooks, and
   reader-facing outputs separated by purpose;
6. an importable `bookstats` package using src layout;
7. Polars as the dataframe implementation used by the supplied analysis;
8. Ruff formatting and linting, NumPy-style docstrings, and a pre-commit hook;
9. a red-green TDD example, an adapted unit test, and a file-processing
   integration test;
10. a descriptive Zipf fit presented in static and interactive forms;
11. a Make DAG with documented `all`, `check`, `clean`, and `help` targets;
12. checks and publication performed by GitHub Actions; and
13. a published, interactive Marimo application on GitHub Pages.

Participants must also attempt one small transfer task in their current OIST
project after each session. Their OIST projects are independent of the public
course repository and never need to be made public.

## Teaching constraints

### Audience and language

The primary audience is OIST interns and students beginning a rotation,
internship, PhD project, or other scientific project. Their projects may be
new, small, short-lived, or exploratory; GECS must not assume that learners
maintain mature software.

The practices are language-transferable, but Python is the worked example
because it is familiar to the instructors and most participants. GECS is not a
Python programming course. Supply analysis code and ask learners to inspect,
run, move, document, format, test, and make small targeted changes to it. Do
not require learners to design the analysis from a blank Python file.

### Supported platforms and interfaces

Support these environments explicitly:

- macOS with Zsh;
- Linux with Bash; and
- Windows through WSL with Bash.

PowerShell and native Windows workflows are outside the supported path. Do not
pair participants according to operating system; participants choose their own
partners.

VS Code is the primary narrated interface. Put terminal equivalents next to
the relevant GUI operation in collapsible callouts. Terminal commands must use
Bash/Zsh syntax. When a backslash continues a command across lines, explain
that it is a shell line-continuation character. Prefer one-line commands when
that avoids unnecessary shell instruction.

Demonstrate environment activation once:

```bash
source .venv/bin/activate
deactivate
```

Thereafter prefer `uv run` in the README, Makefile, and GitHub Actions because
it makes the environment explicit and does not depend on remembered shell
state. In VS Code, show how to select `.venv` as the project interpreter.

### Pace and clinics

Keep the existing once-per-week pace. Sessions are two hours. Sessions 2, 3,
and 4 begin with a project clinic lasting no more than 15 minutes. In a clinic,
participants may share:

- the practice they attempted;
- what changed in their project;
- an obstacle or failed attempt; or
- the result of the change.

An unsuccessful attempt is a valid clinic contribution. Do not grade transfer
tasks or require disclosure of sensitive research code or data.

## Course project and naming conventions

Each learner owns a separate public course repository. The project analyzes
plain-text books selected by the learner from Project Gutenberg. Raw books are
small and redistributable, so commit them for this course. Explain that real
research data may instead be too large, sensitive, licensed, or externally
managed for Git.

Use lowercase hyphenated repository and branch names. Use the following raw
book filename convention:

```text
<five-digit-gutenberg-id>_<hyphenated-short-title>.txt
```

Examples:

```text
00084_frankenstein.txt
00345_dracula.txt
01342_pride-and-prejudice.txt
```

The underscore separates metadata fields; hyphens represent spaces within the
title. Explain that the padded number is the Project Gutenberg ebook ID, not an
arbitrary sequence number. The README must document this convention and give
the title, author, Gutenberg ID, and source URL for every committed book.

Use lowercase `bookstats` for the Python import package. The repository may be
named `gutenberg-analysis`; the repository and import package have different
names because they have different roles.

## Repository evolution

Do not give learners the final tree at the beginning. The project must earn its
structure through concrete problems.

### Initial state

```text
gutenberg-analysis/
├── README.md
├── 00084_frankenstein.txt
└── count_words.py
```

The repository begins empty. The README and books arrive during lesson 1. The
supplied script arrives during lesson 2. This deliberately flat state makes
overwriting, undeclared dependencies, and mixed responsibilities visible.

### Temporary script-oriented state

```text
gutenberg-analysis/
├── data/
│   ├── raw/
│   └── intermediate/
├── scripts/
│   └── count_words.py
├── README.md
├── pyproject.toml
└── uv.lock
```

This state teaches that separating data and executable scripts is useful. Do
not present `scripts/` as inherently wrong; it is appropriate for one-off
commands. It becomes insufficient only when tests, Make, and Marimo need to
reuse the same functions.

### Final state

```text
gutenberg-analysis/
├── .github/
│   └── workflows/
│       └── publish.yml
├── data/
│   ├── raw/
│   │   └── 00084_frankenstein.txt
│   ├── intermediate/
│   │   └── 00084_frankenstein.csv
│   └── processed/
│       └── book-counts.csv
├── notebooks/
│   └── visualize.py
├── output/
│   ├── figures/
│   │   ├── zipf-law.svg
│   │   └── zipf-law.html
│   └── tables/
│       └── zipf-fits.csv
├── src/
│   └── bookstats/
│       ├── __init__.py
│       ├── counts.py
│       └── zipf.py
├── tests/
│   ├── test_counts.py
│   └── test_integration.py
├── .gitignore
├── .pre-commit-config.yaml
├── .python-version
├── Makefile
├── README.md
├── pyproject.toml
└── uv.lock
```

Commit `data/raw/`, source, tests, notebook source, configuration, README, and
workflow definitions. Ignore `.venv/`, `data/intermediate/`,
`data/processed/`, `output/`, and `_site/`. GitHub Actions regenerates ignored
artifacts from committed inputs.

Use src layout as the taught default. Include a short collapsible note showing
flat layout and explaining that src layout keeps importable code separate from
project files. Do not turn this comparison into a packaging lecture.

## Lesson 1: Git and GitHub collaboration

### Learning outcome

Starting from an empty repository, the learner creates a documented data
project and completes an issue-to-merge collaboration workflow with a partner.

### Required sequence

1. Create an empty local repository through VS Code and initialize Git.
2. Add a minimal `README.md` containing the project name, author, purpose, and
   current contents. Include placeholders for running and reproducing the
   project; the learner does not yet know those commands.
3. Select at least one Project Gutenberg book. Download it as plain text and
   name it with the course convention.
4. Commit books individually. If a learner adds several books, each book gets
   its own focused commit.
5. Publish the repository to GitHub as a public repository.
6. Pair participants by their own choice.
7. In the partner's repository, open an issue proposing another book. State
   its title, author, Gutenberg ID, and source URL.
8. Fork the partner's repository, then clone the fork locally. Make the
   fork-versus-clone distinction explicit: the learner clones their own fork
   because they do not have write permission to the partner's repository.
9. Create a short-lived branch such as `add-frankenstein`.
10. Add at least one correctly named book and update the README's book metadata.
11. Commit each added book separately, push the branch, and open a pull request
    against the partner's `main`. Reference the issue with `Closes #N`.
12. Switch roles. Review the diff, leave a comment, approve or request a change,
    and merge with **Create a merge commit**.
13. Delete the merged remote branch and remove or leave the local branch only
    after switching away from it.
14. The repository owner pulls the merged `main` and inspects the branch-and-
    merge shape in VS Code's Source Control Graph.

Make issues part of the core workflow, not an optional extension. Mention
squash and rebase merging only as alternatives; use a merge commit because its
branch join is visible in the history graph. Move conflicts, detailed Git
internals, advanced restoration, licensing, and exhaustive command coverage to
reference material so the collaboration loop fits in two hours.

### README state after lesson 1

The README must contain:

- project name;
- author;
- one-sentence purpose;
- a table or list of books with title, author, Gutenberg ID, and source URL;
- a short explanation of the filename convention;
- a summary of current repository contents; and
- explicit placeholders for setup, running, and reproduction instructions.

### Transfer task

In the learner's current OIST project, do one safe action: initialize local Git,
add or improve a minimal README, or make a focused change on a branch. Publishing
the OIST project is not required.

### Completion criterion

Lesson 1 is complete only when every learner has a public course repository,
has both authored and reviewed a pull request, has merged through a merge
commit, and can identify `main`, the contribution branch, and the merge in the
graph.

## Lesson 2: Reproducible project

### Learning outcome

The learner declares a reconstructable Python environment, experiences an
undeclared dependency, separates data from scripts, converts reusable analysis
into the `bookstats` package, and uses Marimo to inspect a basic visualization.

### Branch and pull-request sequence

Every branch produces a pull request, is merged, and is deleted before the next
branch begins:

1. `set-up-venv`
2. `add-word-counting`
3. `separate-data`
4. `create-package`

Use the same partner when available so participants can update an existing fork
and see the repository evolve. If that partner is unavailable, choose another;
do not block the exercise on a fixed pairing.

### `set-up-venv`

1. Pin Python 3.12 for the project.
2. Initialize uv in the existing, non-empty repository.
3. Inspect `.python-version`, `pyproject.toml`, `uv.lock`, and `.gitignore` in
   VS Code. Explain each file before committing it.
4. Confirm that `.venv/` is ignored.
5. Activate and deactivate `.venv` once to make the environment concrete.
6. Commit the environment files, open a pull request, review, merge, and delete
   the branch.

Before publishing learner instructions, prototype the exact uv initialization
command in a repository that already contains README and data. Confirm that it
preserves existing files, pins Python 3.12, creates src layout at the intended
stage rather than prematurely, and behaves consistently on macOS and Linux.
Use the validated command in the lesson.

Explain that uv does not scan Python imports to infer project dependencies.
Imports express runtime requirements; `pyproject.toml` explicitly declares
installable requirements. uv may obtain a compatible Python interpreter, but
Python itself is not added with `uv add`.

### `add-word-counting`

1. Download a prepared `count_words.py` from the GECS site into the repository
   root.
2. The script begins with a docstring that states its purpose and exact plain-
   Python invocation.
3. Attempt to run it before Polars is declared. The shared intended failure is
   `ModuleNotFoundError` for Polars; absence of a system `python` executable is
   platform-dependent and must not be the only teaching path.
4. Run `uv add polars`. Inspect changes to `pyproject.toml`, `uv.lock`, and the
   environment. Explain that uv resolves and synchronizes as part of this step.
5. Rerun successfully, first after activation and then with `uv run`.
6. Use separate commits such as `Add word-counting script`, `Add Polars
   dependency`, and `Document word-counting command`.
7. Open, review, merge, and delete the pull-request branch.

### `separate-data`

Use a controlled overwrite exercise to motivate the structure:

1. confirm that a raw course book is committed;
2. accidentally supply the raw `.txt` path as the script's output path;
3. inspect the destructive diff in VS Code;
4. restore the committed raw book through Source Control;
5. rerun with a `.csv` output;
6. create `data/raw/`, `data/intermediate/`, and `scripts/`;
7. move books and the script to their appropriate directories; and
8. update commands and README descriptions, then review and merge the branch.

Use course data only for the overwrite exercise. The point is to experience
recovery and discover a boundary, not to endanger participant research data.

Show the project tree through VS Code Explorer. Add a collapsible terminal note
for `tree -L 2`, including platform-specific installation instructions. The
tree utility is illustrative, not a prerequisite for the lesson.

### `create-package`

1. Identify functions reused by commands, tests, and visualization.
2. Move reusable code from `scripts/count_words.py` into
   `src/bookstats/counts.py` and export the intended interface from
   `src/bookstats/__init__.py`.
3. Retain only a minimal executable wrapper if one is still useful; do not
   duplicate processing logic between wrapper and package.
4. Run the package through a validated command such as
   `uv run python -m bookstats.counts INPUT OUTPUT`.
5. Add `notebooks/visualize.py`. It reads generated count data and presents a
   basic interactive Altair histogram or rank-frequency view. It does not own
   the processing pipeline.
6. Update the README with `uv sync`, the exact `uv run` command, the repository
   structure, and how to open the Marimo visualization.
7. Review, merge, and delete the branch.

Validate the module invocation, output paths, and Marimo command in the actual
course repository before documenting them.

### Cross-language and HPC notes

Add a short “Same practice, different ecosystem” note comparing Python uv with
R renv and Julia `Project.toml`/`Manifest.toml`. The point is to declare and lock
dependencies per project, not to present uv as universal.

Add a separate collapsible “Beyond language environments” note. Explain that
containers also capture operating-system libraries and tools; Docker is common
for local/service container workflows; and Apptainer/Singularity is common on
HPC systems and can consume Docker/OCI images. Link to further reading. Do not
teach container commands in this edition.

### Transfer task

In the learner's OIST project, record one undeclared dependency, add or repair
environment setup instructions, improve separation between data and source, or
move one reusable function out of a notebook or script. At the next clinic,
explain the changed boundary and whether the project still ran.

### Completion criterion

Lesson 2 is complete only when the learner can reconstruct the environment from
committed declarations, run the analysis through uv, explain why `.venv` is not
committed, and import reusable code from `bookstats` independently of Marimo.

## Lesson 3: Code quality, testing, and Zipf analysis

### Learning outcome

The learner distinguishes formatting, linting, documentation, unit testing,
integration testing, and scientific interpretation; experiences a red-green
TDD cycle; and extends the project with a descriptive Zipf fit.

### Required sequence

1. Begin after the clinic with a five-to-ten-minute pandas repository
   microscope. Show how much of a mature repository is dedicated to tests and
   how tests are organized. Ask one focused question; do not tour the entire
   repository.
2. Introduce Ruff formatting and linting.
3. Configure NumPy-style public-function docstrings.
4. Install the pre-commit hook and experience a hook-modified commit.
5. Introduce red-green-refactor through one failing unit test written together.
6. Adapt one second unit test.
7. Run a prepared file-to-CSV integration test.
8. Use TDD to add the descriptive Zipf-fit function.
9. Update the existing Marimo notebook to overlay the fit on the log-log plot.

### Ruff and pre-commit

Use branch `add-pre-commit`. Add Ruff as a development dependency and configure
both its linter and formatter. Enable NumPy-style pydocstyle rules by extending,
rather than accidentally replacing, the desired lint rule selection:

```toml
[tool.ruff.lint]
extend-select = ["D"]

[tool.ruff.lint.pydocstyle]
convention = "numpy"
```

Configure the pre-commit hooks to run the Ruff lint hook with safe fixes before
the Ruff formatter hook. Pin tool versions through project configuration and
the lockfile. Teach the prepared configuration, not pre-commit YAML authoring.

Ask the learner to add a small public function with intentionally poor but valid
formatting and an inadequate docstring. Stage it and attempt a commit. The hook
must modify format-fixable code or report a lint failure. The learner then:

1. inspects the working-tree diff in VS Code;
2. distinguishes automatic layout changes from human-authored explanation;
3. writes a useful NumPy-style docstring;
4. stages the corrected file; and
5. commits again.

State explicitly that formatting improves readability and consistency without
changing intended behavior. Ruff can enforce the presence and structure of a
docstring, but a human remains responsible for its meaning. Public functions
receive docstrings describing their interface; comments explain non-obvious
reasoning rather than restating code.

### TDD and unit tests

Use branch `add-tests`. Add pytest as a development dependency. Write this
first test together, adjusting only the exact import to the validated package
interface:

```python
def test_extract_words_normalizes_text():
    assert extract_words("Hello, HELLO! World?") == [
        "hello",
        "hello",
        "world",
    ]
```

The initial implementation must fail for a deliberate, understandable reason.
Commit the failing test on the feature branch, make the smallest code change
that handles case and punctuation, run it again, and commit the passing change.
Do not merge a failing branch.

Participants then copy and adapt one test for another small normalization case.
Keep the core scope to these tests plus one prepared integration test. Place
parametrization, fixtures, coverage, mocking, and extended testing taxonomy in
collapsible reference material.

### Integration test

The prepared integration test creates a tiny temporary text file, calls the
real file-processing boundary, and checks that the expected CSV exists and
contains expected counts. Participants fill in or adapt one expected value.
Teach temporary paths by use, not through an extended fixture lecture.

Use pytest assertions naturally. Add a short comparison among a pytest
assertion, input validation, and a scientific plausibility check. Do not add
arbitrary runtime assertions solely to demonstrate the keyword.

### Descriptive Zipf fit

Add the Zipf analysis in lesson 3, not the automation lesson. Use TDD to add a
function in `src/bookstats/zipf.py` that:

1. ranks words from most to least frequent;
2. calculates log rank and log frequency;
3. performs a descriptive linear fit;
4. returns slope, intercept, and coefficient of determination; and
5. provides fitted values needed for plotting.

Name this a **descriptive log-log linear fit**. State that ordinary least
squares on log-transformed power-law data can give biased estimates and does
not establish that the book follows Zipf's law. Rigorous power-law inference is
outside the course.

Extend `notebooks/visualize.py` rather than creating another notebook. Allow a
reader to select a book and view an interactive Altair rank-frequency plot with
the fitted line. Keep Marimo responsible for inspection and presentation; the
fit implementation remains in `bookstats` and is tested independently.

### Transfer task

Add one meaningful automated check to the learner's OIST project. It may be a
unit test, integration test, data validation, or scientific plausibility check.
At the final clinic, explain what failure it is intended to detect.

### Completion criterion

Lesson 3 is complete only when Ruff and pytest pass, the learner has observed a
pre-commit intervention, the red-green history is visible on a branch, the
integration test exercises the file-processing boundary, and the Marimo plot
uses the tested Zipf-fit implementation.

## Lesson 4: Automation and publication

### Learning outcome

The learner represents the analysis as a file dependency DAG, encodes it in
Make, predicts incremental rebuilds, reproduces all generated results from a
clean checkout, and publishes the existing Marimo visualization through GitHub
Actions and GitHub Pages.

### Data and result semantics

Use these meanings consistently:

- `data/raw/`: original committed Gutenberg inputs;
- `data/intermediate/`: generated per-book count tables;
- `data/processed/book-counts.csv`: generated combined, analysis-ready data;
- `output/tables/zipf-fits.csv`: generated reader-facing fit summary;
- `output/figures/zipf-law.svg`: generated static figure;
- `output/figures/zipf-law.html`: generated standalone interactive Altair
  figure; and
- `_site/`: generated complete Marimo publication artifact.

A table belongs under `data/` when another analysis consumes it. A table belongs
under `output/` when it communicates a result to a reader. File format does not
decide the category.

### DAG

Teach the DAG in three views:

1. conceptual: raw books → per-book counts → combined counts → Zipf fits →
   tables/figures → Marimo website;
2. file-level: show exact paths and one concrete book; and
3. incremental rebuild: highlight only the path invalidated by a changed input.

Use Mermaid or adapt existing GECS DAG assets. Then reveal Make rules one at a
time. Every rule must visibly encode an arrow in the file DAG. Treat source code
as a prerequisite: changing count code invalidates count-derived targets;
changing fit code invalidates fit-derived targets; changing visualization alone
does not rebuild processed data.

Demonstrate these predictions before running Make:

- changing one raw book rebuilds its intermediate count and all downstream
  aggregate/results targets, but not other per-book counts;
- changing word-counting code rebuilds all per-book counts and downstream
  targets;
- changing Zipf-fit code rebuilds fits and downstream figures/site, not counts;
  and
- changing visualization code rebuilds figures/site only.

### Make interface

Define these public targets explicitly; they are project conventions, not Make
built-ins:

```bash
make all
make check
make clean
make help
```

- `all` is the first/default target. Initially it builds processed data, fit
  tables, and static/interactive figure outputs. Add the website as its final
  dependency only at the end of the lesson.
- `check` runs Ruff and pytest.
- `clean` removes `data/intermediate/`, `data/processed/`, `output/`, and
  `_site/` without touching committed raw data or source.
- `help` lists public targets and their purpose.

Use pattern rules for per-book processing only after a concrete single-book
rule is understood. Include the recipe-tab requirement and show how VS Code
reveals or corrects indentation. The Makefile calls project commands through
`uv run`.

The decisive reproduction demonstration is:

```bash
make clean
make check
make all
```

It must recreate every generated artifact from committed inputs.

### GitHub Actions and Pages

Add automation only after the local Make DAG works. The workflow must:

1. check out the repository;
2. install uv and obtain the pinned Python 3.12 interpreter;
3. synchronize the locked environment;
4. run `make check`;
5. run `make all`;
6. export `notebooks/visualize.py` as a read-only interactive Marimo app;
7. ensure every required processed asset is included in or copied into the
   deployment artifact;
8. upload `_site/` as the GitHub Pages artifact; and
9. deploy it with the minimum required Pages permissions.

Before writing the final learner instructions, prototype the Marimo WebAssembly
export with the local `bookstats` package, Polars, Altair, and generated data.
Confirm that the deployed app can load its data on GitHub Pages. If a dependency
is not WebAssembly-compatible, preserve the same notebook and fall back to a
static HTML export rather than changing the course architecture during class.

Generated `_site/` files remain ignored and uncommitted. The workflow publishes
an artifact; it does not use a hand-maintained output branch.

### Transfer task

Identify one OIST-project workflow with multiple dependent steps and encode or
sketch its DAG. Automate one safe path if practical. There is no fifth clinic,
so conclude lesson 4 with a short voluntary share-out or written reflection.

### Completion criterion

Lesson 4 is complete only when `make clean && make check && make all` succeeds
locally, incremental rebuild behavior matches the DAG, the same checks/build
pass in GitHub Actions, and the published Pages URL displays the final Marimo
analysis or the validated static fallback.

## Preparation and reference information architecture

Keep preparation outside the four-session course spine:

- **Getting ready**: installations, GitHub account and MFA, Git
  authentication, VS Code, uv, and a disposable create/clone check;
- **VS Code reference**: interface, Source Control, interpreter selection,
  Explorer, search, settings, and shortcuts; and
- **Terminal reference**: optional Bash/Zsh navigation, files, command anatomy,
  activation, and terminal equivalents.

The collaboration lesson assumes setup is complete. Preserve the optional
pre-course setup session for troubleshooting.

Do not create a separate Sidekicks page. Keep comparisons and optional tools in
the narrative where they become relevant: Polars versus pandas with dependency
management, Marimo versus Jupyter with visualization, `tree` with structure,
Ruff and pre-commit with quality, and containers/HPC after environments.

## Website restructure

Replace the current five-topic schedule with the four-session course spine.
Reconfirm dates, times, locations, registration links, and instructor details
before changing time-sensitive landing-page copy.

The sidebar should distinguish preparation, course sessions, and reference
without presenting Terminal or VS Code as numbered teaching sessions. A target
shape is:

```text
Home
Getting ready
Sessions
  1. Git and GitHub collaboration
  2. Reproducible project
  3. Code quality and testing
  4. Automation and publication
Reference
  VS Code
  Terminal
```

Use the existing Quarto site and native site tooling. Rewrite lesson files in
place when their topic remains recognizable. Preserve old URLs where practical.
If a filename changes, add a redirect or another verified compatibility path.

Remove obsolete parallel exercises, particularly the guacamole/recipe website
workflow, once the Gutenberg course project covers the same concept. Preserve
useful diagrams and explanations selectively. Move advanced but still valuable
material into collapsible reference sections. Do not preserve content solely
because it already exists.

Every main lesson must include:

- explicit learning outcomes;
- a starting-state tree;
- the supported VS Code procedure;
- adjacent terminal equivalents;
- expected intermediate observations or failures;
- focused commit and branch names;
- an ending-state tree;
- a transfer task; and
- a completion checklist.

## Recovery checkpoints

Maintain a separate canonical `gecs-bookstats` support repository for
instructor preparation, downloads, and participant recovery. It must not replace
the learner's empty start.

Name checkpoints by achieved state rather than repeatedly using “complete.” A
target checkpoint set is:

```text
git-collaboration
venv-declared
word-counting
data-separated
bookstats-package
tests-passing
zipf-visualized
make-automated
site-published
```

Implement checkpoints as tags or another immutable, easily downloadable form.
The GECS lessons link to the relevant checkpoint only in recovery callouts.
Provide the prepared `count_words.py`, tiny integration-test fixture, workflow
fragments, and final reference implementation from the same canonical source
so examples cannot drift independently.

## Repository case studies

Use case studies as five-to-ten-minute repository microscopes that answer one
question, never as broad tours.

- pandas is the primary example. In lesson 1 or 2, inspect its README or project
  structure only if time permits. In lesson 3, show how much of the repository
  is dedicated to tests and how those tests are organized.
- The Marimo repository may illustrate a strong visual README when discussing
  communication, but its scale must not become the standard expected of a
  student repository.
- Additional projects such as NumPy, Polars, or Matplotlib remain optional and
  should be used only when they demonstrate a pattern pandas does not show
  clearly.

Always pair a large mature repository with the course's small “good enough”
implementation so learners do not infer that good practice requires industrial
scale.

## Explicit exclusions

This restructuring does not turn GECS into:

- a Python syntax course;
- a statistics course on rigorous power-law inference;
- comprehensive Git or GitHub training;
- comprehensive pytest, packaging, Make, or GitHub Actions training;
- native Windows or PowerShell training;
- Docker or Apptainer/Singularity training;
- an HPC operations course; or
- a requirement to publish OIST research repositories or data.

HPC remains a future extension. The Bash/Zsh terminal equivalents, explicit
environments, Make DAG, and container note should make that future extension
coherent without consuming this edition.

## Implementation order

Implement the restructure in this order:

1. build and validate the complete `gecs-bookstats` reference project;
2. verify macOS and Linux/WSL environment reconstruction;
3. verify every failure, restoration, Ruff, pre-commit, pytest, Make, Marimo,
   GitHub Actions, and Pages interaction used in instruction;
4. create immutable recovery checkpoints and downloadable exercise assets;
5. rewrite lesson 1 and validate its two-hour collaboration path;
6. rewrite lesson 2 against the validated repository transitions;
7. rewrite lesson 3 against the exact test and Ruff output;
8. rewrite lesson 4 against the exact Make DAG and deployment workflow;
9. reorganize preparation and reference pages;
10. update the sidebar and landing-page schedule after reconfirming event data;
11. render the entire Quarto site and inspect navigation, code blocks,
    callouts, diagrams, links, and redirects; and
12. run the course end-to-end from an empty repository on a clean machine or
    clean user environment.

Do not implement lessons against hypothetical commands. Capture exact output
from the reference project and keep source examples, downloadable files,
recovery checkpoints, and lesson snippets synchronized.

## Acceptance criteria

The restructuring is finished only when all of the following are true:

- the rendered site exposes four sessions in the decided order;
- preparation and reference pages are visibly separate from sessions;
- no standalone Sidekicks section exists;
- every session begins and ends in a documented repository state;
- every core VS Code action has a nearby Bash/Zsh equivalent where one exists;
- every branch, commit, issue, pull request, and merge instruction uses the
  agreed lifecycle and naming conventions;
- a fresh clone can reconstruct Python 3.12 and all locked dependencies;
- `bookstats` is importable from src layout;
- Ruff, pre-commit, unit tests, integration tests, and NumPy-style docstrings
  behave exactly as shown;
- the descriptive Zipf fit is tested and described without overstating its
  statistical meaning;
- generated artifacts follow the raw/intermediate/processed/output/site
  boundaries and remain uncommitted;
- `make all`, `make check`, `make clean`, and `make help` work as documented;
- the DAG predicts observed incremental rebuilds;
- GitHub Actions checks and builds from committed source;
- GitHub Pages serves the interactive Marimo application or the validated
  static fallback;
- recovery checkpoints reproduce every major learner state;
- the complete Quarto build succeeds; and
- an end-to-end rehearsal fits each core lesson into two hours, including the
  15-minute clinics in sessions 2–4.
