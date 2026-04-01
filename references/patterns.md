# Architecture Patterns Reference

## Pattern 1: Sequential Workflow Orchestration

Multi-step processes with explicit ordering, dependency management, and rollback.

**When to use:** Deployment pipelines, migration scripts, onboarding flows, CI/CD.

**Structure:**
```markdown
## Workflow: [Name]

### Phase 1: [Setup]
- Validate prerequisites
- If missing: [specific recovery steps]

### Phase 2: [Execute]
- Depends on Phase 1 completion
- On failure: [rollback steps]

### Phase 3: [Verify]
- Confirm success criteria
- On failure: [diagnostic steps]
```

**Key:** Each phase validates before proceeding. Include rollback for every destructive step.

---

## Pattern 2: Multi-MCP Coordination

Spans multiple external services in a single workflow.

**When to use:** Cross-tool automation (Figma -> Drive -> Linear -> Slack), data pipelines.

**Structure:**
```markdown
## Phase 1: Export from [Service A]
- Use MCP tool: [specific tool]
- Validate: [what success looks like]

## Phase 2: Transform
- Process data for [Service B]
- Handle: [edge cases]

## Phase 3: Import to [Service B]
- Use MCP tool: [specific tool]
- Validate: [confirmation]

## Phase 4: Notify via [Service C]
- Post summary to [channel/thread]
```

**Key:** Each phase validates completion independently. Services may be unavailable -- handle gracefully.

---

## Pattern 3: Iterative Refinement

Outputs improve through revision cycles with quality gates.

**When to use:** Code review, content generation, design audit, report writing.

**Structure:**
```markdown
## Round 1: Generate
- Produce initial output based on requirements
- Self-critique against [quality criteria]

## Round 2: Refine
- Fix issues identified in self-critique
- Re-validate against criteria

## Round N: Until quality gate passes
- Max [N] rounds
- Quality gate: [specific binary criteria]
```

**Key:** Define explicit quality gates (binary, not subjective). Set a max iteration count to prevent infinite loops.

---

## Pattern 4: Context-Aware Tool Selection

Routes the same goal differently based on project context.

**When to use:** Tools that adapt to project type, size, framework, or team preferences.

**Structure:**
```markdown
## Context Detection
- Check: [framework, file patterns, config files]
- Classify: [category A, B, or C]

## Route A: [Small project / React / etc.]
- Approach: [specific to this context]

## Route B: [Large project / Vue / etc.]
- Approach: [specific to this context]

## Route C: [Unknown / fallback]
- Approach: [safe default]
```

**Key:** Detection should be fast and reliable (file existence checks, not content analysis). Always have a fallback route.

---

## Pattern 5: Domain-Specific Intelligence

Embeds specialized knowledge that goes beyond tool access.

**When to use:** Compliance, security, design systems, industry-specific workflows.

**Structure:**
```markdown
## Domain Knowledge
[Expert-level mental models, decision frameworks, common traps]

## Decision Matrix
| Situation | Correct Action | Common Mistake |
|-----------|---------------|----------------|

## Gotchas
[Domain-specific failure modes Claude wouldn't know]

## References
[Links to deeper domain files for edge cases]
```

**Key:** This pattern is about knowledge, not tools. The value is in the mental models and decision frameworks that make Claude think like a domain expert.

---

## Combining Patterns

Patterns are composable. Common combinations:

- **Sequential + Multi-MCP:** Each phase of the workflow uses a different service
- **Iterative + Domain-Specific:** Refine output using domain-specific quality criteria
- **Context-Aware + Sequential:** Detect project type, then run the appropriate workflow
- **All five:** A compliance skill that detects context, orchestrates multi-service checks, refines findings iteratively, with deep domain knowledge and rollback

---

## Skill Chaining

Skills can reference each other:
```markdown
After generating the report, suggest the user run `/code-review` on the output.
```

Dependency management isn't built into the skill system yet, but you can:
- Reference other skills by name
- Claude will invoke them if installed
- Check for their existence with a note: "If the [skill-name] skill is installed, use it for [task]."

---

## Shared Context Pattern (boringmarketer)

For skill systems (not individual skills):

1. **Shared brand context** -- A document every skill reads before acting
2. **Learnings loop** -- Skills store what works and feed it back
3. **Self-maintenance** -- Skills update their own gotchas and references

"Without all three, you have a collection of scripts, not an OS."

Implementation:
- Brand context: A shared `context.md` or `brand.md` in the skill's data directory
- Learnings: Append-only log in `${CLAUDE_PLUGIN_DATA}/learnings.md`
- Self-maintenance: Skills add to their own gotchas after encountering new edge cases
