---
name: relais-skill-creator
version: 2.0.0
description: Official guide for creating Relais skills. Triggered when the user needs to empower the model with specialized capabilities, particularly those involving local sandbox operations, automation scripts, or specific professional workflows.
execution_env: hybrid
---

# Relais Skill Blueprint

This guide defines the standards for building efficient, secure, and modular Skills within the Relais ecosystem.

## What is a Relais Skill?

A Skill bridges the "Cloud Brain (AI Provider)" and the "Local Hands (iSH Sandbox)". It transforms a general LLM into a specialized Agent capable of utilizing local tools and maintaining state within a secure environment.

### Core Values
1. **Sandbox Empowerment**: Instructs the model how to safely execute Linux commands within the session workspace.
2. **Workflow Formalization**: Converts complex prompt intentions into structured, repeatable execution flows.
3. **Private Knowledge Injection**: Provides domain-specific API documentation, data schemas, or internal business logic.

## Environment & Paths

All local execution happens inside an Alpine Linux (x86 via iSH) chroot on the user's iOS device.

| Variable | Value |
|---|---|
| Session workspace | `/mnt/relais/{sessHash}/workspace` |
| User uploads | `/mnt/relais/{sessHash}/attachments/uploads/` |
| Shared across sessions | `/mnt/relais/global/` |

- `{sessHash}` is the short hash of the current session ID, injected automatically by Relais.
- **Never hardcode** physical iOS paths (e.g. `/var/...` or `/mnt/...` raw paths). Always use the `$RELAIS_SESS_PATH` environment variable or relative paths from workspace.
- The correct tool for running shell commands is `execute_ish_command`, not `execute_local_shell`.

## Core Design Principles

### 1. Extreme Token Efficiency
The context window is a shared, finite resource. Assume the model is already highly capable.
- **Eliminate Redundancy**: Do not explain common knowledge or basic LLM concepts.
- **Examples Over Prose**: Use short Bash snippets or JSON structures instead of paragraphs.

### 2. Explicit Execution Boundaries

| Mode | When to use |
|---|---|
| `cloud-only` | Text processing, external API calls, no local resources needed |
| `local-ish` | Requires `execute_ish_command`. Always `cd $RELAIS_SESS_PATH/workspace` first. |
| `hybrid` | Combines cloud reasoning with local execution |

### 3. Skill Directory Structure

```text
skill-name/
├── SKILL.md          (Required: frontmatter + core workflow)
└── resources/        (Optional: on-demand assets)
    ├── local_bin/    - Shell/Python scripts for iSH execution
    ├── context/      - Long-form reference material
    └── templates/    - Base templates for file generation
```

### 4. SKILL.md Frontmatter Fields

```yaml
---
name: my-skill          # Required. Unique slug, used as folder name.
version: 1.0.0          # Required. Semantic versioning.
description: ...        # Required. Primary trigger — AI reads this to decide relevance.
execution_env: hybrid   # Required. cloud-only | local-ish | hybrid
---
```

Keep the body under 300 lines. Offload detailed docs to `resources/context/`.

## _meta.json (Skill Index Format)

When publishing a skill to a registry or sharing via URL, provide a `_meta.json` index alongside:

```json
{
  "version": "1.0.0",
  "updated_at": "2026-04-19",
  "skills": [
    {
      "id": "my-skill",
      "name": "My Skill",
      "description": "One-line trigger description.",
      "version": "1.0.0",
      "url": "https://github.com/user/repo/blob/main/skills/my-skill/SKILL.md",
      "category": "automation",
      "author": "your-handle"
    }
  ]
}
```

- `url` points to the GitHub blob page or raw URL of `SKILL.md`. Relais auto-converts blob URLs to raw for download.
- `id` must match the folder name (slug).

## Loading Strategy (Progressive Disclosure)

Relais uses a three-tier loading system:
1. **Metadata Layer**: The `description` of every *enabled* skill is injected into the system prompt on every turn.
2. **Activation Layer**: When intent matches, the full `SKILL.md` body is loaded into context.
3. **Deep Dive Layer**: The model calls `execute_ish_command` to read files in `resources/` as needed.

## Prohibitions
- **No human-centric files**: No `README.md`, `CHANGELOG.md`, or `INSTALL.md`. Skills are for AI consumption only.
- **No absolute iOS paths**: Never reference `/private/var/...` or any physical device path.
- **No hardcoded session hashes**: Always use `$RELAIS_SESS_PATH` or the path injected by the system prompt.
