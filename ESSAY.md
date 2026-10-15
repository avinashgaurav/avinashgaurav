# The model reads, the code decides

*What six evals taught me about where AI belongs in a product. October 2026.*

Every product I have built with AI follows one rule. The model does the reading: messy quotes, call transcripts, grocery photos, a day of scattered work. Plain, tested code does the deciding: totals, rankings, what gets written, what gets shown first. I believed the rule worked. Then I measured it.

I ran a recorded eval on each of six projects, on fictional data, with the answers written down before the first model call. Here is what came back, including the parts that did not flatter me.

## Where the rule held

**kill-the-quote** reads supplier quotes in five formats (Excel, PDF, Word, a phone photo, an email) into one comparison. After two fixes, 107 of 107 priced lines came back exact on price and unit. Across every run, the model invented zero prices for lines a supplier never priced. That second number matters more to a procurement buyer than the first: a missing price costs a phone call, an invented one costs a contract.

**ClientLens** fact-checks sales decks against a knowledge base. I planted 13 false claims across 12 decks: a certification the company does not hold, inflated figures, an invented customer, the wrong price. The validator caught 13 of 13 and flagged none of the 6 clean decks. But the number I care about is the next one: **0 of 13 could reach a doc**. Even where the model's redaction was imperfect, the write gate (code) refused anything not verified. In 9 of 12 cases it held the draft for a person rather than cleaning it, which is the right failure.

**kokum** suggests what to cook from what is in your kitchen. The model ranks dishes and writes one line each. The eval found the model never put a dish you cannot cook above one you can, in 10 of 10 pantry states. It also found that nothing in the code would have stopped it. "Cook now" first was a model habit, not a guarantee. It is now enforced in code.

That is the pattern I keep seeing: **an eval that passes can still reveal a rule you never actually wrote down.**

## Where the model needed a tighter fence

**EOD** turns a day of commits, meetings and docs into a manager update. It kept 30 of 30 facts and invented nothing. Yet on manual review, 5 of 10 days overstated something: "shipped the fix" where the input showed a commit, not a merge; "sent the draft" where the input said to send it. No automated check caught this, because every word was plausible. After two prompt rules, 0 of 10. This is the kind of error that erodes a manager's trust quietly, and it is invisible to accuracy metrics.

**pm-os** drafts PM documents. Its eval measured the thing I was most worried about: figures stated as fact that came from nowhere. 28 such statements before the fix, 2 after. Outputs too long for the app's token limit (which would be cut off mid-document) went from 7 of 10 to 0. Two invented figures remain, and I report them rather than tune them away.

## Where fixing one thing broke another

Every fix had a cost, and the eval is the only reason I saw it.

- **followthrough** extracts client asks and commitments from call transcripts. The first prompt found everything (recall 100%) but padded the list: precision 71%, including 4 items from a friendly check-in that had nothing to act on. The tightened prompt reached 100% precision and returned nothing from the check-in. Recall fell to 92%: it now skips or merges some smaller commitments. For a tool whose output a person triages, I chose precision. That is a product call, not a model property.
- **kill-the-quote**'s fixes made the model stricter about units, so confidence on one supplier's correctly read prices fell from 0.87 to 0.40. The prices are still exact; a person now reviews them unnecessarily.
- **pm-os**'s length budget fixed truncation, then the SWOT module dropped its next-steps section to fit. A second round fixed that.

## Where the eval caught me, not the model

The most useful finding was about my own claims. My kill-the-quote README said low-confidence prices were "routed to a person". A code review of the eval showed confidence only flags a value in the UI; it does not keep it out of the totals. What actually protects the totals is a separate cross-check that re-reads large photo prices with a second model. The eval had that cross-check switched off. The photo taken in low light read only 7 of 21 prices exactly, and every wrong one came back at low confidence. That is good calibration, but it is not a fence. I corrected the README, and the same wording in the prompts is now an open issue.

Writing down what the code guarantees, then testing it, is how you find out which guarantees were really just hopes.

## What these numbers are not

I want these read correctly:

- **Small, fictional sets.** Four calls, ten days, eighteen decks. One run per round, with no variance estimate. A one-item swing moves some of these by four or five points.
- **Tuned on the test.** Most fixes were written from the misses on the same set they are scored on. The after-numbers are an upper bound until a held-out set confirms them.
- **Not the production API path.** The runs went through the Claude Code CLI using each app's own prompts and schemas. EOD is the exception: there, the CLI is the production path.
- **Some judgments are manual.** The EOD and kokum sentence reviews were done by hand, by me.

Every miss, method and raw result is in each repo's `EVALS.md`.

## What I would do next

1. Build a held-out set for each product before touching another prompt.
2. Rerun on the production API path and repeat each run enough times to see the spread.
3. Turn the manual checks (overstated outcomes, wrong ingredients) into automated ones, so they run on every change.
4. Close the gaps the evals exposed in code, starting with letting low confidence actually gate the kill-the-quote totals.

The rule still holds: let the model read, let the code decide. What the evals added is a third step I had been skipping. Write down what the code is supposed to guarantee, then go and check.
