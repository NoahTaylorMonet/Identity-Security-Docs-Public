# CMMC 2.0: Level 2 vs Level 3 Overview

A reference comparing CMMC 2.0 Level 2 and Level 3, including the additional work required to architect and operate to Level 3 instead of Level 2 in a new (greenfield) cloud environment.

## Disclaimer

This document is informational. It does not constitute official guidance, legal advice, or a compliance attestation. CMMC requirements, assessment procedures, and Microsoft product capabilities change over time. Authoritative sources include 32 CFR Part 170 (the CMMC Program rule), 48 CFR / DFARS 252.204-7012, 7019, 7020, and 7021, NIST SP 800-171 Rev. 2, NIST SP 800-172, the DoD-published CMMC Assessment Guides for the level being pursued, and Microsoft service documentation. Independent review by qualified compliance, legal, and security advisors is required before making architecture, contractual, certification, or investment decisions based on this content. Nothing here should be interpreted as a guarantee that any specific configuration will satisfy a CMMC assessment.

---

## Core comparison

| Dimension | Level 2 | Level 3 |
|---|---|---|
| Data scope | CUI | CUI on critical programs or high-value assets |
| Practices | 110 | 134 (110 plus 24 enhanced) |
| Control source | NIST SP 800-171 Rev. 2 | NIST SP 800-171 Rev. 2 plus selected NIST SP 800-172 |
| Threat model | Reasonable adversary | Advanced persistent threat |
| Assessor | C3PAO third-party | DCMA DIBCAC government-led |
| Cadence | Every 3 years | Every 3 years |
| Prerequisite | L1 hygiene | L2 certification must already be held |
| Affirmation | Annual senior official affirmation in SPRS | Annual senior official affirmation in SPRS |
| POA&Ms | Limited items, 180-day close | Limited items, 180-day close |
| Typical contractor | Most defense primes and subcontractors | Smaller subset on the most sensitive programs |

---

## Greenfield work to move from L2 to L3 by architectural layer

The lift assumes a competent L2 build is already in place.

| Layer | L2 build | L3 adds | Lift | Notes |
|---|---|---|---|---|
| Identity | Conditional Access, MFA, PIM, phishing-resistant MFA for admins | Phishing-resistant MFA for all CUI users, dual-authorization in PIM for Tier 0, UEBA on privileged accounts | Small | Mainly a policy delta. The hardest piece is rolling FIDO2 or smart card to the full CUI user base. |
| Endpoint | Compliance policies, security baselines, ASR in Block, BitLocker, EDR, LAPS | Application Control (WDAC) enforced, non-persistence using gold image rebuild, endpoint deception, FIPS mode | Medium | WDAC is the dominant lift. A small starting application inventory makes audit-then-block achievable. |
| Network | NSGs, Private Endpoints, deny-by-default egress | Microsegmentation per workload, Zero Trust Network Access for inbound, no flat networks | Medium | Low marginal cost in greenfield. A retrofit on a running estate is a multi-year effort. |
| Data protection | Sensitivity labels for CUI, DLP across mail and sites and endpoint, retention, IRM | Auto-labeling at audit-grade accuracy, immutable evidence storage with legal hold, Key Vault dual-authorization, HSM-backed CMK | Small to medium | The tooling is the same at both levels. Accuracy and policy rigor are the work. |
| Security operations | SIEM or XDR connectors, content-hub rules, basic playbooks | Proactive hunt program, deeper UEBA, deception with honey accounts and honeytokens, threat intelligence integration, mature SOAR, tabletop exercise program | Medium to large | The tooling is the cheap part. The program (analysts, cadence, hypotheses, evidence) is the lift. |
| Threat and vulnerability management | Vulnerability management with weekly review | Continuous red team or adversary emulation, exposure-management cadence | Medium | Sustained program, not a project. |
| Cryptography | TLS 1.2 minimum, BitLocker, Key Vault | FIPS 140-2 or 140-3 validated modules enforced everywhere CUI resides, automated key rotation, HSM-backed CMK | Small in greenfield | Trivial to enforce at provisioning. Difficult to retrofit. |
| Logging and audit | Diagnostic settings to a log workspace, SIEM retention | Immutable storage tier with legal hold for CUI audit data, tamper-evident retention, extended hot and archive | Small | Increases storage cost. Minimal engineering effort. |
| Supply chain risk management | Vendor reviews at procurement | Formal SCRM program, continuous third-party risk monitoring, SBOM ingest, subcontractor flow-down enforcement | Medium to large | A program build (people and process) more than a tooling change. |
| Incident response and resiliency | Documented IR plan, SOAR playbooks | IR plan tested with quarterly tabletops and an annual technical exercise, image rebuild cadence, multi-region survivability for CUI workloads | Medium | Calendar work that continues indefinitely. |
| Governance | NIST 800-171 compliance template, evidence repository | 800-172 evidence track layered on top, preparation for the DIBCAC engagement style | Small in greenfield | DIBCAC assessments require stricter evidence rigor than C3PAO. |
| Training | Annual security awareness training | Role-based training for privileged users, insider risk training, supplier-facing training | Small | Curriculum addition. |

