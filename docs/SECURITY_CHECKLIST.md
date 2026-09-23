# Security Reference & Setup Checklist

**Project:** Enterprise Personal Investment & Financial Planning Model
**Based on:** BRD §8, PRD §7–§10, §13, §17 · plus additions marked **[NEW]** that close gaps found in review
**Target standard (PROPOSED):** OWASP ASVS 5.0 Level 2 · NIST SSDF (SP 800-218) · NIST SP 800-218A for AI-assisted development
**How to use:** Tick `[x]` only when the item is done **and** there is evidence (a screenshot, a passing CI run, a config file or a test). Write the evidence in the "Proof" note. The **Phase** column shows when each item must be done.

Legend: **P0** = Phase 0 (before any feature code) · **P1+** = the phase named · **REL** = before the first production release (Phase 9)

---

## 1. Accounts, ownership & recovery [NEW] — P0
Why: the owner is non-technical. Losing access to an account or a key is the biggest real-world risk.

- [ ] Password manager set up (e.g. 1Password, Bitwarden). All project credentials live **only** there.
- [ ] MFA on GitHub, cloud provider, auth provider, domain registrar, email. Prefer passkey or authenticator app over SMS.
- [ ] Recovery codes for every account saved in the password manager **and** printed and stored offline.
- [ ] The project email account is the owner's, protected with MFA (it can reset everything else).
- [ ] Accounts register in `HANDOFF.md` §6 filled in (names only, no secrets).
- [ ] Coding agent uses its own limited token (fine-grained GitHub token, repo-scoped, expiring), never the owner's master login.
- [ ] Cloud: root/owner account used only for billing and break-glass. Daily work uses a separate least-privilege identity.
- [ ] Billing alerts and a monthly budget cap set on the cloud account.

## 2. Identity & access (app) — P0 skeleton, P2 complete
- [ ] Managed OIDC/OAuth provider; no custom password storage.
- [ ] MFA required for the owner account.
- [ ] Secure sessions: `HttpOnly`, `Secure`, `SameSite` cookies. PROPOSED: 15 min idle timeout, 8 h absolute. Revocation on logout.
- [ ] Server-side role-based access control, deny by default. Roles: Owner, Researcher, Validator, Developer, Admin.
- [ ] Negative tests: each role is denied actions it shouldn't have (privilege-escalation tests).
- [ ] Every denied privileged action is written to the audit log.
- [ ] Solo mode: the same person holds several roles. Self-approval of high-impact assumptions is flagged in the audit log, never hidden.

## 3. Secrets management — P0
- [ ] `.env` and secret files listed in `.gitignore` before the first commit.
- [ ] Secrets injected from a secret manager (GitHub Actions secrets for CI; cloud secret manager for staging and prod).
- [ ] Secret scanning on every commit and PR (gitleaks locally via pre-commit **and** GitHub secret scanning + push protection).
- [ ] Separate secrets per environment. Production secrets never exist on a laptop or in dev or CI.
- [ ] Rotation schedule documented (PROPOSED: every 90 days, and immediately after any suspected leak).
- [ ] Logs and error messages never print secrets (tested).

## 4. Encryption & keys — P0 design, REL verified
- [ ] TLS 1.2+ everywhere (HTTPS only, HSTS enabled).
- [ ] Database and object storage encrypted at rest (managed keys by default).
- [ ] Backups encrypted.
- [ ] Key ownership and rotation documented in `SECURITY.md`.
- [ ] Money values use decimal/fixed precision (integrity, per PRD §15).

