# The model reads, the code decides

*I tested seven AI products I built. The misses were rarely where I expected. October 2026.*

> **In short**
>
> - My view: in a product, AI should do the reading, and plain, tested code should make every decision that matters.
> - I tested seven of my own products against answers I wrote down first. The AI made up **0 prices**, wrote **0 wrong numbers** in 54 coach replies and let **0 of 13** false sales claims reach a document.
> - The misses that mattered most were not wrong facts: a confident verb, a safety word list in my own code, a README that promised more than the code did.
> - On new cases the fixes had never seen, there were still no made-up prices and no wrong FollowThrough items, but the headline numbers were lower than on the cases I had tuned them on. **Test on new cases before you believe your numbers.**

Every product I build with AI follows one rule. The AI does the reading: messy supplier quotes, call transcripts, a kitchen's contents, a day of scattered work, a message typed in Hinglish. Plain, tested code does the deciding: totals, rankings, plans, what gets shown and what gets sent.

I believed the rule worked. So I tested it.

## How I tested

All the data was made up, so every test could be public. Before the AI saw a single case, I wrote down the correct answers. (For FollowThrough, I corrected those answers once after a review; its EVALS.md says how.) Then I ran it, fixed what broke and kept a record of everything, including the parts that did not flatter me.

## Where the rule held

**Kill the Quote** reads supplier quotes in five formats (Excel, PDF, Word, a phone photo, an email) and puts them into one comparison. After two fixes, it read **107 of 107** priced lines with the exact price and unit. In every run, it made up **zero prices** for lines a supplier never priced. That second number matters more to a buyer: a missing price costs a phone call, a made-up one costs a contract.

**ClientLens** fact-checks sales decks. I hid 13 false claims across 12 decks, such as a made-up customer and the wrong price. It caught **13 of 13** and flagged nothing in 6 clean decks. The number I care about most: **0 of 13 could reach a document**. Code, not the AI, only lets a draft through if every claim checks out or the false ones were cleanly cut. 9 of the 12 decks were held for a person instead, which is the right way to fail. (8 of those were one-sentence test slides with nothing left once the claim was cut, so real drafts would be held less often.)

**Kokum** suggests what to cook from what is in your kitchen. In **10 of 10** tests, it never ranked a dish you cannot cook above one you can. But nothing in the code made it do that; it was a habit of the AI, not a guarantee. The code now enforces it.

That is the pattern I keep seeing: **a test that passes can still reveal a rule you never actually wrote down.**

## Where the AI needed a tighter fence

