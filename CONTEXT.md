# Project Glossary

## GECS

The project acronym for **Good Enough Coding in Science**, pronounced “geeks.”
The lowercase word “in” is part of the name but does not contribute a letter to
the acronym.

## Course Design

**Course project**:
A discipline-neutral analysis of books from Project Gutenberg that each
participant develops in their own initially empty repository throughout the
course.
_Avoid_: Demo project, toy project

**Course repository**:
The participant-owned GitHub repository containing their course project. It
grows from a README and raw book files into a published interactive analysis.
_Avoid_: Shared repository, exercise repository

**Participant project**:
A participant's current OIST research project to which they transfer a small
practice after first learning it on the course project. It may be newly started
or short-lived; it does not need to be mature software.
_Avoid_: Homework project, personal project

**Course contribution**:
A change proposed from a short-lived branch in a fork of a collaborator's course
repository. It adds at least one sourced book and documents it in the README.
_Avoid_: Direct edit, shared file

**Collaborator**:
Another participant with whom a learner completes the reciprocal issue-to-merge
workflow during Session 1.
_Avoid_: Partner

**Completed branch**:
A short-lived contribution branch whose pull request has been reviewed and
merged. Its remote and local copies can then be deleted.
_Avoid_: Main branch, permanent branch

**Project clinic**:
A discussion at the beginning of a session in which participants share a
change they tried in their own project, an obstacle they encountered, or the
outcome of their attempt.
_Avoid_: Homework review, project presentation

**Supported path**:
The VS Code workflow demonstrated directly during the course. Terminal
equivalents are documented for participants who prefer or need them.
_Avoid_: GUI track, beginner track

**Supported platforms**:
macOS with Zsh, Linux with Bash, and Windows through WSL with Bash. Platform
support does not determine how participants are paired.
_Avoid_: Native Windows, PowerShell

**bookstats**:
The small reusable Python package that transforms raw books into analysis-ready
statistics for the course project.
_Avoid_: BookStats, GECS package, Gutenberg package

**Visualization notebook**:
The Marimo notebook that presents processed BookStats results as an interactive
analysis. It does not own or duplicate the processing pipeline.
_Avoid_: Analysis pipeline, processing notebook

**Raw data**:
The original Project Gutenberg book files used as source inputs. They are
committed because the course inputs are small and redistributable.

**Intermediate data**:
Reproducible per-book count tables generated from raw data. They are not
committed.

**Processed data**:
The combined, analysis-ready bookstats table generated from all intermediate
count tables and consumed by the visualization notebook. It is not committed.
_Avoid_: Final results, raw data

**Output**:
Generated, reader-facing results of the analysis, including summary tables and
static or interactive figures. Outputs are reproducible and are not committed.
_Avoid_: Processed data, source data

**Descriptive Zipf fit**:
A linear fit of log word frequency against log frequency rank used to summarize
and visualize an approximate Zipf-like relationship. It is not a rigorous test
that a book follows a power law.
_Avoid_: Proof of Zipf's law, power-law test
