# DevOps GitHub Exercise — Release Simulation (120 minutes)

**Focus:** realistic collaboration, branching strategies, PR reviews, conflict resolution, and a lightweight release process.  
**No application coding** — only Markdown/YAML documents.

---

## 1) Learning Objectives

By the end of this session, you will be able to:
- Apply GitHub Flow / GitFlow in a small release cycle (feature → develop → main).
- Collaborate in roles with structured Pull Requests and reviews.
- Resolve merge conflicts created by concurrent work.
- Prepare release notes and a changelog and create a GitHub Release (tag/release).

**Prerequisites:** know how to clone, branch, commit, push, open PRs, and merge.

---

## 2) Format & Timing

- **Duration:** 120 minutes  
- **Teams:** 2–3 people  
- **Tools:** GitHub account, web browser (local git CLI optional)  
- **File types:** `.md` and `.yaml`

---

## 3) Team Roles & Deliverables (5 teams)

Assign roles (combine if you have smaller groups):

**Release Notes Team** for all teams
   - Files: `release-notes/1.0/summary.md`, `release-notes/1.0/changelog.md`  
   - Task: summarize scope, list changes, and maintain the changelog.

1. **Operations Plan Team**  
   - Files: `ops/rollout-plan.yaml`, `ops/rollback-plan.yaml`  
   - Task: define rollout steps, checks, and a safe rollback plan.

2. **Monitoring & Metrics Team**  
   - Files: `monitoring/playbook.md`, `monitoring/metrics.yaml`  
   - Task: define KPIs/SLIs, thresholds, and monitoring guidelines.

3. **Documentation & Standards Team**  
   - Files: `governance/style-guide.md`, `governance/contribution-standards.md`  
   - Task: enforce headings, tone, structure, and contribution rules.

4. **Incident Response Team**  
   - Files: `runbooks/incident-response.md`, `runbooks/postmortem-template.md`  
   - Task: prepare an incident runbook and a postmortem template.

---

## 4) Repository Structure

Create (or fork) a starter repository with:

devops-release/
├── README.md
├── .github/
│   ├── PULL_REQUEST_TEMPLATE.md
│   └── ISSUE_TEMPLATE/
│       └── task.md
├── release-notes/
│   └── 1.0/
│       ├── summary.md
│       └── changelog.md
├── ops/
│   ├── rollout-plan.yaml
│   └── rollback-plan.yaml
├── monitoring/
│   ├── playbook.md
│   └── metrics.yaml
├── governance/
│   ├── style-guide.md
│   └── contribution-standards.md
└── runbooks/
├── incident-response.md
└── postmortem-template.md

---

## 5) How We Work (Student Instructions)

### A) Branching
1. Create a feature branch: `feature/<team>-r1.0`  
   _Examples:_ `feature/release-notes-r1.0`, `feature/ops-r1.0`
2. Edit files for your role and commit with clear messages:  
   - Format: `docs(release): add 1.0 summary` / `feat(ops): add rollback steps`
3. Push and open a PR to `develop`.

### B) PR Review
1. Assign a reviewer from a different team.  
2. Address comments and update the PR until it’s ready.

### C) Conflict Resolution
1. If conflicts appear, fetch `develop` and merge/rebase or use the GitHub conflict editor.  
2. Merge content properly — don’t delete others’ work.  
3. Commit the resolution and update the PR.

### D) Release
1. After all role PRs are merged into `develop`, the instructor merges `develop` → `main`.  
2. Create **Release v1.0** with tag `v1.0` and include the content from `release-notes/1.0/changelog.md`.

---

## 6) Timeline (120 minutes)

- **00–10 min:** intro and overview  
- **10–20 min:** assign roles and create branches  
- **20–40 min:** parallel work on features → PRs to `develop`  
- **40–60 min:** instructor merges some PRs to provoke conflicts  
- **60–80 min:** teams resolve conflicts and update PRs  
- **80–100 min:** reviews and merge into `develop`  
- **100–110 min:** merge `develop` → `main`, create Release v1.0  
- **110–120 min:** retrospective discussion

---

## 7) Rules & Quality

- **Commit messages:** use conventional commits (e.g., `docs:`, `feat:`, `fix:`).  
- **PR gate:** at least one review from another team.  
- **Changelog discipline:** each team adds bullets under the appropriate section.  
- **Consistency:** follow the style guide for headings, lists, and tone.

---

## 8) Gamification / Scoring (optional)

- ✅ PR accepted without review notes — **5 pts**  
- 💬 Constructive review feedback — **5 pts**  
- ⚡ Conflict resolved without content loss — **10 pts**  
- 🏁 Release v1.0 with complete changelog — **10 pts**  
- 🧭 Style guide improvements applied across files — **5 pts**

---

## 9) Troubleshooting

- **PR out of date:** `git fetch --all && git rebase origin/develop` or use “Update branch” in the PR UI.  
- **Markdown conflict:** merge both sections and keep context from all teams.  
- **YAML conflict:** prefer merging keys/values rather than overwriting entire blocks.  
- **Missing files:** populate templates from the section below.

---

## 10) Instructor Notes — Injecting Conflicts

- Before the session, add/modify lines in `release-notes/1.0/summary.md`, `ops/rollout-plan.yaml`, or `monitoring/metrics.yaml`.  
- While teams open PRs, change the same lines in `develop`.  
- This creates **meaningful, non-destructive** conflicts the teams must resolve.

---

## 11) Starter Templates

### `release-notes/1.0/summary.md`
```markdown
# Release 1.0 — Summary
- Scope: documentation for operations, monitoring, incident response, and standards
- Key Changes: (fill in)
- Risks/Assumptions: (fill in)
```

## 12) Retrospective Prompts
	•	Where did conflicts occur and how could they have been prevented?
	•	Did the PR template improve quality?
	•	What should we standardize next (glossary, diagrams, ownership)?
	•	Should we adjust the branching model (smaller PRs, trunk-based)?

## Final Goal

A clean v1.0 release on main, a consistent develop, a filled-out release-notes/1.0/changelog.md, and well-structured documentation across all five areas: release notes, operations, monitoring, governance, and incident response.