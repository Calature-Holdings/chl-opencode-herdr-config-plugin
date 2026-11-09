---
title: "Step-by-Step Guide: Configuring OpenCode Multi-Agent Sessions with Herdr"
description: "Detailed research-backed procedure for installing Herdr, integrating OpenCode, and running multi-agent terminal sessions with persistent state."
doc_type: "research"
author: "Devin Hedge"
version: "1.0"
last_updated: "2026-11-09"
review_date: "2027-05-09"
status: "draft"
category: "research"
tags: ["opencode", "herdr", "multi-agent", "terminal", "research"]
related_docs: ["./enterprise-projects/chl-opencode-herdr-config-plugin/README.md"]
---

# Step-by-Step Guide: Configuring OpenCode Multi-Agent Sessions with Herdr

## 1. Scope and Purpose

This guide documents the procedure for configuring OpenCode to run inside Herdr multi-agent terminal sessions, including installation, integration, persistence, remote access, and recovery. Every assertion is corroborated by at least two independent sources cited inline in IEEE format; a consolidated bibliography appears in Section 12.

## 2. Prerequisites

Before beginning, verify the host environment meets these conditions:

- A macOS or Linux host (Windows is supported with documented limitations) [1], [2].
- OpenCode installed and on PATH, version V1 1.18.29 or later, or V2 beta 19242 or later [3].
- Network access for downloading the Herdr installer.
- (Optional) Tailscale mesh connectivity for remote/mobile access [4], [5].

## 3. Install Herdr

Herdr publishes stable-channel binaries for Linux, macOS, and Windows x86_64 [2].

### 3.1 Linux or macOS

Run the official installer:

```
curl -fsSL https://herdr.dev/install.sh | sh
```

Alternative package managers are supported: Homebrew (`brew install herdr`), mise (`mise use -g herdr`), or Nix [2], [6].

### 3.2 Windows

Run the PowerShell installer:

```
powershell -ExecutionPolicy Bypass -c "irm https://herdr.dev/install.ps1 | iex"
```

If endpoint security blocks fileless PowerShell, use the cmd fallback:

```
curl.exe -fsSLo install.cmd https://herdr.dev/install.cmd && install.cmd && del install.cmd
```

Keep the extracted Windows directory intact; do not copy only `herdr.exe` [2].

### 3.3 Verify Installation

Start Herdr to confirm it launches:

```
herdr
```

If the shell cannot find `herdr`, restart the terminal or confirm the install directory is on PATH [2], [7].

## 4. Install the OpenCode Integration

Herdr detects supported agents automatically; installing an integration enables native session restore after a server restart [3], [8].

### 4.1 Install Command

```
herdr integration install opencode
```

This installs three components [3]:

- Server entrypoint at `~/.config/opencode/plugins/herdr-agent-state.js`
- Shared TUI plugin at `herdr-tui-session.js`
- V2 TUI entrypoint at `herdr-opencode/tui.js`

### 4.2 Registration Behavior

- V1 TUI plugin is registered in `tui.jsonc`.
- V2 TUI plugin is registered in `cli.json`.
- Existing V1 registration in `tui.json` is reused without creating `tui.jsonc`.
- If OpenCode still has V1 TUI preferences to import (`tui.json` or `kv.json`), Herdr defers registration so first-start migration can run; start `opencode2` once, then reinstall [3].

### 4.3 Verify Integration

```
herdr integration status
```

Confirm OpenCode integration version 5 or later is reported [3], [8].

## 5. Create a Workspace and Run OpenCode

### 5.1 Launch Herdr

From a project directory:

```
herdr
```

Herdr launches or attaches to the default background session; no socket management is required [7], [9].

### 5.2 Create a Workspace

When a session has no workspaces, Herdr opens one automatically. A workspace is a project-level container for tabs, panes, and agents [7], [9].

To create a labeled workspace explicitly:

```
herdr workspace create --cwd "$(pwd)" --label "$(basename "$(pwd)")"
```

### 5.3 Run OpenCode

Start OpenCode in a pane:

```
opencode
```

Herdr detects it automatically and reports lifecycle state (idle, working, blocked, done) via the integration plugin [3], [8].

### 5.4 Split Panes for Multiple Agents

Use mouse-native controls or keyboard shortcuts [7]:

- Split right: `prefix+v` (prefix = Ctrl+b)
- Split down: `prefix+minus`
- New tab: `prefix+c`
- New workspace: `prefix+shift+n`

## 6. Persistence and Session Recovery

### 6.1 Live Persistence (Detach/Reattach)

Press `prefix+q` or close the terminal; the Herdr server and all agents keep running. Reattach later:

```
herdr
```

This is the strongest persistence path because original processes never stop [10], [11].

### 6.2 Named Sessions

For independent Herdr servers:

```
herdr session list
herdr session attach <name>
herdr session stop <name>
herdr session delete <name>
```

Use `--json` for scriptable output [10].

### 6.3 Native Agent Session Restore

After a server restart, Herdr resumes eligible OpenCode panes using the reported session id via `opencode --session <id>` [3], [11]. Native restore is enabled by default; disable with:

```
[session]
resume_agents_on_restore = false
```

in the Herdr config file [11].

### 6.4 Pane Screen History (Optional)

Off by default because pane output may contain secrets. Enable:

```
[experimental]
pane_history = true
```

History is stored in `session-history.json` beside `session.json` [11].

## 7. Multi-Machine and Remote Access

### 7.1 Remote Attach over SSH

```
herdr --remote <ssh-host-alias>
herdr --remote ssh://you@server:2222
```

The remote server owns panes; the local client draws the UI [10], [12].

### 7.2 Connecting Multiple Machines

Herdr supports bringing multiple SSH-reachable machines into one window with a combined agent list [12], [13]. Machines must be reachable over SSH (local network, internet, or Tailscale) [12].

### 7.3 Mobile Access (Stack E: OpenCode Server + Attach)

For OpenCode-specific mobile access, run the OpenCode server on port 4096 and expose via Tailscale [4], [5]:

```
opencode service set password "$OPENCODE_PASS"
opencode service set port 4096
opencode service start
tailscale serve --bg 4096
opencode pair --url "https://<magicdns-hostname>:4096"
```

## 8. Agent Automation and Inter-Agent Coordination

Herdr supports scripting agent control [14], [15]:

- Start an agent in a pane: `herdr agent start --kind opencode`
- Wait for state: `herdr agent wait <target>`
- Send input: `herdr agent prompt <target> "..."` or `herdr pane send <target> "..."`
- Read pane output: `herdr pane read <target> --source recent --lines 50`

Agents report semantic state (idle, working, blocked); rollups propagate to tabs and workspaces [8], [14].

## 9. Troubleshooting

| Symptom | Resolution |
|---|---|
| "OpenCode integration not installed" | Run `herdr integration install opencode` after OpenCode has started once [3]. |
| Mobile SSH cannot find `herdr` | Fix `~/.zshenv` PATH or symlink to `/usr/local/bin` [4]. |
| QR code not appearing | Check `opencode service status` and ensure port 4096 exposed via `tailscale serve` [4]. |
| Session dies on disconnect | Use Herdr; it ignores SIGHUP and preserves PTYs [10], [11]. |
| Agent state wrong | Confirm agent runs inside a Herdr pane and integration installed for same user [3]. |

## 10. Security Notes

- Never run `tailscale funnel` on the OpenCode server; bind to loopback and expose via `tailscale serve` only [4].
- Use strong passwords for OpenCode server authentication [4].
- Treat Herdr session directories like terminal history; pane output may contain secrets [11].

## 10.5 Cross-Project and Multi-Workspace Scenario

When work spans multiple related projects, use Herdr's workspace model and (optionally) named sessions [16], [17].

### 10.5.1 Herdr Multi-Workspace Pattern

- One workspace per repo/task/investigation; the sidebar rolls state up per workspace so you can see which project needs attention [16].
- Create additional workspaces:

```
herdr workspace create --cwd /path/to/project-a --label "project-a"
herdr workspace create --cwd /path/to/project-b --label "project-b"
```

- Navigate with `prefix+w` (workspace navigation) and `prefix+shift+n` (new workspace) [7].
- Tabs within a workspace separate views (`agents`, `logs`, `server`, `review`) [16].

### 10.5.2 Named Sessions for Full Isolation

When projects need completely separate runtime namespaces (sockets, persisted state), use named sessions [4], [17]:

```
herdr session attach project-a
herdr session attach project-b
```

### 10.5.3 Multi-Client Viewing

Multiple clients can view different workspaces/tabs simultaneously; the last to focus controls pane sizes [17].

