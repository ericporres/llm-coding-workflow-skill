# LLM Coding Workflow Skill

A Claude Code skill for AI-augmented software engineering — structured planning, two collaboration modes (interactive pair programming and autonomous delegation), human accountability, and continuous learning. Synthesized from Addy Osmani's methodology, Anthropic's agentic coding research, and 30+ real-world projects.

## What This Is

This is a **skill** for [Claude Code](https://docs.claude.com/en/docs/claude-code) (also works in [Cowork](https://claude.ai)). Once installed, it gives Claude a systematic framework for AI-assisted development that prevents wasted cycles and enables increasingly autonomous execution where appropriate.

The core idea: the human engineer is the accountable owner; the AI is a capable collaborator whose autonomy scales with the quality of the prompt. Well-scoped delegation with clear acceptance criteria is not "blind trust" — it is a higher-leverage operating mode.

## Two Collaboration Modes

| Mode | When | How |
|------|------|-----|
| **Pair Mode** (Conductor) | Ambiguous problems, design decisions, learning new domains | Interactive back-and-forth; AI as thought partner |
| **Delegation Mode** (Orchestrator) | Well-scoped tasks with clear acceptance criteria | Structured prompt → autonomous execution → human review |

The skill teaches Claude when to use each mode and how to scale guardrails with scope.

## The 10 Principles

1. **Plan before coding** — Rapid "waterfall in 15 minutes" planning before any implementation
2. **Break work into small chunks** — Focused, manageable tasks instead of monolithic requests
3. **Provide extensive context** — Relevant files, docs, patterns, and constraints loaded before each task
4. **Choose the right model** — Strategic model switching based on task type (reasoning vs. speed vs. integration)
5. **Leverage agents across the lifecycle** — Pair Mode for interactive work, Delegation Mode for well-scoped autonomous execution
6. **Maintain human accountability** — You own the output; review depth scales with delegation scope
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

### Implementation Phase (Pair Mode)
Interactive back-and-forth. The plan gets broken into small chunks. Claude implements one chunk at a time, you review, test, and commit before moving on. Best for ambiguous problems and design decisions.

### Implementation Phase (Delegation Mode)
You write a clear spec with acceptance criteria, hand it off, and review the deliverable against the spec. Best for well-scoped tasks where you'd trust the output after a code review. The skill includes a delegation prompt template and pre-delegation checklist.

### Review Phase
Review depth scales with delegation scope: a single-file fix needs a diff review; a feature needs spec review + test pass + code review; multi-service changes need integration tests + manual QA. Infrastructure and security stay in pair mode.

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
- **Blind trust** — Delegating without acceptance criteria or review is not delegation, it's abdication
- **Context starvation** — The less context you provide, the more the AI guesses (badly)
- **Model loyalty** — Sticking with one model when it's clearly stuck
- **Big commits** — Hard to debug, impossible to roll back cleanly
- **One-mode thinking** — Using only interactive mode when delegation would be faster, or only delegation when the problem needs exploration

## Background

This skill synthesizes Addy Osmani's LLM coding workflow (Google Chrome engineering lead), Anthropic's agentic coding research, Harper Reed's spec-driven pipeline, and patterns from 30+ real-world projects. The v2.0 rewrite adds two collaboration modes reflecting how experienced practitioners actually work with AI — sometimes as a pair programmer, sometimes as an autonomous agent executing against a clear spec.

The operating model: delegate, review, own.

Built by [Eric Porres](https://github.com/ericporres).

## License

MIT — use it however you'd like.
