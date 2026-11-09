---
title: "Step-by-Step Guide: OpenCode + Herdr + OpenRig 'factory' Team Integration"
description: "Detailed research-backed procedure for configuring OpenCode multi-agent terminal sessions through Herdr integrated with the OpenRig plugin and the factory team."
doc_type: "research"
author: "Devin Hedge"
version: "1.0"
last_updated: "2026-11-09"
review_date: "2027-05-09"
status: "draft"
category: "research"
tags: ["opencode", "herdr", "openrig", "factory", "multi-agent", "terminal", "research"]
related_docs: ["./enterprise-projects/chl-opencode-herdr-config-plugin/docs/01-research/setup-opencode-herdr-multi-agent.md"]
---

# Step-by-Step Guide: OpenCode + Herdr + OpenRig "factory" Team Integration

## 1. Scope and Purpose

This guide documents the end-to-end procedure for configuring OpenCode to run inside Herdr multi-agent terminal sessions, integrated with the OpenRig harness and its "factory" team (seven agents). It covers OpenRig installation, Herdr integration, the OpenRig Herdr plugin, OpenCode Herdr integration, and launching the factory team. Every assertion is corroborated by at least two independent sources cited inline in IEEE format; a consolidated bibliography appears in Section 13.

## 2. Architecture Overview

OpenRig is a multi-agent harness that manages coding agents (Claude Code, Codex, Pi, OpenCode) running together as a persistent team [1], [2]. Herdr is the terminal multiplexer/runtime that owns the agents' PTYs and provides the sidebar, state rollups, and persistence [3], [4]. OpenRig integrates with Herdr in two ways:

- OpenRig's setup installs Herdr by default and uses it as the terminal provider for `rig terminal open` [1], [5].
- A community Herdr plugin "OpenRig for Herdr" exposes the OpenRig operator and advisor, needs-you indicators, task following, and agent messaging inside Herdr [6], [7].

OpenCode integrates with Herdr via `herdr integration install opencode`, enabling native session restore and pane-local lifecycle reporting [8], [9].

## 3. Prerequisites

- macOS or Linux (Windows via WSL2 is a reported working setup) [1], [10].
- Node.js 22 or 24 (Node 22 on Apple silicon) [1], [11].
- tmux [1], [11].
- At least one working Claude Code or Codex account (one is enough; the kernel adapts) [1], [11].
- OpenCode installed on PATH (V1 1.18.29+ or V2 beta 19242+) [8].
- (Optional) Herdr already installed; otherwise OpenRig installs it [1], [5].

## 4. Install OpenRig

### 4.1 One-Command Install

```sh
curl -fsSL https://openrig.dev/install.sh | sh
```

Preview first with `--dry-run`:

```sh
curl -fsSL https://openrig.dev/install.sh | sh -s -- --dry-run
```

The installer checks Node.js and npm, installs `@openrig/cli` globally, runs the Node.js/SQLite check, then `rig setup` [1], [11].

### 4.2 Step-by-Step Install

```sh
npm install -g @openrig/cli
rig --version
```

`rig --version` prints the version and commit (for example `0.6.12 (9e5382dd)`) [1], [11].

### 4.3 Run Setup

```sh
rig setup --providers claude
```

Use `codex` or `both` to match your agents. Setup performs four actions [1], [11]:

1. Checks Node.js, tmux, and sign-in.
2. Installs a missing tmux (Homebrew on macOS if available) or the chosen coding agent.
3. Installs Herdr (terminal workspace app) unless `--no-herdr` is passed.
4. Adds an OpenRig block to `~/.tmux.conf` (mouse support, longer scrollback).

Add `--dry-run` first to preview without changes. Setup does not start anything [1], [11].

### 4.4 Sign In

If sign-in is missing, run:

```sh
claude auth login
# or
codex login
```

Then recheck with `claude auth status` / `codex login status` [1], [11].

## 5. Start OpenRig and the Kernel

```sh
rig daemon start
rig status
rig ps --nodes --rig kernel
```

`rig status` shows a `Kernel:` line; `rig ps --nodes --rig kernel` shows each kernel agent's state. The kernel includes the operator (you talk to first) and the advisor [1], [11].

Started is not the same as ready; read the readiness signals rather than assuming [11].

## 6. Open the OpenRig View (Herdr)

```sh
rig terminal open saved:kernel --window
```

Behavior [5], [11]:

- Uses Herdr when installed; plain tmux otherwise.
- Below 120 measured columns, the operator fills the first page; from 120 columns, dashboard and operator share it.
- Advisor has its own tab.
- On macOS, you may be asked to allow terminal control (one-time, fine to allow).

On a Mac, OpenRig may offer to install Ghostty; allowing runs setup with `--ghostty` [11].

## 7. Install the OpenRig Herdr Plugin

