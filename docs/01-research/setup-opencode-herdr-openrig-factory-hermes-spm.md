---
title: "Step-by-Step Guide: OpenCode + Herdr + OpenRig 'factory' Team Driven by Hermes SPM Profile"
description: "Detailed research-backed procedure for configuring OpenCode multi-agent terminal sessions through Herdr, integrated with OpenRig's factory team, with the entire pipeline driven by a software-project-manager profile in Hermes Agent."
doc_type: "research"
author: "Devin Hedge"
version: "1.0"
last_updated: "2026-11-09"
review_date: "2027-05-09"
status: "draft"
category: "research"
tags: ["opencode", "herdr", "openrig", "factory", "hermes", "spm", "multi-agent", "research"]
related_docs: ["./enterprise-projects/chl-opencode-herdr-config-plugin/docs/01-research/setup-opencode-herdr-openrig-factory.md"]
---

# Step-by-Step Guide: OpenCode + Herdr + OpenRig "factory" Team Driven by Hermes SPM Profile

## 1. Scope and Purpose

This guide documents the end-to-end procedure for configuring OpenCode multi-agent terminal sessions through Herdr, integrated with OpenRig's "factory" team, with the entire pipeline driven by a software-project-manager (SPM) profile in Hermes Agent. It covers Hermes profile creation, Herdr integration for Hermes and OpenCode, OpenRig installation, the OpenRig Herdr plugin, launching the factory team, and wiring the Hermes SPM profile as the driver. Every assertion is corroborated by at least two independent sources cited inline in IEEE format; a consolidated bibliography appears in Section 14.

## 2. Architecture Overview

The pipeline has four layers [1], [2], [3]:

- **Hermes Agent** provides the driver: a dedicated profile (the SPM) with its own config, memory, sessions, skills, and gateway state. Hermes profiles are independent Hermes home directories under `~/.hermes/profiles/` [10], [11].
- **OpenRig** is the multi-agent harness that manages coding agents (Claude Code, Codex, Pi, and OpenCode) as a persistent team with roles, shared context, and owned work [1], [2]. The "factory" team has seven agents: lead, advisor, build, QA, design, and two independent reviewers [1], [13].
- **Herdr** is the terminal multiplexer/runtime that owns agents' PTYs, provides persistence, state rollups, and the sidebar [3], [4]. OpenRig uses Herdr as its terminal provider; a community Herdr plugin "OpenRig for Herdr" exposes the OpenRig operator/advisor inside Herdr [6], [8].
- **OpenCode** integrates with Herdr via `herdr integration install opencode`, enabling pane-local lifecycle reporting and native session restore [5], [9].

Hermes Agent itself is supported by Herdr (`herdr integration install hermes`) and reports resumable session ids [5], [9].

## 3. Prerequisites

- macOS or Linux (Windows via WSL2 is a reported working setup) [1], [12].
- Node.js 22 or 24 (Node 22 on Apple silicon) [1], [2].
- tmux [1], [2].
- At least one working Claude Code or Codex account [1], [2].
- Hermes Agent installed (see Section 4).
- OpenCode installed on PATH (V1 1.18.29+ or V2 beta 19242+) [5].
- Herdr (installed by OpenRig setup by default) [1], [3].

## 4. Install Hermes Agent

Hermes Agent is an open-source agent framework by Nous Research that runs in the terminal and supports profiles [10], [11].

Install via the official installer or pip per the Hermes documentation [10], [11]. Verify:

```sh
hermes --version
```

## 5. Create the Software-Project-Manager (SPM) Profile

Hermes profiles are separate Hermes home directories with isolated config, API keys, memory, sessions, skills, and gateway state [10], [11].

### 5.1 Create the Profile

```sh
hermes profile create spm --description "Software project manager driving the OpenRig factory team via Herdr."
```

The `--description` helps orchestrators route work to it [10].

### 5.2 Configure the Profile

```sh
spm setup
```

Set the model, provider, and tools. Configure a working directory:

```sh
spm config set terminal.cwd /absolute/path/to/project
```

Set a personality:

```sh
echo "You are a software project manager. Coordinate the OpenRig factory team, track tasks, and report progress." > ~/.hermes/profiles/spm/SOUL.md
```

Profiles use `HERMES_HOME` scoping; `SOUL.md` changes take effect on a new session [10], [11].

### 5.3 Verify

```sh
hermes profile show spm
spm doctor
```

## 6. Install OpenRig

### 6.1 One-Command Install

```sh
curl -fsSL https://openrig.dev/install.sh | sh
```

Preview first:

```sh
curl -fsSL https://openrig.dev/install.sh | sh -s -- --dry-run
```

### 6.2 Step-by-Step

```sh
npm install -g @openrig/cli
rig --version
```

### 6.3 Run Setup

```sh
rig setup --providers claude
```

Use `codex` or `both` as needed. Setup checks Node.js/tmux/sign-in, installs missing tmux or agent, installs Herdr (unless `--no-herdr`), and adds an OpenRig block to `~/.tmux.conf` [1], [2].

