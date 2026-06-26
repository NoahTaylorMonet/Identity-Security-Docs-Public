# CMMC 2.0 — Level 2 vs Level 3 Overview

A reference comparing CMMC 2.0 Level 2 and Level 3, including the marginal effort to architect and operate to Level 3 instead of Level 2 in a new (greenfield) cloud environment.

---

## Core comparison

| Dimension | **Level 2** | **Level 3** |
|---|---|---|
| Data scope | CUI | CUI on critical programs / high-value assets |
| Practices | 110 | 134 (110 + 24 enhanced) |
| Control source | NIST SP 800-171 Rev. 2 | NIST SP 800-171 Rev. 2 **+** selected NIST SP 800-172 |
| Threat model | Reasonable adversary | Advanced persistent threat (APT) |
| Assessor | C3PAO (third-party) | DCMA DIBCAC (government-led) |
| Cadence | Every 3 years | Every 3 years |
| Prerequisite | L1 hygiene | **Must hold L2 certification first** |
| Affirmation | Annual senior official affirmation in SPRS | Annual senior official affirmation in SPRS |
| POA&Ms | Limited items, 180-day close | Limited items, 180-day close |
| Typical contractor | Most defense primes and subcontractors | Small subset on most sensitive programs |

---

## Greenfield lift — Level 2 to Level 3 by architectural layer

Marginal lift assumes a competent Level 2 build is already in place.

| Layer | L2 build | L3 adds | Lift | Notes |
|---|---|---|---|---|
| **Identity** | Conditional Access, MFA, PIM, phishing-resistant MFA for admins | Phishing-resistant MFA for **all CUI users**; dual-authorization in PIM for Tier 0; UEBA on privileged accounts | Small | Mostly a policy delta. Hardest piece is rolling FIDO2 / smart card to the full CUI user base. |
| **Endpoint** | Compliance policies, security baselines, ASR in Block, BitLocker, EDR, LAPS | **Application Control (WDAC) enforced**; non-persistence (rebuild from gold images); endpoint deception; FIPS mode | Medium | WDAC is the dominant lift. Greenfield makes audit-then-block feasible. |
| **Network** | NSGs, Private Endpoints, deny-by-default egress | Microsegmentation per workload; Zero Trust Network Access for inbound; no flat networks | Medium | Effectively free in greenfield; significant in a brownfield re-architecture. |
| **Data protection** | Sensitivity labels (CUI), DLP across mail / sites / endpoint, retention, IRM | Auto-labeling at audit-grade accuracy; immutable evidence storage with legal hold; Key Vault dual-authorization; HSM-backed customer-managed keys | Small–Medium | Tooling is identical; accuracy and policy rigor are the work. |
| **Security operations** | SIEM / XDR connectors, content-hub rules, basic playbooks | **Proactive hunt program**; mature UEBA; deception (honey accounts, honeytokens); threat intelligence integrated into operations; mature SOAR; tabletop exercise program | Medium–Large | Tooling is the cheap part. People and program are the lift. |
| **Threat & vulnerability management** | Vulnerability management with weekly review | Continuous red team / adversary emulation; exposure-management cadence | Medium | Sustained program, not a project. |
| **Cryptography** | TLS 1.2+ minimum, BitLocker, Key Vault | FIPS 140-2 / 140-3 validated modules enforced everywhere CUI resides; automated key rotation; HSM-backed CMK | Small in greenfield | Trivial to enforce on day 1; difficult to retrofit. |
| **Logging & audit** | Diagnostic settings to log workspace, SIEM retention | Immutable storage tier with legal hold for CUI audit data; tamper-evident retention; extended hot + archive | Small | Increases storage cost; minimal engineering effort. |
| **Supply chain (SCRM)** | Vendor reviews at procurement | Formal SCRM program; continuous third-party risk monitoring; SBOM ingest; subcontractor flow-down enforcement | Medium–Large | Largely a program-build (people and process), not a tooling effort. |
| **Incident response & resiliency** | Documented IR plan, SOAR playbooks | IR plan **tested** (quarterly tabletop, annual technical exercise); image rebuild cadence; multi-region survivability for CUI workloads | Medium | Calendar work that continues indefinitely. |
| **Governance** | NIST 800-171 compliance template, evidence repository | 800-172 evidence track layered on top; preparation for DIBCAC engagement style | Small in greenfield | DIBCAC assessments require stricter evidence rigor than C3PAO. |
| **Training** | Annual security awareness training | Role-based training for privileged users; insider risk training; supplier-facing training | Small | Curriculum addition. |

