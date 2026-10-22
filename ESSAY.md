# The model reads, the code decides

*What testing six AI projects taught me about where AI belongs in a product. October 2026.*

Every product I have built with AI follows one rule. The model does the reading: messy quotes, call transcripts, grocery lists and bills, a day of scattered work. Plain, tested code does the deciding: totals, rankings, what gets written, what gets shown first. I believed the rule worked. Then I tested it.

I tested each of six projects and kept a record of every test. All the data was fictional. I wrote down the correct answers before the model saw a single case. (For followthrough, I corrected those answers once after a review; its EVALS.md says how.) Here is what came back, including the parts that did not flatter me.

## Where the rule held

**kill-the-quote** reads supplier quotes in five formats (Excel, PDF, Word, a phone photo, an email) and puts them into one comparison. After two fixes, it read 107 of 107 priced lines with the exact price and unit. (A separate, harder test with five photos is covered below.) Every time I ran the test, the model made up zero prices for lines a supplier never priced. That second number matters more to a procurement buyer than the first: a missing price costs a phone call, a made-up one costs a contract.

**ClientLens** fact-checks sales decks against a knowledge base. I hid 13 false claims across 12 decks: a certification the company does not hold, inflated figures, a made-up customer, the wrong price. The checker caught 13 of 13. It also flagged nothing in the 6 clean decks. But the number I care about is the next one: **0 of 13 could reach a doc**. A draft can only be written to a doc if every claim checks out, or if the false claims were cleanly cut out. Code enforces that, not the model. One false claim sat in a slide title, and the code never edits titles, so that deck was held back instead. In all, 9 of 12 decks with false claims were held for a person to review rather than cleaned. That is the right way to fail. But 8 of those were one-sentence test slides with nothing left once the claim was cut, so real drafts would be held less often.

**kokum** suggests what to cook from what is in your kitchen. The model ranks the dishes and writes one line about each. I tested it on different sets of kitchen contents. In 10 of 10, it never ranked a dish you cannot cook above one you can. But the test also showed that nothing in the code would have stopped it. Putting "cook now" dishes first was a habit of the model, not a guarantee. The code now enforces it.

That is the pattern I keep seeing: **a test that passes can still reveal a rule you never actually wrote down.**

## Where the model needed a tighter fence

**EOD** turns a day of commits, meetings and docs into an update for a manager. It kept 30 of 30 facts and made up nothing. But when I read the updates myself, 2 of 10 days claimed more than happened ("shipped the fix" when the input showed the code was committed, not merged). 3 more dressed things up ("sent the draft" when the input only said to send it). No automatic check caught this, because every word sounded believable. I added two rules to the prompt. After that: 0 of 10. This kind of error quietly wears down a manager's trust, and accuracy scores cannot see it.

**pm-os** drafts PM documents. Its test counted the thing I worried about most: figures stated as fact that came from nowhere. There were 28 before the fix and 2 after. Some drafts were too long for the app's length limit, so they would have been cut off mid-document. That went from 7 of 10 to 0. I measured this by running the prompts through a command-line tool, not through the app itself. Two made-up figures remain. I report them rather than tweak the prompt until they disappear.

## Where fixing one thing broke another

Every fix had a cost, and the test is the only reason I saw it.

- **followthrough** pulls client requests and promises out of call transcripts. The first prompt found everything it should have (100%) but padded the list. Of the items it flagged, only 71% were right, including 4 items from a friendly check-in that had nothing to act on. With a tighter prompt, 100% of the items it flagged were right, and it returned nothing from the check-in. But it now found fewer of the real items: 88 to 92%, across two test runs. It skips or merges some smaller promises. A person reviews this tool's output anyway, so I chose being right over finding everything. That is a product decision, not something the model decides.
- **kill-the-quote**'s fixes probably made the model stricter about units. As a side effect, its confidence score on one supplier's correctly read prices fell from 0.87 to 0.40. The prices are still exact, but a person now checks them for no reason.
- **pm-os**'s length limit stopped drafts being cut off. Then the SWOT module dropped its next-steps section to fit. A second round of fixes solved that.

## Where the test caught me, not the model

The most useful finding was about my own claims. My kill-the-quote README said prices the model was unsure of were "routed to a person". A review of the test code showed that was not true. A low confidence score only marks a price on screen. It does not keep that price out of the totals. What actually protects the totals is a separate check: a second model re-reads large prices from photos. That check was switched off during the test. On the photo taken in low light, the model read only 7 of 21 prices exactly, and it marked every wrong one as low confidence. So the model knew when it was unsure, which is good. But knowing is not the same as stopping a wrong price. I corrected the README. The same wording in the prompts is now an open issue.

Writing down what the code guarantees, then testing it, is how you find out which guarantees were really just hopes.

## What these numbers are not

I want these read correctly:

- **Small, fictional test sets.** Four calls, ten days, eighteen decks. I ran each round of tests once, so I cannot say how much the results vary from run to run. One item going the other way moves some of these numbers by four or five points.
- **Fixed and scored on the same cases.** I wrote most fixes by looking at the misses in the same cases I then scored them on. So the after-fix numbers are a best case, until new test cases the tools were never tuned on confirm them.
- **Not the way the live apps run.** I ran the tests through the Claude Code command-line tool, using each app's own prompts (and its output format rules, where the app has them). EOD is the exception: that command-line tool is how EOD actually runs.
- **Some scores came from me or from another AI.** I checked the EOD and kokum sentences by hand, myself. For pm-os, another AI grading the answers counted the made-up figures. For followthrough, another AI matched what the tool found against the correct answers.

Every miss, method and raw result is in each repo's `EVALS.md`.

## What I would do next

1. For each product, build a set of new test cases it was never tuned on, before I touch another prompt.
2. Rerun the tests the way the live apps run, and repeat each one enough times to see how much the results vary.
3. Turn the checks I did by hand (overstated outcomes, wrong ingredients) into automatic ones that run on every change.
4. Fix in code the gaps the tests exposed, starting with making low confidence actually keep a price out of the kill-the-quote totals.

The rule still holds: let the model read, let the code decide. What the tests added is a third step I had been skipping. Write down what the code is supposed to guarantee, then go and check.