**EOD** turns a day of commits, meetings and documents into an update for a manager. It kept **30 of 30** details and made up nothing. But when I read the updates myself, only 5 of 10 days were right. The AI had upgraded the verbs: "shipped the fix" when the code was only committed, "sent the draft" when there was only a note to send it. No automatic check caught it, because every fact was true. Two new rules fixed it on the same 10 days. I wrote the full story up here: [My AI Passed Every Test. Then I Read What It Wrote.](https://avigaurav.substack.com/p/my-ai-passed-every-test-then-i-read)

**PM-OS** drafts PM documents. Its test counted what I worried about most: figures stated as fact that came from nowhere. There were **28 before the fix and 2 after**. Drafts too long for the app's limit went from 7 of 10 to 0. I report the two that remain rather than tweak the prompt until they disappear.

## Where the code was the weak part

**Form & Flow** is a fitness coach for Indian users. Code works out the plan, calories and protein; the AI only words the coach's replies. I tested it on 27 cases, 7 in Hinglish (Hindi and English mixed), and ran the test 3 times. Across 54 AI replies, it wrote **0 wrong numbers**. Two smaller slips did reach a user before the fixes: a user's own figure repeated back, and a made-up meal count.

The biggest problems were in my code. Two of the 7 crisis messages slipped past my safety word list and would have reached the AI. The list knew "hurt myself" but not "hurting myself", and "after eating" but not "after dinner". And my own checker wrongly rejected **23 of 47** good replies, almost all because of one rule about "reps in reserve". After the fixes, **all 9 safety cases were handled correctly** (7 of 7 crisis messages got the fixed safe reply, and 2 harmless lookalikes did not) and wrong rejections fell to **4 of 47**. These are the same cases I fixed against, so they are a best case.

**Kill the Quote** caught me too. My README said prices the AI was unsure of were "routed to a person". They were not: "unsure" only marks a price on screen and does not keep it out of the totals. On a photo taken in low light, the AI read only 7 of 21 prices exactly and marked every wrong one as unsure. The AI knew, but "unsure" alone does not stop anything. In the app, a separate second read of photo prices keeps disputed ones out of the totals; it was switched off in this test. I corrected the README. Making "unsure" actually block a price is still open.

Writing down what the code guarantees, then testing it, is how you find out which guarantees were really just hopes.

## Where fixing one thing broke another

- **FollowThrough** pulls client requests and promises out of call transcripts. The first prompt found everything but padded the list: only 71% of what it flagged was right, including 4 items from a friendly check-in with nothing to act on. A tighter prompt made **100%** of flagged items right, but it found fewer real items. A person reviews this output anyway, so for now I accept finding fewer in exchange for being right. The merging behind those misses is the next thing to fix.
- **Kill the Quote**'s fixes probably made the AI stricter about units. As a side effect, its confidence on one supplier's correct prices fell from 0.87 to 0.40. The prices are still exact, but a person now checks them for no reason.

## What happened on new cases

Fixes written while looking at the misses always look good on those same cases. So for Kill the Quote and FollowThrough, I wrote new test cases, saved the answers before the AI saw them, changed no app code or scoring, and ran each test 3 times.

| | On the cases I fixed against | On new cases |
|---|---|---|
| **Kill the Quote:** prices read exactly | 107 of 107 | **93%** (6 new quotes from 5 new suppliers, 77 prices) |
| **Kill the Quote:** made-up prices | 0 | **0** |
| **FollowThrough:** flagged items that were right | 100% | **100%** |
| **FollowThrough:** real items it found | 88 to 92% | **76%** (4 new calls) |

The two Kill the Quote sets are not a like-for-like comparison: the 107 counts documents only, while the new set includes a very hard photo. Every new-case miss was on that photo; without it, it read 62 of 62 in every run. FollowThrough never flagged a wrong item and ignored a line planted in a call to trick it. But it found fewer real promises, mostly because it merged two promises into one.

No made-up prices and no wrong FollowThrough items, but the headline numbers dropped. That gap is why new cases matter.

## What these numbers are not

- **Small, made-up test sets.** Four calls, ten days, eighteen decks. One item going the other way moves some numbers by 3 to 5 points.
- **Mostly scored on the same cases I fixed against.** Only Kill the Quote and FollowThrough have new-case results. For the other five, the after-fix numbers are a best case.
- **Not the way the live apps run.** I ran the tests through the Claude Code command-line tool, using each app's own prompts. EOD is the exception: that tool is how EOD really runs.
- **One company's AI.** Every test used Anthropic's Claude models.
- **Some scores came from me or from another AI.** I checked EOD, Kokum and Form & Flow by hand. Another AI graded parts of PM-OS and FollowThrough.

Every miss, method and raw result is in each repo's `EVALS.md`.

## What I would do next

1. New test cases for the other five products, before I touch another prompt.
2. Rerun every test the way the live apps run, several times each.
3. Turn the checks I did by hand (upgraded verbs, coach replies, dish sentences) into automatic ones that run on every change.
4. Fix in code the gaps the tests found, starting with making "unsure" keep a price out of Kill the Quote's totals.

The rule still holds: let the model read, let the code decide. What testing added is a third step I had been skipping. Write down what the code is supposed to guarantee, then go and check it, on cases it has never seen.
