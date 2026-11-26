# Branching, protection and promotion policy

This document is the authoritative description of how OFPAY branches are governed. It complements [`CONTRIBUTING.md`](../CONTRIBUTING.md). It reflects the settings currently applied to the GitHub repository.

## 1. Branch roles

| Branch | Role | Who may change it |
| --- | --- | --- |
| `main` | Sacred, release-ready state. | Only a promotion PR from `master` (or an admin-approved emergency PR). |
| `master` | Integration and major feature testing. | Feature PRs from short-lived branches, with review. |
| `feature/*`, `docs/*`, `infra/*`, etc. | Short-lived work branches from `master`. | Anyone with write access; no protection. |

After every promotion, `main` and `master` must have **identical content (the same Git tree)**. GitHub creates a merge commit on `main` when a PR merges, so the two tip SHAs will differ even though the files are identical. "Kept identical" therefore means identical content at release/promotion points, with `main` never containing unreleased work. They are allowed to differ while new work is being tested on `master`.

```
master ─────●───●───●───(tested)───●──────────────
             \          promotion  │
feature/*     ●───PR────●───────────PR──> main ●───
```

## 2. Protection settings (applied to both `main` and `master`)

| Setting | Value | Effect |
| --- | --- | --- |
| Require a pull request before merging | on | No direct pushes. |
| Required approving reviews | 1 | At least one code-owner approval. |
| Dismiss stale approvals on new pushes | on | New commits require a fresh review. |
| Require approval of the most recent push | on | Prevents approval before the final change. |
| Require review from code owners | on | `.github/CODEOWNERS` lists all write-access collaborators; any one may approve. |
| Require conversation resolution | on | All threads resolved before merge. |
| Block force pushes | on | History cannot be rewritten. |
| Block branch deletion | on | Branches cannot be deleted. |
| Enforce protections for administrators | on | The owner is bound by the same rules. |
| Required status checks | none yet | Will be added when DevOps introduces CI; do not add check names before the workflows exist, otherwise all PRs would be blocked. |

To inspect the live settings:

```bash
gh api repos/Tazorx7/OFPAY/branches/main/protection
gh api repos/Tazorx7/OFPAY/branches/master/protection
```

Do not weaken these settings without an explicit decision recorded in an issue or PR.

## 3. Normal workflow

1. Create a branch from `master`:
   ```bash
   git fetch origin
   git switch -c feature/<workstream>/<short-name> origin/master
   ```
2. Commit, push the feature branch, and open a PR targeting **`master`**.
3. Obtain the required review, resolve conversations, and merge into `master`.
4. Test on `master` (integration, security and double-spending scenarios from PRD §15).
5. When `master` is release-ready, open a promotion PR: **base `main` ← compare `master`**.
6. After review and merge, verify the branches have the same tree (identical content):
   ```bash
   git fetch origin
   printf '%s\n%s\n' "$(git rev-parse origin/main^{tree})" "$(git rev-parse origin/master^{tree})"
   ```
   The two printed tree hashes must match. Tip commit SHAs will not match because GitHub added a merge commit on `main`; that is expected. To check via the API, compare the trees behind each branch's tip commit.
7. Continue new work on `master` only. If exact SHA equality is ever required, the admin must approve an explicit out-of-band fast-forward sync and document it in the promotion issue; this is not part of the normal PR flow.

Use a merge or squash strategy consistently. Because both branches are protected, prefer the PR flow over pushing the same SHA to both refs manually.

## 4. Emergency fixes to `main`

`main` is sacred: emergency direct changes are not allowed. If a release blocker is found:

1. Fix on `master` first (or on a branch from `master`) and test.
2. Open an expedited promotion PR to `main` with the admin (`@Tazorx7`) as reviewer.
3. Record the incident in an issue. Do not disable protection as a shortcut.

## 5. Branch deletion

- `main` and `master` are protected from deletion technically; no one, including the admin, can delete them without first changing protection settings.
- **Every other branch requires explicit admin approval (`@Tazorx7`) before deletion**, whether on GitHub or locally. Record the approval in the related PR/issue.
- `delete_branch_on_merge` is disabled: merged branches are retained until approved for deletion.

## 6. Copilot and automated agents

Copilot and other bots are not contributors and must not author or co-author commits here; see [`CONTRIBUTING.md`](../CONTRIBUTING.md#copilot-and-ai-agents-are-not-contributors). This is a documentation policy — no Copilot account or repository settings are changed as part of this setup.

## 7. Ownership

`@Tazorx7` (Prince) is the repository admin and code owner. Workstream ownership and API/database contracts are decided by group discussion before implementation begins; the Rust workstream is currently on hold.
