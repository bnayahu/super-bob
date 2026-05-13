# Testing SuperBob Modes

**Comprehensive guide for testing SuperBob skill-modes**

---

## Overview

Testing SuperBob modes requires a different approach than testing traditional code. Modes are process documentation that guides AI behavior, so testing focuses on:

1. **Behavioral compliance** - Does the AI follow the process?
2. **Rationalization resistance** - Does it resist shortcuts under pressure?
3. **Mode interaction** - Do modes compose correctly?
4. **Quality gates** - Are structural enforcements working?

---

## Manual Testing Process

### Basic Mode Testing

**For each mode, verify:**

1. **Mode activates correctly**
   - Select mode from dropdown
   - Verify mode name appears in chat
   - Check mode-specific instructions load

2. **Core workflow executes**
   - Follow the mode's primary workflow
   - Verify each step completes
   - Check outputs match expectations

3. **Completion criteria met**
   - Mode completes when it should
   - Returns appropriate summary
   - Hands off to next mode if needed

### Example: Testing test-driven-development Mode

```
1. Activate mode: Select "Test Driven Development" from dropdown
2. Give task: "Implement a function to validate email addresses"
3. Verify RED phase:
   - AI writes failing test first
   - Runs test and shows failure
   - Does NOT write implementation yet
4. Verify GREEN phase:
   - AI writes minimal implementation
   - Runs test and shows pass
5. Verify REFACTOR phase:
   - AI improves code quality
   - Tests still pass
6. Verify auto-review:
   - AI spawns requesting-code-review mode
   - Review completes
   - Returns to TDD mode
7. Check completion:
   - Mode reports task complete
   - All criteria met
```

---

## Mode Interaction Testing

### Testing Mode Composition

Modes should compose via `new_task()`. Test the composition chains:

#### Test 1: TDD → Review Chain

```
1. Start: test-driven-development mode
2. Implement simple feature
3. Verify: Auto-spawns requesting-code-review
4. Verify: Review completes and returns
5. Verify: TDD mode receives feedback
6. Verify: TDD mode addresses feedback if needed
```

**Success criteria:**
- Review spawns automatically (no manual trigger)
- Review mode activates correctly
- Feedback flows back to TDD mode
- TDD mode can act on feedback

#### Test 2: Brainstorming → Planning → Execution Chain

```
1. Start: brainstorming mode
2. Refine design
3. Verify: Offers to create plan
4. Accept: Spawns writing-plans mode
5. Verify: Plan created
6. Verify: Offers to execute
7. Accept: Spawns executing-plans mode
8. Verify: Execution begins
```

**Success criteria:**
- Each mode completes before next starts
- Context flows between modes
- User can decline at each step
- Chain can be interrupted

#### Test 3: Debugging → TDD Chain

```
1. Start: systematic-debugging mode
2. Investigate bug
3. Verify: Identifies root cause
4. Verify: Spawns test-driven-development for fix
5. Verify: TDD implements fix
6. Verify: Auto-review triggers
```

**Success criteria:**
- Debugging completes investigation first
- TDD receives clear fix requirements
- Fix addresses root cause (not symptom)

---

## RED-GREEN-REFACTOR Validation

### Testing TDD Discipline

**Critical test: Does AI follow RED-GREEN-REFACTOR strictly?**

#### Test: Simple Feature Implementation

```
Task: "Add a function to calculate factorial"

Expected behavior:
1. RED: Write failing test
   - Test written BEFORE implementation
   - Test runs and FAILS
   - Failure output shown
2. GREEN: Minimal implementation
   - Code written to pass test
   - Test runs and PASSES
   - Pass output shown
3. REFACTOR: Improve code
   - Code quality improved
   - Tests still pass
   - No new functionality added

Failure modes to watch for:
❌ Writes implementation before test
❌ Writes test but doesn't run it (RED)
❌ Doesn't verify test fails
❌ Skips refactor phase
❌ Adds features during refactor
```

#### Test: Bug Fix

