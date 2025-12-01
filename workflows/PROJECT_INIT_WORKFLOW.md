# Project Initialization Workflow

> **Runtime**: Claude Code
> **Recommended Orchestrator**: Opus (for multi-agent coordination)
> **Sub-agents**: Sonnet or Haiku (for specialized tasks)

Transform this template into a project-specific system through agent-orchestrated phases with user approval gates.

## Overview

| Aspect | Description |
|--------|-------------|
| **Purpose** | Transform template into project-specific system |
| **Pattern** | Sequential agent orchestration with incremental complexity |
| **Approval** | User approval required at each phase gate |

## Agent Role Definitions

This workflow uses **abstract roles** instead of specific agent names for portability. During Phase 0, Claude Code discovers available specialists and maps them to these roles.

| Role | Capability | Phase | Example Agents |
|------|------------|-------|----------------|
| `research` | Web research, evidence synthesis, context gathering | 1 | context-engineer, researcher |
| `architecture` | ADRs, system design, MVP scoping | 2 | project-architect, ml-systems-architect |
| `implementation` | Code writing, testing, domain expertise | 3-4 | ai-engineer, backend-engineer, data-engineer, agentic-engineer |
| `review` | Test enforcement, code quality auditing | 3-4 | python-code-quality-auditor |
| `domain-expert` | Business alignment, PRD compliance, user value validation | 3-4 feedback | Inferred from PRODUCT.md domain; product-manager as fallback |
| `architecture-reviewer` | Over-engineering detection, ADR compliance, pattern validation | 3-4 feedback | project-architect, ml-systems-architect |

**Role Matching Keywords**:

| Role | Keywords in Agent Description |
|------|-------------------------------|
| research | research, context, synthesis, investigation, analysis |
| architecture | architect, design, planning, ADR, system, infrastructure |
| implementation | engineer, developer, implement, code, build |
| review | review, audit, quality, test, enforce, validate, check, gate |
| domain-expert | domain, business, product, user, requirements, stakeholder |
| architecture-reviewer | architect, design, patterns, system, simplicity, complexity |

**Note**: The `implementation` role supports **multiple specialists** - different agents can be assigned to different features based on domain expertise.

---

## Discovered Agents

*This section is updated in place during Phase 0.*

| Role | Selected Agent(s) | Source | Status |
|------|-------------------|--------|--------|
| research | [pending discovery] | - | ⏳ |
| architecture | [pending discovery] | - | ⏳ |
| implementation | [pending discovery] | - | ⏳ |
| review | [pending discovery] | - | ⏳ |
| domain-expert | [pending discovery] | - | ⏳ |
| architecture-reviewer | [pending discovery] | - | ⏳ |

---

## Workflow State

*Updated automatically during workflow execution. Clear this section after workflow completion or when starting fresh.*

| Field | Value |
|-------|-------|
| Current Phase | - |
| Current Step | - |
| Last Updated | - |

### Created Agents (pending validation)

*List agents created in Phase 0.3, pending restart and validation.*

| Agent File | Role | Status |
|------------|------|--------|
| - | - | - |

### Linked Skills

*Skills linked to created agents (if any).*

| Agent | Skills |
|-------|--------|
| - | - |

---

## Phase 0: Agent Discovery & Setup

Before starting the workflow, Claude Code discovers available specialists, identifies gaps, and optionally creates missing specialists tailored to the project.

```
Phase 0: Agent Discovery & Setup
├── 0.1: Scan & Match
├── 0.2: Gap Analysis & User Choice
│   ├── [Create] → 0.3
│   ├── [Generalist] → Phase 1
│   └── [Hybrid] → 0.3 for selected roles
└── 0.3: Specialist Creation (conditional)
```

---

### Phase 0.1: Scan & Match

**Actions**:
1. Scan for agents at:
   - Project level: `.claude/agents/*.md`
   - User level: `~/.claude/agents/*.md`
2. Read agent descriptions and match to roles using keywords (see Role Matching Keywords table above)
3. Present candidates to user

**Output**:

```
Phase 0.1: Agent Discovery
──────────────────────────

Scanning for specialists...

✅ Found:
   implementation: backend-engineer (user) - "APIs and databases"
   review:         python-code-quality-auditor (user) - "Quality gate"

❌ Missing:
   research:       No agent found
   architecture:   No agent found
```

If all roles have matches → Present for approval, update "Discovered Agents" table, proceed to Prerequisites.

If any roles missing → Proceed to Phase 0.2.

#### Domain Expert Selection

The `domain-expert` role requires special handling - it should match the project's domain, not just generic keywords.

**Selection Logic**:

1. Analyze `context/PRODUCT.md` for domain keywords
2. Map to appropriate specialist agent
3. If no clear match, ask user

```
Analyzing PRODUCT.md for domain...

Domain keywords found:
- "machine learning", "model training" → ML domain
- "e-commerce", "checkout", "cart" → Commerce domain
- "healthcare", "patient", "clinical" → Healthcare domain
- "API", "backend", "database" → Backend/infrastructure domain

Suggested domain expert:
┌─────────────────────────────────────────────────────────┐
│ Domain: ML/Data Science                                 │
│ Recommended: data-science-modeler (user)                │
│ Reason: Keywords "model training", "pipeline" in PRD    │
└─────────────────────────────────────────────────────────┘

Accept this selection? [Y/n/specify different]
```

**Domain-to-Agent Mapping**:

| Domain Keywords | Suggested Agent | Fallback |
|-----------------|-----------------|----------|
| ML, model, training, prediction | data-science-modeler, ml-research-expert | product-manager |
| API, backend, database, service | backend-engineer | product-manager |
| UI, frontend, component, user interface | frontend-engineer | product-manager |
| Data, ETL, pipeline, warehouse | data-engineer | product-manager |
| Agent, autonomous, LLM, RAG | ai-engineer, agentic-engineer | product-manager |
| (No clear match) | Ask user | product-manager |

**If user must specify**:

```
No clear domain match found in PRODUCT.md.

What domain expertise is most relevant for validating this project?
Examples: "ML/data science", "backend services", "e-commerce", "healthcare"

Domain: _
```

---

### Phase 0.2: Gap Analysis & User Choice

When roles are missing, present options to the user:

```
Phase 0.2: Gap Resolution
─────────────────────────

For missing roles, choose an option:

[1] Create specialists (recommended)
    → I'll design agents based on your PRODUCT.md and ENGINEERING.md
    → Follows Anthropic best practices
    → Requires Claude Code restart after creation

[2] Proceed with generalists
    → Claude Code handles these roles directly
    → Works but less specialized
    → No restart needed

[3] Hybrid approach
    → Select which roles get specialists vs. generalists

Your choice: _
```

**If user chooses [1] Create** → Proceed to Phase 0.3 for all missing roles.

**If user chooses [2] Generalist** → Update "Discovered Agents" table with "generalist" entries, proceed to Prerequisites.

**If user chooses [3] Hybrid** → Ask which roles to create specialists for:

```
Which roles should get specialists? (comma-separated)
Missing: research, architecture

Create specialists for: _
```

Then proceed to Phase 0.3 for selected roles, mark others as "generalist".

---

### Phase 0.3: Specialist Creation

**Prerequisite**: User must have filled out `context/PRODUCT.md` and `context/ENGINEERING.md` seed documents.

#### 0.3.1: Analyze Seeds for Domain Context

Read seed documents and extract:

| From PRODUCT.md | Extract |
|-----------------|---------|
| Problem statement | Domain keywords |
| Target users | Interaction patterns |
| Core requirements | Tool/skill needs |
| Success criteria | Validation focus |

| From ENGINEERING.md | Extract |
|---------------------|---------|
| Tech stack | Specific technology expertise |
| Constraints | What agent should avoid/prefer |
| Architecture patterns | DDD, event-driven, etc. |
| Non-functional requirements | Performance, security focus |

#### 0.3.2: Fetch Best Practices

Use `claude-code-guide` subagent to fetch Anthropic documentation on:
- Agent file structure and frontmatter
- Effective agent prompts
- Skill integration patterns

#### 0.3.3: Draft Agent Files

Generate agent markdown files following this structure:

```markdown
---
name: [role]-specialist
description: [One-line domain-specific description]
---

# [Role] Specialist for [Project Name]

You are a [role] specialist for [domain] projects.

## Expertise
[Derived from seeds - specific technologies, patterns, domain knowledge]

## Responsibilities
[Role-specific tasks from workflow]

## Constraints
[From ENGINEERING.md - what to avoid, preferences]

## Skills
[Optional: linked skills if user requested]
```

#### 0.3.4: Present Drafts for User Approval

```
Phase 0.3.4: Agent Draft Review
───────────────────────────────

I've drafted the following specialists based on your seeds:

┌─────────────────────────────────────────────────────────┐
│ research-specialist.md                                   │
├─────────────────────────────────────────────────────────┤
│ Description: Research ML pipeline technologies and       │
│ synthesize evidence on [domain] patterns                 │
│                                                         │
│ Expertise: [tech stack from seeds]                      │
│ Skills: [suggested skills]                              │
└─────────────────────────────────────────────────────────┘

[View full draft? Y/n]

Options:
[1] Approve all drafts
[2] Modify a draft (opens for editing)
[3] Regenerate with different focus
[4] Cancel specialist creation
```

