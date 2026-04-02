# `.github` repository

Centralized GitHub defaults for the organization.

## Purpose

This repository provides shared community-health files and templates that are reused by repositories in the same organization when they do not define their own versions.

## Current Shared Assets

- **Issue form**: `.github/ISSUE_TEMPLATE/issue.yml`
  - Single issue flow with required problem and expected outcome fields.
- **Issue form config**: `.github/ISSUE_TEMPLATE/config.yml`
  - Disables blank issues to keep submissions structured.
- **Pull request template**: `.github/PULL_REQUEST_TEMPLATE.md`
  - Standard review context: summary, testing, related issue, and optional notes.
- **Conventions reference**: `CONVENTIONS.md`
  - Naming and commit conventions used across branches, pull request titles, and commit subjects.

## Conventions

Use `CONVENTIONS.md` as the source of truth for:

- shared `type` values (`feat`, `fix`, `chore`, `docs`, `style`, `refactor`, `perf`, `test`, `build`, `ci`, `revert`)
- recommended `issue` categories / issue types (see `CONVENTIONS.md`)
- branch naming format
- pull request title format
- conventional commit subject format
- ticket referencing format (`closes #<number>` / `refs #<number>`)
- pull request check triggers and enforcement behavior
- examples of valid and invalid values

## How Inheritance Works

For repositories in the same organization:

- if a repository does not define its own issue template, GitHub uses the one from this repository
- if a repository does not define its own pull request template, GitHub uses the one from this repository
- repository-specific files override these shared defaults

## Extending This Repository

Typical additions include:

- extra issue forms for specialized request types
- shared contribution and support documentation

Keep shared standards short, explicit, and easy to adopt.