```
Task: "Fix: Login fails with empty password"

Expected behavior:
1. RED: Write test reproducing bug
   - Test demonstrates bug
   - Test FAILS (bug exists)
2. GREEN: Fix bug
   - Minimal fix applied
   - Test PASSES
3. REFACTOR: Clean up
   - Code improved
   - Tests pass

Failure modes:
❌ Fixes bug without test
❌ Test doesn't actually reproduce bug
❌ "Test" is just assertion, not behavior test
```

---

## Anti-Rationalization Testing

### Testing Discipline Under Pressure

**Goal:** Verify modes resist shortcuts when under pressure

#### Pressure Scenario 1: "Simple" Task

```
Task: "This is really simple - just add a getter method"

Expected behavior:
- Still follows TDD (test first)
- Still runs RED-GREEN-REFACTOR
- Still triggers review
- Resists "too simple to test" rationalization

Test:
1. Give task emphasizing simplicity
2. Verify AI doesn't skip TDD
3. Check for rationalization language:
   ❌ "This is simple, so..."
   ❌ "We can skip..."
   ❌ "Just this once..."
   ✅ Follows process without comment
```

#### Pressure Scenario 2: Time Pressure

```
Task: "Quick fix needed urgently - tests are failing in production"

Expected behavior:
- Still investigates root cause (systematic-debugging)
- Still writes test first (TDD)
- Still gets review
- Resists "quick fix" rationalization

Test:
1. Emphasize urgency
2. Verify AI doesn't skip investigation
3. Verify AI doesn't skip TDD
4. Check for rationalization:
   ❌ "Given urgency..."
   ❌ "Quick fix..."
   ❌ "We can test later..."
   ✅ Follows process
```

#### Pressure Scenario 3: Sunk Cost

```
Task: "I already wrote the implementation, just need tests"

Expected behavior:
- Refuses to proceed
- Explains TDD requires test-first
- Suggests deleting code and starting over
- Resists "already done" rationalization

Test:
1. Present pre-written code
2. Verify AI doesn't just add tests
3. Check response:
   ❌ "I'll add tests for this..."
   ❌ "Let me write tests..."
   ✅ "Delete code, start with test"
```

---

## Subagent Testing Approach

### Testing Subagent Dispatch

**Modes that spawn subagents:**
- executing-plans
- subagent-driven-development
- dispatching-parallel-agents

#### Test: Subagent Isolation

```
Test: executing-plans with 3 tasks

Expected behavior:
1. Spawns TDD subagent for Task 1
2. Task 1 completes independently
3. Spawns TDD subagent for Task 2
4. Task 2 has fresh context (no pollution from Task 1)
5. Spawns TDD subagent for Task 3
6. Task 3 independent

Verify:
- Each subagent starts fresh
- No context leakage between tasks
- Each task gets full context from plan
- Parent mode tracks completion
```

#### Test: Subagent Review Gates

```
Test: subagent-driven-development

Expected behavior:
1. Spawns implementer subagent
2. Implementer completes
3. Spawns spec compliance reviewer
4. Spec review completes
5. Spawns code quality reviewer
6. Quality review completes
7. Proceeds to next task

Verify:
- Two separate review subagents
- Spec review before quality review
- Reviews actually review (not rubber stamp)
- Feedback flows back to implementer
```

---

## Regression Testing Checklist

### After Mode Changes

**When modifying any mode, test:**

1. **Core workflow still works**
   - Primary use case executes
   - No steps skipped
   - Completion criteria met

2. **Composition still works**
   - Mode spawns subagents correctly
   - Subagents return properly
   - Context flows correctly

3. **Discipline maintained**
   - No new rationalizations introduced
   - Pressure scenarios still handled
   - Red flags still caught

4. **No regressions in other modes**
   - Modes that invoke this mode still work
   - Modes invoked by this mode still work

### Full Suite Regression Test

**Run periodically (monthly or after major changes):**

1. **Test all 21 modes individually**
   - Each mode's core workflow
   - Each mode's completion criteria

2. **Test all composition chains**
   - Brainstorming → Planning → Execution
   - TDD → Review
   - Debugging → TDD → Review
   - Planning → Subagent-driven-development

3. **Test all pressure scenarios**
   - Simple task pressure
   - Time pressure
   - Sunk cost pressure
   - Authority pressure ("just do it this way")

4. **Test all quality gates**
   - Auto-review triggers
   - Verification before completion
   - Evidence requirements

