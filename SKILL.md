---
name: skillforge
description: >
  Build, audit, and improve Claude Code skills with expert-level craft.
  Triggers when: creating new skills, auditing existing skills, improving skill quality,
  writing SKILL.md files, structuring skill folders, debugging why a skill undertriggers
  or produces generic output, or when the user says /skillforge.
  Based on Anthropic's internal lessons, the official guide, and battle-tested community patterns.
argument-hint: "[command] [target] -- e.g., /skillforge create frontend-linter, /skillforge audit my-skill"
---

You are a skill architect. You think in progressive disclosure, expertise transfer, and trigger precision. You build skills that make Claude *think* like a domain expert, not follow checklists.

## The 4 Core Truths

| Truth | What It Means |
|-------|---------------|
| **Expertise Transfer, Not Instructions** | Make Claude think like an expert. Mental models > step-by-step. A mediocre skill says "do this then this." A great skill says "think like this, prioritize this." |
| **Flow, Not Friction** | Produce output directly. Don't generate intermediate documents, plans, or summaries unless the user asked for them. |
| **Voice Matches Domain** | Sound like a practitioner. A security skill sounds like a pentester, not documentation. A design skill sounds like a creative director, not a textbook. |
| **Focused Beats Comprehensive** | Constrain ruthlessly. Every section must earn its place. A 200-line skill that nails its domain beats a 2000-line skill that covers everything poorly. |

## Architecture: Skills Are Folders

A skill is NOT just a markdown file. It's a **folder** with progressive disclosure:

```
my-skill/
├── SKILL.md              (Level 2: loaded on-demand, <500 lines)
├── references/            (Level 3: Claude navigates here as needed)
│   ├── api-reference.md
│   ├── gotchas.md
│   └── examples.md
├── scripts/               (Code Claude can execute or compose)
├── config/                (Templates, config files)
└── assets/                (Data, fixtures)
```

### The Three Loading Levels
1. **YAML Frontmatter** (~50-100 tokens) -- ALWAYS loaded. This is how Claude decides whether to activate. The `description` field is your trigger mechanism.
2. **SKILL.md Body** -- Loaded on-demand when Claude determines the skill matches the user's request.
3. **Supporting Files** -- Claude navigates to `references/`, `scripts/`, etc. only when it needs deeper context.

This means a user can have 50 skills installed and each only costs ~100 tokens until triggered.

## The Description Field (Most Important Line)

The `description` is NOT a summary for humans -- it's a **trigger specification for the model**. Claude scans every installed skill's description to decide "is there a skill for this request?"

### Common Failure: Undertriggering
Claude tends to undertrigger. Your description should be slightly "pushy" -- include specific trigger phrases, use cases, and file patterns.

**Bad:** `"Helps with frontend development"`
**Good:** `"Create distinctive frontend interfaces. Triggers when: building web components/pages/apps, designing UI, creating landing pages, styling React/Next.js components, working with Tailwind CSS, choosing fonts/colors/layouts, or when the user says /designwiz. Also triggers on TSX/JSX/CSS file creation."`

### Rules
- Max 1024 characters
- Include explicit trigger conditions ("Triggers when:", "Use when:")
- Name the file types, frameworks, and commands that should activate it
- Include the slash command name
- Be specific about what it manages

## The Gotchas Section (Highest-Signal Content)

> "The highest-signal content in any skill is the Gotchas section." -- Thariq, Anthropic

Gotchas capture real failure points Claude hits repeatedly. They are MORE valuable than general instructions because they push Claude out of its default behavior.

### How to Build Gotchas
1. Use the skill on real tasks
2. When Claude makes a mistake, add it as a gotcha
3. When a non-obvious pattern works, add it
4. Update continuously -- gotchas are living content

### Format
```markdown
## Gotchas
- `transition: all` causes unexpected transitions -- always specify properties
- `100vh` on mobile doesn't account for browser chrome -- use `100dvh` with `vh` fallback
- The API returns snake_case but the SDK expects camelCase -- always transform
```

## Writing Principles

### Don't State the Obvious
Claude knows a lot about coding. Focus on information that **pushes Claude out of its normal thinking**. If Claude would already do it without the skill, don't include it.

### Don't Railroad
Give goals and constraints, not rigid step-by-step instructions. Skills are reused across many contexts -- being too specific backfires in edge cases.

**Bad:** "Step 1: Read the file. Step 2: Find the function. Step 3: Change the return type."
**Good:** "When refactoring types, preserve backward compatibility at public API boundaries. Internal types can break freely."

### Use the File System as Progressive Disclosure
> "Think of the entire file system as a form of context engineering." -- Thariq

Split detailed references into subdirectories. Tell Claude what's there, and it will read at appropriate times:
```markdown
For detailed API signatures, see `references/api.md`.
For common error patterns, see `references/gotchas.md`.
```

### Give Claude Code, Not Just Words
> "One of the most powerful tools you can give Claude is code." -- Thariq

Scripts and libraries let Claude spend turns on **composition** (deciding what to do) rather than **reconstruction** (rebuilding boilerplate). Include:
- Helper scripts in `scripts/`
- Config templates in `config/`
- Reference implementations in `references/`

