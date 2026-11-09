---
doc_type: "vision"
persona_num: "00"
vision_num: "01"
vision_slug: "opencode-herdr-openrig-factory-hermes-spm"
vision_id: "00-01"
vision_title: "VISION-00-01-opencode-herdr-openrig-factory-hermes-spm"
description: "Enable a single software-project-manager persona to drive a persistent, multi-agent engineering team (OpenRig factory) running in Herdr terminal sessions, with OpenCode as an additional agent, all orchestrated from a scoped Hermes SPM profile."
doc_type_standard: "../standards/VISION_STANDARD.md"
author: "Devin Hedge"
version: "1.0"
last_updated: "2026-11-09"
status: "draft"
category: "vision"
state:
  current: "hypothesized"
  confidence: 0.60
owner: "Devin Hedge"
created: "2026-11-09"
originating_signals:
  - id: "SIG-2026-HerdrMultiAgent"
    type: "technology"
    source: "enterprise-projects/chl-opencode-herdr-config-plugin/docs/01-research/setup-opencode-herdr-multi-agent.md"
  - id: "SIG-2026-OpenRigFactory"
    type: "technology"
    source: "enterprise-projects/chl-opencode-herdr-config-plugin/docs/01-research/setup-opencode-herdr-openrig-factory.md"
  - id: "SIG-2026-HermesSPMDriver"
    type: "technology"
    source: "enterprise-projects/chl-opencode-herdr-config-plugin/docs/01-research/setup-opencode-herdr-openrig-factory-hermes-spm.md"
linked_artifacts:
  personas: []
  uxjs: []
  epics: []
  features: []
  vision_seeds: []
embedded_experiments: []
evidence_ledger:
  - source: "setup-opencode-herdr-multi-agent.md"
    section: "Strategic Context"
    evidence_type: "qualitative"
    confidence: 0.80
    date: "2026-11-09"
    agent_source: "agent.vision.orchestrator"
  - source: "setup-opencode-herdr-openrig-factory.md"
    section: "Strategic Context"
    evidence_type: "qualitative"
    confidence: 0.80
    date: "2026-11-09"
    agent_source: "agent.vision.orchestrator"
  - source: "setup-opencode-herdr-openrig-factory-hermes-spm.md"
    section: "Strategic Context"
    evidence_type: "qualitative"
    confidence: 0.80
    date: "2026-11-09"
    agent_source: "agent.vision.orchestrator"
responsible_agents:
  orchestrator: "agent.vision.orchestrator"
  experiment_agent: "agent.experiment.viability"
  evidence_agent: "agent.vision.evidence"
  synthesis_agent: "agent.strategy.synthesizer"
  lifecycle_agent: "agent.vision.lifecycle"
  signal_agent: "agent.signal.orchestrator.meta"
tags:
  - "vision"
  - "strategy"
  - "agentic"
  - "opencode"
  - "herdr"
  - "openrig"
  - "hermes"
  - "spm"
priority: "high"
related_docs:
  - "../standards/VISION_STANDARD.md"
  - "../processes/PROCESS-vision-01-vision-seed-promotion.md"
  - "./01-research/setup-opencode-herdr-multi-agent.md"
  - "./01-research/setup-opencode-herdr-openrig-factory.md"
  - "./01-research/setup-opencode-herdr-openrig-factory-hermes-spm.md"
dependencies: []
review_date: "2027-05-09"
---

# VISION-00-01-opencode-herdr-openrig-factory-hermes-spm

## Metadata and Governance
**Managed by:** `agent.vision.lifecycle`

### Document Status

- **Current State:** hypothesized
- **Overall Confidence:** 0.60
- **Last Validation:** 2026-11-09
- **Next Review:** 2027-05-09

### Ownership and Responsibility

- **Vision Owner:** Devin Hedge
- **Strategic Lead:** Devin Hedge
- **Primary Stakeholders:** Software project manager persona, engineering team leads, developers using OpenCode

### Lifecycle History

| State | Entry Date | Duration | Exit Reason |
|-------|------------|----------|-------------|
| hypothesized | 2026-11-09 | - | Initial synthesis from research |

