# SkillForge

Build, audit, and improve Claude Code skills with expert-level craft.

SkillForge is a meta-skill that makes you better at creating skills. It synthesizes lessons from Anthropic's internal skill-building team, the official guide, and battle-tested community patterns into a single tool that transfers skill-architecture expertise directly into Claude's thinking.

## Install

Copy the `skillforge` folder to your Claude Code skills directory:

```bash
# Clone and copy
git clone https://github.com/wiziswiz/SkillForge.git
cp -r SkillForge ~/.claude/skills/skillforge
```

Or symlink it:
```bash
git clone https://github.com/wiziswiz/SkillForge.git ~/Projects/SkillForge
ln -s ~/Projects/SkillForge ~/.claude/skills/skillforge
```

## Commands

| Command | What it does |
|---------|-------------|
| `/skillforge create [domain]` | Scaffold a new skill with proper architecture |
| `/skillforge audit [path]` | Score an existing skill across 30 criteria (0-60) |
| `/skillforge improve [path]` | Identify weaknesses and suggest specific fixes |
| `/skillforge test [path]` | Generate trigger/non-trigger test cases |
| `/skillforge autorefine [path]` | Automated improvement loop (binary evals, single mutations) |

## What's Inside

```
skillforge/
├── SKILL.md                    # Core expertise: 4 truths, architecture, writing principles
├── references/
│   ├── testing.md              # Autoresearch method, binary evals, debugging
│   ├── patterns.md             # 5 architecture patterns, chaining, shared context
│   └── checklist.md            # 30-item scored audit checklist
└── README.md
```

## The 4 Core Truths

| Truth | Meaning |
|-------|---------|
| **Expertise Transfer** | Make Claude *think* like an expert, not follow steps |
| **Flow, Not Friction** | Produce output directly, not intermediate documents |
| **Voice Matches Domain** | Sound like a practitioner, not documentation |
| **Focused Beats Comprehensive** | Every section earns its place |

## Key Principles

### Skills Are Folders, Not Files
A skill is a directory with progressive disclosure: `SKILL.md` (loaded on-demand) + `references/` (navigated as needed) + `scripts/` (code Claude can compose).

### The Description Field Is Your Trigger
Claude scans every skill's `description` to decide activation. Write it for the model, not humans. Be specific and slightly "pushy" -- Claude undertriggers by default.

### Gotchas Are the Highest-Signal Content
Real failure points that push Claude out of its defaults are more valuable than general instructions. Build them from real usage and update continuously.

### Don't State the Obvious
If Claude would already do it without the skill, don't include it. Focus on counter-intuitive, domain-specific, or non-obvious guidance.

## The Autoresearch Method

An automated improvement loop based on Karpathy's approach:

1. Define 3-6 **binary** eval criteria (yes/no only, never subjective scales)
2. Establish baseline pass rate
3. Make **single** mutations per experiment
4. Test, keep or discard each change
5. Repeat until 95%+ pass rate

This took a landing page skill from 56% to 92% with zero manual work.

## Audit Scoring

The `/skillforge audit` command scores skills across 5 dimensions (12 points each, 60 total):

- **Structure** -- Folder architecture, line count, progressive disclosure
- **Trigger Precision** -- Description quality, match/non-match testing
- **Expertise Transfer** -- Mental models vs steps, voice, gotchas
- **Content Quality** -- Scannability, examples, anti-patterns
- **Robustness** -- Edge cases, longevity, extensibility

| Score | Rating |
|-------|--------|
| 50-60 | Excellent -- ship it |
| 40-49 | Good -- minor improvements |
| 30-39 | Needs work -- address structure first |
| <30 | Rethink the approach |

## Sources

Built from exhaustive research of:

- **[Thariq / Anthropic](https://x.com/JoshKale/status/2033960196918415667)** -- "Lessons from Building Claude Code: How We Use Skills" (internal Anthropic lessons from hundreds of production skills)
- **[Ole Lehmann](https://x.com/itsolelehmann/status/2033919415771713715)** -- "How to 10x Your Claude Skills" (autoresearch method for automated improvement)
- **[DrCatHicks/learning-opportunities](https://github.com/DrCatHicks/learning-opportunities)** -- Science-based skill architecture with active learning patterns
- **[Anthropic Official Guide](https://resources.anthropic.com/hubfs/The-Complete-Guide-to-Building-Skill-for-Claude.pdf)** -- "The Complete Guide to Building Skills for Claude" (33 pages, 5 design patterns, 9 categories)
- **[boringmarketer](https://x.com/boringmarketer/status/2003846867021693427)** -- Skill architecture systems (shared context, chaining, self-maintenance)

## License

MIT