## 5. Application security (OWASP-aligned) — every phase
- [ ] Threat model (`THREAT_MODEL.md`, STRIDE method) written **before** any real personal financial data is stored, and updated on every architecture change.
- [ ] Every API input validated by schema (Pydantic / Zod). Size and rate limits on all endpoints.
- [ ] Parameterized database queries only (ORM). No string-built SQL.
- [ ] Safe file upload for evidence: type allow-list, size limit, malware scan (PROPOSED), stored outside the web root, random names.
- [ ] Security headers: CSP, X-Content-Type-Options, frame-ancestors, Referrer-Policy.
- [ ] CSRF protection where cookies are used.
- [ ] Output encoding; no raw HTML from user or evidence content.
- [ ] Error pages show no stack traces in staging or production.
- [ ] Fetched web content (evidence) treated as untrusted data, never as instructions **[NEW]**.

## 6. Repository & GitHub protection — P0
- [ ] Private repository.
- [ ] Ruleset on `main`: no direct push, PR required, required status checks, conversations resolved, stale approvals dismissed.
- [ ] Force-push and branch deletion blocked on `main` and release tags.
- [ ] Signed commits where feasible.
- [ ] `CODEOWNERS` for security- and model-critical paths (active once a second reviewer exists).
- [ ] Dependabot (or Renovate) enabled. Its PRs must pass the same checks.
- [ ] Least-privilege `GITHUB_TOKEN` (`permissions:` set in every workflow). Third-party actions pinned to commit SHA.
- [ ] Solo bootstrap: PR workflow still used for every change. Missing human review is documented, not faked.

## 7. CI security pipeline (runs on every PR) — P0
Order per PRD §13. A failure blocks the merge.
- [ ] Lockfile verification (`uv lock --check`, `pnpm install --frozen-lockfile`)
- [ ] Lint + format (ruff, eslint/prettier)
- [ ] Type check (mypy/pyright, `tsc --noEmit`)
- [ ] Unit + property tests
- [ ] Integration + migration tests
- [ ] Build
- [ ] SAST (Semgrep and/or GitHub CodeQL) — critical findings block
- [ ] Dependency / SCA scan (osv-scanner, pip-audit, pnpm audit) — known critical vulnerabilities block
- [ ] Secret scan (gitleaks)
- [ ] Container image scan (Trivy) once Docker images are built
- [ ] Selected E2E tests (Playwright)

## 8. Environments — P0 (dev/CI), P1+ (staging), REL (production)
- [ ] Four separate environments: Local/Dev, CI, Staging, Production.
- [ ] Separate database and secrets per environment.
- [ ] Dev and CI use **synthetic data only**. Staging uses sanitized data. Real personal data exists only in production.
- [ ] Production: debug off, monitoring on, audited changes.
- [ ] Feature flags for incomplete or high-risk modules (e.g. portfolio recommendation mode OFF until research gates pass).
- [ ] Data region matches decision D1 (PROPOSED: UAE region) **[NEW]**.

## 9. Deployment & change control — P1+ staging, REL production
- [ ] Only reproducible, version-controlled builds deployed. No manual edits in production.
- [ ] Merge to `main` → auto-deploy to staging → smoke + critical E2E.
- [ ] Production deploy is a separate protected GitHub environment with **explicit owner approval**.
- [ ] Database migrations versioned and tested. Backup taken before risky production migrations.
- [ ] Rollback procedure documented **and rehearsed** before the first production release.

## 10. Data protection & privacy — P2 onward
- [ ] Data inventory in `DATA_DICTIONARY.md`: every field has a sensitivity label (Public / Internal / Confidential / Highly Confidential).
- [ ] Collect only what the calculations need (minimization). No national ID, passport or full account numbers unless required.
- [ ] Retention periods defined for each data type.
- [ ] Export-all and delete-all functions for the owner's data.
- [ ] Sensitive financial fields redacted in logs.
- [ ] Privacy obligations tracked in `docs/COMPLIANCE_AND_DOCUMENTATION.md` §A.

## 11. Audit trail — P1
- [ ] AuditEvent records actor, action, entity, diff/hash, timestamp (UTC) and correlation id.
- [ ] Tamper resistance: append-only table, hash-chained entries (PROPOSED), app role has no UPDATE/DELETE rights on it.
- [ ] All approvals, assumption changes, admin actions and denied attempts audited.
- [ ] Audit history survives the backup/restore drill.

