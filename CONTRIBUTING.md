# Contributing to SuperBob

Thank you for your interest in contributing to SuperBob! This project provides the [superpowers](https://github.com/obra/superpowers) development methodology for IBM Bob.

---

## How to Contribute

### Reporting Issues

Found a bug or have a feature request?

1. Check [existing issues](https://github.ibm.com/ERANRA/super-bob/issues) first
2. If not found, [create a new issue](https://github.ibm.com/ERANRA/super-bob/issues/new)
3. Include:
   - Clear description
   - Steps to reproduce (for bugs)
   - Expected vs actual behavior
   - IBM Bob version
   - Relevant logs or screenshots

---

### Suggesting Enhancements

Have ideas for improving SuperBob?

1. [Open an issue](https://github.ibm.com/ERANRA/super-bob/issues/new) with the "enhancement" label
2. Describe:
   - The problem you're trying to solve
   - Your proposed solution
   - Why it aligns with SuperBob's core methodology

---

## Development Workflow

**IMPORTANT:** When contributing to SuperBob, use SuperBob! Practice what we preach.

### Setting Up

1. **Fork the repository**
   ```bash
   git clone https://github.com/YOUR_USERNAME/super-bob.git
   cd super-bob
   ```

2. **Install SuperBob locally** (project-specific):
   ```bash
   # Files are already in the repo
   # Just open in IBM Bob and the modes will be available
   ```

3. **Create a branch**
   ```bash
   git checkout -b feature/your-feature-name
   ```

---

### Making Changes

Use the SuperBob methodology for all contributions:

#### For New Features:
1. **Start with using-superpowers mode** (entry point)
2. Use `/brainstorm` or select **brainstorming mode** to refine the design
3. Use `/write-plan` or select **writing-plans mode** to create implementation plan
4. Use `/execute-plan` or select **executing-plans mode**
5. Subtasks will use **test-driven-development mode** (TDD enforced)
6. Auto-review will trigger after each task

**Or go direct:**
1. Select **test-driven-development mode** for implementing
2. Write test first (RED), implement (GREEN), refactor
3. Auto-review triggers automatically

#### For Bug Fixes:
1. **Use systematic-debugging mode** (or `/debug`)
2. Follow the 4-phase debugging framework:
   - Phase 1: Root cause investigation
   - Phase 2: Pattern analysis
   - Phase 3: Hypothesis and testing
   - Phase 4: Implementation (with TDD)
3. Auto-review will trigger after fix

#### For Documentation:
1. **Use brainstorming or writing-plans mode** for substantial docs
2. Or any mode for minor doc changes
3. Request review if significant (use **requesting-code-review mode** or `/review`)

---

### Pull Request Process

1. **Ensure all tests pass**
   ```bash
   # Run verification appropriate to your changes
   ```

2. **Update documentation**
   - Update README.md if adding features
   - Update docs/ARCHITECTURE.md if changing architecture
   - Add/update examples if helpful

3. **Follow commit message conventions**
   ```
   Add feature: brief description

   Longer explanation of what changed and why.

   - Bullet points for details
   - Reference issues: Fixes #123
   ```

4. **Create Pull Request**
   - Clear title describing the change
   - Reference related issues
   - Describe what was changed and why
   - Include test results

5. **Code Review**
   - Address feedback from reviewers
   - Make requested changes
   - Update PR description if scope changes


---

## Pull Request Quality Gates

**Every PR MUST include evidence of the following:**

### 1. Evidence of Testing

**Required:**
- Test output showing all tests pass
- Verification commands run and output shown
- No "I tested it" without proof

**Examples:**
```bash
# Good - shows actual output
$ npm test
✓ 34 tests passing
0 failures

# Bad - no evidence
"I tested it and it works"
```

### 2. Evidence of Verification

**Required:**
- Fresh command output (not cached)
- Actual terminal output (copy-paste)
- All verification steps documented

**For code changes:**
- Show test suite passing
- Show linter passing (if applicable)
- Show build succeeding (if applicable)

**For documentation:**
- Show markdown validation
- Show links working
- Show examples tested

### 3. Code Review Compliance

**Required:**
- Used requesting-code-review mode OR manual review
- Review feedback addressed
- Evidence of fixes (test output, diffs)

**Not acceptable:**
- "I reviewed it myself" (without using review mode)
- Skipping review for "simple changes"
- "Will fix in next PR"

### 4. Documentation Updates

**Required for:**
- New features → Update README.md
- Architecture changes → Update docs/ARCHITECTURE.md
- New modes → Update mode list in ARCHITECTURE.md
- Breaking changes → Update migration guide

**Not required for:**
- Bug fixes (unless behavior changes)
- Internal refactoring
- Test-only changes

### 5. Discipline Enforcement

**Required:**
- TDD followed (test-first for code changes)
- Verification before completion
- Root cause investigation (for bug fixes)
- Review completed

**Evidence:**
- Commit history shows test-first
- PR description shows investigation process
- Review feedback and responses included

---

## Reviewer Checklist

**When reviewing PRs, verify:**

### Quality Gates Met
- [ ] Evidence of testing provided
- [ ] Evidence of verification provided
- [ ] Code review compliance shown
- [ ] Documentation updated (if needed)
- [ ] Discipline enforced

### Technical Quality
- [ ] Tests exist and test behavior (not mocks)
- [ ] Code follows project patterns
- [ ] Error handling appropriate
- [ ] No obvious bugs or edge cases
- [ ] Security considerations addressed

### Process Compliance
- [ ] TDD followed (for code changes)
- [ ] Root cause investigated (for bug fixes)
- [ ] Review feedback addressed
- [ ] Commits are logical and well-described

---

## Red Flags in PRs

**Reject PRs that:**

❌ **Claim without evidence**
- "Tests pass" without showing output
- "I verified it" without proof
- "Looks good" without investigation

❌ **Skip discipline**
- Code before tests
- No review performed
- Quick fixes without investigation
- "Too simple to test"

❌ **Incomplete work**
- Tests failing
- Linter errors
- Build broken
- Documentation missing

❌ **Rationalization**
- "Just this once"
- "Will fix later"
- "Not worth testing"
- "Trust me"

---

## Good PR Examples

### Example 1: Feature Implementation

```markdown
## Summary
Add email validation to user registration

## Evidence of Testing
```
$ npm test
✓ validates email format
✓ rejects invalid emails
✓ accepts valid emails
34/34 tests passing
```

## Evidence of Verification
```
$ npm run lint
✓ No linting errors

$ npm run build
✓ Build successful
```

## Code Review
Used requesting-code-review mode. Feedback:
- Add test for empty email → Fixed in commit abc123
- Extract regex to constant → Fixed in commit def456

Re-review: Approved

## Documentation
Updated README.md with validation rules
```

### Example 2: Bug Fix

```markdown
## Summary
Fix: Login fails with empty password

## Root Cause Investigation
Used systematic-debugging mode:
1. Reproduced bug consistently
2. Traced to missing validation in submitForm()
3. Found working validation in other forms
4. Hypothesis: Missing password check

## Evidence of Testing
```
$ npm test
✓ rejects empty password (NEW TEST)
✓ existing tests still pass
35/35 tests passing
```

## TDD Process
1. Wrote failing test (commit abc123)
2. Verified test fails (output shown)
3. Implemented fix (commit def456)
4. Verified test passes (output shown)

## Code Review
Auto-review triggered. No issues found.
```

---

## Bad PR Examples

### ❌ Example 1: No Evidence

```markdown
## Summary
Fixed the bug

## Changes
Updated the validation logic

## Testing
I tested it and it works now
```

**Problems:**
- No evidence of testing
- No verification output
- No review performed
- No root cause investigation

### ❌ Example 2: Code Before Tests

```markdown
## Summary
Add new feature

## Commits
1. Implement feature
2. Add tests

## Testing
All tests pass
```

**Problems:**
- Tests written after code (not TDD)
- No evidence of test output
- No review mentioned

---

---

## Code Standards

### Core Principles (Non-Negotiable)

All contributions MUST follow SuperBob's core principles:

- 🔴 **NO CODE WITHOUT FAILING TEST FIRST**
- ✅ **NO COMPLETION CLAIMS WITHOUT FRESH VERIFICATION**
- 🔍 **ROOT CAUSE INVESTIGATION BEFORE FIXES**
- 👁️ **REVIEW EARLY, REVIEW OFTEN**

### Mode-Specific Guidelines

When modifying modes:

1. **Preserve core methodology** - Don't weaken the discipline
2. **Test with subagents** - Ensure modes work as intended
3. **Document changes** - Update ARCHITECTURE.md
4. **Maintain fidelity** - Stay true to original superpowers when possible

---

## Types of Contributions Needed

### High Priority

- **Bug fixes** - Modes not working as expected
- **Documentation improvements** - Clarify confusing sections
- **Examples** - Real-world usage examples
- **Testing** - Verify modes work in different scenarios

### Welcome Contributions

- **New slash commands** - Additional helpful workflows
- **Mode enhancements** - Improve existing modes (without weakening discipline)
- **Installation scripts** - Easier setup process
- **Integration guides** - How to use with specific frameworks

### Not Accepting

- **Weakening discipline** - Removing TDD, verification, or review requirements
- **Mode bypass features** - Anything that makes it easier to skip SuperBob modes
- **Anti-patterns** - Changes that encourage bad practices

---

## Community Guidelines

### Be Respectful

- Treat all contributors with respect
- Assume good intentions
- Provide constructive feedback
- Focus on the code, not the person

### Technical Rigor

SuperBob values **technical rigor over politeness**:
- Point out issues clearly and directly
- "This has a race condition" not "Maybe consider..."
- Question assumptions when appropriate
- Never performatively agreeable

But always be respectful and professional.

---

## Questions?

- **General questions:** [Open a discussion](https://github.ibm.com/ERANRA/super-bob/discussions)
- **Bug reports:** [Open an issue](https://github.ibm.com/ERANRA/super-bob/issues)
- **Original superpowers:** See [obra/superpowers](https://github.com/obra/superpowers)

---

## License

By contributing to SuperBob, you agree that your contributions will be licensed under the MIT License.

---

**Thank you for helping make SuperBob better!** 🦘
