<!-- PR TARGET: https://github.com/mjcamba/Melveen-Janeen-Camba | Stage 1.2 -->
# Stage 1.2 review — spec, build, audit

**Spec:** [`capabilities/marginal-analysis/capabilities/marginal-analysis/spec.md`](https://github.com/mjcamba/Melveen-Janeen-Camba/blob/main/capabilities/marginal-analysis/capabilities/marginal-analysis/spec.md)

> Graded 2026-09-07 against your commits of this morning. Six commits went in and none of them landed where the stage looks, and the file named model.xlsx is not a workbook. Everything below is about how to get the files into the right places — your plan for the model itself, which you wrote in the Stage 1.1 thread this morning, is correct.

| Criterion | Where it stands |
|---|---|
| Spec completeness — inputs, structure, calculation flow | There is no specification. capabilities/marginal-analysis/spec.md does not exist. The file that does exist sits two folders deeper, at capabilities/marginal-analysis/capabilities/marginal-analysis/spec.md, and its contents are the critique prompt you meant to send to your assistant — "Here is my model specification. Do not rewrite it" and the three numbered requests — rather than the specification the prompt refers to. The small credit here is for capabilities/marginal-analysis/README.md, which is at the right path and is written in your own words. |
| Spec validation rules | Nothing to assess — there is no specification for rules to live in. |
| Workbook satisfies the contract | There is no workbook. The file at capabilities/marginal-analysis/capabilities/marginal-analysis/capabilities/marginal-analysis/model.xlsx is forty-two bytes of plain text, and the text is the line "capabilities/marginal-analysis/model.xlsx". It has an .xlsx name and no Excel content in it at all — Excel will not open it. |
| Audit note | No audit note against a model, because there is no model. The credit is for the prompt-log entry for the brief, which records the critique prompt you used, what the assistant flagged and what you then changed yourself. That is the audit habit this stage wants, applied to the previous stage. |

> Held rather than entered. Nothing is recorded against you while this stage is still open.

### What actually happened, because it is worth understanding

Your repository now contains capabilities/capabilities/marginal-analysis/, and capabilities/marginal-analysis/capabilities/marginal-analysis/, and one more level below that. That pattern comes from one specific thing.

When you use Add file, then Create new file on github.com, the filename box also accepts folders: any forward slash in it creates a directory. If you have already navigated into capabilities/marginal-analysis/ and then type the full path capabilities/marginal-analysis/spec.md into the name box, GitHub reads that as "make those two folders again, here" — which is exactly what it did, three times.

The fix is to type only the filename. Navigate into capabilities/marginal-analysis/, click Add file, Create new file, and type spec.md and nothing else. The breadcrumb above the box already shows the folder you are in.

### And why model.xlsx is not a workbook

The Create new file form makes text files only. It cannot produce an Excel workbook no matter what you name the file — naming a text file model.xlsx gives you a text file called model.xlsx, which is what happened.

A workbook has to be built in Excel on your own machine, saved, and then uploaded. Navigate into capabilities/marginal-analysis/, click Add file, then Upload files — not Create new file — and drag the .xlsx in.

### The shortest path from here

- Delete the three stray folders: capabilities/capabilities/, and everything under capabilities/marginal-analysis/capabilities/. Open each file, click the bin icon, commit. Deleting the last file in a folder removes the folder.

- Create capabilities/marginal-analysis/spec.md and write the specification into it — not the prompt about it. Inputs with values, units and sources; the sheets the workbook will contain; the labor function; how the farmer's 720 hours and the four temporary workers are consumed; the Solver setup; validation rules with tolerances.

- Then send that document through the critique prompt you already wrote. It is a good prompt and it worked well on your brief — it just needs a document under it.

- Build the workbook in Excel from the finished specification, and upload it.

The order matters and is graded from your commit history: specification committed first, workbook second.

### Your question from this morning

You asked how to make it clear that the labor compounding is crop-specific rather than shared across all crops. The answer is in the shape of the formula, and it is easier than the wording suggests.

Each crop gets its own function, with its own bed count in the exponent and its own rate:

- Tomatoes: hours = q x 2.50 x 36 x 1.10^q

- Carrots: hours = q x (2.5/3) x 36 x 1.025^q

- Mesclun: hours = q x 1.25 x 36 x 1.0125^q

The q in each exponent is that crop's own bed count only. Planting a thirtieth mesclun bed does nothing to the tomato exponent. The three crops meet in exactly one place: you add the three hour figures together, and that total is what has to fit inside the farmer's 720 hours plus the temporary pool. The beds meet in one other place — the three bed counts have to add to 64 or fewer.

So: three independent schedules, joined only by two shared constraints. That is precisely what you described in the thread, and writing those three lines into the specification is what makes it explicit.

Your one-bed and ten-bed tomato checks are the right ones, and here are the values to test against: one bed is 99.0 hours exactly, and ten beds are 2,334.37. A model that has applied the multiplier once instead of compounding it will return 990 at ten beds, and the check catches it immediately.

---

### How to work this review

Treat this PR the way an analyst treats feedback from a senior reviewer — a review is a proposal to engage with, not a checklist to rubber-stamp.

1. **Read it yourself first.** Form your own view before you change anything. Disagreeing *with a documented reason* is a legitimate, senior response.
2. **Stress-test it with an LLM.** Paste this review and your spec into your assistant and ask it to (a) explain anything you are unsure of, and (b) argue the *other side* — where might the reviewer be wrong, and what would you give up by making each change.
3. **Then correct the spec, not the workbook.** This is the rule that makes the stage work: when a check fails, you fix the specification and regenerate, so the document keeps describing what was actually built.
4. **Close the loop.** Reply in this thread with what you changed and what you pushed back on, then commit and push.

*Your score and the per-criterion breakdown are in your Lamaku comment, not here — this repository is public.*

— Adam