## Strategic Context
**Managed by:** `agent.signal.orchestrator.meta`

> This section summarizes originating signals (market, regulatory, technology) referenced in `originating_signals`. Content is derived from the three research artifacts listed above.

### Market Signals (Evidence Summary)

- **Signal 1:** Herdr provides persistent multi-agent terminal sessions that survive disconnects via SIGHUP handling, enabling durable agent teams across detach/reattach cycles.
  - **Source:** setup-opencode-herdr-multi-agent.md, Sections 6 and 7
  - **Confidence:** 0.80
- **Signal 2:** OpenRig ships a "factory" team of seven agents (lead, advisor, build, QA, design, and two independent reviewers) for sustained product work, installable in one command.
  - **Source:** setup-opencode-herdr-openrig-factory.md, Sections 4 and 9
  - **Confidence:** 0.80
- **Signal 3:** Hermes Agent supports profiles as isolated home directories, allowing a dedicated software-project-manager profile with its own config, memory, sessions, skills, and gateway state.
  - **Source:** setup-opencode-herdr-openrig-factory-hermes-spm.md, Sections 5 and 12
  - **Confidence:** 0.80

### Regulatory Signals (Evidence Summary)

- **Signal:** None identified in the research artifacts; no regulatory drivers documented.
  - **Source:** N/A
  - **Confidence:** 0.00

### Technology Signals (Evidence Summary)

- **Signal 1:** OpenCode integrates with Herdr via `herdr integration install opencode`, enabling pane-local lifecycle reporting and native session restore after server restart.
  - **Source:** setup-opencode-herdr-multi-agent.md, Section 4
  - **Confidence:** 0.85
- **Signal 2:** A community "OpenRig for Herdr" plugin exposes the OpenRig operator and advisor, needs-you indicators, task following, and agent messaging inside Herdr.
  - **Source:** setup-opencode-herdr-openrig-factory.md, Section 7
  - **Confidence:** 0.70
- **Signal 3:** Hermes integrates with Herdr via `herdr integration install hermes`, reporting resumable session ids; an optional Hermes-OpenCode bridge plugin lets the SPM profile dispatch coding tasks to OpenCode.
  - **Source:** setup-opencode-herdr-openrig-factory-hermes-spm.md, Sections 10 and 12.1
  - **Confidence:** 0.70

### Macro Trend Summary

The convergence of persistent terminal runtimes (Herdr), multi-agent harnesses (OpenRig), and profile-scoped agent drivers (Hermes) makes it feasible for a single software-project-manager to coordinate a full engineering squad without losing state across sessions. Each layer is independently installable and interoperable, lowering the barrier to a durable, observable multi-agent development workflow.

**Evidence Confidence:** 0.70
**Supporting Evidence:** setup-opencode-herdr-multi-agent.md, setup-opencode-herdr-openrig-factory.md, setup-opencode-herdr-openrig-factory-hermes-spm.md

## Originating Vision Seeds
**Managed by:** `agent.visionseed.strategy`

> Vision Seeds are derived from validated Personas. No validated Persona artifacts exist in this repository yet; the seeds below are hypothesized from the research and marked accordingly.

### Vision Seed 1: VISION-SEED-00-PersistentMultiAgentTeam

- **Summary:** A single project manager can drive a persistent multi-agent engineering team that survives disconnects and restores natively.
- **Source Persona:** persona-00-developer-tooling-user (hypothesized, not yet validated)
- **Trigger Signals:** SIG-2026-HerdrMultiAgent, SIG-2026-OpenRigFactory
- **Confidence:** 0.60
- **Contribution:** Provides the core persistence and team-coordination motivation.
- **Weight:** 0.50
- **Status:** pending
- **Proposed By:** agent.visionseed.strategy

### Vision Seed 2: VISION-SEED-00-SPMDrivenPipeline

