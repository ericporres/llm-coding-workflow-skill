---
name: llm-coding-workflow
description: AI-augmented software engineering workflow synthesized from Addy Osmani's methodology, Anthropic's agentic coding research, and 30+ real-world projects. Supports two collaboration modes — interactive pair programming and autonomous delegation — with structured planning, context management, strategic model selection, human accountability, and continuous learning.
version: 2.0
created_by: "Eric Porres — eric@porres.com"
last_updated: 2026-03-08
---

# LLM Coding Workflow

AI-augmented software engineering workflow that maximizes LLM effectiveness through structured planning, clear context, strategic delegation, and human accountability.

## What This Skill Does

This skill synthesizes Addy Osmani's LLM coding workflow, Anthropic's agentic coding research, Harper Reed's spec-driven pipeline, and patterns from 30+ real-world projects. It supports two collaboration modes — interactive pair programming and autonomous delegation — and provides a systematic approach to AI-assisted development that prevents wasted cycles while enabling increasingly autonomous execution.

**Core Philosophy**: The human engineer is the accountable owner; the AI is a capable collaborator whose autonomy scales with the quality of the prompt. Well-scoped delegation with clear acceptance criteria is not "blind trust" — it is a higher-leverage operating mode.

**Two Collaboration Modes:**

| Mode | When | How | Review |
|------|------|-----|--------|
| **Pair Mode** (Conductor) | Ambiguous problems, design decisions, learning new domains | Interactive back-and-forth; AI as thought partner | Line-by-line as you go |
| **Delegation Mode** (Orchestrator) | Well-scoped tasks with clear acceptance criteria | Structured prompt → autonomous execution → human review | Deliverable review against spec |

**Key Capabilities:**
- **Structured Planning**: Rapid waterfall-style planning before implementation
- **Chunked Execution**: Small, focused tasks with quick course correction
- **Context Management**: Systematic context provision for optimal AI output
- **Model Selection**: Strategic switching between models based on task needs
- **Human Accountability**: You own the output; review depth scales with delegation scope
- **Delegation Prompts**: Spec-driven handoff for autonomous execution
- **Granular Commits**: Frequent save points for easy rollback
- **Continuous Learning**: Skill amplification through AI collaboration

## Prerequisites

**Required:**
- Git repository initialized
- Testing framework configured
- Linter/formatter installed

**Recommended:**
- CI/CD pipeline configured
- Type checker enabled (TypeScript, mypy, etc.)
- Context packaging tool (gitingest, repo2txt)

## Quick Start

### Begin a New Feature
```bash
# Step 1: Plan with AI
/llm-workflow plan "Implement user authentication with JWT"

# Step 2: Execute in chunks
/llm-workflow implement --chunk-size small

# Step 3: Review and commit
/llm-workflow review --commit
```

### Debug an Issue
```bash
/llm-workflow debug "Memory leak in event handlers"
```

### Refactor Code
```bash
/llm-workflow refactor src/legacy/utils.js --modernize
```

---

## Complete Guide

### The 10 Principles

This workflow is built on 10 core principles that maximize AI effectiveness:

#### 1. Plan Before Coding

**Never start implementation without a plan.** Use AI to rapidly create detailed specifications.

```
You: I need to implement [feature]. Help me create a detailed spec including:
- Functional requirements
- Edge cases
- Technical constraints
- Preferred patterns
- Success criteria
```

**Workflow:**
```bash
# Create comprehensive plan
/llm-workflow plan "[feature description]"

# Output includes:
# - Requirements breakdown
# - Task decomposition
# - Risk assessment
# - Testing strategy
```

This is "waterfall in 15 minutes" - rapid structured planning that prevents wasted cycles.

#### 2. Break Work Into Small Chunks

**Never request monolithic outputs.** Feed the LLM focused, manageable tasks one at a time.

**Bad:**
```
Write a complete authentication system with login, logout,
password reset, 2FA, session management, and user profiles.
```

