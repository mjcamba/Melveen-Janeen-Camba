<!-- PR TARGET: https://github.com/mjcamba/Melveen-Janeen-Camba | Stage 1.1 (2.5 pts) -->
# Stage 1.1 review — engagement brief · **11 / 100** (F) · 0.28 / 2.5 pts

**Brief:** [`docs/briefs/perfect-competition-brief.md`](https://github.com/mjcamba/Melveen-Janeen-Camba/blob/main/docs/briefs/perfect-competition-brief.md)

> Graded 2026-08-27. I am not entering this score. The file is committed at the right path and the writing in it is accurate — but it answers a different question from the one this stage asks, and a number like this on work that is really a misunderstanding would be the wrong record of what happened. Nothing is due yet. Read the first section, rewrite it, and I will re-grade from scratch.

| Criterion | Earned | Notes |
|---|---|---|
| Problem restated in your own voice | 5 / 30 | What you have written is a correct textbook summary of perfect competition as a market structure — many buyers and sellers, homogeneous products, free entry and exit, price takers, zero economic profit in the long run. None of that is wrong. But it is not the farm's problem. The brief never mentions the farmer, the 64 beds, tomatoes, carrots, mesclun, the 36-week season, or a single figure from the case. What this criterion asks for is the specific decision in front of this specific farm, said in your words. |
| Hypothesis names a specific mix | 0 / 25 | There is no hypothesis and no mix. This is the criterion the stage is built around, so it is where the score mostly goes. |
| Economic mechanism | 6 / 25 | P = MC appears, and so does the price-taker point, and both are stated correctly. That is worth something and it is the part of your file to keep. What is missing is the mechanism applied to this farm: which crop stops where, and why. |
| Falsifiability and process | 0 / 20 | There is no statement of what result would show a prediction wrong, because there is no prediction yet. Process credit where it is due — the file is at the correct path, docs/briefs/perfect-competition-brief.md, and committed with a clear message. When you rewrite it, that part is already right. |
| **Final** | **11 / 100** | earned on merit |

### What happened, and why i am not entering a score

This reads as a brief written about the topic rather than about the case. Perfect competition is the economics the case is teaching; the assignment is to apply it to one farm and commit to a number. It is a very easy assignment to misread that way, and the file you produced would be a good answer to "explain perfect competition" — which is not what was asked.

One detail that tells you the same thing: the last line of the file reads "This brief can be expanded with specific examples, diagrams, or mathematical models as needed." That is an assistant offering to continue, left in the committed version. When you see a sentence like that in your own file, it usually means the draft stopped before the thinking started.

You went from 80 to 97 on Stage 0 in a single session by doing exactly what the feedback asked, item by item. This is the same kind of task. The list below is what to write.

### What the brief needs to say

Half a page to a page. Replace what is there; do not add to it.

- State the farm's problem in your own words. A market garden has 64 beds and one 36-week season. It must choose how many beds of tomatoes, carrots, and mesclun to plant. What is fixed: the season, $20,000 of fixed costs, the prices ($8,800 a bed for tomatoes, $2,094 for carrots, $2,700 for mesclun), the per-crop bed caps of 20 / 20 / 30, the farmer's 720 field hours, and up to four temporary workers at 1,440 hours each. What is chosen: the three bed counts. What limits the choice: labor, and the fact that each additional bed of a crop makes every bed of that crop more labor-hungry.

- Name a specific mix. "I expect X tomato beds, Y carrot beds, Z mesclun beds." Real integers. Not a range, not percentages, not "a balanced mix."

- Say why, using the case's numbers. The mechanism that decides this case is diminishing returns: labor hours for q beds of a crop are q x hours-per-week-per-bed x 36 x (1 + rate)^q, where the rate is 10 percent a bed for tomatoes, 2.5 percent for carrots, and 1.25 percent for mesclun. That compounding is why marginal cost rises, and why the answer is not simply "plant the crop with the highest price." Tomatoes earn $8,800 a bed against carrots' $2,094 — but the 20th tomato bed costs roughly 6.7 times the labor per bed of the first. Say which crops you think stop because marginal cost catches the price, and which stop because they run into a bed cap.

- Say how you would know you were wrong. Two or three named outcomes, each tied to the assumption it would break. "If carrots finish below their 20-bed cap, then something other than the cap stopped them, and my reasoning about diminishing returns is wrong." A prediction that survives every possible result is not a prediction.

- Log the AI critique in prompt-log.md. The stage asks you to have a draft attacked — ask a model to name your unstated assumptions and to judge whether your hypothesis is falsifiable, and to not rewrite anything. Then make the changes in your own words and record the session.

### Why the order matters

This stage is graded before the model exists, and that is deliberate. A prediction written before the model runs can be shown wrong. The same sentence written afterwards is a summary of the output and it teaches you nothing, because you can no longer tell "I understood the economics" from "I read the answer cell." Stage 3 asks you to compare what you predicted against what the model found, and that comparison cannot be reconstructed later.

A wrong hypothesis, precisely reasoned, is worth as much here as a correct one and considerably more than a lucky one. Do not try to guess the right answer. Say what you actually expect and why, and let the model disagree with you.

### Looking ahead

Stage 1.2 asks for capabilities/marginal-analysis/spec.md, written before the workbook exists. Your capabilities/ folder has a README but no capability folder inside it yet — that is the one canonical path still missing from your Stage 0, and it is where this next deliverable goes. Create capabilities/marginal-analysis/README.md when you get a moment and both problems close at once.

The reasoning you put in this brief is the reasoning that spec runs on, so it is worth the hour.

---

### How to work this review

Treat this PR the way an analyst treats feedback from a senior reviewer — a review is a proposal to engage with, not a checklist to rubber-stamp.

1. **Read it yourself first.** Form your own view before you change anything. Disagreeing *with a documented reason* is a legitimate, senior response.
2. **Stress-test it with an LLM.** Paste this review and your brief into your assistant and ask it to (a) explain anything you are unsure of, and (b) argue the *other side* — where might the reviewer be wrong, and what would you give up by making each change.
3. **Then write the changes yourself.** For a brief, this matters more than usual: a hypothesis you did not generate cannot be honestly compared against your model in Stage 3, and that comparison is the entire point of writing the brief first.
4. **Close the loop.** Reply in this thread with what you changed and what you pushed back on, then commit and push.

*One standing rule for this stage: do not revise your hypothesis to match what your model later tells you. If the model contradicts the brief, that is a finding, not an error — Stage 3 asks you to explain the gap, and a brief quietly edited to be right afterwards has nothing left to explain.*

— Adam
