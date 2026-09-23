# HANDOFF — Enterprise Personal Investment & Financial Planning Model

> **Read this first in every session.** Update it last before every session ends.
> Newest session entry goes at the TOP of the Session Log. Keep entries short (5–10 lines).
> Items marked **PROPOSED** are defaults suggested by Claude and still need the owner's approval.

---

## 1. Project snapshot

| Item | Value |
|---|---|
| Product | Secure, auditable personal investment & financial-planning web app for one UAE-based individual |
| Owner | Zahabiya (zero coding experience; first AI-built project) |
| Users | **Owner only** (personal tool; no other users planned) |
| Source of truth | `Enterprise_Investment_Planning_Model_BRD.docx`, `Enterprise_Investment_Planning_Model_PRD.docx` (v1.0, 16 Sep 2026) |
| Rules for the agent | `CLAUDE.md` (Guidelines & Rulebook) |
| Security checklist | `docs/SECURITY_CHECKLIST.md` |
| Compliance & docs tracker | `docs/COMPLIANCE_AND_DOCUMENTATION.md` |
| Coding tool | Claude Code |
| Repository | _Not created yet_ (Phase 0) — private GitHub repo |
| Current phase | **Pre-Phase 0: planning & documentation** |

## 2. Goals (what "success" means)

1. Every number traces back: evidence → provenance → assumption → portfolio → risk/goal output.
2. Unverified, stale or missing data **blocks** production results (fail closed).
3. Compare investments by return, loss risk, drawdown, liquidity, fees, inflation and goal probability.
4. Low-to-medium risk planning with visible downside and stress scenarios.
5. The owner can run and understand the system using plain-language docs.
6. Enterprise-grade security and delivery discipline, even as a one-person project.

**Not in scope:** trade execution, moving money, leverage/derivatives, guaranteed returns, multi-user SaaS.

## 3. Tech stack (from PRD §10)

| Layer | Choice | Status |
|---|---|---|
| Frontend | Next.js + TypeScript | Agreed |
| Backend / API | Python 3.13 + FastAPI | Agreed |
| Calculations | NumPy, pandas, SciPy | Agreed |
| Database | PostgreSQL + SQLAlchemy + Alembic | Agreed |
| Auth | Managed OIDC provider with MFA | **PROPOSED:** Auth0 or cloud-native (e.g. AWS Cognito) |
| Hosting | Managed cloud, UAE region | **PROPOSED:** AWS `me-central-1` (UAE) or Azure UAE North |
| CI/CD | GitHub Actions | Agreed |
| Testing | pytest, Vitest, Playwright | Agreed |
| Local dev | Docker | Agreed |
| Infra as code | Terraform / OpenTofu (from Phase 9 or first cloud resource) | Agreed |

## 4. Phase tracker

| # | Phase | Status | Research-gated? |
|---|---|---|---|
| 0 | Foundation & guardrails | Not started | No |
| 1 | Evidence & provenance | Not started | No |
| 2 | Personal finance profile | Not started | No |
| 3 | Assumption engine | Not started | No |
| 4 | Portfolio engine | Not started | Yes (production mode) |
| 5 | Risk & historical analytics | Not started | Yes |
| 6 | Monte Carlo & goals | Not started | Yes |
| 7 | UAE product layer | Not started | Yes |
| 8 | Reporting & explainability | Not started | No |
| 9 | Hardening & release | Not started | No |

**PROPOSED first usable version (MVP):** Phases 0 → 1 → 2 + a basic read-only report page, so the owner has a working app early.

Research gates (all must pass before production portfolio recommendations): 0 of 6 met — see BRD §7.

## 5. Open decisions (owner to confirm)

| # | Decision | Proposed default | Status |
|---|---|---|---|
| D1 | Cloud provider & region | AWS me-central-1 (UAE) — keeps data in the UAE | Open |
| D2 | Auth provider | Managed OIDC with MFA (Auth0 or Cognito) | Open |
| D3 | Market data sources | Manual upload of institutional reports first; paid APIs later only if licence allows storage | Open |
| D4 | Freshness limits | Prices/FX: 1 business day · CPI: 45 days after release · Capital-market assumptions: 12 months | Open |
| D5 | Backup targets | RPO 24 h (max data loss) · RTO 8 h (max downtime) | Open |
| D6 | Monte Carlo defaults | 10,000 paths, fixed seed; stable if goal probability moves < 1 pp between 5k and 10k paths | Open |
| D7 | Solo-mode review | AI reviewer subagent acts as second checker; limitation documented, never faked as human approval | Open |
| D8 | Security standard | OWASP ASVS 5.0 Level 2 | Open |
| D9 | Monthly budget cap (hosting + tools + data) | Owner to set | Open |
| D10 | Target date for MVP | Owner to set | Open |
| D11 | UI language | English only | Open |
| D12 | Research owner for the 6 gates | Owner + Claude research sessions; each item verified before approval | Open |

## 6. Accounts & access register (names only — NEVER write passwords or keys here)

| Account | Owner | MFA on? | Recovery codes stored? |
|---|---|---|---|
| GitHub | Owner | ☐ | ☐ |
| Cloud provider | Owner | ☐ | ☐ |
| Auth provider | Owner | ☐ | ☐ |
| Password manager | Owner | ☐ | ☐ (printed copy kept offline) |
| Domain registrar (if any) | Owner | ☐ | ☐ |

## 7. Decisions log (short; full reasoning goes in `docs/adr/`)

| Date | Decision | Why |
|---|---|---|
| 2026-09-23 | Use Markdown docs in the project folder; rulebook lives in `CLAUDE.md` | Claude Code loads `CLAUDE.md` automatically every session |
| 2026-09-23 | App is for the owner only | Keeps privacy and licensing obligations lighter; revisit if that changes |
| 2026-09-23 | Created root `.gitignore` (secrets + build/dev artifacts only) | Enforces `SECURITY_CHECKLIST.md` §3 before first commit; BRD/PRD/CLAUDE.md/HANDOFF.md/docs/ stay tracked in git per `COMPLIANCE_AND_DOCUMENTATION.md` Part B1 |

---

## 8. Session log (newest first)

### 2026-09-23 — Session 1 (Claude, Cowork)
- **Done:** Reviewed BRD & PRD. Built a project dashboard artifact. Wrote gap review. Created `HANDOFF.md`, `CLAUDE.md`, `docs/SECURITY_CHECKLIST.md`, `docs/COMPLIANCE_AND_DOCUMENTATION.md`.
- **Tests run:** None (no code yet).
- **Blockers:** Open decisions D1–D12 above.
- **Note to next session opener:** Start by walking the owner through Open Decisions D1–D12 (approve or change each). Then begin **Phase 0**: create the private GitHub repo, enable MFA on every account, and set up branch protection. Follow `CLAUDE.md` → "Session start". Don't write feature code until Phase 0 exit criteria pass.

<!-- Template for new entries (copy above the latest entry):
### YYYY-MM-DD — Session N (tool)
- **Phase / task:**
- **Done:**
- **Tests run & result:** (command + pass/fail)
- **Checked by:** (self-check / reviewer subagent / owner)
- **Blockers / risks:**
- **Note to next session opener:**
-->
