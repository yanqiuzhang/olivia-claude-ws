# Hackathon Workspace

Multi-repo workspace for Ombori hackathon.

## Key Rules

1. **Plan-mode-first**: ALL features start with spec creation in `specs/` folder
2. **TDD where applicable**: Write tests before implementation
3. **Specs in workspace**: All specs centralized in `specs/` folder
4. **Evolve the config**: Update CLAUDE.md and agents with learnings as you build

## Continuous Improvement

As you build, update these files with learnings:
- `CLAUDE.md` - Add new patterns, gotchas, project-specific conventions
- `.claude/agents/*.md` - Refine agent instructions based on what works
- `apps/macos-client/CLAUDE.md` - Swift-specific learnings
- `services/api/CLAUDE.md` - Python/FastAPI-specific learnings

Examples of things to capture:
- "Always use X pattern for Y"
- "Don't forget to run Z after changing W"
- "API endpoint naming follows this convention..."
- "Database migrations require this step..."

## Quick Start

```bash
# Run WebSocket service (port 3000)
cd services/api && uv run fastapi dev --port 3000

# Assumes backend API is running at http://localhost:8000
```

## Structure
- `apps/macos-client/` - SwiftUI desktop app (submodule)
- `services/api/` - Queue WebSocket Service (port 3000)
- `specs/` - Feature specifications (plan-mode output)

## WebSocket Service
Real-time queue status broadcasting:
- Connect: `ws://localhost:3000/ws`
- Polls backend API every 3 seconds
- Broadcasts queue changes to all connected clients

## Skills (Commands)
Available in `.claude/skills/`:
- `/feature` - **Start here!** Asks questions → creates spec → TDD implementation

## Agents
Available in `.claude/agents/`:
- `/architect` - System design, API contracts → outputs to `specs/`
- `/swift-coder` - Swift client development
- `/python-coder` - FastAPI backend development
- `/reviewer` - Code review across all repos
- `/debugger` - Issue investigation
- `/tester` - Test-driven development

## Development Workflow

### New Features (MANDATORY)
1. **Plan mode first** - Create spec in `specs/YYYY-MM-DD-feature-name.md`
2. **Write tests** - TDD: tests before implementation
3. **Implement** - Use coder agents (can run in parallel)
4. **Review** - Use reviewer agent
5. **Commit** - Submodules first, then workspace

### Git Workflow (use `gh` CLI, not GitHub web)
Always use feature branches and `gh pr create`:
```bash
# In each submodule
git checkout -b feature/<name>
git add . && git commit -m "feat: ..."
git push -u origin feature/<name>
gh pr create --title "feat: ..." --body "Description"

# After PRs merged, update workspace
cd ../..
git add . && git commit -m "Update submodules"
git push
```

## API Reference
### WebSocket Service (port 3000)
- Swagger: http://localhost:3000/docs
- Health: http://localhost:3000/health
- WebSocket: ws://localhost:3000/ws
- Notify: POST http://localhost:3000/notify

### Backend API (port 8000)
- GET /queue/list - Get queue items
- GET /queue/current - Get current serving number
