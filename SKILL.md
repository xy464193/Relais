```
---
name: relais-skill-blueprint
version: 1.0.0
description: Official guide for creating Relais skills. Triggered when the user needs to empower the model with specialized capabilities, particularly those involving local sandbox operations, automation scripts, or specific professional workflows.
execution_env: hybrid # Options: cloud-only, local-ish, hybrid
---

# Relais Skill Blueprint

This guide defines the standards for building efficient, secure, and modular Skills within the Relais ecosystem.

## What is a Relais Skill?

In the Relais architecture, a Skill acts as the bridge between the "Cloud Brain (AI Provider)" and the "Local Hands (iSH Sandbox)". Skills transform a general LLM into a specialized Agent capable of utilizing local tools and maintaining state within a secure environment.

### Core Values of a Skill
1. **Sandbox Empowerment**: Instructs the model on how to safely execute Linux commands within `/var/relais`.
2. **Workflow Formalization**: Converts complex prompt intentions into structured, repeatable execution flows.
3. **Private Knowledge Injection**: Provides domain-specific API documentation, data schemas, or internal business logic.

## Core Design Principles

### 1. Extreme Token Efficiency (Concise is Key)
The context window is a shared and finite resource. Skill definitions must assume the model is already highly capable.
- **Eliminate Redundancy**: Do not explain common knowledge or basic LLM concepts.
- **Examples Over Prose**: Use short Bash snippets or JSON structures instead of long paragraphs of explanation.

### 2. Explicit Execution Boundaries
When defining a skill, clearly specify where the work happens:
- **Cloud-only**: Tasks involving text processing or external API calls with no local resource requirements.
- **Local-iSH**: Tasks requiring the `execute_local_shell` tool. Always specify the working directory (e.g., `cd /var/relais/workspace`).

### 3. Relais Skill Directory Anatomy

A standard Relais skill package follows this structure:

```text
skill-name/
├── SKILL.md          (Required: Triggers and core workflow)
└── resources/        (Optional: On-demand assets)
    ├── local_bin/    - Executable Shell or Python scripts for iSH
    ├── context/      - Long-form reference material for RAG
    └── templates/    - Base templates for file generation
```

#### SKILL.md Frontmatter
- `name`: Unique identifier for the skill.
- `description`: The primary triggering mechanism. The model reads this to decide if the skill is relevant to the user's request.
- `execution_env`: Defines the skill's dependency on the local iSH engine.

#### SKILL.md Body
Keep the body under 300 lines. Offload detailed documentation to `resources/context/` to keep the core prompt lean.

## Loading Strategy (Progressive Disclosure)

Relais utilizes a three-tier loading system to optimize performance:
1. **Metadata Layer**: The `description` of every skill is always present in the system prompt.
2. **Activation Layer**: When a user's intent matches, the full `SKILL.md` body is loaded into context.
3. **Deep Dive Layer**: The model calls local tools to read files within the `resources/` directory only as needed.

## Skill Creation Prohibitions
- **No Human-Centric Files**: Do not include `README.md`, `CHANGELOG.md`, or `INSTALL.md`. Skills are strictly for AI consumption.
- **No Absolute Paths**: Never hardcode physical paths. Always use relative paths from the sandbox root or the standardized `/var/relais` mount point.
```