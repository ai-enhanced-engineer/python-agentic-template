# Python Agentic Template

> Describe what you want to build. Let agents build it.

An autonomous Python project template that bootstraps itself into production-ready code through a multi-agent workflow.

## Why This Template?

Starting an AI/ML project means three days of ceremony before writing real code. Project structure. Logging. Tests. CI/CD. Type hints. Pre-commit hooks.

Most developers copy-paste from old repos or configure from scratch. Both approaches carry technical debt from day one.

This template bootstraps itself. Describe the project in two seed files; agents research, plan, and implement it with production patterns built in.

## How It Works

```
┌─────────────────────────────────────────────────────────────────┐
│  YOU: Fill context/PRODUCT.md + context/ENGINEERING.md         │
│       (Describe what you're building and technical preferences) │
└─────────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────────┐
│  PHASE 0: Agent Discovery                                       │
│           Claude Code finds available specialists and maps      │
│           them to roles (research, architecture, implementation,│
│           review)                                               │
└─────────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────────┐
│  PHASE 1: Research                                              │
│           Expands your seeds into full PRD                      │
│           Researches best practices, grades evidence            │
│           → context/PRD.md, context/RESEARCH_SYNTHESIS.md       │
└─────────────────────────────────────────────────────────────────┘
                              ↓ [User Approval]
┌─────────────────────────────────────────────────────────────────┐
│  PHASE 2: Architecture                                          │
│           Creates ADRs and project plan                         │
│           Defines MVP scope with MoSCoW prioritization          │
│           → ADR.md, context/PROJECT_PLAN.md                     │
└─────────────────────────────────────────────────────────────────┘
                              ↓ [User Approval]
┌─────────────────────────────────────────────────────────────────┐
│  PHASE 3: MVP Implementation                                    │
│           Builds must-have features with tests                  │
│           Per deliverable: IMPLEMENT → REVIEW → FIX → PASS      │
│           Review enforces 80% coverage, test quality            │
│           → Working code in src/                                │
└─────────────────────────────────────────────────────────────────┘
                              ↓ [User Approval]
┌─────────────────────────────────────────────────────────────────┐
│  PHASE 4: Feature Enhancement                                   │
│           Adds features from roadmap (same validation loop)     │
│           Each feature: IMPLEMENT → REVIEW → FIX → User Approval│
└─────────────────────────────────────────────────────────────────┘
```

You approve each phase before continuing.

## Quick Start

1. **Create your repository**: Click "Use this template" on GitHub
2. **Set up environment**: `make init`
3. **Fill out your seeds**: Edit `context/PRODUCT.md` and `context/ENGINEERING.md`
4. **Start building**: In Claude Code, say `"Run the project initialization workflow"`

### Filling Out Seeds

| File | What to Include |
|------|-----------------|
| `context/PRODUCT.md` | What you're building, for whom, why, success criteria |
| `context/ENGINEERING.md` | Technical preferences, constraints, architecture ideas |

**Tips for better results**:
- Be specific about the problem: "Users waste 2 hours/day on X" > "Users have problems"
- Define success measurably: "50% reduction in Y" > "Improve Y"
- State constraints clearly: "Must run on GCP" > "Cloud deployment"

See `workflows/PROJECT_INIT_WORKFLOW.md` for the complete workflow specification.

## What You Get

The autonomous workflow is the main attraction, but the template also provides a production-ready foundation:

### Modern Python Tooling
- Python 3.12+ with type hints throughout
- uv for fast dependency management
- Ready for any framework you choose

### Production Logging
Structured JSON logging with structlog. Correlation ID tracking. Dual-mode output: human-readable for development, JSON for production.

### Development Automation
- Ruff (linting), Black (formatting), mypy (types)
- Pre-commit hooks as quality gates
- `make validate-branch` runs everything

### Testing Structure
Unit, functional, and integration test directories with pytest markers. The logging system alone has 21+ tests as examples.

### CI/CD Ready
GitHub Actions workflows included. Semantic versioning. Docker-ready structure.

## Project Structure

```
my-project/
├── context/                   # Project seeds + workflow outputs
│   ├── PRODUCT.md             # Your product requirements (seed)
│   ├── ENGINEERING.md         # Your technical preferences (seed)
│   ├── PRD.md                 # Expanded PRD (generated)
│   ├── RESEARCH_SYNTHESIS.md  # Research findings (generated)
│   └── PROJECT_PLAN.md        # MVP scope + roadmap (generated)
├── workflows/                 # Autonomous workflow system
│   ├── PROJECT_INIT_WORKFLOW.md  # Complete workflow specification
│   └── templates/             # Output format contracts
├── src/                       # Your service code
│   ├── __init__.py
│   ├── main.py                # Entry point with logging demo
│   └── logging.py             # Production logging system
├── tests/                     # Test suite
│   ├── test_main.py
│   └── test_logging.py        # 21+ logging tests
├── research/                  # Notebooks and experiments
├── ADR.md                     # Architecture decisions (generated)
├── Makefile                   # All automation commands
└── pyproject.toml             # Project configuration
```

## Development Commands

```bash
# Environment
make init              # Complete development setup
make sync              # Update dependencies
make clean-env         # Reset environment

# Code Quality
make format            # Auto-format code
make lint              # Fix linting issues
make type-check        # Validate type hints
make validate-branch   # Run all checks (required before commits)

# Testing
make test              # Standard test suite
make test-unit         # Fast unit tests
make test-functional   # Feature tests
make test-integration  # Integration tests
make test-all          # Complete test suite
```

## The Production-First Philosophy

This template embodies the principle that **production AI requires engineering discipline**:

- **90% infrastructure, 10% model code**: Most production AI is validation, monitoring, error handling, and cost controls—not algorithms
- **Reliability over novelty**: Production systems must work consistently, not just impressively
- **Plan for failure**: Every external call needs error handling; every assumption needs validation

The autonomous workflow ensures these patterns are built in from the start, not bolted on later.

## Who Should Use This

Starting an AI/ML project and don't want to spend days on infrastructure? This is for you. Senior engineer who expects production-grade tooling from day one? This is for you. Leading a team and want a consistent starting point that embodies engineering discipline? This is for you.

The common thread: reliability over speed, describing what you want over configuring it manually.

## Learn More

### This Template
- `workflows/PROJECT_INIT_WORKFLOW.md` - Complete workflow specification
- `workflows/templates/` - Output format examples

### Production AI Engineering
- [A Production-First Approach to AI Engineering](https://aienhancedengineer.substack.com/p/a-production-first-approach-to-ai)
- [Google's Rules for ML](https://developers.google.com/machine-learning/guides/rules-of-ml)
- [Hidden Technical Debt in ML Systems](https://papers.nips.cc/paper/5656-hidden-technical-debt-in-machine-learning-systems.pdf)

### Technologies
- [structlog](https://www.structlog.org/) - Structured logging
- [uv](https://docs.astral.sh/uv/) - Fast Python package management
- [pytest](https://docs.pytest.org/) - Testing framework
- [Ruff](https://docs.astral.sh/ruff/) - Fast Python linter

## Contributing

Prioritize reliability over features, simplicity over cleverness, documentation over assumptions, tests over trust.

## License

Apache License 2.0 - See [LICENSE](LICENSE) file.

---

*"Describe what you want. Let agents build it."*
