# Software Configuration Management (SCM) Workflow

**Project:** DevOps Metrics Dashboard  
**Audience:** Entry-level software engineers  
**Document type:** Foundational engineering guideline

## 1. Purpose and scope

This document defines how a Configuration Item (CI)—for example, a source file, test, configuration file, or document—moves from an engineer's local working copy through Git's staging area and commit history to the shared upstream repository.

In this document, **Configuration Item** means a version-controlled project artifact. 
The workflow establishes a lightweight branching model:

- `main`: stable, reviewed, release-ready baseline.
- `develop`: shared integration branch for completed feature work.
- `feature/<issue-id>-<short-description>`: short-lived branch for one issue or coherent change.
- `fix/<issue-id>-<short-description>`: short-lived branch for a defect correction.

Changes are developed on a topic branch, integrated into `develop` through a pull request (PR), and promoted from `develop` to `main` through a separately reviewed PR when the project is ready for a stable release.

## 2. Core concepts

        ## A- Working tree / working copy:
                The checked-out files in the engineer's local repository, including edits not yet staged.It is the workspace in which changes are made and inspected.
        ## B-Staging area / index:
                The set of exact file changes selected for the next commit. It allows a commit to contain a deliberate, reviewable unit of work rather than every local edit.
        ## C-Commit:
                A recorded snapshot of staged changes, identified by a hash and accompanied by metadata and a message.It creates a traceable point in project history.
        ## D-Local branch:
                A movable reference to a sequence of commits in the local repository.It isolates work and provides a named line of development.
        ## E- Remote / upstream:
                A shared repository hosted on a service such as GitHub. `origin` is the conventional remote name; `upstream` may refer to a canonical repository in a fork-based workflow.It provides a common source of truth for collaboration. 


### 2.1 The Configuration Item lifecycle

A file does not move physically from one repository to another at each stage. Git tracks changes and records snapshots. The conceptual lifecycle is:

1. **Working copy:** an engineer edits a tracked file or creates a new file.
2. **Review and selection:** the engineer inspects the diff and selects the intended changes.
3. **Staging area:** `git add` places selected changes in the index.
4. **Commit:** `git commit` records the staged snapshot in local history.
5. **Remote synchronization:** `git push` transfers the commit(s) to the corresponding remote branch.
6. **Review and integration:** a PR is reviewed; required checks run; authorized reviewers approve; the change is merged into the target branch.

A change that has only been staged is not committed. A committed change is not necessarily on GitHub until it is pushed. A pushed change is not part of `develop` or `main` until it is merged.

## 3. Branching strategy

### 3.1 Branch responsibilities

| Branch      | Purpose                                                                       | Direct commits? |

| `main`      | Stable baseline; should always represent an accepted, coherent project state. | No, except an explicitly authorized repository-maintenance procedure. |
| `develop`   | Integration branch where reviewed feature work is combined and tested.        | Prefer PR merges; avoid routine direct commits. |
| `feature/*` | New functionality or documentation associated with an issue.                  | Yes, by the branch owner. |
| `fix/*`     | A focused correction associated with an issue.                                | Yes, by the branch owner. |

Branch names should be descriptive and traceable to an issue, for example:
`feature/42-add-repository-guidelines`

### 3.2 Standard feature workflow

```text
                         SHARED REMOTE REPOSITORY
                 +------------------------------------+
                 |  origin/main     origin/develop    |
                 +---------+--------------+-----------+
                           |              ^
                           | branch from  | PR merge after
                           v              | review + checks
                    +---------------------------+
                    | feature/42-repository-docs|
                    +-------------+-------------+
                                  |
                 edit files      | inspect diff
                 in working copy | git status / git diff
                                  v
                         +----------------+
                         |  Working copy  |
                         +-------+--------+
                                 |
                          git add <paths>
                                 v
                         +----------------+
                         | Staging / index|
                         +-------+--------+
                                 |
                          git commit
                                 v
                         +----------------+
                         | Local commits  |
                         +-------+--------+
                                 |
                           git push -u
                                 v
                         +----------------+
                         | Remote feature |
                         +-------+--------+
                                 |
                           Open pull request
                                 v
                 +------------------------------------+
                 | Review, discussion, required checks|
                 +----------------+-------------------+
                                  |
                         approval + merge
                                  v
                         +----------------+
                         |     develop    |
                         +-------+--------+
                                 |
                     integration testing / validation
                                 |
                     release-ready PR: develop -> main
                                 v
                         +----------------+
                         |      main      |
                         +----------------+
```

### 3.3 Step-by-step procedure

The standard beginner workflow is:

    Clone the remote GitHub repository
        |
        v
    Enter the local repository
        |
        v
    Pull the latest upstream changes
        |
        v
    Create and check out a task branch
        |
        v
    Edit files in the working copy
        |
        v
    Stage selected changes in the staging area
        |
        v
    Commit the changes to the local repository
        |
        v
    Push the branch to GitHub

If a pull produces a merge conflict:

    Identify conflicted files with git status
        |
        v
    Review and edit the conflicting sections
        |
        v
    Remove conflict markers and save the files
        |
        v
    Stage the resolved Configuration Item(s)
        |
        v
    Commit the conflict resolution
        |
        v
    Confirm the working copy is clean with git status



## 5. Completion criteria

A change is considered integrated only when it is committed, pushed, reviewed, passes the repository's required checks, and is merged into the intended target branch. A local commit or an open PR alone does not satisfy this definition.
