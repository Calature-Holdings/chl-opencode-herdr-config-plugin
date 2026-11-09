---
title: "Workflow Design: SPM-Driven Factory Pipeline as a Complex Adaptive System"
description: "Workflow design for the software-project-manager profile driving the OpenRig factory team through Herdr, integrating the Calature PDLC, SDLC, and PMI PM lifecycles via work-type classification and a Drum-Buffer-Rope governance cadence, grounded in the SDLC_GUIDE.md."
doc_type: "design"
author: "Devin Hedge"
version: "1.1"
last_updated: "2026-11-09"
status: "draft"
category: "design"
tags: ["workflow", "spm", "openrig", "herdr", "cas", "drum-buffer-rope", "sdlc", "design"]
related_docs:
  - "../01-research/setup-opencode-herdr-openrig-factory-hermes-spm.md"
  - "../03-vision/VISION.md"
  - "../../organization/ctl-portfolio/ctl-claude-code-opencode-herdr-delegation-plugin/docs/standards/guides/SDLC_GUIDE.md"
dependencies: []
review_date: "2027-05-09"
---

# Workflow Design: SPM-Driven Factory Pipeline as a Complex Adaptive System

## 1. Purpose and Scope

This design defines how a software-project-manager (SPM) profile in Hermes drives the OpenRig "factory" team running in Herdr. It integrates three competing lifecycles — the Calature Product Development Lifecycle (PDLC), the Software Development Lifecycle (SDLC, per SDLC_GUIDE.md), and the PMI Project Management Lifecycle — through work-type classification and a Drum-Buffer-Rope governance cadence.

Note: The SDLC_GUIDE (Section 7) names the broader nesting as PDLC contains OCDL (Organizational Capability Development Lifecycle) contains ECDL (Enterprise Capability Development Lifecycle), with SDLC as the innermost execution layer. The PMI PM lifecycle is an additional overlay the user has identified. This design treats PDLC, SDLC, and PMI PM as the three active lifecycles the SPM coordinates, while acknowledging OCDL/ECDL as outer constraints.

## 2. Grounding in SDLC_GUIDE.md

The SDLC is a prerequisite-gated, iterative chain (Signals -> Personas -> Market Problem -> Strategic Themes -> Funding/Bet Sizing -> Opportunity Framing -> Vision -> UXJ -> Requirements/Epic -> UXD -> Solution Architecture -> Solution Design -> Features -> User Stories -> Story Map -> Tasks -> ... -> Release). Key governance mechanisms the SPM must respect:

- **Evidence Ledger**: every artifact carries a confidence score; no artifact advances without a recorded score.
- **State transitions** are executed ONLY by the designated lifecycle agent, never by orchestrators or implementers.
- **HIP reviews** (Human In Process): human escalation points embedded in process steps.
- **ARB**: formal enterprise governance body for architectural documents (SAD, SDD), distinct from HIP.
- **Loop02 Bet Sizing**: funding gate requiring portfolio steward consent before implementation.
- **Epic HIP gates**: Portfolio Funnel, Portfolio Approval, Portfolio Release (go/no-go).
- **Feature sequencing HIP gates**: HIP-1, HIP-2.
- **Story decomposition HIP gates**: HIP-1, HIP-2, HIP-3.

## 3. Core Model: Complex Adaptive System

The three lifecycles are not a single pipeline. They are a Complex Adaptive System:

- Each lifecycle has its own cadence, artifacts, and governance.
- They run concurrently, not sequentially.
- Integration happens at governance gates, not through handoffs.
- The cadence (Drum-Buffer-Rope) is the single synchronization mechanism.

## 4. Work-Type Classification (Explicit and Documented)

The SPM classifies every stakeholder-initiated request before routing. Classification is recorded with rationale.

### Work Types

1. New Product
2. New Feature for an Existing Product
3. Enhancement or Change to an Existing Product Feature on an Existing Product
4. Bug Fix to an Existing Product Feature on an Existing Product
5. New Platform
6. New Feature for an Existing Platform
7. Enhancement or Change to an Existing Product Feature on an Existing Platform
8. Bug Fix to an Existing Platform Feature on an Existing Product
9. Technical Proof of Concept or Research/Spike
10. New Infrastructure Commissioning
11. Existing Infrastructure Change
12. Infrastructure Replacement and Replenishment
13. New Third-Party Integration
14. Change to Existing Third-Party Integration