---

## What is identical between Level 2 and Level 3

| Item | Why it doesn't change |
|---|---|
| Base 110 NIST 800-171 controls | L3 is additive, not a different framework |
| Microsoft licensing footprint (E5 + MDO P2 + MDE P2 + MDI + Sentinel + Purview) | Same shape both levels |
| FedRAMP Moderate equivalency for cloud service providers handling CUI | Required at both |
| Microsoft 365 GCC High + Azure Government as the platform | Same |
| DFARS 7012 flow-down obligations to subcontractors | Required at both |
| Conditional Access, PIM, and phishing-resistant MFA foundation | Foundation, not delta |
| Defender XDR + Sentinel integration backbone | Same |
| Compliance Manager as the auditor-facing scorecard | Same |

---

## Where greenfield genuinely collapses the lift

| L3 area | Why greenfield wins |
|---|---|
| Network microsegmentation | Designed from scratch in one set of decisions versus a multi-year re-segmentation on a running estate |
| WDAC application control | Small initial application inventory makes audit-then-block achievable in weeks |
| FIPS crypto enforcement | A single policy on day 1 inherits to every subsequent resource |
| Immutable audit storage | Defined upfront with no historical backfill required |
| Dual-authorization patterns | PIM + Key Vault configuration completed in minutes |
| Private Link everywhere | A design choice, not a migration project |

---

## Where greenfield does not help

| L3 area | Why it remains hard |
|---|---|
| Proactive hunt program | Requires analysts, cadence, and documented hypotheses — no tool installs this |
| Supply chain risk management | A department-level program, not a feature |
| DIBCAC assessment readiness | Higher evidence rigor than commercial C3PAO assessments; first engagements typically require advisory help |
| IR exercise program | Quarterly tabletops + annual technical exercises continue indefinitely |
| WDAC lifecycle operations | Every new application requires catalog management and signing oversight |
| Insider risk program | Combines Purview IRM, HR partnership, legal review, and a defined investigator role |
| Continuous red team | A real budget line, sustained over time |

---

## Lift summary

| Component | Build cost (greenfield) | Steady-state run cost |
|---|---|---|
| L3 **architecture** (technical configuration) | Low marginal cost | Modest |
| L3 **certification + program** (people, process, evidence) | High | High (people-heavy) |

The technical L3 deltas are small in greenfield. The programmatic L3 deltas are unchanged regardless of greenfield status.

---

## Recommended posture

| Posture | Description |
|---|---|
| **Build L3-ready, certify to L2** | Make architectural choices on day 1 (microsegmentation, FIPS, Private Link, WDAC in audit, deception, phishing-resistant MFA org-wide). Defer the L3 *program* lift (hunt team, SCRM, DIBCAC preparation) until a contract requires it. |
| **Run audit-only compliance assessment now** | Use audit-only regulatory compliance assessments (NIST 800-171 Rev 2 + FedRAMP High + 800-172 deltas) to score against L3 while operating to L2. |
| **Pursue L3 certification only when contractually required** | The Department of Defense designates L3 on specific contracts. The associated program investment should be tied to that contractual trigger. |

---

## Summary

> In a clean environment you can build to L3 architecture for very little marginal cost. Certifying to L3 — passing the DIBCAC assessment and sustaining the program — is a separate decision that should be tied to a contractual requirement. Designing the environment to be L3-ready while certifying to L2 preserves optionality without paying for the L3 program prematurely.

---

*Reference document. Verify framework alignment against the current 32 CFR rule (CMMC 2.0) and the DoD-published assessment guides before relying on this for formal program planning.*
