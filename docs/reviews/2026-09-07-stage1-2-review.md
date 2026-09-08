<!-- PR TARGET: https://github.com/mjcamba/Melveen-Janeen-Camba | Stage 1.2 -->
# Stage 1.2 review — spec, build, audit

> **Hurricane Lowell.** If you are boarding up, packing, or hauling the patio furniture indoors, put this review down — it will keep, and nothing in it needs you today. And if you are reading a review while a hurricane bears down on the islands: I am writing one in the same weather, so there is no judgement coming from this end. :) Look after your people first — the coursework will survive whatever Lowell does.

**Spec:** [`capabilities/marginal-analysis/spec.md`](https://github.com/mjcamba/Melveen-Janeen-Camba/blob/main/capabilities/marginal-analysis/spec.md)

> Re-graded 2026-09-08. The previous pass found six commits and none of them at the graded path. What is there now is a real specification — detailed, organised, and buildable. What is missing is the workbook, and the two criteria that depend on it are the reason this is still held rather than entered.

| Criterion | Where it stands |
|---|---|
| Spec completeness — inputs, structure, calculation flow | A genuine specification. Every input named with a unit and a source, the labor functions with the exponent in the right place, revenue, fertilizer, the farmer-first allocation, the blended rate, profit, the constraint checks and the marginal-cost schedules, plus seventeen conventions and a definitions section. Three input values are rounded where they should be derived, and one cap is wrong. |
| Spec validation rules | Structural validation, the q = 1 hand calculation, constraint checks, an independent cross-check, a two-starting-point Solver rule, and acceptance criteria naming the mix, the bed count, the profit and all three crossings. Complete for this criterion, and the crossings you predicted are the right ones. |
| Workbook satisfies the contract | There is no model.xlsx in the repository. The earlier files at the wrong paths were deleted and nothing replaced them, so there is nothing to assess. |
| Audit note | Cannot exist without a build. This is not a judgment about your work — the criterion has no input yet. |

### What you fixed, and it was the right fix

The last review's finding was that six commits had landed and none of them at the path that gets graded — there were nested folders like capabilities/capabilities/marginal-analysis, and a spec.md that was one byte.

You deleted all of it and rebuilt at the correct path. That is the right response and it is a braver one than patching around the mess, which is what most people do. The path is clean now.

### The three numbers to change before you build

Your input tables carry CAR_HRS as 0.83, FARMER_LABOR_RATE as 34.72 and TEMP_LABOR_RATE as 17.36. All three are the case's printed display values, and all three are rounded.

The real ones are derivations. The farmer's rate is $50,000 across 1,440 hours. The temporary rate is $25,000 across 1,440 hours. Carrot labor is tomato labor divided by three — 0.8333…, and note that your 0.83 is rounded one place further than the case's own 0.833.

Your acceptance criterion says season profit must equal $42,762 when rounded to the nearest dollar. With those three rounded inputs it will not, and you will spend an evening looking for a bug that is not in your formulas. Fix them in the spec now, before the build, and the acceptance test will pass the first time.

### One cap is wrong, and the spec says it overrules me

Your conventions section states that the authoritative bed caps are 14 tomato, 20 carrot and 30 mesclun, and that these limits override conflicting values from other materials.

The tomato cap in the case is 20, not 14. As it happens this changes nothing in the answer, because the optimum plants 10 tomato beds and 10 is under both numbers — so your model will still produce the right result.

It is worth correcting anyway, for a reason that has nothing to do with this case. A specification that declares itself authoritative over its sources is making a strong claim, and the strength of the claim is exactly why a reader will trust it and not check. When you write "this overrides other materials", you are taking responsibility for having verified it. Keep the sentence — it is good practice to say which source wins — and make sure the value is right.

### What to do next, in order

- Change the three rounded inputs to their derivations and fix the tomato cap. Ten minutes in the spec.

- Build the workbook from the spec at capabilities/marginal-analysis/model.xlsx. Your calculation logic is complete enough that you can hand the specification to an assistant and have it build from the document rather than from a conversation — that is what the document is for.

- Run Solver from both starting points you already named, and record what each returned.

- Write the audit note: at least three checks, each saying what it would have caught, and any defect you found with what you did about it. Your validation section already lists the checks, so this is recording results rather than inventing tests.

The spec-side criteria are most of the way there. The build and the audit are the half that is missing, and they are the half you can finish.

---

### How to work this review

Treat this PR the way an analyst treats feedback from a senior reviewer — a review is a proposal to engage with, not a checklist to rubber-stamp.

1. **Read it yourself first.** Form your own view before you change anything. Disagreeing *with a documented reason* is a legitimate, senior response.
2. **Stress-test it with an LLM.** Paste this review and your spec into your assistant and ask it to (a) explain anything you are unsure of, and (b) argue the *other side* — where might the reviewer be wrong, and what would you give up by making each change.
3. **Then correct the spec, not the workbook.** This is the rule that makes the stage work: when a check fails, you fix the specification and regenerate, so the document keeps describing what was actually built.
4. **Close the loop.** Reply in this thread with what you changed and what you pushed back on, then commit and push.

*Your score and the per-criterion breakdown are in your Lamaku comment, not here — this repository is public.*

— Adam
