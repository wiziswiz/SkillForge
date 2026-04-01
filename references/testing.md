# Testing & Improvement Reference

## Three Testing Dimensions

### 1. Triggering Tests
Does the skill load when it should? Does it stay dormant when it shouldn't?

**How to test:**
- Write 3-5 prompts that SHOULD trigger the skill
- Write 3-5 prompts that should NOT trigger it
- Ask Claude: "Which skills would you activate for this request?" -- Claude will quote the description verbatim, revealing keyword gaps

**Common fix:** The description field is too vague. Add specific trigger phrases, file types, and command names.

### 2. Functional Tests
Does the skill produce correct, high-quality output?

**How to test:**
- Run the skill on 3-5 representative real-world tasks
- Compare output with and without the skill
- Check: Does it follow its own gotchas? Does it transfer expertise or just follow steps?

### 3. Performance Comparisons
Does the skill actually improve outcomes?

**Metrics to track:**
- Messages to complete task (fewer = better)
- Token usage (less = more efficient)
- Quality of output (binary evals)
- Error rate (fewer retries)

Example: "Skill reduced back-and-forth from 15 messages to 2, cut tokens in half."

## The Autoresearch Method (Ole Lehmann / Karpathy)

An automated skill improvement loop that can take a skill from 56% to 92% pass rate with zero manual work.

### Setup Phase

**1. Gather Context**
- Target skill path
- 3-5 test inputs (real-world prompts)
- 3-6 binary evaluation criteria

**2. Define Binary Evals**
Evals MUST be binary (yes/no). Never use subjective scales (1-5).

Good evals:
- "Does the output avoid using Inter as the primary font?" (yes/no)
- "Does the response include a gotchas section?" (yes/no)
- "Is the description under 1024 characters?" (yes/no)
- "Does it transfer expertise rather than list steps?" (yes/no)

Bad evals:
- "How good is the output?" (subjective)
- "Rate the quality 1-5" (introduces variability)
- "Is it well-written?" (vague)

**3. Sweet spot: 3-6 criteria**
Fewer than 3: not enough signal.
More than 6: over-optimization, diminishing returns.

### Improvement Loop

```
1. Establish Baseline
   - Run original skill on all test inputs N times
   - Record pass/fail for each eval criterion
   - Calculate baseline pass rate

2. Analyze Failures
   - Which criteria fail most?
   - What patterns cause failures?
   - Form a targeted hypothesis

3. Make ONE Change
   - Single mutation per experiment
   - Never change multiple things at once
   - Examples: add a gotcha, rewrite a section, add an example

4. Test the Mutation
   - Run modified skill on all test inputs
   - Record pass/fail
   - Compare to baseline

5. Keep or Discard
   - Improvement? Keep the change.
   - No improvement or regression? Revert.

6. Repeat
   - Continue until:
     - 95%+ pass rate across 3 consecutive experiments
     - Budget exhaustion
     - User intervention
```

### Output Artifacts
- **Improved SKILL.md** -- the refined skill
- **results.tsv** -- score log per experiment
- **changelog.md** -- mutation record (what changed, why, result)
- **dashboard.html** -- self-contained HTML with score visualization
- **SKILL.md.baseline** -- backup of original

### Key Principles
- **Single mutations only** -- isolate variables
- **Binary evals only** -- eliminate subjectivity
- **3-6 criteria** -- the sweet spot
- **Real test inputs** -- not synthetic prompts
- **Keep the changelog** -- learn what kinds of changes work

## Debugging Common Issues

### Skill Not Triggering
**Cause:** Description field doesn't match user's phrasing.
**Fix:** Ask Claude "Which skills would you activate for: [prompt]?" -- it quotes descriptions verbatim. Add missing keywords.

### Triggering Too Often
**Cause:** Description is too broad.
**Fix:** Add specificity: "Use when X, NOT when Y." Narrow the trigger conditions.

### Not Following Instructions
**Cause:** Instructions are ambiguous, conflicting, or too prescriptive.
**Fix:**
- Check for contradictions
- Reduce prescription (goals > steps)
- Add concrete examples of desired output
- Move detailed instructions to `references/` so they're not always loaded

### Generic Output
**Cause:** Skill states the obvious instead of pushing Claude's defaults.
**Fix:** Remove anything Claude would already do. Add counter-intuitive guidance, gotchas, and domain-specific mental models.

## Measuring Skill Usage

Use a PreToolUse hook to log skill activations:
```json
{
  "event": "PreToolUse",
  "hooks": [{
    "type": "command",
    "command": "echo \"$(date): skill=$SKILL_NAME\" >> /tmp/skill-usage.log"
  }]
}
```
This lets you find skills that are popular, undertriggering, or overtriggering.
