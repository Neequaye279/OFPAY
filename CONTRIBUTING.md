# Contributing to OFPAY

Thank you for helping build OFPAY — a cryptographically secured offline peer-to-peer payment prototype using a permissioned blockchain. Please read [`docs/PRD.md`](docs/PRD.md) before proposing changes. The PRD defines the scope, architecture and transaction lifecycle that all contributions must respect.

## Current phase: governance only

We are in the setup phase. **No application code is being merged yet.**

- The Rust workstream (cryptographic payment engine and Axum backend) is **on hold** until the group finishes its discussion. The project owner will take that workstream afterwards.
- DevOps will add the initial infrastructure code (containers, CI, local network configuration) in a separate, later change.
- For now, only governance and documentation changes should be proposed. Do not open PRs adding Flutter, Rust, Solidity, database or CI code until the group agrees on the API/database contracts and workstream ownership.

## Branch model (summary)

| Branch | Purpose |
| --- | --- |
| `main` | **Sacred.** Release-ready state only. Receives promotion PRs from `master` — nothing else. |
| `master` | Integration and testing branch. Feature PRs land here; major testing of new features happens here. |
| `feature/<workstream>/<short-name>` | Short-lived work branches, created from `master`. |

The full flow is:

```
feature branch ──PR + review──> master ──test on master──> PR + review──> main
```

After each promotion, `main` and `master` must have **identical content** (GitHub's merge commit on `main` means the tip SHAs differ). See [`docs/BRANCHING.md`](docs/BRANCHING.md) for the exact procedure and verification commands.

**Never push directly to `main` or `master`.** Both branches are protected for everyone, including administrators.

## Pull request rules

1. Every change to `main` or `master` must go through a pull request. Direct pushes are blocked by branch protection.
2. Every PR needs **at least one approving review** from a listed code owner (see `.github/CODEOWNERS`) before it can merge. GitHub does not allow authors to approve their own PRs, so someone else must review it.
3. All review conversations must be resolved before merge.
4. Feature work targets `master`. A PR may target `main` **only** to promote an already-tested `master` commit, or for a documented emergency fix approved by the admin.
5. Keep PRs focused and describe what changed, how you tested it, and which PRD requirement it supports.
6. Do not force-push to `main` or `master`. Force pushes are blocked.
7. Once CI is introduced by DevOps, required status checks will be added to the protections. Do not try to merge with failing checks.

## Copilot and AI agents are not contributors

**This policy applies to every contributor and is stated here so nobody is confused about attribution.**

- GitHub Copilot (the cloud/coding agent, bot accounts such as `copilot-swe-agent[bot]`, and any other automated agent) is **not a contributor** to OFPAY and must not be credited in the repository's contributor list.
- Do not assign issues to Copilot, mention `@copilot` to request changes, or use the Copilot cloud agent to open PRs or author commits in this repository.
- Do not add `Co-authored-by: Copilot ...` (or any bot/AI co-author) trailers to commit messages.
- If you use local AI assistance while writing code yourself, you remain the sole author: commit under your own name, disclose AI assistance in the PR description, and take full responsibility for the result. Human review is still mandatory.
- Repository owners may decline or revert any contribution that violates this policy.

## Branch deletion and force pushes

- `main` and `master` are technically protected against deletion and force pushes. They cannot be deleted by anyone without first changing the protection settings.
- **All other branches may be deleted only with explicit admin (`@Tazorx7`) approval.** Record that approval in the PR or issue before deleting a branch, locally or on GitHub.
- Do not enable automatic branch deletion on merge. The repository setting `delete_branch_on_merge` must stay disabled until further notice.
- If a branch must be restored, ask the admin rather than recreating it under a different name.

## Workstreams

Workstreams are areas of responsibility, not fixed assignments. Claim one only after group discussion:

- **A — Mobile application** (Flutter, Android, QR, SQLite).
- **B — Backend and database** (Rust/Axum, PostgreSQL, migrations).
- **C — Cryptographic payment engine** (Rust, Ed25519, hashing, canonical encoding) — *on hold, owner to take after discussion*.
- **D — Blockchain infrastructure** (Besu/QBFT, Solidity contract) — DevOps will handle initial infrastructure.
- **E — Integration, testing and documentation**.

Agree on API contracts and database schemas before implementing in parallel (PRD §11–12).

## Before you open a PR

- [ ] Based on `master` (unless this is a promotion PR to `main`).
- [ ] No direct pushes to protected branches.
- [ ] Changes match the PRD scope; no real money, bank or mobile-money integrations.
- [ ] Tests/demonstration steps described (no code yet during this phase).
- [ ] No Copilot/bot authorship or co-authorship.
- [ ] Branch deletion not requested without admin approval.
- [ ] Documentation updated where behaviour or process changed.