### Classification Record

Each classification records: work type, dominant lifecycle, supporting lifecycles, rationale, and assigned drum cadence.

## 5. Lifecycle Routing (Approach C)

For each work type, one lifecycle is dominant; the other two run as concurrent supporting streams synchronized at governance gates.

| Work Type | Dominant Lifecycle | Supporting |
|-----------|-------------------|------------|
| New Product | PDLC | SDLC, PMI |
| New Feature (Product) | SDLC | PDLC, PMI |
| Enhancement/Change (Product) | SDLC | PDLC, PMI |
| Bug Fix (Product) | SDLC | PMI |
| New Platform | PDLC | SDLC, PMI |
| New Feature (Platform) | SDLC | PDLC, PMI |
| Enhancement/Change (Platform) | SDLC | PDLC, PMI |
| Bug Fix (Platform) | SDLC | PMI |
| POC/Research/Spike | SDLC (timeboxed) | PMI |
| New Infrastructure Commissioning | PMI | SDLC |
| Existing Infrastructure Change | PMI | SDLC |
| Infrastructure Replacement | PMI | SDLC, PDLC |
| New Third-Party Integration | SDLC | PDLC, PMI |
| Change to Third-Party Integration | SDLC | PMI |

## 6. Drum-Buffer-Rope Governance Cadence

- **Drum**: The slowest constrained resource sets the pace (typically the factory team's review capacity).
- **Buffer**: A protective buffer of validated, ready-to-start work ahead of the drum.
- **Rope**: Signals that release new work into the pipeline only when the buffer is consumed.

Governance gates synchronize the three lifecycles at buffer boundaries, leveraging SDLC governance mechanisms:

- **Gate 1 (Intake)**: Work classified and admitted to the buffer. Requires Opportunity Frame / Vision alignment per SDLC prerequisite gates.
- **Gate 2 (Ready)**: Work meets Definition of Ready for the dominant lifecycle (Vision validated, UXJ present, Epic in implementing state before Features).
- **Gate 3 (Review)**: Work reviewed by the factory team's reviewers; HIP review or ARB where architectural scope warrants.
- **Gate 4 (Release)**: Work satisfies Definition of Done, Epic Portfolio Release gate passed, and is released.

## 7. SPM Workflow (End-to-End)

1. **Intake**: Stakeholder submits a project request.
2. **Classify**: SPM classifies the work type and records rationale.
3. **Route**: SPM assigns the dominant lifecycle and supporting streams.
4. **Admit to Buffer**: Work enters the buffer at the appropriate cadence, subject to SDLC prerequisite gates (Vision, UXJ, Epic state).
5. **Dispatch**: SPM dispatches to `orch-lead@factory` via `rig send`.
6. **Track**: SPM monitors via `rig queue` and `rig ps`.
7. **Governance Gates**: Work passes gates 2-4 with concurrent lifecycle sync, respecting evidence ledger confidence thresholds and lifecycle-agent state ownership.
8. **Release**: Completed work released; buffer replenished via rope signal.
9. **Report**: SPM reports progress to the stakeholder.

## 8. Complex Adaptive Behavior

Because the system is adaptive, the SPM may reclassify work mid-flight when new information emerges. Reclassification is recorded as a governance event, not a failure. The cadence absorbs variability through the buffer. State transitions remain owned by designated lifecycle agents per SDLC_GUIDE.

## 9. Open Questions

- Exact drum cadence duration (sprint/iteration length) — TBD.
- Buffer size policy — TBD.
- Gate criteria definitions per work type — TBD.
- Escalation path when a gate is blocked — TBD.
- How OCDL/ECDL constraints shape capacity envelopes — TBD (Loop02 bet sizing).

## 10. Next Steps

- Define gate criteria per work type, aligned to SDLC HIP/ARB gates.
- Calibrate drum cadence with the factory team.
- Build the classification record template.
- Design the UI mockup for the SPM dashboard reflecting this workflow.
- Produce per-feature design specs.