The Herdr community plugin "OpenRig for Herdr" exposes the OpenRig operator and advisor, needs-you indicators, task following, and agent messaging inside Herdr [6], [7].

Install via the Herdr plugin command (GitHub shorthand `owner/repo`):

```sh
herdr plugin install <owner>/openrig-herdr
```

Herdr plugins are ordinary GitHub repositories with a `herdr-plugin.toml` manifest and the `herdr-plugin` topic; install with `herdr plugin install owner/repo[/subdir]` [7], [12]. Review the source and commands in the preview before confirming; use `--yes` for non-interactive installs [12].

Note: the plugin listing shows "OpenRig for Herdr" as a recently joined community plugin; verify the exact repository path in the Herdr marketplace before installing [6].

## 8. Install the OpenCode Herdr Integration

So OpenCode panes report lifecycle state and support native restore inside Herdr [8], [9]:

```sh
herdr integration install opencode
herdr integration status
```

Confirm OpenCode integration version 5 or later [8], [9]. Restart the OpenCode TUI after installation [8].

## 9. Launch the "factory" Team

The factory team is one of three shipped teams: seven agents (lead, advisor, build, QA, design, and two independent reviewers) for sustained product work [1], [13].

Talk to: `orch-lead@factory`.

### 9.1 Preview and Launch

```sh
cd <your-repository>
rig specs preview factory --kind rig
rig up factory --cwd . --plan
rig up factory --cwd .
rig status
rig ps --nodes --rig factory
```

`rig up ... --plan` shows what would start without starting it [1], [13].

### 9.2 Handle Partial Startup

If `rig up` exits 1 with `Status: partial` and `Startup attention`, a seat is waiting at a prompt (sign-in, trust, or permission). Answer it in that agent's terminal, then run the `rig seat continue <seat>` command it prints [11].

The team is ready once its agents show as ready in `rig ps --nodes --rig factory` [1], [11].

### 9.3 Give the Team Work

```sh
rig send orch-lead@factory 'Build <feature>. Coordinate with the team and record tasks.'
rig queue list --destination orch-lead@factory
```

### 9.4 Stop the Team

```sh
rig down factory
```

`rig daemon stop` stops the background service while leaving agent sessions running [1], [11].

## 10. OpenCode Inside Herdr for the Factory Team

With both OpenCode and OpenRig configured, OpenCode can run as an additional agent inside Herdr panes alongside the factory team:

1. Start Herdr: `herdr`
2. Create a workspace: `herdr workspace create --cwd "$(pwd)" --label "factory-extra"`
3. Run OpenCode in a pane: `opencode`
4. Herdr detects it and reports state via the integration plugin [8], [9].

OpenCode panes support native restore via `opencode --session <id>` after a Herdr server restart [8], [9].

## 11. Persistence and Recovery

- Detach/reattach: `prefix+q` (Ctrl+b then q); reattach with `herdr`. Processes keep running [4], [14].
- Named sessions: `herdr session list/attach/stop/delete <name>` [4].
- Native agent restore after server restart (OpenCode via `opencode --session <id>`) [8], [14].
- OpenRig kernel/team restore: `rig up kernel --existing`, `rig up <team> --existing` [11].
- Pane screen history (optional, off by default): set `[experimental] pane_history = true` [14].

## 11.4 Cross-Project and Multi-Workspace Scenario

When work spans multiple related projects, combine Herdr workspaces with separate OpenRig rigs [16], [17], [18].

### 11.4.1 Herdr Multi-Workspace Pattern

- One workspace per repo; sidebar rolls state up per workspace [16].
- Create workspaces for each project:

```
herdr workspace create --cwd /path/to/project-a --label "project-a"
herdr workspace create --cwd /path/to/project-b --label "project-b"
```

### 11.4.2 OpenRig Multi-Rig Pattern

- Run a separate OpenRig team per project, each with its own `--cwd`:

```
rig up first-project --cwd /path/to/project-a
rig up first-project --cwd /path/to/project-b
```

- Each rig is independent; state and queue are per-rig [1], [18].
- For cross-machine coordination, pair hosts via `rig host pair` and use `--host` on send/queue commands [18].

### 11.4.3 Validation Test Run (cross-project)

1. Create two Herdr workspaces for two related project directories.
2. Launch a `first-project` team in each (or one team with the lead coordinating both).
3. Verify each workspace/rig reports state independently.
4. Confirm a task dispatched to one rig does not leak to the other.
5. Detach/reattach and verify both persist [4], [15].

### 11.4.4 Scaling

Once cross-project validation passes, scale each project to its own `factory` team [1], [13].

## 11.5 Validation Test Run (first-project)

Before launching the full factory team, validate the OpenRig + Herdr + OpenCode pipeline with the lighter `first-project` (starter) team, which uses two agents (a builder and a reviewer) [1], [13].