#### 0.3.5: Skills Configuration

Before creating agent files, ask about skills:

```
Skills Configuration
────────────────────

Would you like to link specific skills to these agents?

Options:
[1] Auto-suggest skills based on project domain
    → I'll recommend skills matching your seeds
[2] Manually specify skills for each agent
[3] Skip - let Claude Code auto-load skills as needed
[4] Create new project-specific skills
    → I'll help design skills tailored to this project (sub-workflow)

Your choice: _
```

If user chooses [4], trigger skill creation sub-workflow before continuing.

#### 0.3.6: Create Agent Files

Create approved agents in `.claude/agents/`:

```
Creating agents...
  ✅ .claude/agents/research-specialist.md
  ✅ .claude/agents/architecture-specialist.md

Updating workflow state...
```

Update the "Discovered Agents" table with created agents (status: "created, pending validation").

#### 0.3.7: Update Workflow State

Update the "Workflow State" section (below) with:
- Current phase: 0.3
- Current step: pending_restart
- Created agents list
- Linked skills (if any)

#### 0.3.8: Prompt User to Restart

```
───────────────────────────────────────────────────────────
Agent files created successfully!

Claude Code needs to restart to load the new agents.

Please:
1. Exit this session (Ctrl+C or type 'exit')
2. Restart Claude Code in this directory
3. Say: "Continue project initialization workflow"

Progress saved to this file (Workflow State section below).
───────────────────────────────────────────────────────────
```

#### 0.3.9: Post-Restart Validation

After user restarts and continues, validate each created agent:

```
Phase 0.3.9: Agent Validation
─────────────────────────────

Validating created agents...

research-specialist:
  Invoking: "Introduce yourself in one sentence."
  Response: "I research ML pipeline technologies, synthesize
            evidence on XGBoost and FastAPI patterns, and prepare
            knowledge packages for architecture decisions."
  ✅ Agent responds correctly.

architecture-specialist:
  Invoking: "Introduce yourself in one sentence."
  Response: "I design system architectures for data pipelines,
            create ADRs, and define MVP scope with MoSCoW prioritization."
  ✅ Agent responds correctly.
```

**If validation fails**:

```
❌ Agent 'research-specialist' failed to load.
   Error: [error message]

   Options:
   [1] Review and fix the agent file manually
   [2] Delete and recreate
   [3] Skip - use generalist for this role
```

#### 0.3.10: Finalize Discovery

After validation:
1. Update "Discovered Agents" table with final status (✅)
2. Clear "Workflow State" section (workflow complete)
3. Proceed to Prerequisites

---

### Generalist Fallback Behavior

When proceeding with generalists (by choice or after failed creation):

- Claude Code handles the role directly without a specialist agent
- Quality is acceptable but less domain-specific
- User can create specialists later and re-run Phase 0

Update "Discovered Agents" table:
```markdown
| research | generalist (Claude Code) | - | ✅ |
```

---

## Inter-Phase Communication

Templates in `workflows/templates/` serve as **structured contracts** between phases:

| Template | Output Location | Produced By | Consumed By |
|----------|-----------------|-------------|-------------|
| `TEMPLATE-PRD.md` | `context/PRD.md` | `research` role (Phase 1) | `architecture` role (Phase 2) |
| `TEMPLATE-ADR.md` | `ADR.md` (root) | `architecture` role (Phase 2) | `implementation` role (Phase 3) |
| `TEMPLATE-PROJECT-PLAN.md` | `context/PROJECT_PLAN.md` | `architecture` role (Phase 2) | `implementation` role (Phase 3-4) |

Templates remain in `workflows/templates/` as reusable scaffolds.

**Why templates matter:**
- **Predictable handoffs**: Each phase knows exactly what format to expect
- **Agent independence**: Agents don't need to negotiate formats
- **Human readability**: User can review outputs at each approval gate

---

## Prerequisites

Before running this workflow:

1. **Fill out seed documents** in `context/`:
   - `context/PRODUCT.md` - Product/business perspective (what & why)
   - `context/ENGINEERING.md` - Technical perspective (how & constraints)

2. **Run Phase 0**: Discover and confirm available specialists (or accept generalist fallback)

3. **Support files** in `workflows/`:
   - Templates for agent outputs
   - Agent specifications

---

## Phase 1: Context Research

**Role**: `research`
**Agent**: *[See Discovered Agents table]*

**Input**:
- `context/PRODUCT.md` - User-provided product seed
- `context/ENGINEERING.md` - User-provided engineering seed

