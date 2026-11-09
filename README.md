---
title: "chl-opencode-herdr-config-plugin"
description: "Documentation and configuration for running OpenCode inside Herdr multi-agent terminal sessions, integrated with the OpenRig factory team and driven by a Hermes software-project-manager profile."
doc_type: "project-root-readme"
doc_type_standard: "standards/README_STANDARD.md"
author: "Devin Hedge"
version: 1.0
last_updated: "2026-11-09"
status: draft
category: integration
audience:
  - "new-users"
  - "stakeholders"
  - "external-developers"
  - "ai-agents"
purpose: "project-overview-and-onboarding"
tags:
  - "opencode"
  - "herdr"
  - "openrig"
  - "hermes"
  - "directory"
release: "0.1.0"
priority: "high"
related_docs:
  - "./docs/01-research/setup-opencode-herdr-multi-agent.md"
  - "./docs/01-research/setup-opencode-herdr-openrig-factory.md"
  - "./docs/01-research/setup-opencode-herdr-openrig-factory-hermes-spm.md"
  - "./docs/03-vision/VISION.md"
dependencies: []
review_date: "2027-05-09"
---

# chl-opencode-herdr-config-plugin

Documentation and configuration for running OpenCode inside Herdr multi-agent terminal sessions, integrated with the OpenRig "factory" team and driven by a Hermes software-project-manager (SPM) profile.

## Overview

This project captures the research, vision, and setup guidance for a durable multi-agent engineering pipeline. It combines four layers:

- **Hermes Agent** provides the driver via a scoped SPM profile with isolated config, memory, sessions, skills, and gateway state.
- **OpenRig** manages a persistent seven-agent "factory" team (lead, advisor, build, QA, design, and two independent reviewers).
- **Herdr** owns the agents' PTYs, providing persistence, state rollups, and the sidebar across detach/reattach.
- **OpenCode** runs as an additional observable agent inside Herdr panes with native session restore.

## Getting Started

### Prerequisites

- macOS or Linux (Windows via WSL2 reported working)
- Node.js 22 or 24
- tmux
- OpenCode installed on PATH (V1 1.18.29+ or V2 beta 19242+)
- At least one working Claude Code or Codex account

### Quick Setup

1. Read the research guides under `docs/01-research/`.
2. Start with `setup-opencode-herdr-multi-agent.md` to install Herdr and integrate OpenCode.
3. Add OpenRig and the factory team via `setup-opencode-herdr-openrig-factory.md`.
4. Wire the Hermes SPM driver via `setup-opencode-herdr-openrig-factory-hermes-spm.md`.

### Initial Configuration

```bash
# Install Herdr
curl -fsSL https://herdr.dev/install.sh | sh

# Install OpenCode integration
herdr integration install opencode

# Install OpenRig
curl -fsSL https://openrig.dev/install.sh | sh
rig setup --providers claude
```

## Key Concepts

### Core Terminology

- **Herdr**: Terminal multiplexer/runtime that owns agent PTYs and provides persistence and state rollups.
- **OpenRig**: Multi-agent harness managing coding agents as a persistent team with roles and shared context.
- **Factory team**: OpenRig's seven-agent team for sustained product work, addressed via `orch-lead@factory`.
- **Hermes SPM profile**: A scoped Hermes Agent profile acting as the driver for the whole pipeline.
- **OpenCode integration**: Herdr plugin enabling pane-local lifecycle reporting and native session restore.

### Architecture Overview

The pipeline flows Hermes SPM (driver) -> OpenRig factory team (agents) -> Herdr (runtime/persistence) -> OpenCode panes (additional agent), with state surviving detach/reattach and server restarts.

## Quick Start

### For New Users

1. Read `docs/01-research/setup-opencode-herdr-multi-agent.md`.
2. Validate the Herdr + OpenCode integration with the `first-project` starter team.
3. Scale to the `factory` team once validation passes.

### For Experienced Users

- Launch the factory team: `rig up factory --cwd .`
- Drive work from the SPM profile: `rig send orch-lead@factory '...'`
- Open the OpenRig view in Herdr: `rig terminal open saved:kernel --window`

### Common Tasks

- **Install integrations**: `herdr integration install hermes` and `herdr integration install opencode`
- **Launch factory team**: `rig up factory --cwd .`
- **Stop factory team**: `rig down factory`

## Related Documentation

- **[Research: Herdr + OpenCode Multi-Agent](docs/01-research/setup-opencode-herdr-multi-agent.md)**: Herdr installation and OpenCode integration.
- **[Research: OpenRig Factory Team](docs/01-research/setup-opencode-herdr-openrig-factory.md)**: OpenRig installation and factory team launch.
- **[Research: Hermes SPM Driver](docs/01-research/setup-opencode-herdr-openrig-factory-hermes-spm.md)**: Hermes SPM profile driving the pipeline.
- **[Vision](docs/03-vision/VISION.md)**: Strategic vision for the SPM-driven factory pipeline.

## Support and Troubleshooting

### Common Issues

- **OpenCode integration not installed**: Run `herdr integration install opencode` after OpenCode has started once.
- **Factory team partial startup**: Answer the prompt in the seat terminal, then run `rig seat continue <seat>`.
- **Session dies on disconnect**: Use Herdr; it ignores SIGHUP and preserves PTYs.

### Getting Help

- Consult the research guides under `docs/01-research/`.
- Check `herdr integration status` and `rig status` for runtime state.
- Review Herdr logs with `herdr log follow`.