---

## What is identical between Level 2 and Level 3

| Item | Why it does not change |
|---|---|
| Base 110 NIST 800-171 controls | L3 is additive, not a different framework |
| Microsoft licensing footprint (E5, MDO P2, MDE P2, MDI, Sentinel, Purview) | Same shape at both levels |
| FedRAMP Moderate equivalency for cloud service providers handling CUI | Required at both |
| Microsoft 365 GCC High and Azure Government as the platform | Same |
| DFARS 7012 flow-down obligations to subcontractors | Required at both |
| Conditional Access, PIM, and phishing-resistant MFA foundation | Foundational, not a delta |
| Defender XDR and Sentinel integration backbone | Same |
| Compliance Manager as the auditor-facing scorecard | Same |

---

## Where greenfield reduces the L3 lift

| L3 area | Why greenfield helps |
|---|---|
| Network microsegmentation | Designed once at provisioning, instead of a multi-year re-segmentation of running workloads |
| WDAC application control | Small initial application inventory makes audit-then-block achievable in weeks |
| FIPS crypto enforcement | A single policy at provisioning inherits to every subsequent resource |
| Immutable audit storage | Defined upfront with no historical backfill required |
| Dual-authorization patterns | PIM and Key Vault configuration completed in minutes |
| Private Link everywhere | A design choice, not a migration |

---

## Where greenfield does not help

| L3 area | Why it remains hard |
|---|---|
| Proactive hunt program | Requires analysts, cadence, and documented hypotheses. No tool installs this. |
| Supply chain risk management | A department-level program, not a feature |
| DIBCAC assessment readiness | Stricter evidence rigor than commercial C3PAO assessments. First engagements often require advisory support. |
| IR exercise program | Quarterly tabletops and annual technical exercises that continue indefinitely |
| WDAC lifecycle operations | Every new application requires catalog management and signing oversight |
| Insider risk program | Purview IRM combined with HR partnership, legal review, and a defined investigator role |
| Continuous red team | A real budget line that is sustained over time |

---

## Lift summary

| Component | Build cost (greenfield) | Steady-state run cost |
|---|---|---|
| L3 architecture (technical configuration) | Low marginal cost above an L2 build | Modest |
| L3 certification and program (people, process, evidence) | High | High and people-heavy |

The technical L3 deltas are small in greenfield. The programmatic L3 deltas are unchanged regardless of whether the environment is new or existing.

---

## Recommended posture

| Posture | Description |
|---|---|
| Build L3-ready, certify to L2 | Make the architectural choices at provisioning: microsegmentation, FIPS, Private Link, WDAC in audit, deception, and phishing-resistant MFA across all users. Defer the L3 program lift (hunt team, SCRM, DIBCAC preparation) until a contract requires it. |
| Run audit-only compliance assessment | Use audit-only regulatory compliance assessments (NIST 800-171 Rev 2, FedRAMP High, and 800-172 deltas) to score against L3 while operating to L2. |
| Pursue L3 certification when contractually required | The Department of Defense designates L3 on specific contracts. The associated program investment should be tied to that contractual trigger. |

---

## Summary

In a new environment the technical work to be L3-aligned is a small increment above a competent L2 build. Certifying to L3, passing the DIBCAC assessment, and sustaining the program is a separate decision that should be tied to a contractual requirement. Designing the environment to be L3-ready while certifying to L2 keeps the option open without paying for the L3 program before it is needed.

---

This document is provided as-is with no warranty. It does not represent official Microsoft, DoD, or CMMC Accreditation Body guidance. Independent verification by qualified compliance and legal advisors is required before reliance.