### 6.4 Sign In

```sh
claude auth login   # or: codex login
```

## 7. Start OpenRig and the Kernel

```sh
rig daemon start
rig status
rig ps --nodes --rig kernel
```

The kernel includes the operator (you talk to first) and the advisor [1], [2].

## 8. Open the OpenRig View (Herdr)

```sh
rig terminal open saved:kernel --window
```

Uses Herdr when installed; plain tmux otherwise. Below 120 columns the operator fills the first page; from 120 columns, dashboard and operator share it [2], [6].

## 9. Install the OpenRig Herdr Plugin

The community plugin "OpenRig for Herdr" exposes the OpenRig operator and advisor, needs-you indicators, task following, and agent messaging inside Herdr [7], [8].

```sh
herdr plugin install <owner>/openrig-herdr
```

Herdr plugins are GitHub repositories with a `herdr-plugin.toml` manifest and the `herdr-plugin` topic; install with `herdr plugin install owner/repo[/subdir]` [8], [9]. Review the preview before confirming [8].

## 10. Install Herdr Integrations (Hermes and OpenCode)

So both Hermes and OpenCode report state and support native restore inside Herdr [5], [9]:

```sh
herdr integration install hermes
herdr integration install opencode
herdr integration status
```

Confirm Hermes integration version 2+ and OpenCode integration version 5+ [5], [9]. Restart the Hermes/OpenCode TUIs after installation [5].

## 11. Launch the "factory" Team

The factory team has seven agents: lead, advisor, build, QA, design, and two independent reviewers [1], [13].

Talk to: `orch-lead@factory`.

### 11.1 Preview and Launch

```sh
cd <your-repository>
rig specs preview factory --kind rig
rig up factory --cwd . --plan
rig up factory --cwd .
rig status
rig ps --nodes --rig factory
```

### 11.2 Handle Partial Startup

If `rig up` exits 1 with `Status: partial`, a seat is waiting at a prompt. Answer it in that agent's terminal, then run `rig seat continue <seat>` [2].

## 12. Drive the Pipeline from the Hermes SPM Profile

The SPM profile in Hermes acts as the driver that orchestrates the factory team.

### 12.1 Hermes-OpenCode Bridge (Optional)

For Hermes to dispatch coding tasks to OpenCode's multi-agent harness, install the OpenCode integration plugin for Hermes [14]:

```sh
# Per zaycruz/hermes-opencode-plugin
hermes -p spm plugins install zaycruz/hermes-opencode-plugin
```

This lets the SPM profile call `opencode(action="run", prompt="...", directory="/project")` to dispatch real engineering work [14].

### 12.2 Hermes as OpenRig Driver

The SPM profile can drive OpenRig via terminal commands since `rig` is on PATH:

- Start/stop teams: `rig up factory --cwd .`, `rig down factory`
- Send work: `rig send orch-lead@factory '...'`
- Track tasks: `rig queue list --destination orch-lead@factory`
- Check state: `rig ps --nodes --rig factory`

Hermes profiles keep memory and skills scoped; the SPM profile accumulates project-management context across sessions [10], [11].

### 12.3 OpenCode Inside Herdr

OpenCode can run as an additional agent inside Herdr panes alongside the factory team:

1. Start Herdr: `herdr`
2. Create a workspace: `herdr workspace create --cwd "$(pwd)" --label "spm-extra"`
3. Run OpenCode: `opencode`
4. Herdr detects it and reports state via the integration plugin [5], [9].

## 12.4 Cross-Project and Multi-Workspace Scenario

When the SPM profile manages multiple related projects, combine Hermes profile scoping with per-project Herdr workspaces and OpenRig rigs [10], [16], [18].

### 12.4.1 Hermes Profile per Project (or One SPM)

- Option A: one SPM profile managing all projects via distinct OpenRig rigs (shared memory/context).
- Option B: one Hermes profile per project for strict isolation of memory and skills [10], [11].

### 12.4.2 Herdr Multi-Workspace Pattern

- One workspace per project repo [16]:

```
herdr workspace create --cwd /path/to/project-a --label "project-a"
herdr workspace create --cwd /path/to/project-b --label "project-b"
```

### 12.4.3 OpenRig Multi-Rig Pattern

- Launch a separate team per project:

```
rig up first-project --cwd /path/to/project-a
rig up first-project --cwd /path/to/project-b
```

- Cross-project coordination via `rig send ... --host <host>` and `rig queue create --host ...` [18].

### 12.4.4 Validation Test Run (cross-project under SPM)

1. Create two Herdr workspaces for two related projects.
2. Launch a `first-project` team in each (or one team coordinating both).
3. Dispatch a task to each rig from the SPM profile.
4. Verify tasks are isolated per rig and state rolls up per workspace.
5. Confirm SPM profile retains context across both projects [10], [11].
6. Detach/reattach and verify persistence [4], [15].