**Good:**
```
Task 1: Create the login endpoint with basic validation
Task 2: Add password hashing utility
Task 3: Implement session token generation
...
```

```bash
# Auto-decompose large tasks
/llm-workflow decompose "Build authentication system"

# Execute one chunk at a time
/llm-workflow next-chunk
```

Each iteration carries forward context from completed work.

#### 3. Provide Extensive Context

**LLMs are only as good as the context you provide.** Supply all relevant information:

- Codebase sections (related files, patterns)
- API documentation
- Technical constraints
- Preferred approaches
- Examples of good solutions
- Warnings about problematic approaches

```bash
# Pack context for a task
/llm-workflow context-pack \
  --files "src/auth/**/*.ts" \
  --docs "docs/api.md" \
  --patterns "prefer-composition-over-inheritance"

# Include in prompt
/llm-workflow implement --with-context
```

**Context Loading Commands:**
```
/context add [file|directory|glob]
/context add-doc [url|file]
/context add-constraint "[constraint]"
/context add-pattern "[pattern-name]"
/context show
/context clear
```

#### 4. Choose the Right Model Strategically

Practice "model musical chairs" - different models have different strengths:

| Model | Strengths | Use For |
|-------|-----------|---------|
| Claude | Reasoning, nuance, long context | Architecture, complex logic |
| GPT-4 | Broad knowledge, tool use | Integration, APIs |
| Gemini | Multimodal, speed | Quick iterations, docs |

```bash
# Set preferred model for task type
/llm-workflow config set model.architecture "claude"
/llm-workflow config set model.quickfix "gemini"
/llm-workflow config set model.integration "gpt4"

# Auto-select based on task
/llm-workflow implement --auto-model

# Switch when stuck
/llm-workflow switch-model
```

**Tip:** Use the newest pro-tier versions for quality results. Switch models when one gets stuck.

#### 5. Leverage AI Agents Across Development Lifecycle

Modern AI coding agents (Claude Code, Cursor, Copilot Workspace) can read files, run tests, fix bugs, open PRs, and execute multi-step tasks autonomously. The question is not whether to delegate, but how much supervision to apply.

**Pair Mode — Interactive agent use:**
```bash
# Short, focused agent tasks with immediate review
/llm-workflow agent-task "Fix all TypeScript errors in src/"
/llm-workflow agent-task "Update tests for changed files"
/llm-workflow agent-review  # review before moving on
```

**Delegation Mode — Autonomous agent execution:**
```bash
# Well-scoped delegation with clear acceptance criteria
# (See "Delegation Mode" section below for full prompt template)
/llm-workflow delegate "Implement user auth API" --spec spec.md
/llm-workflow delegate-status   # check progress
/llm-workflow delegate-review   # review deliverable against spec
```

**Guardrails scale with scope, not with distrust:**

| Delegation Scope | Guardrails |
|-----------------|------------|
| Single file fix | Commit diff review |
| Feature implementation | Spec review + test pass + code review |
| Multi-service change | Spec review + integration tests + manual QA |
| Infrastructure/security | Never fully delegate; pair mode only |

#### 6. Maintain Human Accountability

**You own the output.** Review depth should scale with delegation scope, not default to maximum suspicion.

**Pair Mode review** — You reviewed as you went. A quick diff and test pass suffices:
```bash
/llm-workflow test --affected
/llm-workflow commit
```

**Delegation Mode review** — You delegated a scoped task. Review the deliverable against the spec:
```bash
# Review deliverable against acceptance criteria
/llm-workflow delegate-review --spec spec.md

# Run full test suite
/llm-workflow test --full

# Security review for sensitive areas
/llm-workflow review --security
```

**Review Checklist (adapt to scope):**
- [ ] Deliverable matches spec / acceptance criteria
- [ ] Tests pass (unit + integration as appropriate)
- [ ] No security regressions
- [ ] No unintended side effects outside scope
- [ ] Code style consistent with project conventions

**The operating model is "delegate, review, own"** — not "distrust and micromanage." A well-scoped prompt with clear acceptance criteria earns a deliverable-level review, not a line-by-line audit.