- **Summary:** A scoped Hermes SPM profile can orchestrate the OpenRig factory team and OpenCode panes through terminal commands and optional bridge plugins.
- **Source Persona:** persona-00-developer-tooling-user (hypothesized, not yet validated)
- **Trigger Signals:** SIG-2026-HermesSPMDriver
- **Confidence:** 0.55
- **Contribution:** Provides the driver/orchestration motivation.
- **Weight:** 0.50
- **Status:** pending
- **Proposed By:** agent.visionseed.strategy

### Synthesis Rationale

- **Synthesis Method:** simple_average
- **Weight Justification:** Equal weights because both seeds address complementary dimensions (persistence/team vs. driver/orchestration) with no evidence favoring one over the other.
- **Thematic Coherence:** The two seeds converge on a single outcome: a durable, SPM-driven multi-agent engineering pipeline.
- **Conflict Resolution:** None required; seeds are complementary.

**Evidence Confidence:** 0.57
**Supporting Evidence:** setup-opencode-herdr-multi-agent.md; setup-opencode-herdr-openrig-factory-hermes-spm.md

## Vision Hypothesis
**Managed by:** `agent.vision.orchestrator`

> The Vision Hypothesis is synthesized from the research artifacts. Confidence is moderate because no validated Persona evidence exists yet.

### Core Hypothesis Statement

**When** developers need to coordinate multiple AI coding agents across sessions without losing state or context,

**We believe** a software project manager working from a scoped Hermes profile

**Want to** drive a persistent OpenRig factory team in Herdr while observing OpenCode panes in the same workspace,

**So that** engineering work flows through a durable, observable, multi-agent pipeline instead of ad-hoc terminal sessions.

**Therefore, our Vision is to:**
Provide a documented, reproducible setup where a Hermes SPM profile orchestrates the OpenRig factory team through Herdr, with OpenCode as an additional observable agent, preserving state across detach/reattach and server restarts.

### Evidence Mapping

| Clause | Evidence Source | Evidence Type | Confidence |
|--------|-----------------|---------------|------------|
| WHEN | setup-opencode-herdr-multi-agent.md, Section 6 | qualitative | 0.80 |
| WE BELIEVE | setup-opencode-herdr-openrig-factory-hermes-spm.md, Section 5 | qualitative | 0.70 |
| WANT TO | setup-opencode-herdr-openrig-factory-hermes-spm.md, Section 12 | qualitative | 0.70 |
| SO THAT | setup-opencode-herdr-multi-agent.md, Section 6.1 | qualitative | 0.80 |

**Evidence Confidence:** 0.75
**Supporting Evidence:** setup-opencode-herdr-multi-agent.md; setup-opencode-herdr-openrig-factory.md; setup-opencode-herdr-openrig-factory-hermes-spm.md

## Guiding Principles
**Managed by:** `agent.strategy.synthesizer`

> Principles are derived from the research artifacts' documented behaviors. No validated Persona values exist yet.

### Principle 1: State Survives Disconnection

- **Statement:** Agent sessions must remain running and restorable across detach/reattach and server restarts.
- **Application:** Prefer Herdr's live persistence and native agent restore (`opencode --session <id>`) over ephemeral terminal sessions.
- **Trade-offs:** Requires running a persistent Herdr server and accepting pane-history storage risks.
- **Evidence Source:** setup-opencode-herdr-multi-agent.md, Section 6

### Principle 2: One Driver, Many Agents

- **Statement:** A single SPM profile should coordinate the whole team rather than each agent being driven independently.
- **Application:** Route all work through the Hermes SPM profile, which dispatches to the OpenRig factory team and observes OpenCode panes.
- **Trade-offs:** Creates a single point of coordination; requires the SPM profile to retain context across projects.
- **Evidence Source:** setup-opencode-herdr-openrig-factory-hermes-spm.md, Section 12

### Principle 3: Validate Before Scaling

- **Statement:** Prove the chain with the lighter `first-project` starter team before launching the full factory team.
- **Application:** Run the validation test run (Section 12.5 of the Hermes SPM research) with `first-project` before `factory`.
- **Trade-offs:** Delays full-team productivity but reduces partial-startup risk.
- **Evidence Source:** setup-opencode-herdr-openrig-factory-hermes-spm.md, Section 12.5

