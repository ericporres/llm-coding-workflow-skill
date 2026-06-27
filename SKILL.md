---
name: llm-coding-workflow
description: AI-augmented software engineering workflow synthesized from Addy Osmani's methodology, Anthropic's agentic coding research, and 30+ real-world projects. Supports two collaboration modes — interactive pair programming and autonomous delegation — with structured planning, context management, strategic model selection, human accountability, and continuous learning.
version: 3.1
created_by: "Eric Porres — eric@porres.com"
last_updated: 2026-06-10
---

# LLM Coding Workflow

AI-augmented software engineering workflow that maximizes LLM effectiveness through structured planning, clear context, strategic delegation, and human accountability.

## Surface Contract

This skill is invoked primarily in **Cowork sessions**. In Cowork, its output is always an **artifact** — a plan, a spec, a delegation prompt, or a review. It is not an execution surface for code.

| Surface | This skill produces | Execution happens in |
|---|---|---|
| **Cowork** (primary) | Plan, spec, delegation prompt, review, handoff doc | Claude Code CLI (via terminal) or Ruflo (swarm/scale) |
| **Claude Code CLI** | Can load for reference; informs pair-mode coaching | In place (the CLI itself writes/edits files) |

When an agent running in Cowork reads an example that says "implement the feature" or "run the tests," the correct action is to **produce a delegation prompt and hand off to Claude Code**, not to attempt implementation in-session. Cowork's Desktop Commander MCP can invoke Claude Code remotely, but the skill's own advice is about the handoff artifact, not in-session edits.

## What This Skill Does

This skill synthesizes Addy Osmani's LLM coding workflow, Anthropic's agentic coding research, Harper Reed's spec-driven pipeline, and patterns from 30+ real-world projects. It supports two collaboration modes — interactive pair programming and autonomous delegation — and provides a systematic approach to AI-assisted development that prevents wasted cycles while enabling increasingly autonomous execution.

**Core philosophy.** The human engineer is the accountable owner; the AI is a capable collaborator whose autonomy scales with the quality of the prompt. Well-scoped delegation with clear acceptance criteria is not "blind trust" — it is a higher-leverage operating mode.

**Two Collaboration Modes:**

| Mode | When | How | Review |
|---|---|---|---|
| **Pair Mode** (Conductor) | Ambiguous problems, design decisions, learning new domains | Interactive back-and-forth; AI as thought partner | Line-by-line as you go |
| **Delegation Mode** (Orchestrator) | Well-scoped tasks with clear acceptance criteria | Structured prompt → autonomous execution → human review | Deliverable review against spec |

## Cowork Quick Start

From a Cowork session, this skill's three most common outputs are:

1. **A plan** for a feature or change, negotiated through the interview prompt (Principle 0) and written as a short plan doc.
2. **A delegation prompt** using the template in the *Delegation Mode* section below, ready to paste into Claude Code or hand to a Task agent.
3. **A review artifact** — a critique of a draft plan, spec, or diff brought back from Claude Code.

For actual file edits, builds, or test runs, hand off to Claude Code via Desktop Commander (`cd <repo> && claude`) or via a Task agent. See `iac/github-playbook` for the handoff conventions.

## The 10 Principles

### 0. Interview Before Planning

**Before you plan, make sure you're planning the right thing.** Most people arrive with a solution in their head ("build me a dashboard") when the real need is something different ("I need three numbers emailed to me every Monday"). One prompt closes the gap:

> *"I'm about to start this project. Interview me until you have 95% confidence about what I actually want, not what I think I should want."*

This flips the dynamic. Instead of pitching an idea and hoping the AI reads your mind, the AI asks the questions that surface assumptions you did not know you were making. It separates the *what* from the *why* before a single line of planning gets done.

How to use it: start every new project or feature with this prompt (or something close). Answer honestly, including "I don't know yet" — the gaps are where the value is. Let the AI play it back ("So what you actually need is ___. Is that right?"). Only after confirmation should you move to Principle 1.

This is especially valuable in Delegation Mode — if the spec is wrong, everything downstream is wrong. Five minutes of interviewing saves five hours of rework.

### 1. Plan Before Coding

Never start implementation without a plan. Use AI to rapidly create detailed specifications covering functional requirements, edge cases, technical constraints, preferred patterns, and success criteria. This is "waterfall in 15 minutes" — rapid structured planning that prevents wasted cycles. For the full spec format, see `iac/spec-driven-development`.

### 2. Scope Work Into Reviewable Units

Decompose large asks into bounded, independently verifiable units — not because models can't handle scope, but because review, rollback, and commits operate at the unit level. A unit is the largest slice you can check against acceptance criteria in one pass; each gets its own commit.

### 3. Provide Extensive Context

Supply related files and patterns, API documentation, technical constraints, preferred approaches, and known anti-patterns. Context quality still dominates output quality at any model tier — a precise context package outperforms a vague prompt to a stronger model.