#### 7. Use Frequent, Granular Git Commits

Treat commits as **save points in a game**. Commit after each small task completes successfully.

```bash
# After each successful chunk
/llm-workflow commit --auto-message

# With verification
/llm-workflow commit --verify-tests

# Quick save point
/llm-workflow checkpoint "Implemented login validation"
```

**Benefits:**
- Easy rollback if AI suggestions introduce bugs
- Clear documentation of development progression
- Safe experimentation with AI suggestions

#### 8. Customize AI Behavior

Create rules files to guide AI outputs toward team idioms and patterns.

**CLAUDE.md / GEMINI.md / .cursorrules:**
```markdown
# Project Conventions

## Code Style
- Use functional components with hooks (React)
- Prefer composition over inheritance
- Maximum function length: 50 lines
- Always include error boundaries

## Patterns
- Repository pattern for data access
- Service layer for business logic
- DTOs for API responses

## Avoid
- Any usage of `any` type
- Nested ternaries
- Magic numbers without constants

## Testing
- Unit tests for all business logic
- Integration tests for API endpoints
- Minimum 80% coverage
```

```bash
# Initialize rules file
/llm-workflow init-rules

# Add convention
/llm-workflow add-rule "Always use async/await over .then()"

# Validate against rules
/llm-workflow validate-rules
```

#### 9. Embrace Strong Testing and Automation

Robust CI/CD pipelines **enhance AI productivity**. Automated feedback loops help refine outputs.

```bash
# Setup automation integration
/llm-workflow setup-ci

# Run full quality pipeline
/llm-workflow quality-check

# This runs:
# - Linters
# - Type checkers
# - Unit tests
# - Integration tests
# - Security scans
```

**Pipeline Feedback:**
```bash
# Feed CI results back to AI for fixes
/llm-workflow fix-ci-errors

# Auto-iterate until passing
/llm-workflow iterate-until-green --max-attempts 3
```

#### 10. Continuous Learning

Using AI doesn't dull skills - it **amplifies existing expertise**. LLMs reward best practices.

```bash
# Review AI code to learn
/llm-workflow explain-code

# Debug AI mistakes
/llm-workflow analyze-error

# Track learning moments
/llm-workflow log-learning "[insight]"
```

**Learning Practices:**
- Review every piece of AI-generated code
- Understand why AI made specific choices
- Debug AI mistakes yourself first
- Keep notes on effective prompts

---

### Delegation Mode: Spec-Driven Autonomous Execution

When a task is well-scoped with clear acceptance criteria, delegation mode lets you hand off implementation to an AI agent and review the deliverable rather than supervising each step.

#### Pre-Delegation Checklist

Before delegating, verify:
- [ ] Task has a clear, bounded scope (one feature, one fix, one refactor)
- [ ] Acceptance criteria are explicit and testable
- [ ] The codebase has existing tests or a test framework in place
- [ ] No security-sensitive changes (auth, payments, PII handling) — those stay in pair mode
- [ ] You can verify the deliverable against the spec without reading every line

#### Delegation Prompt Template

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

#### Three-Tier Execution Model

For complex projects, delegation can span multiple tiers:

| Tier | Tool | Role | Example |
|------|------|------|---------|
| **Design** | Cowork / Chat | Architecture, spec writing, review | "Design the auth system" |
| **Build** | Claude Code | Autonomous implementation | "Implement the auth API per spec.md" |
| **Scale** | Ruflo / Swarms | Parallel multi-agent execution | "Run 4 agents: API, tests, docs, migration" |

The spec is the handoff document between tiers. Write it in tier 1, execute it in tier 2, scale it in tier 3.

#### Recovery from Delegation Failures

When a delegated task goes off-track:
1. **Stop** — don't let it compound. `git stash` or `git reset` to last checkpoint.
2. **Diagnose** — was the spec ambiguous? Was context missing? Was the task too broad?
3. **Adjust** — tighten the spec, add missing context, break into smaller delegations.
4. **Re-delegate or switch to pair mode** depending on diagnosis.