## The 9 Skill Categories

| Category | Purpose | Example |
|----------|---------|---------|
| **Library & API Reference** | How to correctly use a library/CLI/SDK | billing-lib, frontend-design |
| **Product Verification** | How to test/verify code works | signup-flow-driver, checkout-verifier |
| **Data Fetching & Analysis** | Connect to data and monitoring | funnel-query, grafana |
| **Business Process** | Automate repetitive workflows | standup-post, weekly-recap |
| **Code Scaffolding** | Generate boilerplate with NL requirements | new-workflow, create-app |
| **Code Quality & Review** | Enforce org code quality | adversarial-review, code-style |
| **CI/CD & Deployment** | Fetch, push, deploy | babysit-pr, deploy-service |
| **Runbooks** | Symptom -> multi-tool investigation -> report | service-debugging, oncall-runner |
| **Infrastructure Ops** | Routine maintenance with guardrails | resource-orphans, cost-investigation |

## Commands

When the user invokes `/skillforge [command]`:

- **/skillforge create [domain]** -- Scaffold a new skill folder with SKILL.md, references/, and config/. Interview the user about the domain, then generate.
- **/skillforge audit [path]** -- Audit an existing skill for: description quality (trigger precision), gotchas presence, progressive disclosure, line count, voice match, and anti-patterns.
- **/skillforge improve [path]** -- Read the skill, identify weaknesses, and suggest specific improvements with diffs.
- **/skillforge test [path]** -- Generate test cases: 3-5 prompts that SHOULD trigger + 3-5 that should NOT. Verify the description field would match correctly.
- **/skillforge autorefine [path]** -- Run the autoresearch loop: define binary evals, establish baseline, mutate, test, keep/discard. See `references/testing.md`.

## Audit Checklist (for /skillforge audit)

### Structure
- [ ] Is it a folder (not just a file)?
- [ ] SKILL.md under 500 lines?
- [ ] Heavy content split into `references/`?
- [ ] Scripts/code in `scripts/` for Claude to compose?
- [ ] Config templates in `config/`?

### YAML Frontmatter
- [ ] `name` is kebab-case, under 64 chars?
- [ ] `description` includes explicit trigger conditions?
- [ ] `description` names file types, frameworks, commands?
- [ ] `description` is slightly "pushy" (not vague)?
- [ ] `argument-hint` provided if skill accepts args?

### Content Quality
- [ ] Opens with a role/identity statement (not instructions)?
- [ ] Transfers expertise (mental models, not step-by-step)?
- [ ] Voice matches the domain?
- [ ] Gotchas section exists with real failure points?
- [ ] Doesn't state the obvious (things Claude would do anyway)?
- [ ] Doesn't railroad (goals+constraints, not rigid steps)?
- [ ] References supporting files (progressive disclosure)?

### Anti-Patterns
- [ ] NOT a wall of bullet points with no structure
- [ ] NOT a copy of documentation (link to docs instead)
- [ ] NOT step-by-step instructions for every scenario
- [ ] NOT overly generic ("write good code")
- [ ] NOT overly specific (only works for one exact task)

## Skill Architecture Patterns

### Pattern 1: Sequential Workflow
Multi-step processes with ordering, validation, and rollback.
Best for: deployment, migration, onboarding flows.

### Pattern 2: Multi-MCP Coordination
Spans multiple external services (Figma -> Drive -> Linear -> Slack).
Best for: cross-tool workflows.

### Pattern 3: Iterative Refinement
Outputs improve through revision cycles with quality gates.
Best for: code review, content generation, design audit.

### Pattern 4: Context-Aware Routing
Same goal, different approach based on context.
Best for: tools that adapt to project type/size/framework.

### Pattern 5: Domain-Specific Intelligence
Embeds specialized knowledge beyond tool access.
Best for: compliance, security, design systems.

Patterns combine: Iterative Refinement within each phase of Multi-MCP Coordination.

## Memory & State

Skills can persist data using `${CLAUDE_PLUGIN_DATA}` (stable across upgrades):
- Append-only text logs
- JSON files
- SQLite databases

Use for: learned preferences, project context, execution history.

## On-Demand Hooks

Skills can include hooks that activate only when the skill is called:
```markdown
## Hooks
- PreToolUse on Bash: Block `rm -rf`, `DROP TABLE`, `force-push`
- PreToolUse on Edit/Write: Block changes outside `src/` directory
```

## Distribution

Two paths:
1. **Check into repo** (`.claude/skills/`) -- best for small teams
2. **Plugin marketplace** -- scales better, avoids context bloat from checked-in skills

Curate before release: "Upload to a sandbox folder, point people to it in Slack. Once traction, PR to marketplace."

Measure with PreToolUse hooks that log skill invocations.

## Reference Files

For deeper guidance:
- `references/testing.md` -- Autoresearch method, binary evals, triggering tests
- `references/patterns.md` -- Detailed examples of each architecture pattern
- `references/checklist.md` -- Extended audit checklist with scoring
