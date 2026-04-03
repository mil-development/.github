# Conventions

This document defines conventions for repository naming, branching, commit messages, pull request merge strategy, dependency locking, tests and lint, and release workflows used across the organization.

## Table of contents

* [Scope](#scope)
* [Repository naming](#repository-naming)
  * [Repository types](#repository-types)
* [Shared types](#shared-types)
  * [Issue types](#issue-types)
* [Format rules](#format-rules)
  * [Branch name](#branch-name)
  * [Pull request title](#pull-request-title)
  * [Commit message subject](#commit-message-subject)
  * [Commit message body and footer](#commit-message-body-and-footer)
* [Ticket referencing](#ticket-referencing)
* [Branching strategy](#branching-strategy)
  * [Main branch](#main-branch)
  * [Develop branch](#develop-branch)
  * [Feature branches](#feature-branches)
* [Pull request merge strategy](#pull-request-merge-strategy)
* [Release and deployment](#release-and-deployment)
  * [Semantic versioning](#semantic-versioning)
* [Code review and ownership](#code-review-and-ownership)
* [Dependency versions](#dependency-versions)
* [Tests and linting](#tests-and-linting)
* [Check triggers](#check-triggers)
* [Enforcement](#enforcement)

## Scope

These conventions apply to repositories maintained within the organization and define rules for:

* repository names
* branch names
* pull request titles
* commit message subjects (the first line) in a pull request
* commit message body and footer (when used) per [Commit message body and footer](#commit-message-body-and-footer)
* pull request merge strategy and the relationship between pull request titles and merged commit messages on the target branch
* dependency version locking and lock files
* tests and lint—recommended tooling in [Tests and linting](#tests-and-linting)
* release and deployment workflows and semantic version tags
* code review expectations and ownership (CODEOWNERS) per [Code review and ownership](#code-review-and-ownership)

---

## Repository Naming

Repositories must follow a consistent naming convention that reflects the system architecture and repository type.

Format:

`<type>-<domain>-<module>`

The `<module>` segment is optional.

Examples:

* `svc-auth-api`
* `svc-files-converter`
* `web-geo-ui`
* `mob-geo-app`
* `lib-geo-core`
* `ops-infra`
* `dev-ci`

Rules:

* repository names must use lowercase letters and numbers only
* words must be separated using `-`
* repository names must start with a valid repository `type`
* repository names must not contain spaces or underscores

### Repository Types

Use one of the following repository type prefixes:

* `web`: frontend web application
* `mob`: mobile application
* `ext`: browser extension
* `app`: desktop application
* `svc`: backend service
* `lib`: shared library
* `ops`: infrastructure or DevOps configuration
* `dev`: developer tooling or engineering productivity tools

Examples:

Valid:

* `web-geo-ui`
* `mob-geo-app`
* `ext-geo-tools`
* `app-geo-desktop`
* `svc-auth-api`
* `svc-files-converter`
* `lib-geo-core`
* `ops-infra`
* `dev-ci`

Invalid:

* `backend-service-auth`
* `frontend-client`
* `geo-service`
* `milgeo-backend`
* `web_geo_ui`

---

## Shared Types

Use one of the following `type` values in all supported formats:

* `feat`: new functionality or capability.
* `fix`: bug fix that changes behavior from incorrect to correct.
* `chore`: maintenance work not classified as feature, fix, or docs.
* `docs`: documentation-only changes.
* `style`: formatting or stylistic-only changes with no behavior impact.
* `refactor`: code restructuring with no behavior change.
* `perf`: performance optimization with no behavior change.
* `test`: adding or updating tests.
* `build`: build system, dependencies, packaging, or artifact tooling.
* `ci`: CI/CD workflow or automation configuration changes.
* `revert`: revert of a previous commit or change.

### Issue Types

If issues are categorized (for example via labels or issue templates), use the following values as issue categories.

Each issue type maps to a corresponding **Conventional Commit type**.

* `bug`: incorrect behavior that needs to be fixed (maps to `fix`)
* `feature`: new functionality or capability (maps to `feat`)
* `docs`: documentation updates or creation with no code behavior changes (maps to `docs`)
* `refactor`: internal code restructuring that does not change external behavior (maps to `refactor`)
* `perf`: performance optimization that improves speed or efficiency without changing functionality (maps to `perf`)
* `test`: adding, updating, or fixing automated tests (maps to `test`)
* `infra`: infrastructure, deployment, CI/CD pipelines, environments, or DevOps configuration (maps to `ci` or `build` depending on context)
* `chore`: maintenance work with no user-facing change (maps to `chore`)
* `research`: investigation, experimentation, or analysis tasks before implementation (maps to `chore`)
* `security`: security vulnerabilities, hardening, or security-related fixes (maps to `fix`)

---

## Format Rules

### Branch Name

Format:

`type/short-kebab-description`

If a ticket exists, append it to the end of the branch name:

`type/short-kebab-description-<TICKET-ID>`

Rules:

* `type` must be one of the shared types (e.g. `feat`, `fix`, `refactor`, etc.)
* description segment: lowercase letters and numbers only
* description segment: words separated with `-`
* optional ticket suffix: `-<TICKET-ID>` appended at the very end
* ticket id format: `<PROJECTKEY>-<number>` (e.g. `MIL-46`), where `PROJECTKEY` is uppercase letters
* concise and descriptive description

Examples:

Valid:

* `feat/add-login-flow`
* `fix/handle-null-user-id`

Valid (with ticket):

* `feat/focus-on-object-MIL-46`
* `fix/null-user-MIL-51`
* `refactor/map-renderer-MIL-77`

Valid (`revert` uses the shared `revert` type):

* `revert/broken-login-MIL-90`

Invalid:

* `feature/add-login-flow`
* `Feat/Add-Login-Flow`

---

### Pull Request Title

Format (Conventional Commits):

`type: short description`

Notes:

* `!` is allowed for breaking changes.

Examples:

Valid:

* `feat: add refresh token rotation`
* `docs: clarify onboarding steps`
* `refactor!: remove legacy endpoint`

Invalid:

* `Add refresh token rotation`
* `feature: add refresh token rotation`

---

### Commit Message Subject

Format (Conventional Commits):

`type: short description`

Examples:

Valid:

* `fix: handle null user id`
* `test: add token expiry cases`

Invalid:

* `fixed api bug`

---

### Commit message body and footer

Beyond the subject line, commit messages may include a **body** and **footer** following [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/).

**Body**

* Leave one blank line after the subject, then add paragraphs that explain *why* the change was made or provide context that does not fit in the subject.

**Footer**

* Separate the footer from the body with a blank line when both are present.
* Use footer lines for metadata, including:
  * `BREAKING CHANGE: <description>` when the change breaks compatibility for consumers of a public API or contract and the subject does not use `!` (see below).
  * References to issues in the form agreed for the repository (see [Ticket referencing](#ticket-referencing)).

**Breaking changes**

* In the subject line, a `!` immediately after the type denotes a breaking change (for example `feat!: ...`, `fix!: ...`).
* If the subject does not use `!`, document the breaking change in the footer as `BREAKING CHANGE:`.

**Scope**

* This document does not require or standardize a parenthesized scope in commit messages. Teams may use scopes locally if their tooling supports them; they are not part of these organization-wide rules.

---

## Ticket Referencing

Reference issue or ticket IDs in pull request descriptions and optionally in titles or commit subjects.

Recommended approach:

* use `closes #<number>` in the pull request description to auto-close GitHub issues
* use `refs #<number>` when linking related work without auto-closing

Examples (pull request description):

* `closes #123`
* `refs #456`

Examples (title or commit subject):

* `feat: add refresh token rotation (refs #123)`
* `fix: handle timeout when upstream is slow (closes #456)`

Notes:

* use explicit words such as `refs` or `closes` when mentioning ticket IDs
* if using external trackers (for example Jira), include the key in the summary text unless regex rules are updated
* a ticket suffix on the branch name (`-<PROJECTKEY>-<number>`) is sufficient to associate the branch with an external ticket; integrations that rely on the pull request description may still require the key there
* when both a GitHub issue and an external tracker (for example Jira) apply to the same change, include both references explicitly in the pull request description so linking does not depend on a single integration

---

## Branching Strategy

Repositories use two primary branches:

* `main`
* `develop`

All development work must be done in short-lived feature branches created from `develop`.

### Main Branch

`main` contains stable, production-ready code.

Rules:

* only verified and tested code may be merged
* direct pushes are not allowed
* merge allowed only by repository owners
* releases from this branch deploy to production

Typical flow:

```
develop → pull request → main → release → production
```

### Develop Branch

`develop` is the primary integration branch.

Rules:

* all feature branches must be created from `develop`
* pull requests from feature branches must target `develop`
* maintained by repository owners
* releases from this branch deploy to staging or testing environments
* releases are triggered manually

Typical flow:

```
feature branch → pull request → develop → manual release → staging
```

### Feature Branches

Feature branches are short-lived branches created from `develop`.

Typical flow:

```
feat/* or fix/* → pull request → develop
```

Feature branches:

* must follow the branch naming convention
* must not deploy directly
* should be deleted after merge

---

## Pull request merge strategy

Repositories in this organization use **squash merge** as the default and expected way to merge pull requests into `develop` and `main`.

Rules:

* With **squash merge**, the merge produces a **single** commit on the target branch. The **first line** (subject) of that commit is taken from the **pull request title** by default in GitHub. Therefore the pull request title must follow [Pull request title](#pull-request-title) and match what you want in permanent history.
* Optional additional lines for the squash commit (body or footer) may be edited at merge time; when only the subject matters for checks, a correct pull request title is still required.
* **Merge commit** and **rebase and merge** are not defined as organization-wide standards. If a repository uses a different merge method, repository owners must document the exception and ensure resulting history still satisfies these conventions where applicable.

---

## Release and Deployment

Deployment is performed through CI workflows.

In this organization, deployment is equivalent to creating a release.

The deployment environment is determined by the source branch used for the release.

Releases from `develop` deploy to staging or testing environments and must be triggered manually.

Releases from `main` deploy to production and may be triggered automatically or manually.

Feature branches must not trigger deployment workflows.

Typical release process:

```
feature branch
      ↓
pull request → develop
      ↓
manual release → staging
      ↓
pull request → main
      ↓
release → production
```

Production releases may optionally create semantic version tags.

Example:

```
v1.2.0
v1.2.1
v2.0.0
```

### Semantic versioning

Production release tags should follow [Semantic Versioning 2.0.0](https://semver.org/) using the form `vMAJOR.MINOR.PATCH` (for example `v1.2.0`).

* **MAJOR** — incompatible changes to a public API or other contracts that require consumers to change how they integrate.
* **MINOR** — new backward-compatible functionality.
* **PATCH** — backward-compatible bug fixes.

Pre-release versions (for example `v1.3.0-beta.1`) are optional and may be used when the repository’s release process defines them; they must remain consistent with the same semver rules for incrementing identifiers.

Release workflows must enforce the following rules:

* production releases may only be triggered from `main`
* staging releases may only be triggered from `develop`
* feature branches must not trigger deployment workflows
* any project build run in continuous integration must **fail** when tests or lint fail; deployments and releases must not proceed from a failing test or lint run

---

## Code review and ownership

* Changes to protected branches (`main`, `develop`) must land **only** through pull requests; direct pushes remain disallowed as described in [Branching strategy](#branching-strategy).
* The minimum number of approving reviews required before merge is set by branch protection rules for the repository or organization.
* Reviewers should verify that changes follow these conventions (branch name, pull request title, tests, and lint).
* Repositories with clear ownership boundaries should use a **`CODEOWNERS`** file so that changes under critical paths request review from the right **team** (`@org/team-name`). Name a **primary contact (DRI)** for escalations—either in a comment in `CODEOWNERS`, on the same owner line if their review is required on every change, or in the repository `README`. Use `.github/CODEOWNERS.template` in the organization `.github` repository as a starting point.
* Who may click **merge** after required approvals is governed by GitHub organization and repository settings (including repository owners); exceptions are decided by repository owners.

---

## Dependency versions

Dependency versions must be pinned so that installs are reproducible across local development, continuous integration, and deployment environments.

### Node.js (npm / Yarn)

Each repository must use one JavaScript package manager and commit its lock file.

Use one of the following:

* `package-lock.json` (npm)
* `yarn.lock` (Yarn)

Rules:

* the lock file for the chosen package manager must be committed to version control
* do not omit the lock file or commit conflicting lock files for different tools

### Python (Poetry)

Python projects must use [Poetry](https://python-poetry.org/) for dependency management.

Rules:

* `poetry.lock` must be committed to version control
* installs must resolve dependencies using the committed lock file

### Lock files in version control

Rules:

* lock files must always be committed to version control
* CI, staging, and production must use the same pinned dependency graph as local development

---

## Tests and linting

### Commands and CI

Rules:

* tests and lint must each be runnable with one documented command from the repository root
* continuous integration must execute both commands (for example on every pull request)
* document both commands in the repository `README` or in the project manifest

### Node.js (npm)

Expose tests through the `test` script (`npm run test`) and lint through the `lint` script (`npm run lint`) from the repository root.

Example `package.json` fragment:

```json
"scripts": {
  "test": "vitest",
  "lint": "eslint ."
}
```

### Python (Poetry)

Expose tests through a Poetry script named `test` (`poetry run test`) and lint through a Poetry script named `lint` (`poetry run lint`) from the repository root.

Example `pyproject.toml` fragment:

```toml
[tool.poetry.scripts]
test = "pytest:console_main"
lint = "ruff.__main__:main"
```

---

## Check Triggers

Conventions checks run on pull request events:

* `opened`
* `reopened`
* `synchronize`
* `edited` (only when title changes, for title validation)

---

## Enforcement

If repository names, branch names, pull request titles, or commit subjects do not match these rules, the conventions workflow fails and the pull request cannot pass required checks until fixed.

Because the organization uses **squash merge** by default, the **pull request title** is the primary subject line that lands on the target branch; it must satisfy the same format rules as a commit message subject. When checks validate the merged commit message after squash, that validation applies to the title used at merge time.