---

### Workflow Commands

#### Planning Phase

```bash
# Start new feature planning
/llm-workflow plan "[feature]"

# Create detailed spec
/llm-workflow spec "[feature]" --include-edge-cases

# Decompose into tasks
/llm-workflow decompose --output-format todolist

# Generate project plan
/llm-workflow project-plan --milestones
```

#### Implementation Phase

```bash
# Implement current chunk
/llm-workflow implement

# Implement specific task
/llm-workflow implement "[task description]"

# With full context
/llm-workflow implement --with-context --verify

# Quick iteration
/llm-workflow iterate "[feedback]"
```

#### Review Phase

```bash
# Full review
/llm-workflow review

# Security-focused review
/llm-workflow review --security

# Performance review
/llm-workflow review --performance

# Review with comparison
/llm-workflow review --compare-original
```

#### Testing Phase

```bash
# Run affected tests
/llm-workflow test

# Generate missing tests
/llm-workflow test --generate

# Coverage check
/llm-workflow test --coverage

# Full test suite
/llm-workflow test --full
```

#### Commit Phase

```bash
# Commit with auto-generated message
/llm-workflow commit

# Commit specific files
/llm-workflow commit [files...] --message "[msg]"

# Create checkpoint
/llm-workflow checkpoint "[description]"

# Rollback last change
/llm-workflow rollback
```

---

### In-Session Commands

#### Context Commands
```
/context add [path]          Add file/directory to context
/context add-doc [url]       Add documentation
/context add-pattern [name]  Add pattern constraint
/context show                Show current context
/context clear               Clear context
/context pack                Bundle for LLM
```

#### Model Commands
```
/model switch [name]         Switch to specific model
/model auto                  Auto-select based on task
/model compare               Compare outputs from multiple models
```

#### Chunk Commands
```
/chunk list                  Show all chunks
/chunk current               Show current chunk
/chunk next                  Move to next chunk
/chunk skip                  Skip current chunk
/chunk done                  Mark chunk complete
```

#### Review Commands
```
/review code                 Review current changes
/review security             Security-focused review
/review perf                 Performance review
/review all                  Comprehensive review
```

#### Git Commands
```
/save                        Quick commit checkpoint
/rollback                    Undo last change
/history                     Show session history
/diff                        Show current changes
```

---

### Configuration

#### Basic Configuration

Create `.llm-workflow.json`:

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

#### Complete Configuration

```json
{
  "workflow": {
    "planning": {
      "required": true,
      "includeEdgeCases": true,
      "includeRisks": true,
      "template": "detailed"
    },

    "chunks": {
      "size": "small",
      "maxLines": 50,
      "autoDecompose": true,
      "carryContext": true
    },

    "context": {
      "autoLoad": true,
      "includePatterns": true,
      "includeDocs": true,
      "maxTokens": 100000
    },

    "models": {
      "default": "claude",
      "architecture": "claude",
      "quickfix": "gemini",
      "integration": "gpt4",
      "autoSwitch": true
    },

    "review": {
      "required": true,
      "security": true,
      "performance": true,
      "beforeCommit": true
    },

    "commits": {
      "granular": true,
      "autoMessage": true,
      "verifyTests": true,
      "signOff": false
    },

    "testing": {
      "autoRun": true,
      "framework": "jest",
      "coverage": {
        "minimum": 80,
        "enforce": true
      }
    },

    "rules": {
      "file": "CLAUDE.md",
      "enforce": true,
      "validate": true
    },

    "learning": {
      "logInsights": true,
      "trackPatterns": true,
      "exportNotes": true
    }
  }
}
```

---

### Real-World Examples

#### Example 1: New Feature Implementation

**Task:** Add user profile editing with avatar upload