**Actions**:
1. Read user-provided seeds
2. Research technologies, patterns, best practices
3. Grade evidence quality (High/Medium/Low confidence)
4. Expand seeds into full PRD using `workflows/templates/TEMPLATE-PRD.md`
5. Synthesize findings into structured document

**Output**:
- `context/PRD.md` - Expanded PRD based on product seed
- `context/RESEARCH_SYNTHESIS.md` - Research synthesis

**Success Criteria**: Research synthesis with sourced, graded claims

### [APPROVAL GATE 1]

User reviews:
- Expanded PRD in `context/PRD.md`
- Research synthesis document

**Options**:
- ✅ **Approve** - Proceed to Phase 2
- 🔄 **Request more research** - Specify areas needing deeper investigation
- ✏️ **Modify scope** - Adjust seeds and re-run

---

## Phase 2: Architecture Planning

**Role**: `architecture`
**Agent**: *[See Discovered Agents table]*

**Input**:
- `context/PRD.md` - Expanded PRD from Phase 1
- Research synthesis from Phase 1
- `context/ENGINEERING.md` - Technical preferences
- Template skeleton (pyproject.toml, ADR.md, README.md, src/)

**Actions**:
1. Create ADRs using `workflows/templates/TEMPLATE-ADR.md`
2. Define MVP scope using MoSCoW prioritization
3. Plan incremental feature roadmap using `workflows/templates/TEMPLATE-PROJECT-PLAN.md`
4. Customize template (pyproject.toml, README.md)

**Output**:
- `ADR.md` - Overwritten with project-specific decisions
- `context/PROJECT_PLAN.md` - MVP scope + feature roadmap

**Success Criteria**: Clear MVP definition, actionable roadmap

### [APPROVAL GATE 2]

User reviews:
- Architecture decisions in `ADR.md`
- MVP scope and feature roadmap in `context/PROJECT_PLAN.md`

**Options**:
- ✅ **Approve** - Proceed to Phase 3
- 🔧 **Adjust scope** - Modify MVP boundaries
- 📊 **Change priorities** - Reorder feature roadmap

---

## Phase 3: MVP Implementation

**Roles**: `implementation` + `review`
**Agents**: *[See Discovered Agents table - may use multiple specialists]*

Phase 3 uses a structured **Implement → Review → Fix** loop for each deliverable to enforce test creation and code quality.

Select implementation agent based on project type:

| Project Type | Recommended Specialty |
|--------------|----------------------|
| RAG, embeddings, LLM integration | AI/ML specialist |
| APIs, databases, DDD | Backend specialist |
| Autonomous agents | Agentic specialist |
| ETL, data pipelines | Data specialist |

**Input**:
- Approved `ADR.md` and `context/PROJECT_PLAN.md` from Phase 2
- Customized template skeleton

---

### 3.1 Implementation Sub-phase

**Role**: `implementation`

**Actions**:
1. Implement one deliverable (feature/module) from PROJECT_PLAN.md
2. Write unit tests following naming convention: `test__<what>__<expected>`
3. Ensure tests are behavioral (test outcomes, not mocks)
4. Run `make test` locally to verify tests pass
5. Mark deliverable as "Ready for Review"

**Test Requirements**:
- Every new module must have corresponding unit tests
- Tests must follow naming convention from CLAUDE.md
- Minimum 80% coverage for new code
- No tautological tests (testing mocks, assignment, or nothing)

---

### 3.2 Review Sub-phase

**Role**: `review` (read-only)

