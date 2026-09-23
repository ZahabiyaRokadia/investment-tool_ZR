# Compliance & Documentation Tracker

**Project:** Enterprise Personal Investment & Financial Planning Model
**Scope assumption:** the app is used **only by the owner**, for personal planning, with no other users, no advice to others and no money movement.
**If that changes** (family members, clients, a public product), re-review Part A before any other person's data is entered.

> ⚖️ Claude is not a lawyer or financial advisor. Part A lists what to check and why. Anything marked **Confirm with professional** needs a qualified UAE adviser before it's treated as settled (per BRD §7 and PRD §18).

Status key: `Not started` · `In progress` · `Done` · `N/A (reason)` · `Confirm with professional`

---

## Part A — Compliance register

### A1. UAE Personal Data Protection Law (Federal Decree-Law No. 45 of 2021, "PDPL")
| Item | Finding | Action | Status |
|---|---|---|---|
| Applicability | Article 2 excludes "a Data Subject who processes his/her data for personal purposes". An owner-only personal tool appears to fall under this exclusion. | Record this assessment. Re-check if anyone else's data is added. | Confirm with professional |
| Executive Regulations | Reported as still pending in 2026. Breach-notice timelines depend on them. | Check the UAE Official Gazette each quarter. Log the result here. | In progress |
| Good practice anyway | Even if exempt, follow PDPL-style controls: minimization, security, retention, export/delete. | Covered in `SECURITY_CHECKLIST.md` §10–11. | Not started |
| Cloud provider as processor | The provider stores your data. | Review the provider's data processing terms and region (decision D1). | Not started |
| Free zones | DIFC and ADGM have their own data-protection laws; they apply only if the app or its operator is based there. | N/A unless hosting or operating from DIFC/ADGM. | N/A (onshore, personal) |

### A2. Financial-services regulation
| Item | Finding | Action | Status |
|---|---|---|---|
| Investment advice licensing | Advising **other people** on investments in the UAE is regulated (SCA onshore; DFSA in DIFC; FSRA in ADGM). Planning your own money is not advice to others. | Keep the app owner-only. Add a startup screen: "Personal decision-support. Not regulated financial advice." | Confirm with professional |
| Trading / custody | Out of scope (no execution, no money movement). | Keep out of scope; any change needs a new review. | N/A (out of scope) |
| Product information | Fund/ETF facts must come from issuer documents (prospectus, KID/factsheet) and be dated. | Enforced by M11 Product Due Diligence + evidence gating. | Not started |

### A3. Tax, estate & cross-border (research gate — BRD §7)
| Item | Finding | Action | Status |
|---|---|---|---|
| UAE personal tax | Generally no personal income tax on investment income for individuals; verify current rules. | Source and date the rule in the Evidence Registry. | Confirm with professional |
| Foreign withholding tax | Dividends from foreign-domiciled funds may be taxed at source (e.g. US-domiciled ETFs). | Tag every product with domicile and withholding assumptions. | Confirm with professional |
| Foreign estate/inheritance exposure | Some countries tax non-residents' assets on death (e.g. US-situs assets above a small threshold). | Flag in the product layer (Phase 7). | Confirm with professional |
| UAE succession / wills | Relevant to estate goals. | Out of app scope; note for owner. | Confirm with professional |

### A4. Data & content licensing
| Item | Finding | Action | Status |
|---|---|---|---|
| Market data / research reports | Many providers forbid storing or redistributing their data. | For every source, record its licence and whether storage is allowed in `DATA_SOURCES.md`. | Not started |
| Web-scraped data | PRD excludes unverified scraped inputs from production. | Scraping only where site terms allow; never in production without verification. | Not started |

### A5. Software & vendor compliance
| Item | Action | Status |
|---|---|---|
| Open-source licences | Automated licence check in CI. Avoid licences with obligations that conflict with your use (e.g. AGPL) without review. | Not started |
| SBOM | Generate per release (see Security §14). | Not started |
| Vendor terms | Keep a list of every paid or free service (cloud, auth, error tracking, data), its terms link and its data location. | Not started |
| AI tool terms | Check Claude plan's data-use and retention settings for the account used to code. | Not started |

### A6. Model governance (internal "compliance" with BRD rules)
| Item | Proof required | Status |
|---|---|---|
| No production result from unverified inputs | Automated gating tests + E2E "blocked with reason" journey | Not started |
| Every metric traceable | Traceability drill-down test | Not started |
| Outputs labelled with data-as-of, model version and limitations | UI test + report export check | Not started |
| Research gates (6) complete before recommendation mode | Validation Register entries, all approved | Not started |
| No guaranteed-return wording anywhere | Copy review checklist at each release | Not started |

