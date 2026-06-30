# Renesas Skills

Renesas Skills is a collection of skills that help AI coding assistants work better with Renesas projects in VS Code. It includes one skill for now, and we will keep adding more over time to cover more tasks.

## Skills

### `configure-renesas-debug`

Helps AI agents (Claude, GitHub Copilot, Cursor, and others):

- Generate correct `.vscode/launch.json` debug configurations for Renesas devices
- Configure supported debuggers (J-Link, E2, E2Lite, E1, E20, EZ, IECUBE, COM Port, Simulator)
- Modify debug settings (server parameters, GDB commands, hotplug, RTOS debugging)
- Explain Renesas-specific parameters and GDB commands

## Requirements

- [Node.js](https://nodejs.org/) (for running `npx`)
- Internet access to the npm registry

> **Tip:** If you are using the Renesas VS Code extension, a notification will appear when you open VS Code, letting you know these skills are available. Click **View User Guide** to open the installation instructions.

## Installation

Install Renesas Skills using the [skills.sh](https://www.skills.sh/) CLI. Run the following command from your project root.

> **Note:** After installing, restart your editor/agent so it loads the new skill.

### Project-level (default)

Skills are installed into the current project only. You will need to reinstall for each new project.

```bash
npx skills add renesas/renesas-skills
```

### Global (all projects)

Skills are installed once to your home directory and available across all projects automatically. No reinstall needed for new projects.

```bash
npx skills add renesas/renesas-skills -g
```

### Install for a specific agent

Use `-a <agent>` with the agent identifier from skills.sh:

```bash
# Claude Code only
npx skills add renesas/renesas-skills -a claude-code

# GitHub Copilot only
npx skills add renesas/renesas-skills -a github-copilot

# Cursor only
npx skills add renesas/renesas-skills -a cursor

# Multiple agents at once
npx skills add renesas/renesas-skills -a claude-code -a github-copilot
```

See the [skills.sh agent list](https://www.skills.sh/) for all `--agent` identifiers (e.g. `codex`, `gemini-cli`, `windsurf`, …).

### Skip confirmation prompts

```bash
npx skills add renesas/renesas-skills -y
```

### Preview available skills before installing

```bash
npx skills add renesas/renesas-skills --list
```

### Installation locations

Renesas Skills are **agent-agnostic** — they follow the shared Agent Skills specification and work with **any of the 50+ agents supported by skills.sh**, including:

Claude Code · GitHub Copilot · Cursor · Codex · Gemini CLI · OpenCode · Windsurf · Cline · Continue · Roo Code · Qwen Code · Goose · Kilo Code · Zencoder · Amp · Trae *(and 40+ more)*

The CLI automatically detects which agents you have installed and places the skill in the correct folder for each. Common examples:

| Agent | Project path | Global path (`-g`) |
|---|---|---|
| Claude Code | `.claude/skills/configure-renesas-debug/` | `~/.claude/skills/configure-renesas-debug/` |
| GitHub Copilot | `.agents/skills/configure-renesas-debug/` | `~/.copilot/skills/configure-renesas-debug/` |
| Cursor | `.agents/skills/configure-renesas-debug/` | `~/.cursor/skills/configure-renesas-debug/` |

For the complete, always-up-to-date list of agents and their paths, see [skills.sh](https://www.skills.sh/).

### Installation method

By default, skills.sh creates **symlinks** from each agent directory to a single canonical copy. To copy files instead (e.g. when symlinks are not supported):

```bash
npx skills add renesas/renesas-skills --copy
```

## Check installed skills

List all currently installed skills:

```bash
npx skills list
```

List only global skills:

```bash
npx skills list -g
```

Filter by agent:

```bash
npx skills list -a claude-code
npx skills list -a github-copilot
```

## Update

> **Note:** The `add` command takes the **repository** (`renesas/renesas-skills`), while `update` and `remove` take the installed **skill name** (`configure-renesas-debug`). Run `npx skills list` first to see the exact skill names installed on your system.

Update all installed skills to the latest version:

```bash
npx skills update
```

Update project-level skills only:

```bash
npx skills update -p
```

Update global skills only:

```bash
npx skills update -g
```

Update the `configure-renesas-debug` skill specifically:

```bash
npx skills update configure-renesas-debug
```

Skip confirmation prompt (auto-detects scope: project if in a project directory, else global):

```bash
npx skills update -y
```

## Remove

Remove interactively (select from installed skills):

```bash
npx skills remove
```

Remove a specific skill:

```bash
npx skills remove configure-renesas-debug
```

Remove from global scope:

```bash
npx skills remove configure-renesas-debug -g
```

Remove from a specific agent only:

```bash
npx skills remove configure-renesas-debug -a claude-code
```

Remove all skills without confirmation:

```bash
npx skills remove --all
```

## Supported Debuggers

| Debugger | Supported Devices |
|---|---|
| J-Link (Segger) | RA, RX, RZ, RISC-V MCU, Dialog |
| E2 / E2Lite | RA, RX, RL78, RH850, RISC-V MCU |
| E1 | RA, RX, RL78, RH850 |
| E20 | RA, RX, RH850 |
| EZ | RL78 |
| IECUBE | RL78 |
| COM Port | RL78 |
| Simulator | RX, RL78 |

## Troubleshooting

**Skill not loading in Claude Code**
- Run `npx skills list -a claude-code` to verify installation
- Check that `.claude/skills/configure-renesas-debug/SKILL.md` exists (project-level) or `~/.claude/skills/configure-renesas-debug/SKILL.md` (global)
- Ensure the `SKILL.md` frontmatter contains valid `name` and `description` fields

**Skill not loading in GitHub Copilot**
- Run `npx skills list -a github-copilot` to verify installation
- Check that `.agents/skills/configure-renesas-debug/SKILL.md` exists (project-level) or `~/.copilot/skills/configure-renesas-debug/SKILL.md` (global)

**Permission errors**
Ensure you have write access to the target directory.

**Symlink issues (Windows)**
Use `--copy` flag to install file copies instead of symlinks:
```bash
npx skills add renesas/renesas-skills --copy
```