In Cowork, context is provided via Read, MCP tool calls (Drive, Confluence, Notion), and explicit reference to repo files. In Claude Code, use `/context add` or `@file` references.

### 4. Choose the Right Model for the Job

Match model to task — deep reasoning and architecture, fast iteration, multimodal input — and use current pro-tier versions; specific model rankings shift faster than this document. When output is wrong, fix the spec and context before reaching for a different model: that is where the leverage is.

### 5. Leverage AI Agents Across the Development Lifecycle

Modern AI coding agents (Claude Code, Cursor, Copilot Workspace) read files, run tests, fix bugs, open PRs, and execute multi-step tasks autonomously. The question is not whether to delegate, but how much supervision to apply.

**Guardrails scale with scope, not with distrust:**

| Delegation scope | Guardrails |
|---|---|
| Single file fix | Commit diff review |
| Feature implementation | Spec review + test pass + code review |
| Multi-service change | Spec review + integration tests + manual QA |
| Infrastructure / security | Never fully delegate; pair mode only |

### 6. Maintain Human Accountability

**You own the output.** Review depth scales with delegation scope, not with suspicion.

- **Pair-mode review**: you reviewed as you went. A quick diff and test pass suffices.
- **Delegation-mode review**: review the deliverable against the spec. Did the agent meet the acceptance criteria? Did the tests pass? Did it stay in scope? See `iac/code-review-and-quality` for the five-axis framework and sizing thresholds.

The operating model is **"delegate, review, own"** — not "distrust and micromanage." A well-scoped prompt with clear acceptance criteria earns a deliverable-level review, not a line-by-line audit.

### 7. Use Frequent, Granular Git Commits

Treat commits as **save points in a game**. Commit after each small task completes successfully. Benefits: easy rollback if AI suggestions introduce bugs, clear progression in history, safe experimentation. Commit messages follow your team's convention (Conventional Commits, short imperative, etc.) — for Eric's setup, see `iac/github-playbook`.

### 8. Customize AI Behavior

Create rules files to guide AI outputs toward team idioms. For Claude Code, `CLAUDE.md` at repo root is loaded automatically; for other agents, the equivalent is `.cursorrules`, `GEMINI.md`, or the system prompt. Capture: code style, preferred patterns, anti-patterns, testing conventions, and anything else that would otherwise be repeated in every prompt.

### 9. Embrace Strong Testing and Automation

Robust CI/CD **enhances AI productivity**. Automated feedback loops help refine outputs: linter errors, type errors, and failing tests are exactly the kind of signal agents can act on autonomously. Wire up the pipeline and feed failures back to the agent — most will iterate to green without further guidance.

### 10. Continuous Learning

Using AI doesn't dull skills — it amplifies existing expertise. LLMs reward best practices. Review every piece of AI-generated code to understand why it made specific choices. Debug AI mistakes yourself first before asking for a fix. Keep notes on effective prompts. The insights compound.

## Delegation Mode: Spec-Driven Autonomous Execution

When a task is well-scoped with clear acceptance criteria, delegation mode lets you hand off implementation to an AI agent and review the deliverable rather than supervising each step.

### Pre-Delegation Checklist

Before delegating, verify:
- Task has a clear, bounded scope (one feature, one fix, one refactor)
- Acceptance criteria are explicit and testable
- The codebase has existing tests or a test framework in place
- No security-sensitive changes (auth, payments, PII handling) — those stay in pair mode
- You can verify the deliverable against the spec without reading every line

### Delegation Prompt Template

```markdown
## Task
[One-sentence description of what to build/fix/refactor]

## Context
- Project: [repo name, relevant paths]
- Stack: [language, framework, key libraries]
- Related files: [list specific files the agent should read]

## Acceptance Criteria
1. [Specific, testable criterion]
2. [Specific, testable criterion]
3. [Specific, testable criterion]

## Constraints
- Follow existing patterns in [file/directory]
- Do not modify [protected areas]
- All tests must pass before marking complete

## Out of Scope
- [Explicitly list what NOT to do]
```

### Three-Tier Execution Model

For complex projects, delegation spans multiple tiers. Each tier is optimized for a different scope of work, and the spec is the handoff document between them.

| Tier | Surface | Role | Example |
|---|---|---|---|
| **Design** | Cowork | Architecture, spec writing, review, decision capture | "Design the auth system; write spec.md" |
| **Build** | Claude Code CLI | Autonomous multi-file implementation | "Implement the auth API per spec.md; make tests pass" |
| **Scale** | Ruflo | Parallel multi-agent execution | "Run 4 agents: API, tests, docs, migration" |

Cowork is where the thinking happens. Claude Code is where the code gets written. Ruflo is where work parallelizes across agents. Always write the spec in Cowork before handing off to the next tier.

### Recovery from Delegation Failures

Delegation fails. Plan for it. When a delegated task returns a broken deliverable:

1. **Stop.** Do not re-delegate the same prompt. The failure mode will repeat.
2. **Diagnose.** Read the diff, the test output, and the agent's stated reasoning. Identify whether the failure was spec ambiguity, missing context, scope creep, or a genuine bug.
3. **Adjust.** Tighten the spec, add missing context, or narrow the scope. If the agent misread a requirement, the spec was unclear — fix the spec, not the agent.
4. **Re-delegate or take it back.** If the spec is now crisp, re-delegate. If the work is revealing deep architectural questions, pull it back into pair mode and work through it interactively.

## Cowork-Native Patterns

These are the three patterns that actually fit the Cowork surface. Each produces an artifact Eric can carry forward — to Claude Code, to a PR description, to a tracker — not an in-session code edit.

### Pattern 1: Plan-Before-Delegate

**Input:** An underspecified ask ("add SSO to LogiQ").
**Cowork action:** Run the Principle 0 interview. Produce a written plan with clarifying questions resolved, acceptance criteria, and a file/path map. Write it to the project's `SESSION.md` or a new `spec.md`.
**Handoff:** Eric opens Claude Code in the repo and says "implement spec.md." The plan is the contract.

### Pattern 2: Review-and-Redline

**Input:** A diff, PR link, or AI-generated deliverable.
**Cowork action:** Apply the five-axis review from `iac:code-review-and-quality` (correctness, clarity, security, performance, testability). Produce a numbered redline with evidence: file:line, what's wrong, what to do instead.
**Handoff:** Eric pastes the redline into the PR or hands it to Claude Code as a fix prompt.

### Pattern 3: Debug-and-Document

**Input:** A failing test, broken build, or runtime error.
**Cowork action:** Run the `iac:debugging-and-error-recovery` triage (Reproduce → Localize → Reduce → Root-cause → Guard → Verify). Produce a written root-cause note and a minimal reproduction.
**Handoff:** The root-cause note goes into SESSION.md "Known Issues" and feeds the next Claude Code session.

In all three patterns, Cowork writes the spec or the redline; Claude Code executes it. That division of labor is the point.

## Best Practices

### Session Hygiene

- **Keep context tight.** In Cowork, every file read consumes the window. Read only what the task requires. When the task touches >2 source files, hand off to Claude Code.
- **Write it down before you forget it.** When a non-obvious decision lands ("we chose Supabase over Neon because pgvector is built-in"), capture it in SESSION.md or a decision doc before the next compaction.
- **One mode at a time.** Don't toggle between pair and delegation mid-task. Finish the current mode's deliverable, then decide whether the next step is pair or delegate.

### Review Discipline

- **Diff before merge.** Always read the diff, even when delegation produced green tests. Tests prove behavior; diffs reveal intent and maintainability.
- **Verify the happy path with real input.** Compilation is not correctness. Run one real case through the new code and show the output.
- **Root-cause before patch.** If a test fails, understand why before changing code. Patches on top of misdiagnoses compound into debt.

### Anti-Patterns to Avoid

- **Aspirational CLI.** Never document commands that don't exist. If a workflow is "you'd invoke /foo," write out the actual prompt or handoff instead.
- **Silent re-delegation.** Re-running the same prompt after a failure without changing the spec wastes tokens and produces identical failures.
- **Context sprawl.** Loading every reference file "just in case" is the fastest way to hit a compaction mid-task. Load on demand.
- **Edit-first-reason-later.** Modifying code before reading the surrounding call path is how regressions ship.

## Summary

Two modes, one philosophy: AI is a collaborator, not a command shell. Pair mode is for thinking together. Delegation mode is for handing off bounded work with a written spec. Cowork is where specs and reviews live. Claude Code is where the code gets written. Ruflo is where work parallelizes.

The ten principles — Interview-Before-Plan, Plan-Before-Code, Reviewable Units, Extensive Context, Right Model for the Job, Agent Lifecycle Guardrails, Human Accountability, Granular Commits, Customize AI, Testing/Automation, Continuous Learning — apply to both modes. Which principle dominates depends on what you're doing, but none of them is optional.

## Sources & References

- Addy Osmani — *Beyond the Hype: A Realistic Look at Large Language Models in Software Engineering* (2024–2026 series)
- Harper Reed — codegen pipeline essays and the "spec as handoff" pattern
- Anthropic — Claude Code and Agent SDK documentation, agentic coding research
- Greptile — AI code review practitioner notes
- Geoffrey Huntley — LLM coding discipline essays
- Eric Porres — "WhatsApp as an AI copilot surface" (Beyond Reason, 2026)

## Related Skills

- `iac:spec-driven-development` — write the spec before writing the code
- `iac:code-review-and-quality` — five-axis review methodology
- `iac:debugging-and-error-recovery` — structured triage when things break
- `iac:builder-playbook` — project archetypes and scaffold recipes
- `iac:github-playbook` — repo workflow and multi-machine setup
- `iac:agent-landscape` — frameworks, personas, marketplace strategy
