DevOps GitHub Exercise — Full Instructions (120 minutes)

Focus: realistic collaboration, branching strategies, PR reviews, conflicts, and release process. No app coding — only Markdown/YAML files.

⸻

1) Learning Objectives

By the end of this session, students should be able to:
	•	Apply GitHub Flow / GitFlow within a small release cycle (feature → develop → main).
	•	Collaborate in teams using roles and structured Pull Requests with reviews and checks.
	•	Resolve merge conflicts introduced by concurrent work.
	•	Prepare release notes and changelog and create a GitHub Release (tag/release).
	•	Automate basic checks (lint/validate) using GitHub Actions.

Prerequisites: students already familiar with clone, commit, push, PR, and merge.

⸻

2) Format & Timing
	•	Duration: 120 minutes
	•	Teams: 2–3 people
	•	Tools: GitHub account, browser (local git CLI optional)
	•	File types: .md and .yaml (+ 2 demo shell scripts)

⸻

3) Team Roles & Responsibilities

Assign roles (combine if you have smaller groups):
	•	Release Team: works on release-notes/1.0/summary.md and changelog.md
	•	Ops Team: edits rollout-plan.yaml and rollback-plan.yaml
	•	Monitoring Team: adds monitoring.md and updates ci-pipelines/deploy.yaml for observability
	•	CI/CD Team: updates ci-pipelines/build.yaml, ci-pipelines/test.yaml + small checks

⸻

4) Repository Structure

Create (or distribute) a starter repository:

devops-release/
├── README.md
├── .github/
│   ├── workflows/
│   │   └── validate.yaml
│   └── PULL_REQUEST_TEMPLATE.md
├── release-notes/
│   └── 1.0/
│       ├── summary.md
│       ├── changelog.md
│       ├── rollout-plan.yaml
│       ├── rollback-plan.yaml
│       └── monitoring.md
├── ci-pipelines/
│   ├── build.yaml
│   ├── deploy.yaml
│   └── test.yaml
└── scripts/
    ├── validate_docs.sh
    └── version_check.sh

Example Starter Files

README.md

# DevOps Release Simulation
Goal: simulate release 1.0 with parallel work, PR reviews, conflicts, and GitHub Release.

## Branching Strategy
- Feature branches: `feature/<team>-r1.0`
- Hotfix branches: `hotfix/<team>-patch1`
- Integration branch: `develop`
- Production branch: `main`

## Workflow
1. Work on feature branch → PR to `develop` → review + checks.
2. Resolve conflicts when needed.
3. Merge `develop` → `main` for release 1.0 + create GitHub Release.

.github/PULL_REQUEST_TEMPLATE.md

## Description
What does this PR change?

## Checklist
- [ ] Updated files in my scope (MD/YAML)
- [ ] All GitHub Actions checks passed
- [ ] Updated `changelog.md` if relevant

## Testing
How was it verified?

release-notes/1.0/summary.md

# Release 1.0 — Summary
- Scope: CI/CD, Ops procedures, monitoring
- Key Changes: (fill in)
- Risks/Assumptions: (fill in)

release-notes/1.0/changelog.md

# Changelog — 1.0
- Added: (new sections/files)
- Changed: (edits)
- Fixed: (corrections)
- Removed: (if any)

release-notes/1.0/rollout-plan.yaml

version: 1.0
stages:
  - name: prechecks
    actions:
      - validate-docs
      - verify-owners
  - name: rollout
    strategy: rolling
    steps:
      - enable-feature-flags
      - apply-ci-pipeline
  - name: verify
    actions:
      - smoke-tests
      - check-monitoring-dashboards

release-notes/1.0/rollback-plan.yaml

version: 1.0
triggers:
  - failed-smoke-tests
  - critical-metrics-alerts
steps:
  - revert-to-prev-tag: v0.9
  - disable-feature-flags
  - notify-stakeholders: [release, ops]

release-notes/1.0/monitoring.md

# Monitoring Playbook
- KPIs: build success rate, lead time, MTTR (mean time to restore)
- Dashboards: (describe)
- Alerts: (thresholds/channels)

ci-pipelines/build.yaml

pipeline: build
steps:
  - name: markdown-compile
    run: echo "Build docs" # placeholder

ci-pipelines/test.yaml

pipeline: test
steps:
  - name: validate-structure
    run: ./scripts/validate_docs.sh

ci-pipelines/deploy.yaml

pipeline: deploy
steps:
  - name: pre-deploy-check
    run: ./scripts/version_check.sh 1.0
  - name: notify-monitoring
    run: echo "Notifying monitoring system..."

scripts/validate_docs.sh

#!/usr/bin/env bash
set -euo pipefail

[ -f "release-notes/1.0/summary.md" ] || { echo "Missing summary.md"; exit 1; }
[ -f "release-notes/1.0/changelog.md" ] || { echo "Missing changelog.md"; exit 1; }

