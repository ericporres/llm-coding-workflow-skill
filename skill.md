---
name: LLM Coding Workflow
description: AI-augmented software engineering workflow based on Addy Osmani's methodology. Implements structured planning, chunked implementation, extensive context provision, strategic model selection, human oversight, granular commits, and continuous learning. Treats LLMs as pair programmers requiring clear direction rather than autonomous agents.
---

# LLM Coding Workflow

AI-augmented software engineering workflow that maximizes LLM effectiveness through structured processes, clear context, and human oversight.

## What This Skill Does

This skill implements Addy Osmani's proven LLM coding workflow methodology, treating AI as a powerful pair programmer that requires clear direction, context, and oversight. It provides a systematic approach to AI-assisted development that prevents wasted cycles and ensures high-quality output.

**Core Philosophy**: AI-augmented engineering, not autonomous AI coding. The human engineer remains the director; the AI is a capable collaborator.

**Key Capabilities:**
- **Structured Planning**: Rapid waterfall-style planning before implementation
- **Chunked Execution**: Small, focused tasks with quick course correction
- **Context Management**: Systematic context provision for optimal AI output
- **Model Selection**: Strategic switching between models based on task needs
- **Human Oversight**: Review-first approach treating AI output as junior developer code
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

CLI tools can handle multiple tasks: reading files, running tests, fixing bugs, opening PRs.

```bash
# Let agent handle routine tasks
/llm-workflow agent-task "Fix all TypeScript errors in src/"
/llm-workflow agent-task "Update tests for changed files"
/llm-workflow agent-task "Generate PR description"

# But ALWAYS maintain supervision
/llm-workflow agent-status
/llm-workflow agent-review
```

**Critical:** Never let agents run unattended for extended periods.

#### 6. Maintain Human Oversight

**Never blindly trust LLM output.** Treat AI-generated code as if it came from a junior developer.

```bash
# Review all generated code
/llm-workflow review --thorough

# Run tests after every change
/llm-workflow test --affected

# Verify before accepting
/llm-workflow verify
```

**Review Checklist:**
- [ ] Logic correctness
- [ ] Edge case handling
- [ ] Security implications
- [ ] Performance considerations
- [ ] Code style consistency
- [ ] Test coverage

You remain the **accountable engineer** regardless of AI involvement.

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

The LLM Coding Workflow treats AI as a powerful pair programmer, not an autonomous coder. Success comes from:

1. **Plan** - Rapid structured planning before coding
2. **Chunk** - Small, focused tasks
3. **Context** - Extensive relevant information
4. **Select** - Right model for the task
5. **Oversee** - Human review of all output
6. **Commit** - Granular save points
7. **Learn** - Continuous skill amplification

**Remember:** AI coding assistants are incredible force multipliers, but the human engineer remains the director of the show.

---

### Related Commands

- `/llm-workflow --help` - Show all commands
- `/llm-workflow config` - Configuration management
- `/llm-workflow init` - Initialize in project
- `/llm-workflow status` - Show current session status