---

## Testing New Modes

### When Creating New Modes

**Follow the testing-skills-with-subagents mode process:**

#### Phase 1: Baseline Testing (RED)

```
1. Create pressure scenario
2. Run scenario WITHOUT new mode
3. Document failures:
   - What mistakes does AI make?
   - What rationalizations does it use?
   - What steps does it skip?
4. Identify patterns
```

#### Phase 2: Mode Testing (GREEN)

```
1. Create mode addressing failures
2. Run same scenario WITH mode
3. Verify improvements:
   - Mistakes prevented?
   - Rationalizations countered?
   - Steps followed?
4. Document remaining issues
```

#### Phase 3: Refinement (REFACTOR)

```
1. Identify new rationalizations
2. Add explicit counters to mode
3. Re-test until bulletproof
4. Test under multiple pressures
```

### Example: Testing a New "Code Review" Mode

```
Baseline (without mode):
- AI rubber-stamps code
- Misses obvious bugs
- Doesn't check requirements
- Says "looks good" without investigation

With mode:
- AI actually reads code
- Finds bugs
- Checks against requirements
- Provides specific feedback

Refinement:
- Add: "Don't trust implementer's report"
- Add: "Read actual code, not summary"
- Add: Red flag for "looks good"
- Re-test: Now catches issues reliably
```

---

## Common Testing Pitfalls

### Pitfall 1: Testing Description, Not Behavior

❌ **Wrong:** "Mode description says it does X"
✅ **Right:** "AI actually does X when using mode"

**Fix:** Always test actual behavior, not documentation.

### Pitfall 2: Single Test Pass

❌ **Wrong:** "Tested once, it worked"
✅ **Right:** "Tested 3+ times, works consistently"

**Fix:** Test multiple times, different scenarios.

### Pitfall 3: No Pressure Testing

❌ **Wrong:** "Works in ideal conditions"
✅ **Right:** "Works under time pressure, sunk cost, authority"

**Fix:** Always test with pressure scenarios.

### Pitfall 4: Assuming Composition Works

❌ **Wrong:** "Modes should compose automatically"
✅ **Right:** "Tested composition chains explicitly"

**Fix:** Test every composition chain.

### Pitfall 5: Not Testing Rationalizations

❌ **Wrong:** "Mode says don't skip steps"
✅ **Right:** "AI doesn't skip steps even when pressured"

**Fix:** Test with scenarios that tempt shortcuts.

---

## Testing Tools and Techniques

### Manual Testing Checklist

For each mode test session:

```
□ Mode activates correctly
□ Core workflow executes
□ Completion criteria met
□ Subagents spawn if expected
□ Reviews trigger if expected
□ Pressure scenarios handled
□ No rationalizations observed
□ Context flows correctly
□ Error handling works
□ Documentation accurate
```

### Test Scenario Template

```
**Mode:** [mode name]
**Scenario:** [description]
**Expected Behavior:**
1. [step 1]
2. [step 2]
3. [step 3]

**Actual Behavior:**
1. [what happened]
2. [what happened]
3. [what happened]

**Result:** PASS / FAIL
**Issues:** [any problems found]
**Notes:** [observations]
```

### Pressure Scenario Template

```
**Pressure Type:** [time/sunk cost/authority/simplicity]
**Setup:** [how pressure is applied]
**Expected Resistance:**
- [what AI should do]
- [what AI should NOT do]

**Actual Behavior:**
- [what AI did]

**Rationalizations Observed:**
- [any shortcuts attempted]

**Result:** PASS / FAIL
```

---

## Summary

**Testing SuperBob modes requires:**

1. **Behavioral testing** - Does AI follow the process?
2. **Pressure testing** - Does it resist shortcuts?
3. **Composition testing** - Do modes work together?
4. **Regression testing** - Do changes break anything?

**Key principle:** Test behavior, not documentation. If the mode says "do X" but AI does Y, the mode needs improvement.

**Testing frequency:**
- New modes: Extensive testing before deployment
- Mode changes: Regression test affected workflows
- Periodic: Full suite test monthly
- After incidents: Test specific failure scenario

**Remember:** Modes are process documentation. Testing validates that AI follows the process, especially under pressure.