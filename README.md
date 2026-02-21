# LLM Coding Workflow Skill

A Claude Code skill that implements Addy Osmani's AI-augmented software engineering methodology — structured planning, chunked execution, human oversight, and continuous learning. Treats LLMs as pair programmers that need clear direction, not autonomous agents.

## What This Is

This is a **skill** for [Claude Code](https://docs.claude.com/en/docs/claude-code) (also works in [Cowork](https://claude.ai)). Once installed, it gives Claude a systematic framework for AI-assisted development that prevents wasted cycles and produces higher-quality output.

The core idea: AI coding assistants are incredible force multipliers, but the human engineer remains the director. This skill encodes that philosophy into a repeatable workflow.

## The 10 Principles

1. **Plan before coding** — Rapid "waterfall in 15 minutes" planning before any implementation
2. **Break work into small chunks** — Focused, manageable tasks instead of monolithic requests
3. **Provide extensive context** — Relevant files, docs, patterns, and constraints loaded before each task
4. **Choose the right model** — Strategic model switching based on task type (reasoning vs. speed vs. integration)
5. **Leverage agents across the lifecycle** — Let AI handle routine tasks under supervision
6. **Maintain human oversight** — Treat AI output like a junior developer's code: review everything
7. **Use granular commits** — Every passing test is a save point; commit early, commit often
8. **Customize AI behavior** — Rules files (CLAUDE.md, .cursorrules) that encode team conventions
9. **Embrace testing and automation** — CI/CD pipelines as feedback loops for AI-generated code
10. **Continuous learning** — Review AI decisions, debug mistakes, track what works

## Getting Started

### Install as a Claude Code skill

```bash
# Option 1: Copy into your project
cp SKILL.md /path/to/your/project/.claude/skills/llm-coding-workflow/SKILL.md

# Option 2: Add to your home directory for global access
mkdir -p ~/.claude/skills/llm-coding-workflow
cp SKILL.md ~/.claude/skills/llm-coding-workflow/SKILL.md
```

### Start using it

The skill activates automatically when you work on development tasks. You can also invoke it explicitly:

```
/llm-workflow plan "Implement user authentication with JWT"
/llm-workflow implement --chunk-size small
/llm-workflow review --commit
```

Or use natural language:

- *"Plan out the auth feature before we start coding"*
- *"Break this into smaller tasks"*
- *"Review what we just wrote before committing"*

## How It Works

### Planning Phase
You describe a feature. Claude creates a detailed spec — requirements, edge cases, technical constraints, task decomposition, risk assessment — in about 15 minutes. This prevents the most expensive mistake in AI-assisted development: building the wrong thing.

### Implementation Phase
The plan gets broken into small, focused chunks. Each chunk carries forward context from completed work. Claude implements one chunk at a time, you review, test, and commit before moving on.

### Review Phase
Every piece of generated code goes through review — logic correctness, security implications, edge cases, style consistency. The skill enforces this as a required step, not an optional one.

### Commit Phase
Granular commits after each successful chunk. If something goes wrong three steps later, you roll back to the last good state instead of untangling a massive diff.

## Quick Reference

| Command | What it does |
|---------|-------------|
| `/llm-workflow plan "[feature]"` | Create a detailed implementation plan |
| `/llm-workflow decompose "[task]"` | Break a large task into chunks |
| `/llm-workflow implement` | Implement the current chunk |
| `/llm-workflow review` | Review generated code |
| `/llm-workflow test` | Run tests for affected code |
| `/llm-workflow commit` | Commit with auto-generated message |
| `/llm-workflow debug "[issue]"` | Analyze and fix a bug |
| `/llm-workflow refactor [file]` | Modernize legacy code |

## Configuration

Create `.llm-workflow.json` in your project root:

```json
{
  "workflow": {
    "chunkSize": "small",
    "autoCommit": true,
    "autoTest": true,
    "humanReview": "required"
  }
}
```

See `SKILL.md` for the full configuration reference.

## Anti-Patterns This Prevents

- **Monolithic requests** — "Build me the entire feature" produces worse results than 8 focused chunks
- **Blind trust** — Accepting AI code without review leads to subtle bugs that compound
- **Context starvation** — The less context you provide, the more the AI guesses (badly)
- **Model loyalty** — Sticking with one model when it's clearly stuck
- **Big commits** — Hard to debug, impossible to roll back cleanly

## Background

This skill implements the methodology described by Addy Osmani (Google Chrome engineering lead) for getting the most out of AI coding assistants. The original framework comes from his practical experience using LLMs in production engineering workflows. This skill packages those principles into a repeatable workflow for Claude Code.

Built by [Eric Porres](https://github.com/ericporres).

## License

MIT — use it however you'd like.