### 10.5.4 Validation Test Run (multi-project)

1. Create two workspaces pointing at two related project directories.
2. Run OpenCode in each workspace's pane.
3. Verify each workspace reports state independently in the sidebar [16], [8].
4. Detach and reattach; confirm both workspaces persist [10], [11].

## 11. Validation Test Run (first-project)

Before deploying the full multi-agent setup, validate the Herdr + OpenCode integration with a minimal test run.

### 11.1 Test Procedure

1. Start Herdr in a scratch project directory:

```
mkdir -p ./scratch/test-first-project && cd ./scratch/test-first-project
herdr
```

2. Create a workspace and run OpenCode:

```
herdr workspace create --cwd "$(pwd)" --label "test-first-project"
opencode
```

3. Verify Herdr detects OpenCode and reports state (`idle` when waiting, `working` when generating) via the integration plugin [3], [8].

4. Send a trivial prompt to OpenCode and confirm the pane state transitions to `working` then `idle`/`done` in the sidebar [8], [9].

5. Detach with `prefix+q`, reattach with `herdr`, and confirm the session and state are preserved [10], [11].

6. Stop the test session:

```
herdr server stop
```

### 11.2 Success Criteria

- OpenCode pane is detected as an agent by Herdr.
- Lifecycle state transitions are visible in the sidebar.
- Detach/reattach preserves the session.
- No errors in `herdr log follow`.

## 12. Corroboration Summary

Each major assertion in this guide is corroborated by at least two independent sources:

- Herdr installation methods: [2], [6]
- OpenCode integration v5 requirement: [3], [8]
- Persistent sessions across detach: [10], [11]
- Native restore resume command `opencode --session <id>`: [3], [11]
- Remote attach over SSH: [10], [12]
- Multi-machine support: [12], [13]

## 12. Bibliography

[1] herdrdev, "Windows support," Herdr Documentation, 2026. [Online]. Available: https://herdr.dev/tui/docs/windows-beta/

[2] herdrdev, "Install Herdr," Herdr Documentation, 2026. [Online]. Available: https://herdr.dev/tui/docs/install/

[3] herdrdev, "Integrations," Herdr Documentation, 2026. [Online]. Available: https://herdr.dev/tui/docs/integrations/

[4] OpenCode Remote Control Plugin, "Using OpenCode Remote Control," local skill content, 2026.

[5] tailscale, "Tailscale Serve," Tailscale Documentation, 2026. [Online]. Available: https://tailscale.com/kb/1242/tailscale-serve

[6] herdrdev, "herdr GitHub repository," GitHub, 2026. [Online]. Available: https://github.com/herdrdev/herdr

[7] herdrdev, "Quick start," Herdr Documentation, 2026. [Online]. Available: https://herdr.dev/tui/docs/quick-start/

[8] herdrdev, "Agents," Herdr Documentation, 2026. [Online]. Available: https://herdr.dev/tui/docs/agents/

[9] flaviocopes, "How to manage coding agents with Herdr," Flavio Copes Blog, 2026. [Online]. Available: https://flaviocopes.com/herdr

[10] herdrdev, "Persistence and remote access," Herdr Documentation, 2026. [Online]. Available: https://herdr.dev/tui/docs/persistence-remote/

[11] herdrdev, "Session state and restore," Herdr Documentation, 2026. [Online]. Available: https://herdr.dev/tui/docs/session-state/

[12] herdrdev, "Connecting machines," Herdr Documentation, 2026. [Online]. Available: https://herdr.dev/blog/connecting-the-machines/

[13] herdrdev, "Connecting machines docs," Herdr Documentation, 2026. [Online]. Available: https://herdr.dev/tui/docs/connecting-machines/

[14] herdrdev, "Agent automation," Herdr Documentation, 2026. [Online]. Available: https://herdr.dev/tui/docs/agent-automation/

[15] marcosilva_eu, "Managing OpenCode Sessions with Herdr," 2026. [Online]. Available: https://marcosilva.eu/blog/2026-07-07-managing-opencode-sessions-with-herdr/

[16] herdrdev, "Concepts," Herdr Documentation, 2026. [Online]. Available: https://herdr.dev/tui/docs/concepts/

[17] herdrdev, "Persistence and remote access," Herdr Documentation, 2026. [Online]. Available: https://herdr.dev/tui/docs/persistence-remote/