---

## Part B — Documentation tracker

Every document lives in the repo (`/docs` unless noted), is written in plain language, and is updated **in the same PR** as the change that affects it (PRD §16).

### B1. Project governance documents (exist now)
| Document | Purpose | Status |
|---|---|---|
| BRD (`.docx`) | Business requirements, scope, gates | Done v1.0 |
| PRD (`.docx`) | Product and engineering requirements | Done v1.0 |
| `HANDOFF.md` (root) | Session-to-session progress and open decisions | Done |
| `CLAUDE.md` (root) | Agent guidelines & rulebook | Done |
| `docs/SECURITY_CHECKLIST.md` | Security setup checklist | Done |
| `docs/COMPLIANCE_AND_DOCUMENTATION.md` | This tracker | Done |

### B2. Required engineering documents (PRD §16 + gaps)
| Document | What it contains | Create by | Update when | Status |
|---|---|---|---|---|
| `README.md` (root) | Purpose, quick start, architecture summary, limitations | P0 | Product or setup change | Not started |
| `SETUP.md` | Step-by-step setup for a zero-coding owner, with screenshots | P0 | Tooling or dependency change | Not started |
| `ARCHITECTURE.md` | System diagram, modules M1–M13, data flow | P0 | Architecture change | Not started |
| `adr/NNNN-title.md` | One decision per file (context, options, decision, consequences) | P0 (first: stack & hosting) | Every significant decision | Not started |
| `SECURITY.md` | Controls, secrets, key ownership, vulnerability policy, risk acceptances | P0 | Control change | Not started |
| `THREAT_MODEL.md` | STRIDE threats and mitigations | Before P2 real data | Architecture change | Not started |
| `TESTING.md` | Test strategy, commands, fixtures, E2E journeys | P0 | Test or gate change | Not started |
| `DEPLOYMENT.md` | Environments, CI/CD, approvals, rollback | P0 | CI/CD or environment change | Not started |
| `DATA_DICTIONARY.md` | Fields, units, allowed values, sensitivity labels | P1 | Schema change | Not started |
| `EVIDENCE_POLICY.md` | **Source tiers defined** (e.g. Tier 1 regulator/central bank/issuer docs; Tier 2 major institutions; Tier 3 other), verification steps, freshness limits, gating | P1 | Source/verification rule change | Not started |
| `DATA_SOURCES.md` **[NEW]** | Each data source: provider, access method, cost, licence, storage allowed?, refresh cadence | P1 | New or changed source | Not started |
| `MODEL_METHODOLOGY.md` | Formulas, assumptions, backtest and simulation methods, limitations | P3 | Formula or method change | Not started |
| `RESEARCH_TRACKER.md` **[NEW]** | The 6 research gates: owner, sources, status, validation evidence | P1 | Research progress | Not started |
| `RISK_REGISTER.md` **[NEW]** | BRD §13 risks + new ones, with owner, likelihood, impact, status | P0 | Monthly or on a new risk | Not started |
| `RUNBOOK.md` | Operations, backup/restore, incident scenarios | P5 (draft), P9 (final) | Ops or incident change | Not started |
| `USER_GUIDE.md` | How to use each screen, in plain language, with a glossary | P8 | User-facing workflow change | Not started |
| `CHANGELOG.md` | What changed in each release | P0 | Every release | Not started |
| `UAT_SIGNOFF.md` **[NEW]** | Owner acceptance of each phase's exit criteria, with date | P0 | End of every phase | Not started |
| `.github/pull_request_template.md` | Checklist: tests, docs, security, handoff updated | P0 | Process change | Not started |

### B3. Phase exit documentation (proof a phase is "done")
For each phase, before marking it Done in `HANDOFF.md`:
1. Exit criteria (PRD §12) each linked to a passing test or CI run.
2. Relevant documents above updated (the PR template checklist is ticked).
3. Security checklist items for that phase ticked with proof.
4. `CHANGELOG.md` entry written.
5. Owner sign-off line added to `UAT_SIGNOFF.md`.

---

## Part C — Review calendar
| Frequency | Check |
|---|---|
| Every session | `HANDOFF.md` updated |
| Every PR | Affected docs updated in the same PR |
| End of each phase | Part B3 complete; this tracker's statuses refreshed |
| Quarterly | PDPL Executive Regulations status · data-source licences · vendor terms · risk register |
| Before first production release | All of Part A reviewed; "Confirm with professional" items resolved or accepted in writing |

**Change log**
| Date | Change | By |
|---|---|---|
| 2026-09-23 | Initial tracker | Claude |
