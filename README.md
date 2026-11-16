# DevOps Metrics Dashboard

## 1. Project overview

The DevOps Metrics Dashboard is an internal project intended to support engineering visibility into software delivery practices. This repository provides the project's foundational documentation and contribution methodology.

The current scope is **engineering process documentation**. These guidelines do not define application logic, implement a CI/CD pipeline, or deploy infrastructure.

## 2. Repository structure

The repository should maintain a clear separation between project guidance and implementation artifacts.


### 2.1 Documentation map

| Document                            | Purpose |

| `SCM_Workflow_Diagram.md`           | Explains the branching model, Configuration Item lifecycle, and local/remote review and integration states. |
| `Command_Sequences.txt`             | Provides annotated commands for branch creation, staging, commits, pushing, synchronization, and conflict resolution. |
| `/Debugging_and_Linting_Summary.md` | Defines debugging concepts and a repeatable local linting and verification workflow. |

## 3. Source control model

The repository uses Git for version control and GitHub for hosted collaboration.

### 3.1 Branching conventions

- `main` is the stable, reviewed, release-ready baseline.
- `develop` is the shared integration branch.
- `feature/<issue-id>-<short-description>` is used for new work.
- `fix/<issue-id>-<short-description>` is used for defect corrections.

Example:
`feature/42-add-repository-guidelines`

Create feature and fix branches from the latest `develop`. Submit PRs from those branches into `develop`. Promote a release-ready `develop` branch to `main` through a separately reviewed PR.

Direct commits to `main` are not part of the normal workflow. Avoid routine direct commits to `develop`; use PRs so changes have traceable review and verification.

See [SCM Workflow Diagram](SCM_Workflow_Diagram.md) for the lifecycle and branching diagram, and [Command Sequences](Command_Sequences.txt) for exact commands.

### 3.2 Configuration Item lifecycle

A Configuration Item is any project artifact managed under version control, such as source code, tests, or documentation. Its normal Git lifecycle is:

**Working tree → Staging area (index) → Local commit → Remote branch → Pull request → Reviewed merge**

`git add` selects changes for the next commit. `git commit` records the staged snapshot locally. `git push` publishes commits to the remote branch. A pull request then provides the review and integration process. These are separate states; staging does not commit, committing does not push, and pushing does not merge.

## 4. GitHub issue management

GitHub Issues are the team's record of work, defects, and improvement requests. Create or update an issue before starting substantial work so that the change has a clear purpose and can be traced through review.

### 4.1 Issue content

Each issue should include, where applicable:

- **Title:** concise and action-oriented.
- **Context:** why the work is needed.
- **Description:** what is requested or what is failing.
- **Expected result:** observable outcome or acceptance criteria.
- **Reproduction steps:** for defects, a reliable sequence that demonstrates the problem.
- **Evidence:** relevant logs, screenshots, error messages, or stack-trace excerpts, with secrets and personal data removed.
- **Scope:** known constraints and items explicitly out of scope.

### 4.2 Issue workflow

1. Search existing issues to avoid creating duplicates.
2. Create an issue with a clear title and sufficient context.
3. Add the appropriate labels, such as `bug`, `documentation`, `enhancement`, or `question`, according to repository conventions.
4. Assign an owner when responsibility is known.
5. Reference the issue in the branch name and PR.
6. Update the issue when scope or acceptance criteria change.
7. Close the issue only when its acceptance criteria are met and the associated work is integrated. Use GitHub's supported closing keywords in the PR description when appropriate (for example, `Closes #42`).

Do not put credentials, tokens, private keys, or sensitive personal information in an issue.

## 5. Pull-request standards

A pull request is a proposal to merge a focused set of changes into a target branch. It is both a technical review mechanism and a permanent record of why the change was made.

### 5.1 Before opening a PR

The author must:

1. Confirm the branch is based on the latest appropriate `develop`.
2. Keep the change limited to the issue's scope.
3. Inspect `git status`, `git diff`, and `git diff --staged`.
4. Run the repository's documented linter and relevant tests.
5. Resolve conflicts deliberately and rerun verification.
6. Check that no secrets, generated files, or unrelated local changes are included.
7. Push the intended feature or fix branch.

### 5.2 PR description

Every PR should provide:

- **Summary:** what changed and why.
- **Related issue:** link or closing reference, such as `Closes #42`.
- **Implementation notes:** important design or documentation decisions.
- **Verification:** exact lint/test commands run and their outcomes.
- **Screenshots or logs:** where they materially help reviewers, with sensitive data removed.
- **Risks or follow-up work:** known limitations, compatibility concerns, or deferred items.

### 5.3 Review and merge requirements

Reviewers should evaluate:

- Correctness against the issue and acceptance criteria.
- Scope control and clarity of the change.
- Readability, maintainability, and consistency with repository conventions.
- Appropriate tests and verification evidence.
- Documentation updates where behavior or process has changed.
- Potential security, privacy, and operational risks.

The author should respond to review comments, make requested corrections, and resolve conversations according to team practice. Merge only after required approvals and repository checks have passed and conflicts are resolved.

Do not approve your own PR unless an explicit project policy permits it. Do not merge merely because a PR has been open for a long time.

### 5.4 PR title and commit message conventions

Use a concise title that states the change. A lightweight conventional prefix is encouraged:

- `docs: clarify contribution workflow`
- `feat: add dashboard metric summary`
- `fix: handle empty metric response`
- `test: cover metric aggregation edge case`
- `refactor: simplify metric parsing`

Use the prefixes that match the actual work; do not label documentation-only changes as features.

## 6. Debugging, linting, and local verification

Engineers are expected to investigate defects systematically and run the repository's documented local checks before requesting review.

- Use breakpoints to pause execution and inspect live state.
- Read call stacks to understand the active function-call sequence.
- Read stack traces to locate the error and follow relevant project frames.
- Reproduce defects reliably and record expected versus actual behavior.
- Run the linter configured for the project; do not assume a tool or command that is not documented.
- Run focused tests relevant to the change and any additional tests required by project guidance.
- Inspect the final diff after automated formatting or lint fixes.

See [Debugging and Linting Summary](Debugging_and_Linting_Summary.md) for definitions and the recommended workflow.

## 7. Engineering hygiene and repository safety

- Never commit secrets, credentials, access tokens, private certificates, or personal data.
- Use `.gitignore` for local-only files and generated artifacts that should not be versioned.
- Do not commit editor settings or machine-specific files unless the team has explicitly adopted them.
- Keep changes focused and commits understandable.
- Do not rewrite shared branch history or force-push to `main` or `develop`.
- Do not resolve conflicts by blindly accepting one side; understand the intended combined result.
- Keep documentation accurate when repository conventions change.

## 8. Definition of done

A contribution is complete when:

- [ ] The related issue and acceptance criteria are clear.
- [ ] The change is on the correct feature or fix branch.
- [ ] Only intended files and changes are included.
- [ ] The staged snapshot and commit have been reviewed.
- [ ] Relevant linting and tests have been run.
- [ ] The PR links the issue and documents verification.
- [ ] Required review approvals and checks are complete.
- [ ] Conflicts are resolved and the PR is merged into the intended target branch.
- [ ] The issue is closed when its acceptance criteria are satisfied.

## 9. Change control for this documentation

Propose changes to these guidelines through the same issue-and-PR workflow used for other repository changes. Explain the reason for the change, identify affected documents, and request review from the maintainers responsible for engineering practices. This keeps onboarding instructions consistent and traceable.