```bash
# Step 1: Plan
/llm-workflow plan "User profile editing with avatar upload"

# Output:
# - Requirements: Edit name, email, bio, avatar
# - Constraints: Max 5MB avatar, image validation
# - Tasks: 8 chunks identified
# - Risks: File upload security, image processing

# Step 2: Context
/context add src/components/Profile/
/context add src/services/user.ts
/context add-doc "docs/api/users.md"
/context add-pattern "use-react-hook-form"

# Step 3: Implement chunk by chunk
/llm-workflow implement "Create ProfileEditForm component structure"
/llm-workflow test
/llm-workflow commit

/llm-workflow implement "Add form validation with react-hook-form"
/llm-workflow test
/llm-workflow commit

/llm-workflow implement "Create avatar upload component"
/llm-workflow review --security
/llm-workflow test
/llm-workflow commit

# Continue until complete...

# Step 4: Final review
/llm-workflow review --all
/llm-workflow test --coverage
```

#### Example 2: Bug Fix

**Task:** Fix memory leak in event handlers

```bash
# Step 1: Gather context
/context add src/hooks/useEventListener.ts
/context add src/components/Dashboard.tsx

# Step 2: Analyze with AI
/llm-workflow debug "Memory leak - listeners not cleaned up"

# Step 3: Get fix suggestion
/llm-workflow implement "Add cleanup in useEffect"

# Step 4: Verify
/llm-workflow review
/llm-workflow test
/llm-workflow commit "fix: add cleanup for event listeners"
```

#### Example 3: Code Refactoring

**Task:** Modernize legacy utility file

```bash
# Step 1: Plan refactoring
/llm-workflow plan "Modernize utils.js - callbacks to async/await"

# Step 2: Safety first
/llm-workflow test --generate  # Generate tests for current behavior
/llm-workflow commit "test: add tests before refactor"

# Step 3: Refactor in chunks
/llm-workflow implement "Convert getUserData callback to async"
/llm-workflow test
/llm-workflow commit

/llm-workflow implement "Convert fetchOrders callback to async"
/llm-workflow test
/llm-workflow commit

# Step 4: Verify no regression
/llm-workflow test --full
/llm-workflow review --compare-original
```

#### Example 4: API Development

**Task:** Build REST API for blog posts

```bash
# Step 1: Design with AI
/llm-workflow plan "REST API for blog posts CRUD"

# Step 2: Generate OpenAPI spec
/llm-workflow implement "OpenAPI specification for blog posts"
/llm-workflow commit

# Step 3: Implement endpoints
/llm-workflow implement "POST /api/posts with validation"
/llm-workflow test --generate
/llm-workflow commit

/llm-workflow implement "GET /api/posts with pagination"
/llm-workflow test
/llm-workflow commit

# Continue for remaining endpoints...

# Step 4: Security review
/llm-workflow review --security
/llm-workflow implement "Add rate limiting and input sanitization"
/llm-workflow test
/llm-workflow commit
```

---

### Best Practices

#### Session Practices
1. **Always plan first** - 15 minutes of planning saves hours of rework
2. **Small chunks** - If it feels big, break it down more
3. **Context is king** - More relevant context = better output
4. **Switch models** - Don't get stuck; try a different model
5. **Review everything** - You're the accountable engineer
6. **Commit often** - Every passing test is a save point

#### Anti-Patterns to Avoid
1. **Monolithic requests** - "Build me the entire feature"
2. **Blind trust** - Accepting AI code without review
3. **Context starvation** - Not providing enough information
4. **Model loyalty** - Sticking with one model when stuck
5. **Big commits** - Waiting until everything is done
6. **Skipping tests** - "It looks right" is not verification

#### When to Switch Models

| Situation | Action |
|-----------|--------|
| AI gives repetitive wrong answers | Switch model |
| Complex reasoning needed | Try Claude |
| Need speed for iterations | Try Gemini |
| API/integration task | Try GPT-4 |
| Getting stuck | Always try switching |

---

### Troubleshooting

#### AI Producing Poor Output
1. Check context completeness
2. Verify clear task description
3. Try switching models
4. Break task into smaller chunks
5. Add more examples/patterns

