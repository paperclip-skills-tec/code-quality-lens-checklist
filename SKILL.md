---
name: code-quality-lens-checklist
description: "Systematic code quality review using 10 named quality lenses (Naming Clarity, SRP, OCP, DRY, YAGNI, Cognitive Complexity, Function Contract Clarity, Consistency, Readability over Cleverness, Error Surface). Use this skill at the start of every code quality review session — it ensures every lens is explicitly checked and produces an auditable Lens Coverage table in the final review comment. Invoke whenever you are doing a code quality review, PR review, or code analysis session, even if you think you already know the lenses — skipping the checklist is how reviews end up with inconsistent coverage."
---

# Code Quality Lens Checklist

This skill enforces systematic coverage across all 10 quality lenses on every review.
The goal is auditability: a reader of the review should be able to tell whether each
lens was checked and found clean, not just which lenses produced findings.

## Why systematic coverage matters

Ad-hoc lens application naturally drifts toward the lenses you find most often in the
current codebase. The lenses you rarely find anything in are the ones most likely to
be skipped — and therefore the most likely to harbour undetected issues over time.
Running every lens every time closes that gap.

---

## Step 1 — Set up the lens checklist

At the start of the review, create a TodoWrite checklist with one item per lens.
Use this exact set, in order:

1. Naming Clarity — are names honest, specific, and unambiguous?
2. SRP — does each unit do one thing?
3. OCP — is existing behaviour extended without modification?
4. DRY — is knowledge expressed in exactly one place?
5. YAGNI — is every piece of code earned by a current requirement?
6. Cognitive Complexity — can a reader follow the control flow without holding a mental model?
7. Function Contract Clarity — are preconditions, postconditions, and side effects evident?
8. Consistency — do the changes follow the conventions already present in the codebase?
9. Readability over Cleverness — is the clearest solution preferred over the most compact one?
10. Error Surface — are error paths explicit, narrow, and handled close to their source?

---

## Step 2 — Evaluate each changed file per lens

Work through the checklist in order. For each lens:

- Apply it to every changed file or function in scope
- If you find an issue: note the file, line(s), principle, why it matters, and the fix direction
- Mark the lens complete in the checklist

Don't batch or skip. Lenses that seem less likely to apply are still worth the scan
— a clean result is itself useful information.

---

## Step 3 — Include a Lens Coverage table in the review comment

After completing all lenses, include this table at the end of the review comment
(or as its own section if the review is long):

```
## Lens Coverage

| Lens | Checked | Finding |
|------|---------|---------|
| Naming Clarity | ✓ | No issues |
| SRP | ✓ | `UserService.save()` handles both validation and persistence — see Finding #1 |
| OCP | ✓ | No issues |
| DRY | ✓ | No issues |
| YAGNI | ✓ | No issues |
| Cognitive Complexity | ✓ | No issues |
| Function Contract Clarity | ✓ | No issues |
| Consistency | ✓ | No issues |
| Readability over Cleverness | ✓ | No issues |
| Error Surface | ✓ | No issues |
```

Rules for the table:
- Every lens must appear, even if clean
- "Checked" is always ✓ (if a lens was skipped for scope reasons, explain in a note below the table)
- "Finding" is either "No issues" or a one-line summary referencing the specific finding in the review body
- Do not omit a row because it's clean — that's the whole point

---

## Lens Quick Reference

Use these as the minimum evaluation questions per lens. They are starting points,
not exhaustive checklists.

**Naming Clarity**
Does the name accurately describe what the thing *is* or *does*? Would a new team
member understand it without reading the implementation?

**SRP**
Does this function/class/module have exactly one reason to change? If the unit touches
multiple concerns (e.g. validation + persistence, formatting + I/O), flag it.

**OCP**
When new behaviour is needed, can it be added by extending rather than modifying
existing code? If the change required editing existing switch/if-chains or base
implementations, consider whether an extension point was missed.

**DRY**
Is this logic or knowledge duplicated? Look for copy-pasted blocks, parallel
conditionals that encode the same rule, and constants repeated across files.

**YAGNI**
Is every abstraction, parameter, or configuration option earned by something that
is actually used now? Flag unused generality — interfaces with one implementation,
flags that are never false, config that is never varied.

**Cognitive Complexity**
Can you trace through the control flow without holding a stack in your head?
Watch for deep nesting, multiple early returns through different paths, and
boolean logic that requires a truth table to evaluate.

**Function Contract Clarity**
Can you determine what a function expects, what it returns, and what it changes —
without reading its body? Look for missing null guards on parameters that could
be null, undocumented side effects, and return types that vary based on hidden state.

**Consistency**
Does this code follow the same conventions as the surrounding codebase? Check
naming style, error handling patterns, module structure, and test conventions.
Inconsistency is often a sign of copy-paste from a different context.

**Readability over Cleverness**
Is the intent of the code clear on first read? Flag one-liners that compress too
much logic, clever use of language features where a plain form would be clearer,
and premature micro-optimisations that trade clarity for performance with no
measured benefit.

**Error Surface**
Are errors represented explicitly (typed, structured) and handled close to their
source? Watch for swallowed exceptions, overly broad catch clauses, silent
fallbacks that hide failures, and error types that don't carry enough context
for diagnosis.

---

*TEC Custom Skill — maintained by the Deltek Technical Services Engineering team.*