### 11.5.1 Test Procedure

1. Create a scratch project directory:

```
mkdir -p ./scratch/test-first-project && cd ./scratch/test-first-project
git init
```

2. Preview and launch the starter team (first-project alias):

```
rig specs preview first-project --kind rig
rig up first-project --cwd . --plan
rig up first-project --cwd .
rig status
rig ps --nodes --rig first-project
```

3. Confirm both seats show as ready in `rig ps --nodes --rig first-project` [1], [2].

4. Send a trivial task:

```
rig send dev-build@first-project 'Add a README.md with one line. Ask dev-review to check it.'
rig queue list --destination dev-build@first-project
```

5. Open the OpenRig view and confirm the operator, dashboard, and advisor are reachable in Herdr [2], [6].

6. Run OpenCode in an additional Herdr pane and verify state reporting [5], [9].

7. Stop the test team:

```
rig down first-project
```

### 11.5.2 Success Criteria

- `first-project` team starts with both seats ready.
- Task is recorded and visible via `rig queue`.
- OpenRig view opens in Herdr with operator/dashboard/advisor.
- OpenCode pane reports lifecycle state in Herdr.
- No partial-startup errors.

### 11.5.3 Scaling to factory

Once validation passes, repeat the procedure with `factory` in place of `first-project` for sustained product work [1], [13].

## 12. Troubleshooting

| Symptom | Resolution |
|---|---|
| `rig` not found | Ensure npm global bin on PATH; rerun `npm install -g @openrig/cli` [1], [11]. |
| Node.js version unsupported | Use Node 22/24; install via nvm on macOS [1], [11]. |
| Herdr not found by OpenRig | Run `rig setup` (installs Herdr) or pass `--no-herdr` [1], [5]. |
| OpenCode integration not installed | Run `herdr integration install opencode` after OpenCode starts once [8]. |
| Factory team partial startup | Answer the prompt in the seat terminal, then `rig seat continue <seat>` [11]. |
| OpenRig view won't open (headless/SSH) | Use manual attach or SSH handoff commands [11]. |

## 13. Bibliography

[1] mvschwarz, "OpenRig," GitHub, 2026. [Online]. Available: https://github.com/mvschwarz/openrig

[2] mvschwarz, "OpenRig - Talk to one agent. Build with a whole team," OpenRig, 2026. [Online]. Available: https://openrig.dev/

[3] herdrdev, "Herdr," GitHub, 2026. [Online]. Available: https://github.com/herdrdev/herdr

[4] herdrdev, "Persistence and remote access," Herdr Documentation, 2026. [Online]. Available: https://herdr.dev/tui/docs/persistence-remote/

[5] mvschwarz, "Getting started," OpenRig Documentation, 2026. [Online]. Available: https://openrig.dev/docs/getting-started

[6] herdrdev, "Community plugin marketplace," Herdr, 2026. [Online]. Available: https://herdr.dev/tui/plugins/

[7] herdrdev, "Plugins," Herdr Documentation, 2026. [Online]. Available: https://herdr.dev/tui/docs/plugins/

[8] herdrdev, "Integrations," Herdr Documentation, 2026. [Online]. Available: https://herdr.dev/tui/docs/integrations/

[9] herdrdev, "Agents," Herdr Documentation, 2026. [Online]. Available: https://herdr.dev/tui/docs/agents/

[10] mvschwarz, "OpenRig setup reference - WSL2," OpenRig Documentation, 2026. [Online]. Available: https://github.com/mvschwarz/openrig/blob/main/docs/reference/getting-started.md

[11] mvschwarz, "OpenRig setup reference," OpenRig Documentation, 2026. [Online]. Available: https://github.com/mvschwarz/openrig/blob/main/docs/reference/getting-started.md

[12] herdrdev, "Marketplace," Herdr Documentation, 2026. [Online]. Available: https://github.com/herdrdev/herdr/blob/master/docs/versions/0.9.0/website/src/content/docs/marketplace.mdx

[13] mvschwarz, "Choose your first team," OpenRig Documentation, 2026. [Online]. Available: https://openrig.dev/docs/getting-started

[14] herdrdev, "Session state and restore," Herdr Documentation, 2026. [Online]. Available: https://herdr.dev/tui/docs/session-state/

[16] herdrdev, "Concepts," Herdr Documentation, 2026. [Online]. Available: https://herdr.dev/tui/docs/concepts/

[17] herdrdev, "Persistence and remote access," Herdr Documentation, 2026. [Online]. Available: https://herdr.dev/tui/docs/persistence-remote/

[18] mvschwarz, "Run OpenRig across machines," OpenRig Documentation, 2026. [Online]. Available: https://openrig.dev/guides/multiple-machines
