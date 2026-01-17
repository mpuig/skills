# mpuig-skills

Python-focused agent skills, following the [Agent Skills](https://agentskills.io) open format.

Inspired by [getsentry/skills](https://github.com/getsentry/skills) and adapted for Python development.

## Installation

### Claude Code (via Marketplace)

```bash
# Add the marketplace
claude plugin marketplace add mpuig/skills

# Install the plugin
claude plugin install mpuig-skills@mpuig-skills
```

### Claude Code (from local clone)

```bash
# Clone the repository
git clone git@github.com:mpuig/skills.git ~/mpuig-skills

# Install the marketplace from the local clone
claude plugin marketplace add ~/mpuig-skills

# Install the plugin directly
claude plugin install mpuig-skills
```

After installation, restart Claude Code. The skills will be automatically invoked when relevant to your task.

### Updating

```bash
# Update the marketplace index
claude plugin marketplace update

# Update the plugin
claude plugin update mpuig-skills@mpuig-skills
```

Or use `/plugin` to open the interactive plugin manager.

### Other Agents

Copy the `skills/` directory to your agent's skills location, or reference the SKILL.md files directly according to your agent's documentation.

## Available Skills

| Skill | Description |
|-------|-------------|
| [code-review](plugins/mpuig-skills/skills/code-review/SKILL.md) | Python code review guidelines and checklist |
| [commit](plugins/mpuig-skills/skills/commit/SKILL.md) | Conventional commit message conventions |
| [create-pr](plugins/mpuig-skills/skills/create-pr/SKILL.md) | Create pull requests following best practices |
| [find-bugs](plugins/mpuig-skills/skills/find-bugs/SKILL.md) | Find bugs and security vulnerabilities in branch changes |
| [iterate-pr](plugins/mpuig-skills/skills/iterate-pr/SKILL.md) | Iterate on a PR until CI passes and feedback is addressed |
| [claude-settings-audit](plugins/mpuig-skills/skills/claude-settings-audit/SKILL.md) | Analyze repo and generate recommended Claude Code settings.json permissions |
| [agents-md](plugins/mpuig-skills/skills/agents-md/SKILL.md) | Maintain AGENTS.md with concise agent instructions |

## Available Subagents

| Subagent | Description |
|----------|-------------|
| [code-simplifier](plugins/mpuig-skills/agents/code-simplifier.md) | Simplifies and refines Python code for clarity, consistency, and maintainability while preserving all functionality |

## Repository Structure

```
mpuig-skills/
├── .claude-plugin/
│   └── marketplace.json      # Marketplace manifest
├── plugins/
│   └── mpuig-skills/
│       ├── .claude-plugin/
│       │   └── plugin.json   # Plugin manifest
│       ├── agents/
│       │   └── code-simplifier.md
│       └── skills/
│           ├── code-review/
│           │   └── SKILL.md
│           └── commit/
│               └── SKILL.md
├── AGENTS.md                 # Agent-facing documentation
├── CLAUDE.md                 # Symlink to AGENTS.md
└── README.md                 # This file
```

## Creating New Skills

Skills follow the [Agent Skills specification](https://agentskills.io/specification). Each skill requires a `SKILL.md` file with YAML frontmatter.

### Skill Template

Create a new directory under `plugins/mpuig-skills/skills/`:

```
plugins/mpuig-skills/skills/my-skill/
└── SKILL.md
```

**SKILL.md format:**

```yaml
---
name: my-skill
description: A clear description of what this skill does and when to use it. Include keywords that help agents identify when this skill is relevant.
---

# My Skill Name

## Instructions

Step-by-step guidance for the agent.

## Examples

Concrete examples showing expected input/output.

## Guidelines

- Specific rules to follow
- Edge cases to handle
```

### Naming Conventions

- **name**: 1-64 characters, lowercase alphanumeric with hyphens only
- **description**: Up to 1024 characters, include trigger keywords
- Keep SKILL.md under 500 lines; split longer content into reference files

### Optional Fields

| Field | Description |
|-------|-------------|
| `license` | License name or path to license file |
| `compatibility` | Environment requirements (max 500 chars) |
| `model` | Override model for this skill (e.g., `sonnet`, `opus`, `haiku`) |
| `allowed-tools` | Space-delimited list of tools the skill can use |
| `metadata` | Arbitrary key-value pairs for additional properties |

```yaml
---
name: my-skill
description: What this skill does
license: Apache-2.0
model: sonnet
allowed-tools: Read Grep Glob
---
```

## References

- [Agent Skills Specification](https://agentskills.io/specification)
- [Python Documentation](https://docs.python.org/3/)

## License

Apache-2.0
