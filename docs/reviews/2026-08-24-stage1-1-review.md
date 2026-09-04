<!-- PR TARGET: https://github.com/mjcamba/Melveen-Janeen-Camba | Stage 1.1 -->
# Stage 1.1 review — engagement brief

**Brief:** [`docs/briefs/perfect-competition-brief.md`](https://github.com/mjcamba/Melveen-Janeen-Camba/blob/main/docs/briefs/perfect-competition-brief.md)

> Re-graded 2026-09-04 against your rewrite of 3 September. The previous pass was a hold rather than a grade, because the brief was about a different scenario entirely. You rewrote it for this case and it is a genuinely good brief. As promised, there is no penalty for the delay.

| Criterion | Where it stands |
|---|---|
| Problem restated in your own voice | Real and yours. You separate what is fixed from what is chosen, name the consequence, and identify the constraint correctly: "each additional bed of a crop makes every crop more labor intensive." Two things hold it back. That sentence has one word wrong — an additional bed of a crop makes every bed of *that* crop more labor intensive, not every crop; the three crops compete for the same hours but they do not inflate each other's labor requirement. And the section is compressed to a single paragraph where the stage asks for closer to half a page, so several things are named without being developed — most notably what it actually costs the farm to get this wrong. |
| Hypothesis names a specific mix | 14 tomato, 20 carrot, 30 mesclun, totalling exactly 64. Three real integers, every one inside its cap, and you say which crops you expect to be stopped by their cap rather than by economics. That is precisely what this criterion asks for. |
| Economic mechanism | You transcribed the labor engine correctly — "q x hours-per week per bed x 36 (1 +rate)^q" — with the exponent on q, which is the single thing most likely to be got wrong in this case and which two finished workbooks in this cohort still get wrong. You then use it: the twentieth tomato bed needs about 6.7 times the labor of the first, so you do not expect all 20 to be worth planting. That is a real argument. What is still open is that you attribute the 6.7 figure to the case — the case gives you the 10% rate and you have to raise 1.10 to the twentieth yourself to get 6.7. It is your arithmetic and you should claim it. And 14 is never derived; you argue tomatoes stop short of the cap without arguing for 14 in particular. |
| Falsifiability and process | Three conditions, each naming an outcome the model could produce and the claim it would break, and a closing sentence that ties all three back to the same underlying question — which crops are limited by marginal cost and which by their bed cap. That framing is better than the conditions themselves. What is still open is that none carries a tolerance: "if tomatoes reach the full 20-bed cap" treats 19 beds and 20 beds as completely different verdicts when they are almost the same result. |

### What changed, and why it matters more than the number

Your previous brief was about a different scenario. That was a hold rather than a grade precisely because it reads as a misread assignment rather than weak work — and the rewrite confirms it. This brief has a correct labor function, a committed mix, a mechanism that uses the rates, and three falsification conditions. That is the whole deliverable.

It is also worth saying that you did this after the review had been sitting unread for a while. The pull request notifications were not reaching anyone in this cohort — that was my fault, not yours, and it is fixed now.

### The one sentence to fix, and it is a real distinction

"Each additional bed of a crop makes every crop more labor intensive."

It makes every bed of *that* crop more labor intensive. Planting an eleventh tomato bed does not make your carrots harder to grow.

The distinction matters because it is the difference between three separate compounding curves and one shared one. The crops interact only through the shared pool of hours and the shared 64 beds — not through each other's labor rate. When you build the model in Stage 1.2 you will write three independent labor schedules and then add them up, and if you were thinking of it the other way the structure would come out wrong.

### What to carry into Stage 1.2

You now have capabilities/marginal-analysis/ with a README, which closed the last gap in your Stage 0. The specification goes in the same folder and the stage is due 6 September.

Your 6.7 multiplier is the seed of a validation rule. In the workbook it becomes a hand-check: labor hours for one tomato bed must come to 1 x 2.50 x 36 x 1.10 = 99 exactly, and for ten beds to 10 x 2.50 x 36 x 1.10^10 = 2,334.37. The second one is the check that catches a builder who applies the rate once instead of compounding it, which is the defect your own sentence already guards against.

---

### How to work this review

Treat this PR the way an analyst treats feedback from a senior reviewer — a review is a proposal to engage with, not a checklist to rubber-stamp.

1. **Read it yourself first.** Form your own view before you change anything. Disagreeing *with a documented reason* is a legitimate, senior response.
2. **Stress-test it with an LLM.** Paste this review and your brief into your assistant and ask it to (a) explain anything you are unsure of, and (b) argue the *other side* — where might the reviewer be wrong, and what would you give up by making each change.
3. **Then write the changes yourself.** For a brief this matters more than usual: a hypothesis you did not generate cannot be honestly compared against your model in Stage 3, and that comparison is the entire point of writing the brief first.
4. **Close the loop.** Reply in this thread with what you changed and what you pushed back on, then commit and push.

*One standing rule: do not revise your hypothesis to match what your model later tells you. If the model contradicts the brief, that is a finding, not an error.*

*Your score and the per-criterion breakdown are in your Lamaku comment, not here — this repository is public.*

— Adam