grep -q "\S" release-notes/1.0/summary.md || { echo "summary.md is empty"; exit 1; }
grep -q "\S" release-notes/1.0/changelog.md || { echo "changelog.md is empty"; exit 1; }

echo "Docs validated ✅"

scripts/version_check.sh

#!/usr/bin/env bash
set -euo pipefail
REQ=${1:-"1.0"}
TAG_LINE=$(grep -E "^# Changelog —" release-notes/1.0/changelog.md || true)
if [[ "$TAG_LINE" == "" ]]; then
  echo "Changelog header missing"; exit 1
fi

echo "Version $REQ checks passed ✅"

.github/workflows/validate.yaml

name: Validate Docs
on:
  pull_request:
    branches: [ develop ]
  push:
    branches: [ develop ]

jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Make scripts executable
        run: chmod +x scripts/*.sh || true
      - name: Run validators
        run: |
          ./scripts/validate_docs.sh
          ./scripts/version_check.sh 1.0


⸻

5) GitHub Setup
	•	Create branch develop from main.
	•	Branch protection:
	•	main: require PR, 1+ review, no direct push.
	•	develop: require successful Actions status checks.
	•	Labels: feature, hotfix, review-needed, blocked.
	•	(Optional) Add CODEOWNERS for specific folders.

Example CODEOWNERS

/release-notes/*   @release-team
/ci-pipelines/*    @cicd-team
/scripts/*         @ops-team


⸻

6) Timeline (120 minutes)
	•	00–10 min: intro and overview.
	•	10–20 min: assign roles and create branches.
	•	20–40 min: parallel work on features → PR to develop.
	•	40–60 min: instructor merges some PRs → inject conflicts.
	•	60–80 min: teams resolve conflicts and update PRs.
	•	80–100 min: review + merge into develop.
	•	100–110 min: merge develop → main, create Release v1.0.
	•	110–120 min: retrospective discussion.

⸻

7) Student Instructions

A) Branching
	1.	Create feature branch: feature/<team>-r1.0.
	2.	Edit files according to your role and commit with clear messages.
	•	Format: feat(ops): add rollback plan / docs(release): update summary.
	3.	Push and open a PR to develop.

B) PR Review
	1.	Assign a reviewer from another team.
	2.	Wait for Actions checks.
	3.	Address review comments and update PR.

C) Conflicts
	1.	Update develop or use GitHub’s conflict editor.
	2.	Merge content properly — don’t delete others’ work.
	3.	Commit resolution and update PR.

D) Release
	1.	Once all PRs are merged and checks pass, instructor merges develop → main.
	2.	Create Release v1.0 using changelog.md content.

⸻

8) Instructor Notes: Injecting Conflicts
	•	Before the session, modify lines in summary.md and deploy.yaml.
	•	While teams open PRs, manually change the same lines in develop.
	•	This will trigger meaningful (non-destructive) merge conflicts.

⸻

9) Rules & Quality
	•	Commit messages: conventional commits format.
	•	PR gate: 1 review + green CI checks.
	•	Changelog: each team adds bullets under the correct section.
	•	Consistency: unified Markdown headings and formatting.

⸻

10) Gamification / Scoring
	•	✅ PR accepted without review notes: 5 pts
	•	💬 Constructive review comments: 5 pts
	•	⚡ Conflict resolved without content loss: 10 pts
	•	🏁 Successful Release v1.0 with changelog: 10 pts
	•	♻️ Bonus: added automated check in Actions: 5 pts

⸻

11) Adaptations
	•	Small groups: merge roles (Release+Ops, Monitoring+CI/CD).
	•	Large groups: two parallel repos or extended scope (runbooks/, incident-response.md).

⸻

12) Troubleshooting
	•	PR out of date: git fetch --all && git rebase origin/develop or “Update branch” in UI.
	•	Markdown conflict: merge both sections — keep context.
	•	Actions fail due to permissions: chmod +x scripts/*.sh and commit.
	•	Validation fails: fill in missing files from §4.

⸻

13) Templates (optional)

Issue Template (.github/ISSUE_TEMPLATE/task.md)

---
name: Task
about: A small task
labels: feature
---

## Description
What needs to be done?

## Acceptance Criteria
- [ ] Changelog updated if relevant
- [ ] Actions checks pass

CODEOWNERS

/release-notes/1.0/*   @release-team
/ci-pipelines/*        @cicd-team
/scripts/*             @ops-team

Example Conventional Commits

feat(release): add initial summary for 1.0
fix(ci): make scripts executable in workflow
docs(monitoring): add KPIs and alerting overview


⸻

14) Retrospective Discussion
	•	Where did conflicts occur and how could they have been prevented?
	•	Did the PR template improve quality?
	•	What could be automated next (lint, link check, spellcheck)?
	•	Should we adjust the branching model (smaller PRs, trunk-based)?

⸻

Final Goal

Have a clean v1.0 release on main, consistent develop, a filled-out changelog.md, and working GitHub Actions checks. This is real DevOps practice — collaboration, process, and automation without needing to code. The next step is easily attaching real CI/CD pipelines.