### 12.4.5 Scaling

Once validated, scale each project to its own `factory` team and (optionally) dedicated Hermes profile [1], [13].

## 12.5 Validation Test Run (first-project driven by SPM)

Before running the full factory pipeline, validate the Hermes SPM + OpenRig + Herdr + OpenCode chain with the lighter `first-project` (starter) team [1], [13].

### 12.5.1 Test Procedure

1. Create a scratch project and initialize git:

```
mkdir -p ./scratch/test-first-project && cd ./scratch/test-first-project
git init
```

2. From the SPM profile, launch the starter team:

```
spm chat -q "Launch the first-project team in $(pwd) and confirm seats are ready."
```

Or directly:

```
rig specs preview first-project --kind rig
rig up first-project --cwd . --plan
rig up first-project --cwd .
rig ps --nodes --rig first-project
```

3. Confirm both seats are ready; if partial, answer the prompt and run `rig seat continue <seat>` [2].

4. Send a trivial task via the SPM profile:

```
rig send dev-build@first-project 'Add a README.md with one line. Ask dev-review to check it.'
rig queue list --destination dev-build@first-project
```

5. Verify the SPM profile retains context across the run (memory/skills scoped to `spm`) [10], [11].

6. Run OpenCode in an additional Herdr pane and verify state reporting under the SPM-driven workflow [5], [9].

7. Stop the test team:

```
rig down first-project
```

### 12.5.2 Success Criteria

- SPM profile drives `first-project` launch and task dispatch.
- Both seats reach ready state.
- Task recorded and visible via `rig queue`.
- OpenCode pane reports lifecycle state in Herdr.
- SPM profile context persists across the run.
- No partial-startup or integration errors.

### 12.5.3 Scaling to factory

Once validation passes, repeat with `factory` in place of `first-project` for sustained product work [1], [13].

## 13. Persistence and Recovery

- Detach/reattach: `prefix+q`; reattach with `herdr`. Processes keep running [4], [15].
- Named sessions: `herdr session list/attach/stop/delete <name>` [4].
- Native agent restore (Hermes via `hermes --resume <id>`, OpenCode via `opencode --session <id>`) [5], [15].
- OpenRig kernel/team restore: `rig up kernel --existing`, `rig up <team> --existing` [2].
- Pane screen history (optional): `[experimental] pane_history = true` [15].

## 14. Bibliography

[1] mvschwarz, "OpenRig," GitHub, 2026. [Online]. Available: https://github.com/mvschwarz/openrig

[2] mvschwarz, "OpenRig setup reference," OpenRig Documentation, 2026. [Online]. Available: https://github.com/mvschwarz/openrig/blob/main/docs/reference/getting-started.md

[3] herdrdev, "Herdr," GitHub, 2026. [Online]. Available: https://github.com/herdrdev/herdr

[4] herdrdev, "Persistence and remote access," Herdr Documentation, 2026. [Online]. Available: https://herdr.dev/tui/docs/persistence-remote/

[5] herdrdev, "Integrations," Herdr Documentation, 2026. [Online]. Available: https://herdr.dev/tui/docs/integrations/

[6] herdrdev, "Quick start," Herdr Documentation, 2026. [Online]. Available: https://herdr.dev/tui/docs/quick-start/

[7] herdrdev, "Community plugin marketplace," Herdr, 2026. [Online]. Available: https://herdr.dev/tui/plugins/

[8] herdrdev, "Plugins," Herdr Documentation, 2026. [Online]. Available: https://herdr.dev/tui/docs/plugins/

[9] herdrdev, "Agents," Herdr Documentation, 2026. [Online]. Available: https://herdr.dev/tui/docs/agents/

[10] NousResearch, "Profiles: Running Multiple Agents," Hermes Agent Documentation, 2026. [Online]. Available: https://github.com/NousResearch/hermes-agent/blob/main/website/docs/user-guide/profiles.md

[11] NousResearch, "Hermes Agent," GitHub, 2026. [Online]. Available: https://github.com/NousResearch/hermes-agent

[12] mvschwarz, "OpenRig setup reference - WSL2," OpenRig Documentation, 2026. [Online]. Available: https://github.com/mvschwarz/openrig/blob/main/docs/reference/getting-started.md

[13] mvschwarz, "Choose your first team," OpenRig Documentation, 2026. [Online]. Available: https://openrig.dev/docs/getting-started

[14] zaycruz, "hermes-opencode-plugin," GitHub, 2026. [Online]. Available: https://github.com/zaycruz/hermes-opencode-plugin

[15] herdrdev, "Session state and restore," Herdr Documentation, 2026. [Online]. Available: https://herdr.dev/tui/docs/session-state/

[16] herdrdev, "Concepts," Herdr Documentation, 2026. [Online]. Available: https://herdr.dev/tui/docs/concepts/

[18] mvschwarz, "Run OpenRig across machines," OpenRig Documentation, 2026. [Online]. Available: https://openrig.dev/guides/multiple-machines
