# Extended Audit Checklist

Score each item 0 (missing), 1 (partial), or 2 (solid). Total possible: 60.

## Structure (12 points)

| # | Check | Score |
|---|-------|-------|
| 1 | Skill is a folder, not a standalone file | /2 |
| 2 | SKILL.md is under 500 lines | /2 |
| 3 | Heavy content split into `references/` | /2 |
| 4 | Scripts/code available for Claude to compose | /2 |
| 5 | Config templates provided where applicable | /2 |
| 6 | File tree documented in SKILL.md so Claude knows what's available | /2 |

## Trigger Precision (12 points)

| # | Check | Score |
|---|-------|-------|
| 7 | `name` is kebab-case, under 64 characters | /2 |
| 8 | `description` includes "Triggers when:" or "Use when:" phrases | /2 |
| 9 | `description` names specific file types/frameworks/commands | /2 |
| 10 | `description` would match 5+ realistic user prompts | /2 |
| 11 | `description` would NOT match 5+ unrelated prompts | /2 |
| 12 | `argument-hint` provided if skill accepts args | /2 |

## Expertise Transfer (12 points)

| # | Check | Score |
|---|-------|-------|
| 13 | Opens with role/identity statement (not instructions) | /2 |
| 14 | Transfers mental models, not step-by-step procedures | /2 |
| 15 | Voice matches the domain (sounds like a practitioner) | /2 |
| 16 | Gotchas section exists with 3+ real failure points | /2 |
| 17 | Includes counter-intuitive or non-obvious guidance | /2 |
| 18 | Doesn't state things Claude would already do | /2 |

## Content Quality (12 points)

| # | Check | Score |
|---|-------|-------|
| 19 | Scannable structure (headers, tables, code blocks) | /2 |
| 20 | Examples of desired output format included | /2 |
| 21 | Anti-patterns explicitly listed (what NOT to do) | /2 |
| 22 | Commands documented with clear syntax | /2 |
| 23 | References external docs/tools rather than duplicating them | /2 |
| 24 | User preferences/context section (if applicable) | /2 |

## Robustness (12 points)

| # | Check | Score |
|---|-------|-------|
| 25 | Works across different project types/sizes | /2 |
| 26 | Handles edge cases gracefully (not just happy path) | /2 |
| 27 | Doesn't conflict with other common skills | /2 |
| 28 | Instructions aren't contradictory | /2 |
| 29 | Would still be useful 6 months from now (not ephemeral) | /2 |
| 30 | Can be extended by adding files without rewriting SKILL.md | /2 |

## Scoring Guide

| Score | Rating | Action |
|-------|--------|--------|
| 50-60 | Excellent | Ship it. Add gotchas over time. |
| 40-49 | Good | Minor improvements needed. Focus on weak areas. |
| 30-39 | Needs Work | Significant gaps. Address structure and trigger precision first. |
| 20-29 | Early Draft | Rethink the approach. May need restructuring. |
| <20 | Scaffold | Start over with the 4 Core Truths in mind. |

## Quick Audit (5-Minute Version)

If you only have 5 minutes, check these 5 things:
1. **Description field** -- Would it trigger for the right prompts?
2. **Gotchas section** -- Does it have real, non-obvious failure points?
3. **Line count** -- Is SKILL.md under 500 lines with overflow in references/?
4. **Voice** -- Does it sound like a practitioner or documentation?
5. **First paragraph** -- Does it set a role/identity or just list instructions?
