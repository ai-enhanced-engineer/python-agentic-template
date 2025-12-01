# Python Agentic Template

Autonomous Python template. Describe what you want; agents build it.

## Workflow

Run `workflows/PROJECT_INIT_WORKFLOW.md` for the complete specification.

Phases: **Research → Architecture → MVP → Enhancement** with user approval gates.

## Development Standards

### Validation (Required Before Commits)
```bash
make validate-branch   # Runs format, lint, type-check, test
```

## Artifact Locations

| Output | Location |
|--------|----------|
| User seeds | `context/PRODUCT.md`, `context/ENGINEERING.md` |
| PRD | `context/PRD.md` |
| Architecture | `ADR.md` |
| Project plan | `context/PROJECT_PLAN.md` |
| Code | `src/*.py` |
| Tests | `tests/test_*.py` |

## Key Rules

1. Read all context files before acting
2. Use templates in `workflows/templates/` for output formats
3. Stop at approval gates—don't jump phases
4. Max 3 fix cycles per deliverable, then escalate