**Evidence Confidence:** 0.75
**Supporting Evidence:** setup-opencode-herdr-multi-agent.md; setup-opencode-herdr-openrig-factory-hermes-spm.md

## Strategic Objectives
**Managed by:** `agent.strategy.analytics`

> Objectives are hypothesized. No quantitative market sizing or viability data exists in the research.

### Objective 1: Reproducible Setup

- **Description:** Produce a documented, step-by-step setup that a developer can follow to stand up the SPM-driven factory pipeline.
- **Target Metric:** Setup guide completeness (all steps from research covered).
- **Baseline:** Three research documents exist; no consolidated setup guide yet.
- **Evidence Source:** setup-opencode-herdr-openrig-factory-hermes-spm.md, Sections 4-13
- **Owner:** agent.vision.orchestrator

### Objective 2: Validated Chain

- **Description:** Validate the Hermes SPM + OpenRig + Herdr + OpenCode chain end-to-end with the `first-project` starter team.
- **Target Metric:** All success criteria in Section 12.5.2 of the Hermes SPM research met.
- **Baseline:** Not yet run.
- **Evidence Source:** setup-opencode-herdr-openrig-factory-hermes-spm.md, Section 12.5.2
- **Owner:** agent.experiment.viability

### Objective 3: Persistent Sessions

- **Description:** Confirm detach/reattach and server-restart recovery preserve all agent state.
- **Target Metric:** Zero state loss across detach/reattach and native restore.
- **Baseline:** Not yet measured.
- **Evidence Source:** setup-opencode-herdr-multi-agent.md, Section 6
- **Owner:** agent.vision.evidence

**Evidence Confidence:** 0.50
**Supporting Evidence:** setup-opencode-herdr-openrig-factory-hermes-spm.md; setup-opencode-herdr-multi-agent.md

tbd:
  status: "awaiting-evidence"
  required_evidence_type: "quantitative"
  suggested_experiment: "objective-validation"
  minimum_confidence: 0.75
  blocking: true
  notes: "Quantitative baseline measurements and market sizing for adoption targets are not yet available."

## Alignment Metrics
**Managed by:** `agent.strategy.analytics`

> Alignment metrics are hypothesized; no baseline measurements exist yet.

### Vision-to-UXJ Alignment

| Metric | Baseline | Target | Measurement Source |
|--------|----------|--------|-------------------|
| Agent state transitions visible in Herdr sidebar | Not measured | 100% of panes | Herdr sidebar rollup |

### Vision-to-Epic Alignment

| Metric | Baseline | Target | Measurement Source |
|--------|----------|--------|-------------------|
| Factory team seats reaching ready state | Not measured | 7/7 | rig ps --nodes --rig factory |

### Strategic KPIs

| KPI | Baseline | Target | Frequency | Owner |
|-----|----------|--------|-----------|-------|
| Setup reproducibility (steps completed without error) | Not measured | 100% | Per setup run | agent.vision.evidence |
| State loss events across detach/reattach | Not measured | 0 | Per session | agent.vision.evidence |

**Evidence Confidence:** 0.40
**Supporting Evidence:** setup-opencode-herdr-multi-agent.md; setup-opencode-herdr-openrig-factory-hermes-spm.md

tbd:
  status: "awaiting-evidence"
  required_evidence_type: "quantitative"
  suggested_experiment: "metric-framework"
  minimum_confidence: 0.70
  blocking: false
  notes: "Baseline measurements for KPIs are not yet available."

## Linked Artifacts
**Managed by:** `agent.knowledge.graph`

### Originating Personas

- None yet. Persona artifacts are not present in this repository.

### User Experience Journeys

- None yet.

### Related Epics

- None yet.

### Related Features

- None yet.

### Related Visions

- None yet.

## Evidence Ledger
**Managed by:** `agent.vision.evidence`

### Evidence Summary

