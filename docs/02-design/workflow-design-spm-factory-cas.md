---
title: "Workflow Design: SPM-Driven Factory Pipeline as a Complex Adaptive System"
description: "Workflow design for the software-project-manager profile driving the OpenRig factory team through Herdr, integrating PDLC, SDLC, and PMI lifecycles via work-type classification and a Drum-Buffer-Rope governance cadence."
doc_type: "design"
author: "Devin Hedge"
version: "1.0"
last_updated: "2026-11-09"
status: "draft"
category: "design"
tags: ["workflow", "spm", "openrig", "herdr", "cas", "drum-buffer-rope", "design"]
related_docs:
  - "../01-research/setup-opencode-herdr-openrig-factory-hermes-spm.md"
  - "../03-vision/VISION.md"
dependencies: []
review_date: "2027-05-09"
---

# Workflow Design: SPM-Driven Factory Pipeline as a Complex Adaptive System

## 1. Purpose and Scope

This design defines how a software-project-manager (SPM) profile in Hermes drives the OpenRig "factory" team running in Herdr, integrating three competing lifecycles (Product Development Lifecycle, Software Development Lifecycle, PMI Project Management Lifecycle) through work-type classification and a Drum-Buffer-Rope governance cadence.

## 2. Core Model: Complex Adaptive System

The three lifecycles are not a single pipeline. They are a Complex Adaptive System:

- Each lifecycle has its own cadence, artifacts, and governance.
- They run concurrently, not sequentially.
- Integration happens at governance gates, not through handoffs.
- The cadence (Drum-Buffer-Rope) is the single synchronization mechanism.

## 3. Work-Type Classification (Explicit and Documented)

The SPM classifies every stakeholder-initiated request into one of the following work types before routing. Classification is recorded with rationale.

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

## 4. Lifecycle Routing (Approach C)

For each work type, one lifecycle is dominant; the other two run as concurrent supporting streams with governance-gate synchronization.

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

## 5. Drum-Buffer-Rope Governance Cadence

- **Drum**: The slowest constrained resource sets the pace (typically the factory team's review capacity).
- **Buffer**: A protective buffer of validated, ready-to-start work ahead of the drum.
- **Rope**: Signals that release new work into the pipeline only when the buffer is consumed.

Governance gates synchronize the three lifecycles at buffer boundaries:

- **Gate 1 (Intake)**: Work classified and admitted to the buffer.
- **Gate 2 (Ready)**: Work meets Definition of Ready for the dominant lifecycle.
- **Gate 3 (Review)**: Work reviewed by the factory team's reviewers.
- **Gate 4 (Release)**: Work satisfies Definition of Done and is released.

## 6. SPM Workflow (End-to-End)

1. **Intake**: Stakeholder submits a project request.
2. **Classify**: SPM classifies the work type and records rationale.
3. **Route**: SPM assigns the dominant lifecycle and supporting streams.
4. **Admit to Buffer**: Work enters the buffer at the appropriate cadence.
5. **Dispatch**: SPM dispatches to `orch-lead@factory` via `rig send`.
6. **Track**: SPM monitors via `rig queue` and `rig ps`.
7. **Governance Gates**: Work passes gates 2-4 with concurrent lifecycle sync.
8. **Release**: Completed work released; buffer replenished via rope signal.
9. **Report**: SPM reports progress to the stakeholder.

## 7. Complex Adaptive Behavior

Because the system is adaptive, the SPM may reclassify work mid-flight when new information emerges. Reclassification is recorded as a governance event, not a failure. The cadence absorbs variability through the buffer.

## 8. Open Questions

- Exact drum cadence duration (sprint/iteration length) — TBD.
- Buffer size policy — TBD.
- Gate criteria definitions per work type — TBD.
- Escalation path when a gate is blocked — TBD.

## 9. Next Steps

- Define gate criteria per work type.
- Calibrate drum cadence with the factory team.
- Build the classification record template.
- Design the UI mockup for the SPM dashboard reflecting this workflow.
- Produce per-feature design specs.
