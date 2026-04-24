## Workflow Orchestration

### 1. Plan Node Default
- Enter plan mode for ANY non-trivial task (3+ steps or architectural decisions)
- If something goes sideways, STOP and re-plan immediately – don't keep pushing
- Use plan mode for verification steps, not just building
- Write detailed specs upfront to reduce ambiguity

### 2. Subagent Strategy
- Use subagents liberally to keep main context window clean
- Offload research, exploration, and parallel analysis to subagents
- For complex problems, throw more compute at it via subagents
- One tack per subagent for focused execution

### 3. Self-Improvement Loop
- After ANY correction from the user: update `tasks/lessons.md` with the pattern
- Write rules for yourself that prevent the same mistake
- Ruthlessly iterate on these lessons until mistake rate drops
- Review lessons at session start for relevant project

### 4. Verification Before Done
- Never mark a task complete without proving it works
- Diff behavior between main and your changes when relevant
- Ask yourself: "Would a staff engineer approve this?"
- Run tests, check logs, demonstrate correctness

### 5. Demand Elegance (Balanced)
- For non-trivial changes: pause and ask "is there a more elegant way?"
- If a fix feels hacky: "Knowing everything I know now, implement the elegant solution"
- Skip this for simple, obvious fixes – don't over-engineer
- Challenge your own work before presenting it

### 6. Autonomous Bug Fixing
- When given a bug report: just fix it. Don't ask for hand-holding
- Point at logs, errors, failing tests – then resolve them
- Zero context switching required from the user
- Go fix failing CI tests without being told how

### 7. Context Management (Context Rot)
- Model degrades past ~50% of the 200K window; becomes unreliable past ~80%
- When approaching 50%: run `/clear`, then reload state from `tasks/progress.md` instead of continuing in a bloated session
- Offload research, exploration, and file-reads to subagents so raw tool output never pollutes the main context
- Prefer fresh sessions over long ones for complex work — small context = sharp model

### 8. Micro-tasks
- Break plans into slices that take 2–5 minutes of work each
- Each slice must be a verifiable unit: a test passes, a lint runs clean, a feature demonstrably works
- Small enough that a human can read the diff in 30 seconds
- If a task is too big to slice this finely, re-plan before coding

### 9. Independent Review
- Reviews are done by a DIFFERENT subagent than the one that wrote the code
- Authors don't catch their own bugs — use the `code-reviewer` subagent for a second pass
- Present review findings before marking a task complete
- For security-sensitive changes: also run `/security-review`

## Task Management
1. **Plan First**: Write plan to `tasks/todo.md` with checkable items
2. **Track Phases**: Maintain `tasks/progress.md` as a state file across phases (spec → backend → integrations → frontend → security → perf → infra). This is the single source of truth when context resets.
3. **Micro-task**: Each item in `todo.md` = 2–5 minutes of work, independently verifiable
4. **Verify Plan**: Check in with the user before starting implementation
5. **Track Progress**: Mark items complete as you go — update `progress.md` after each phase transition
6. **Explain Changes**: High-level summary at each step
7. **Document Results**: Add review section to `tasks/todo.md`
8. **Capture Lessons**: Update `tasks/lessons.md` after corrections

## Test-Driven Development
For non-trivial features, follow the RED → GREEN → REFACTOR cycle:
1. **RED**: Write a failing test that captures the requirement (acceptance or unit level)
2. **GREEN**: Write the simplest code that makes the test pass — no extra features
3. **REFACTOR**: Clean up with the tests as a safety net
4. **Commit** after each complete cycle, then move to the next slice

Never mark work complete without a green test proving it.

## Core Principles
- **Simplicity First**: Make every change as simple as possible. Impact minimal code.
- **No Laziness**: Find root causes. No temporary fixes. Senior developer standards.
- **Minimal Impact**: Changes should only touch what's necessary. Avoid introducing bugs.
- **YAGNI**: Don't build what isn't asked for. No speculative abstractions or "just in case" flexibility.
- **DRY (balanced)**: Remove real duplication; don't over-abstract single-use code.
- **Ask, don't tell**: Open-ended questions to the user produce better specs than directives — use `grill-me` for stress-testing plans.