## 12. Logging, monitoring & alerting — P1 basic, REL complete
- [ ] Structured JSON logs with correlation id.
- [ ] Error tracking (e.g. Sentry) with PII scrubbing on.
- [ ] Health-check endpoint and uptime monitor.
- [ ] Alerts go to the owner's email/phone: failed login bursts, CI security failure, backup failure, error spike, budget alert.
- [ ] Log retention defined (PROPOSED: 90 days app logs, 1 year audit/security logs).

## 13. Vulnerability management — P0 policy, ongoing
- [ ] Severity fix deadlines (PROPOSED): Critical 7 days · High 30 days · Medium 90 days · Low when practical.
- [ ] Critical/high issues block release unless formally risk-accepted (written in `SECURITY.md` with an expiry date).
- [ ] Weekly dependency-update PRs reviewed.
- [ ] `SECURITY.md` explains how to report an issue.

## 14. Supply chain — P0, REL
- [ ] Lockfiles committed. Exact versions pinned.
- [ ] Packages only from official registries. New dependencies justified in the PR description.
- [ ] Licence check: no copyleft licences that conflict with use (see Compliance §A5).
- [ ] SBOM generated per release (Syft, CycloneDX format).
- [ ] Build provenance / artifact attestation where practical (GitHub attestations, SLSA level 2 target).

## 15. Resilience, backup & restore — P1 backups, REL drill
- [ ] Automated encrypted daily backups. Point-in-time recovery if the provider supports it.
- [ ] Backup targets met: RPO 24 h / RTO 8 h (PROPOSED, D5).
- [ ] Restore drill into an **isolated** environment. Key records and audit history verified. Result logged.
- [ ] Monte Carlo and backtests have bounded resources (max paths, timeouts) so one run can't take the system down.
- [ ] Graceful failures: clear error messages, never silently wrong numbers.

## 16. Incident response — REL
- [ ] `RUNBOOK.md` covers these scenarios: credential leak, data exposure, dependency compromise, model/formula defect, lost account access.
- [ ] Each scenario lists: how to detect it, who to contact, first 3 actions, how to recover, and a blameless write-up afterwards.
- [ ] Contact list: cloud support, auth provider support, GitHub support.
- [ ] Tabletop exercise of "leaked API key" done once before release.

## 17. AI coding-agent security [NEW] — P0
- [ ] Agent works only on feature branches through PRs, never on `main`.
- [ ] Claude Code permissions: deny-list for `.env*`, secrets folders, `git push --force`, `git push origin main`, production CLIs.
- [ ] Hooks enforce rules deterministically, e.g. a pre-tool hook blocks edits to `.env*` (see `CLAUDE.md` §9).
- [ ] MCP servers connect only to dev resources. **No MCP access to production data or production credentials.**
- [ ] Only trusted, official MCP servers and plugins installed; list kept in `CLAUDE.md` §9.
- [ ] Web or evidence content read by the agent is treated as untrusted (prompt-injection risk).
- [ ] AI-written code goes through the same CI gates plus a separate reviewer-subagent pass (security focus).
- [ ] The agent never invents financial data. Fixtures are synthetic or sourced and approved.

## 18. Release security gate (tick all before each production release) — REL
- [ ] Sections 1–17 items for this phase complete, with proof
- [ ] Threat model reviewed for changes since the last release
- [ ] DAST scan (e.g. OWASP ZAP baseline) on the staging release candidate; false positives documented
- [ ] Zero unresolved critical/high findings (or signed risk acceptance)
- [ ] Restore drill passed within the last 90 days
- [ ] Accessibility and performance checks passed
- [ ] Owner approval recorded in the GitHub production environment

---
**Change log**
| Date | Change | By |
|---|---|---|
| 2026-09-23 | Initial checklist from BRD/PRD + review gaps | Claude |
