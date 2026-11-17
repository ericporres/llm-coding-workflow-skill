---
name: llm-coding-workflow
description: AI-augmented software engineering workflow synthesized from Addy Osmani's methodology, Anthropic's agentic coding research, and 30+ real-world projects. Supports two collaboration modes — interactive pair programming and autonomous delegation — with structured planning, context management, strategic model selection, human accountability, and continuous learning.
version: 3.2
created_by: "Eric Porres — eric@porres.com"
last_updated: 2026-11-17
---

# LLM Coding Workflow

AI-augmented software engineering workflow that maximizes LLM effectiveness through structured planning, clear context, strategic delegation, and human accountability.

## Surface Contract

This skill is invoked primarily in **Cowork sessions**. Cowork is the default surface for the whole loop: it writes the plan or spec and usually executes it. (Revised 2026-11-17. Earlier versions treated Cowork as artifact-only and sent every build to Claude Code or Ruflo.)

| Surface | This skill produces | Execution happens in |
|---|---|---|
| **Cowork** (primary) | Plan, spec, delegation prompt, review, and usually the implementation | In session. The main loop first; subagents or a `Workflow` run when slices are parallel or the reading would crowd the session's context |
| **Claude Code CLI** (fallback) | Can load for reference; informs pair-mode coaching | In place, for a multi-file refactor with a build-and-test loop, an interactive process you watch and steer, or when Cowork can't reach the machine. A parallel build across disjoint folders runs there as an agent team (`iac:agent-teams`) |

When execution leaves the session that wrote the spec (a subagent, a `Workflow` run, or Claude Code), the spec is the handoff document. Pass it whole; a summary drops the boundaries.

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
2. **A delegation prompt** using the template in the *Delegation Mode* section below, ready to hand to a subagent or paste into Claude Code.
3. **A review artifact** — a critique of a draft plan, spec, or diff, wherever it was written.

File edits, builds, and test runs happen in the same session by default. Hand off to Claude Code (`cd <repo> && claude`) for the fallback cases in the Surface Contract; see `iac/github-playbook` for the conventions.

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

## Ownership
[One of the two lines below. Never leave this section out.]
- This codebase is yours: what it's for and how it's built are your call, and you'll keep building on it.
- This is someone else's code. Make the smallest change that meets the acceptance criteria and leave the structure as it is.

## Out of Scope
- [Explicitly list what NOT to do]
```

**Why the Ownership section is required (added 2026-11-10).** A prompt that says nothing about ownership gets the same result as one that says the code is someone else's: the agent adds to the existing structure, even when that repeats code. Anthropic's Fable 5.2 prompting guide (read 2026-11-09) reports that with the "yours" sentence the model merged repeated code in about half of tries, against under a tenth without it. The cost: about 1.5x output tokens, and unrequested changes to existing behavior in one try in six to ten, often missing from the agent's summary. Pick "yours" for Eric's own projects where consolidation is wanted. Pick "someone else's" for consumed plugins, public repos that take outside PRs, and any fix where minimal change is the goal. A scope rule such as "no drive-by refactors" does not cancel the "yours" line, so do not send both. Tested on Fable 5.2 only; untested on Opus 5.5 and Sonnet 5.5. Details: `iac:fable-5-scaffolding`.

### Execution Model: Two Tiers and a Fallback

The spec is the handoff document whenever execution leaves the session that wrote it.

| Tier | Surface | Role | Example |
|---|---|---|---|
| **Design** | Cowork | Architecture, spec writing, review, decision capture | "Design the auth system; write spec.md" |
| **Build** | Cowork | Execute against the spec in the main loop; add subagents or a `Workflow` run for parallel slices and fresh-context verification | "Implement the auth API per spec.md; make tests pass" |
| **Fallback** | Claude Code CLI | Multi-file refactor with a build-and-test loop; an interactive process; Cowork can't reach the machine. Agent team (`iac:agent-teams`) when the build splits into disjoint folders with a contract between them | "Migrate 40 files to the new client; keep the suite green" |

Start in one loop. An agent that has seen every earlier result connects them better than one reading summaries, so add a subagent only when context bloat measurably hurts the loop, when slices can run in parallel in windows that get discarded, or for a verifier that must not see the author's reasoning. Ruflo was the scale tier until 2026-09-16 and is retired; do not route work to it.

### Recovery from Delegation Failures

Delegation fails. Plan for it. When a delegated task returns a broken deliverable:

1. **Stop.** Do not re-delegate the same prompt. The failure mode will repeat.
2. **Diagnose.** Read the diff, the test output, and the agent's stated reasoning. Identify whether the failure was spec ambiguity, missing context, scope creep, or a genuine bug.
3. **Adjust.** Tighten the spec, add missing context, or narrow the scope. If the agent misread a requirement, the spec was unclear — fix the spec, not the agent.
4. **Re-delegate or take it back.** If the spec is now crisp, re-delegate. If the work is revealing deep architectural questions, pull it back into pair mode and work through it interactively.

## Cowork-Native Patterns

These three patterns fit the Cowork surface. Each produces an artifact that outlives the session (a spec, a redline, a root-cause note), whether the session then executes it or hands it off.

### Pattern 1: Plan-Before-Delegate

**Input:** An underspecified ask ("add SSO to the internal dashboard").
**Cowork action:** Run the Principle 0 interview. Produce a written plan with clarifying questions resolved, acceptance criteria, and a file/path map. Write it to the project's `SESSION.md` or a new `spec.md`.
**Next step:** Execute the spec in the same session, or hand it whole to a subagent, a `Workflow` run, or Claude Code (fallback cases above). The plan is the contract.

### Pattern 2: Review-and-Redline

**Input:** A diff, PR link, or AI-generated deliverable.
**Cowork action:** Apply the five-axis review from `iac:code-review-and-quality` (correctness, clarity, security, performance, testability). Produce a numbered redline with evidence: file:line, what's wrong, what to do instead.
**Next step:** The redline goes into the PR, or becomes the fix prompt in this session or in Claude Code.

### Pattern 3: Debug-and-Document

**Input:** A failing test, broken build, or runtime error.
**Cowork action:** Run the `iac:debugging-and-error-recovery` triage (Reproduce → Localize → Reduce → Root-cause → Guard → Verify). Produce a written root-cause note and a minimal reproduction.
**Next step:** The root-cause note goes into SESSION.md "Known Issues" and feeds the fix, in this session or the next.

In all three patterns the written artifact comes first. Who executes it is a routing choice, and the main loop is the default.

## Best Practices

### Session Hygiene

- **Keep context tight.** In Cowork, every file read consumes the window. Read only what the task requires. When the reading would crowd the window, hand it to a subagent that writes its findings to a file; a multi-file refactor with a build-and-test loop goes to Claude Code.
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

Two modes, one philosophy: AI is a collaborator, not a command shell. Pair mode is for thinking together. Delegation mode is for handing off bounded work with a written spec. Cowork is where specs and reviews live, and usually where the code gets written. Claude Code is the fallback for build-and-test refactors. Parallel work runs as subagents or an agent team, and only where one loop measurably falls short.

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
- `iac:agent-teams` — lead, builders, and adversary in Claude Code, with a single-loop comparison arm