- **Total Evidence Sources:** 3
- **Validated Sections:** 2 (Guiding Principles, Vision Hypothesis at 0.75)
- **Total Sections:** 6
- **Overall Confidence:** 0.60
- **Last Evidence Update:** 2026-11-09

### Confidence by Section

| Section | Confidence | Last Updated | Primary Evidence | Blocking |
|---------|------------|--------------|------------------|----------|
| Strategic Context | 0.70 | 2026-11-09 | Research artifacts | Yes |
| Originating Vision Seeds | 0.57 | 2026-11-09 | Research artifacts | Yes |
| Vision Hypothesis | 0.75 | 2026-11-09 | Research artifacts | Yes |
| Guiding Principles | 0.75 | 2026-11-09 | Research artifacts | No |
| Strategic Objectives | 0.50 | 2026-11-09 | Research artifacts | Yes |
| Alignment Metrics | 0.40 | 2026-11-09 | Research artifacts | No |

### Detailed Evidence Log

| Date | Section | Source | Evidence Type | Confidence | Agent |
|------|---------|--------|---------------|------------|-------|
| 2026-11-09 | Strategic Context | setup-opencode-herdr-multi-agent.md | qualitative | 0.80 | agent.vision.orchestrator |
| 2026-11-09 | Strategic Context | setup-opencode-herdr-openrig-factory.md | qualitative | 0.80 | agent.vision.orchestrator |
| 2026-11-09 | Strategic Context | setup-opencode-herdr-openrig-factory-hermes-spm.md | qualitative | 0.80 | agent.vision.orchestrator |
| 2026-11-09 | Originating Vision Seeds | Research artifacts (hypothesized) | qualitative | 0.60 | agent.visionseed.strategy |
| 2026-11-09 | Vision Hypothesis | Research artifacts | qualitative | 0.75 | agent.vision.orchestrator |
| 2026-11-09 | Guiding Principles | Research artifacts | qualitative | 0.75 | agent.strategy.synthesizer |
| 2026-11-09 | Strategic Objectives | Research artifacts (hypothesized) | quantitative | 0.50 | agent.strategy.analytics |
| 2026-11-09 | Alignment Metrics | Research artifacts (hypothesized) | quantitative | 0.40 | agent.strategy.analytics |

## Readiness Lifecycle
**Managed by:** `agent.vision.lifecycle`

### Current State Assessment

- **Current State:** hypothesized
- **State Entry Date:** 2026-11-09
- **State Duration:** Initial
- **Next State Criteria:** Complete blocking sections (Strategic Objectives, Originating Vision Seeds) to validated confidence >= 0.75; validate with first-project test run.

### Lifecycle History

| State | Entry Date | Duration | Exit Reason | Decision Record |
|-------|------------|----------|-------------|-----------------|
| hypothesized | 2026-11-09 | Initial | Initial synthesis from research | N/A |

### Readiness Indicators

- **Evidence Completeness:** 67% (4 of 6 sections at or above 0.70)
- **Blocking Sections Complete:** No (Strategic Objectives and Originating Vision Seeds below 0.75)
- **Confidence Threshold:** 0.60 vs 0.75 required
- **Consent Circle Status:** Not yet convened

### Lifecycle State Governance

> **STOP**: Before changing any `status` or lifecycle state field, verify:
> 1. Are you `agent.vision.lifecycle`? If NO - DO NOT PROCEED
> 2. Is this a governed transition? If YES - Follow governance rules below

> **CRITICAL: STATE OWNERSHIP INVARIANT**
>
> `agent.vision.lifecycle` is the ONLY agent authorized to change the lifecycle
> state field of this artifact.

### Next Steps

| Action | Responsible | Target Date | Status |
|--------|-------------|-------------|--------|
| Validate Persona evidence for vision seeds | agent.visionseed.strategy | 2027-01-09 | Planned |
| Run first-project validation test run | agent.experiment.viability | 2027-01-09 | Planned |
| Measure baseline KPIs | agent.vision.evidence | 2027-02-09 | Planned |
| Convene consent circle for validated transition | agent.vision.lifecycle | 2027-03-09 | Planned |