**Actions**:
1. Verify tests exist for all new implementation files
2. Check test naming conventions (`test__<what>__<expected>`)
3. Detect tautological tests (testing mocks instead of behavior)
4. Verify coverage >= 80% for new code
5. Check for hallucinated packages (imports that don't exist)
6. Scan for security vulnerabilities (bare except, SQL injection)
7. Generate review report

**Blocking Criteria** (must fix before progression):
- [ ] Tests exist for all new implementation files
- [ ] Tests follow `test__<what>__<expected>` naming convention
- [ ] No tautological tests detected
- [ ] Coverage >= 80% for new code
- [ ] No hallucinated package imports
- [ ] No security vulnerabilities

**Review Report Format**:
```
## Test & Quality Review Report
**Deliverable**: [name]
**Status**: PASS | FAIL
**Coverage**: X% (required: 80%)
**Review Cycle**: N of 3

### Blocking Issues (must fix)
### Warnings (should fix)
### Passed Checks
### Remediation Required
```

---

### 3.3 Fix Sub-phase (If Review Fails)

**Role**: `implementation`

**Actions**:
1. Address each blocking issue from remediation list
2. Add missing tests if required
3. Fix tautological tests to be behavioral
4. Re-run `make test` locally
5. Return to 3.2 for re-review

**Loop Control**:
- Maximum **3 fix cycles** per deliverable
- If still failing after 3 cycles → **escalate to user**

---

### Validation Loop Diagram

```
PER DELIVERABLE:

┌─────────────────┐
│   IMPLEMENT     │ ← implementation role (code + tests)
└────────┬────────┘
         │
         v
┌─────────────────┐
│     REVIEW      │ ← review role (read-only)
└────────┬────────┘
         │
    ┌────┴────┐
    │  PASS?  │
    └────┬────┘
     Yes │ No
         │  │
    ┌────┘  └────┐
    v            v
┌──────┐    ┌──────┐
│ DONE │    │ FIX  │ ← implementation role
└──────┘    └──┬───┘
               │
          ┌────┴────┐
          │Cycle < 3│
          └────┬────┘
           Yes │ No
               │  │
         (back to REVIEW)  → ESCALATE TO USER
```

---

### Escalation Behavior

If a deliverable fails review 3 times:

1. **Generate Escalation Report** with:
   - All persistent blocking issues
   - Summary of attempted fixes across cycles
   - Recommendations for resolution

2. **Halt workflow** and present to user

3. **User Options**:
   - ⚠️ **Override** - Proceed anyway (document risk in PR)
   - 🔧 **Manual fix** - User addresses issues directly
   - ⏭️ **Descope** - Move feature to Phase 4 or backlog
   - ✂️ **Split** - Break into smaller deliverables

---

### Deliverable Tracking

| Deliverable | Status | Cycle | Blocking Issues | Owner |
|-------------|--------|-------|-----------------|-------|
| *[Feature 1]* | Implementing | 0 | - | implementation |
| *[Feature 2]* | In Review | 1 | Missing tests for X | review |
| *[Feature 3]* | Passed | 2 | - | - |

---

**Output**: Working MVP with tests (all deliverables passed review)

**Success Criteria**:
- All deliverables pass review
- `make validate-branch` passes
- Coverage >= 80% for new code

---

### 3.4 Domain & Architecture Feedback

**Roles**: `domain-expert` + `architecture-reviewer` (read-only, sequential)
**Triggered**: After all deliverables pass code review, before phase gate approval.

This feedback gate ensures implementations are aligned with business outcomes and not over-engineered.

#### 3.4.1 Domain Expert Review (First)

**Role**: `domain-expert`

**Input**:
- Implemented code from Phase 3
- `context/PRD.md` - Business requirements
- `context/PROJECT_PLAN.md` - MVP scope

**Checks**:
1. Does implementation serve stated user needs from PRD?
2. Is it aligned with success criteria?
3. Does it address the core problem statement?
4. Are there unnecessary features not in scope?

**Report Format**:

```
## Domain Alignment Review
**Phase**: 3 (MVP)
**Status**: ALIGNED | MISALIGNED | OVER-SCOPED

### Alignment with PRD
- [ ] Addresses problem statement
- [ ] Serves target users
- [ ] Meets success criteria
- [ ] Within defined scope

### Concerns
[List any business/domain misalignments]

### Recommendations
[Suggested adjustments]
```

**If MISALIGNED or OVER-SCOPED** → Blocking. Implementation must fix issues before proceeding.

#### 3.4.2 Architecture Review (Second)

**Role**: `architecture-reviewer`

**Input**:
- Implemented code from Phase 3
- `ADR.md` - Architecture decisions
- Domain review findings (from 3.4.1)

**Checks**:
1. Does implementation follow patterns in ADR.md?
2. Is complexity justified by requirements?
3. Are there simpler alternatives for current phase?
4. Is it over-engineered for MVP scope?

**Over-Engineering Signals**:
- Abstractions without current use cases
- Premature optimization
- Features beyond Must-Have scope
- Configuration/flexibility not requested
- Layers/indirection without clear benefit

**Report Format**:

```
## Architecture Review
**Phase**: 3 (MVP)
**Status**: APPROPRIATE | OVER-ENGINEERED | UNDER-DESIGNED

### ADR Compliance
- [ ] Follows ADR-001: [decision]
- [ ] Follows ADR-002: [decision]
...

### Complexity Assessment
- Complexity level: [Simple | Moderate | Complex]
- Justified by requirements: [Yes | No]

### Over-Engineering Detected
[List any unnecessary complexity]

### Simplification Recommendations
[Concrete suggestions to reduce complexity]
```

**If OVER-ENGINEERED** → Blocking. Implementation must simplify before proceeding.

#### 3.4.3 Combined Feedback Summary

```
Phase 3.4: Domain & Architecture Feedback
─────────────────────────────────────────

┌─────────────────────────────────────────────────────────┐
│ DOMAIN REVIEW                                           │
│ Status: ALIGNED                                         │
│ ✓ Addresses problem statement                           │
│ ✓ Serves target users                                   │
│ ✓ Within MVP scope                                      │
└─────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────┐
│ ARCHITECTURE REVIEW                                     │
│ Status: OVER-ENGINEERED                                 │
│ ✗ Abstract factory pattern not needed for 2 variants   │
│ ✗ Configuration system beyond requirements             │
│                                                         │
│ Recommendations:                                        │
│ 1. Replace factory with simple if/else                  │
│ 2. Hardcode config values, add flexibility in Phase 4   │
└─────────────────────────────────────────────────────────┘

Blocking Issues: 2
Action Required: Simplify implementation before approval gate
```

#### 3.4.4 Fix Feedback Issues

**Role**: `implementation`

If domain or architecture review identifies blocking issues:

1. Address each blocking issue from feedback
2. Apply simplification recommendations
3. Re-run `make test` to ensure changes don't break tests
4. Return to 3.4.1 for re-review (max 2 feedback cycles)

**Loop Control**:
- Maximum **2 feedback cycles** per phase
- If still failing after 2 cycles → **escalate to user**

---

### [APPROVAL GATE 3]

User validates:
- MVP functionality (end-to-end)
- All deliverables passed code review loop
- Domain alignment review passed (ALIGNED)
- Architecture review passed (APPROPRIATE)
- Test coverage meets requirements

**Options**:
- ✅ **Approve** - Proceed to Phase 4
- 🐛 **Request fixes** - Specify additional issues to address
- ✏️ **Adjust scope** - Modify MVP boundaries
- 🔄 **Re-run feedback** - Request another domain/architecture review

---

## Phase 4: Iterative Enhancement

**Roles**: `implementation` + `review`
**Agents**: *[Same specialists from Phase 3, or different per feature]*

Phase 4 follows the same **Implement → Review → Fix** loop as Phase 3 for each feature in the roadmap.

**Input**:
- Working MVP from Phase 3
- Feature roadmap from `context/PROJECT_PLAN.md`

---

### 4.1 Feature Implementation

**Role**: `implementation`

**Actions**:
1. Select next feature from PROJECT_PLAN.md roadmap (by priority)
2. Implement feature with unit tests
3. Follow same test requirements as Phase 3:
   - Tests for all new modules
   - `test__<what>__<expected>` naming
   - 80% coverage for new code
   - Behavioral tests (not tautological)
4. Run `make test` locally
5. Mark feature as "Ready for Review"

---

### 4.2 Feature Review

**Role**: `review` (read-only)

**Actions**:
1. Run same validation as Phase 3.2 (tests, naming, coverage, quality)
2. Generate review report
3. If PASS → proceed to domain/architecture feedback
4. If FAIL → implementation agent fixes (max 3 cycles)

---

### 4.3 Domain & Architecture Feedback

**Roles**: `domain-expert` + `architecture-reviewer` (read-only, sequential)
**Triggered**: After feature passes code review, before feature approval.

Same structure as Phase 3.4, applied per feature:

#### 4.3.1 Domain Expert Review

**Checks**:
1. Does this feature serve stated user needs from PRD?
2. Is it aligned with roadmap priorities?
3. Does it integrate well with existing MVP?
4. Is it within the Should-Have/Could-Have scope for this phase?

**Report Format**: Same as Phase 3.4.1

**If MISALIGNED** → Blocking. Must fix before approval.

#### 4.3.2 Architecture Review

**Checks**:
1. Does feature follow established patterns from Phase 3?
2. Is complexity appropriate for a Phase 4 enhancement?
3. Does it introduce unnecessary technical debt?
4. Is it over-engineered for the stated requirement?

**Report Format**: Same as Phase 3.4.2

**If OVER-ENGINEERED** → Blocking. Must simplify before approval.

#### 4.3.3 Fix Feedback Issues

If blocking issues found:
1. Address domain/architecture concerns
2. Re-run tests
3. Return to 4.3.1 for re-review (max 2 cycles)

---

### 4.4 Feature Approval

### [APPROVAL GATE per feature]

After code review AND domain/architecture feedback pass, user validates:
- Feature functionality
- Code review report shows PASS
- Domain alignment review passed (ALIGNED)
- Architecture review passed (APPROPRIATE)
- Integration with existing MVP

**Options**:
- ✅ **Approve** - Add next feature from roadmap
- 🔄 **Request changes** - Modify current feature
- ⏭️ **Skip to next** - Move to next roadmap item
- 🛑 **Stop** - End enhancement phase
- 🔄 **Re-run feedback** - Request another domain/architecture review

---

**Output**: Enhanced system with additional features

**Success Criteria**:
- Each feature passes code review loop
- Each feature passes domain/architecture feedback
- `make validate-branch` passes
- Coverage maintained >= 80%

---

## Folder Structure

```
project-root/
├── context/                      # User seeds + workflow outputs
│   ├── README.md                 # Instructions for seeds
│   ├── PRODUCT.md                # User seed: product/business perspective
│   ├── ENGINEERING.md            # User seed: technical perspective
│   ├── PRD.md                    # Phase 1 output: expanded PRD
│   └── PROJECT_PLAN.md           # Phase 2 output: MVP scope + roadmap
├── workflows/                    # Workflow support files
│   ├── PROJECT_INIT_WORKFLOW.md  # This file
│   ├── QUICK-START.md            # Step-by-step guide
│   ├── README.md                 # Template docs
│   └── templates/                # Output templates
│       ├── TEMPLATE-PRD.md
│       ├── TEMPLATE-ADR.md
│       └── TEMPLATE-PROJECT-PLAN.md
├── ADR.md                        # Overwritten in Phase 2
└── src/                          # Implementation (Phase 3-4)
```

---

## Artifact Locations

| Artifact | Location | Created By |
|----------|----------|------------|
| User seeds | `context/PRODUCT.md`, `context/ENGINEERING.md` | **User** (before workflow) |
| Output templates | `workflows/templates/` | System (part of template) |
| Expanded PRD | `context/PRD.md` | `research` role (Phase 1) |
| Research synthesis | `context/RESEARCH_SYNTHESIS.md` | `research` role (Phase 1) |
| Architecture decisions | `ADR.md` | `architecture` role (Phase 2) |
| Project plan | `context/PROJECT_PLAN.md` | `architecture` role (Phase 2) |
| Implementation | `src/*.py` | `implementation` role (Phase 3-4) |

---

## Invocation Examples

**Step 0** - Fill out seeds:
```
Edit context/PRODUCT.md and context/ENGINEERING.md with your project details
```

**Phase 0** - Discover agents:
```
Run the project initialization workflow
```
Claude Code will scan for specialists, present them, and update this file.

**Phase 1** - Start context research:
```
Research context for this project using [discovered research agent]
```

**Phase 2** - Plan architecture:
```
Plan architecture for this project using [discovered architecture agent]
```

**Phase 3** - Implement MVP:
```
Implement MVP using [discovered implementation agent(s)]
```

**Phase 4** - Add features:
```
Add [feature name] from the roadmap using [appropriate implementation agent]
```

---

## Template Usage

Templates in `workflows/templates/` define output formats. Agents use these to produce consistent documents.

### PRD Template → `context/PRD.md`

**Used by**: `research` role (Phase 1)

**Key sections**:
- Problem Statement (from user seeds)
- Objectives (from success criteria)
- User Stories with acceptance criteria
- Constraints (from engineering seed)

### ADR Template → `ADR.md`

**Used by**: `architecture` role (Phase 2)

**Typical decisions**:
- ADR-001: Database choice
- ADR-002: Web framework
- ADR-003: Authentication
- ADR-004: Deployment
- ADR-005: State management

**Each ADR must have**: Context, Decision, Consequences, Alternatives

### Project Plan Template → `context/PROJECT_PLAN.md`

**Used by**: `architecture` role (Phase 2)

**Key sections**:
- Overview table with project details
- Phase status tracking
- MVP Scope (Must-Have features only)
- Feature Roadmap (Should/Could/Won't)

---

## Troubleshooting

### "Agent doesn't understand requirements"

**Cause**: User description too vague or context missing

**Fix**:
- Provide more specific problem statement
- Describe target users explicitly
- Include examples of desired functionality
- State constraints clearly

### "ADR doesn't match expectations"

**Cause**: PRD unclear or research missed key requirements

**Fix**:
- Review PRD - is problem statement clear?
- Provide direct feedback on what to change
- Edit ADR directly if easier

### "MVP scope too large/small"

**Cause**: Unclear boundaries in PRD or misaligned priorities

**Fix**:
- Review Must-Have vs. Should-Have in PRD
- Explicitly state what's NOT in MVP
- Adjust PROJECT_PLAN.md scope directly

### "Feature implementation diverges from plan"

**Cause**: Agent didn't read context or context unclear

**Fix**:
- Ensure ADR has "Accepted" status
- Ask agent to re-read specific context file
- Update context if requirements changed

### "Created agent doesn't load after restart"

**Cause**: Syntax error in agent file or invalid frontmatter

**Fix**:
- Check agent file has valid YAML frontmatter (between `---` markers)
- Verify `name` and `description` fields are present
- Look for special characters that need escaping
- Try recreating the agent using Phase 0.3

### "Agent validation fails with error"

**Cause**: Agent file malformed or Claude Code can't invoke it

**Fix**:
- Review error message for specifics
- Compare agent file structure to working examples in `~/.claude/agents/`
- Delete and recreate the agent
- Fall back to generalist for this role

### "Workflow state shows pending_restart but I already restarted"

**Cause**: Claude Code didn't detect the continuation prompt

**Fix**:
- Say exactly: "Continue project initialization workflow"
- Check the "Workflow State" section shows the correct phase
- Manually update "Current Step" to "validation" and re-run

### "Seeds are empty but I want to create specialists"

**Cause**: Phase 0.3 requires PRODUCT.md and ENGINEERING.md to design domain-specific agents

**Fix**:
- Fill out `context/PRODUCT.md` with at least: problem statement, target users, tech stack
- Fill out `context/ENGINEERING.md` with at least: tech preferences, constraints
- Re-run Phase 0.3

### "Domain review says MISALIGNED but implementation matches PRD"

**Cause**: PRD may have evolved since implementation, or domain expert misunderstood context

**Fix**:
- Review PRD - has it changed since Phase 2?
- Ask domain expert to re-read specific PRD section
- If PRD is outdated, update it and re-run domain review
- Override with user approval if alignment is subjective

### "Architecture review flags everything as over-engineered"

**Cause**: Reviewer may be too aggressive, or project genuinely needs simplification

**Fix**:
- Review specific recommendations - are they actionable?
- Check if ADR.md justifies the complexity (documented decisions)
- If patterns are intentional, add rationale to ADR and re-run review
- Escalate to user if disagreement persists

### "Implementation keeps failing feedback after simplification"

**Cause**: Feedback criteria may be too strict, or fundamental approach needs rethinking

**Fix**:
- After 2 feedback cycles, workflow escalates to user
- User can override with documented risk
- Consider splitting deliverable into smaller pieces
- May need to revisit architecture decisions in ADR.md

### "No domain expert found for my project type"

**Cause**: Project domain doesn't match available agents

**Fix**:
- Use `product-manager` as fallback (general business alignment)
- Create a domain-specific agent using Phase 0.3
- Specify domain manually when prompted during Phase 0.1

### "Domain and architecture reviews contradict each other"

**Cause**: Different perspectives on what's appropriate

**Fix**:
- Domain review runs first, architecture considers its findings
- If conflict persists, escalate to user
- User decides which feedback takes priority
- Document decision in ADR.md for future reference

---

## Agent Best Practices

1. **Read all context before acting**: Don't skip to implementation
2. **Ask clarifying questions**: Don't guess user intent
3. **One phase at a time**: Don't jump ahead
4. **Follow templates exactly**: Consistency matters
5. **Cross-reference liberally**: Link to sources and related docs

---

## Workflow Refinement

After each workflow execution, update this file to:

- [ ] Refine phase instructions based on learnings
- [ ] Add project-type-specific guidance
- [ ] Improve success criteria
- [ ] Document common issues and solutions

### Refinement Log

| Date | Change | Reason |
|------|--------|--------|
| *Initial* | Created workflow | Template adaptation needs |
| 2025-11-28 | Reorganized: context/ for seeds, workflows/ for support | Clearer separation of user inputs vs. system files |
| 2025-11-28 | Added Phase 0 agent discovery, abstract roles | Workflow portability - users may have different specialists |
| 2025-11-28 | Added `review` role with Implement → Review → Fix loop | Enforce test creation and code quality before progression |
| 2025-11-29 | Enhanced Phase 0 with sub-phases 0.1-0.3 for agent creation | Handle missing specialists by offering creation vs. generalist fallback |
| 2025-11-29 | Added Workflow State section for restart recovery | Claude Code requires restart to load new agents; state persistence enables continuation |
| 2025-11-29 | Added skills configuration step in Phase 0.3.5 | Allow users to link existing skills or create project-specific ones |
| 2025-11-29 | Added `domain-expert` and `architecture-reviewer` roles | Validate business alignment and prevent over-engineering |
| 2025-11-29 | Added Phase 3.4 and 4.3 Domain & Architecture Feedback gates | Blocking feedback loop after code review, before approval |
| 2025-11-29 | Added domain expert selection logic to Phase 0.1 | Infer domain from PRODUCT.md, ask user if unclear |