#### Tests Failing After AI Changes
1. Use `/rollback` to restore
2. Review the failing change
3. Ask AI to explain its approach
4. Implement manually with AI guidance
5. Commit more granularly next time

#### Context Too Large
1. Focus on most relevant files
2. Summarize documentation
3. Use pattern references instead of examples
4. Split into multiple sessions

#### Model Not Following Conventions
1. Check rules file is loaded
2. Explicitly state conventions in prompt
3. Provide examples of correct style
4. Add constraints to context

---

### Integration with Tools

#### Context Packaging
- **gitingest** - Bundle repo for LLM ingestion
- **repo2txt** - Convert repo to text
- **Context7** - Advanced context management

#### Code Review
- **Chrome DevTools MCP** - Debugging and quality loops
- Built-in `/review` commands

#### Testing
- Integrates with Jest, pytest, Mocha, etc.
- Auto-generates tests with `/test --generate`
- Coverage tracking with `/test --coverage`

---

### Environment Variables

```bash
export LLM_WORKFLOW_MODEL="claude"
export LLM_WORKFLOW_CHUNK_SIZE="small"
export LLM_WORKFLOW_AUTO_COMMIT="true"
export LLM_WORKFLOW_HUMAN_REVIEW="required"
export LLM_WORKFLOW_TEST_FRAMEWORK="jest"
```

---

### Summary

The LLM Coding Workflow treats AI as a capable collaborator whose autonomy scales with prompt quality. Success comes from:

1. **Plan** — Rapid structured planning before coding
2. **Chunk** — Small, focused tasks (pair mode) or well-scoped delegations (delegation mode)
3. **Context** — Extensive relevant information; context injection beats model upgrades
4. **Select** — Right model for the task
5. **Delegate or Pair** — Match the collaboration mode to the task
6. **Own** — Review depth scales with scope; you own the output regardless
7. **Commit** — Granular save points for easy rollback
8. **Learn** — Continuous skill amplification through AI collaboration

**The operating model is "delegate, review, own."** AI coding agents are force multipliers — and increasingly, autonomous executors of well-scoped work. The human engineer sets direction, writes specs, reviews deliverables, and takes accountability.

---

### Sources & References

This skill synthesizes insights from the following sources:

**Primary Frameworks:**
- Addy Osmani, "The Human Side of AI Coding: Conductor vs. Orchestrator" (addyosmani.com, 2025) — Two collaboration modes framework
- Addy Osmani, "LLM Coding Workflow" (addyosmani.com, 2025) — 10 principles for AI-augmented development
- Harper Reed, "My LLM Codegen Workflow" (harper.blog, 2025) — Spec-driven pipeline: spec.md → plan.md → prompt_plan.md

**Research & Industry Reports:**
- Anthropic, "Agentic Coding Trends in 2026" (code.claude.com/blog, 2026) — Agents complete ~20 actions autonomously; "delegate, review, own" operating model
- Anthropic, "Claude Code Best Practices" (docs.anthropic.com, 2025-2026) — CLAUDE.md conventions, context injection, permission management
- Greptile, "Measuring the Impact of AI on Developer Productivity" (greptile.com, 2025) — Quantitative analysis of AI coding impact

**Practitioner Accounts:**
- Eric Porres, "How I Built a WhatsApp MCP Server Without Writing a Single Line of Code" (promptedbyeric.substack.com, 2026) — Real-world Cowork → Claude Code delegation pattern
- Geoffrey Huntley, "Ultra-Large Context and Agentic Coding" (ghuntley.com, 2025) — Context window management strategies

**Related Skills:**
- `builder-playbook` — Project archetypes, scaffold recipes, and infrastructure patterns extracted from 45+ projects
- `github-playbook` — Git workflow, multi-machine setup, daily development patterns

### Related Commands

- `/llm-workflow --help` - Show all commands
- `/llm-workflow config` - Configuration management
- `/llm-workflow init` - Initialize in project
- `/llm-workflow status` - Show current session status
