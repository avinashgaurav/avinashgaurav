# The model reads, the code decides

*I tested seven AI products I built, then tested them again on cases they had never seen. November 2026.*

> **In short**
>
> - My view: in a product, AI should do the reading, and plain, tested code should make every decision that matters.
> - I tested seven of my own products against answers I wrote down first. The AI made up **0 prices**, wrote **0 wrong numbers** in 54 coach replies and let **0 of 13** false sales claims reach a document.
> - The misses that mattered most were not wrong facts: a confident verb, a safety word list in my own code, a README that promised more than the code did.
> - On new cases, there were still no made-up prices, but some results dropped: FollowThrough found 76% of real items (88 to 92% before), and Kill the Quote read 93% of prices exactly (107 of 107 before, on a set that is not like for like). The biggest gap was Form & Flow's old safety word list: it caught 1 of 14 new safety messages. An AI safety screen closed that gap in testing (on a small fresh set), but it is not live yet. So test on new cases before you believe your numbers.

Every product I build with AI follows one rule. The AI does the reading: messy supplier quotes, call transcripts, a kitchen's contents, a day of scattered work, a message typed in Hinglish. Plain, tested code does the deciding: totals, rankings, plans, what gets shown and what gets sent.

I believed the rule worked. So I tested it.

## How I tested

All the data was made up, so every test could be public. Before the AI saw a single case, I wrote down the correct answers. (For FollowThrough, I corrected those answers once after a review; its EVALS.md says how.) Then I ran it, fixed what broke and kept a record of everything, including the parts that did not flatter me.

## Where the rule held

**Kill the Quote** reads supplier quotes in five formats (Excel, PDF, Word, a phone photo, an email) and puts them into one comparison. After two fixes, it read **107 of 107** priced lines with the exact price and unit, and made up **zero prices** for lines a supplier never priced. The second number matters more to a buyer: a missing price costs a phone call, a made-up one costs a contract.

**ClientLens** fact-checks sales decks. I hid 13 false claims across 12 decks. It caught **13 of 13**, flagged nothing in 6 clean decks, and **0 of 13 could reach a document**: code only lets a draft through if every claim checks out or the false ones were cleanly cut. 9 of the 12 were held for a person instead, the right way to fail (8 were one-sentence test slides left empty by the cut, so real drafts would be held less often). A second test found its knowledge-base search, not the AI, was the weak point. After I fixed 9 bugs, keyword search answered 57 of 69 right (up from 36) and Gemini search 66 of 69, both on the questions I fixed against.

**Kokum** suggests what to cook from what is in your kitchen. In **10 of 10** tests, it never ranked a dish you cannot cook above one you can. But nothing in the code made it do that; it was a habit of the AI, not a guarantee. The code now enforces it.

That is the pattern I keep seeing: **a test that passes can still reveal a rule you never actually wrote down.**

## Where the AI needed a tighter fence

**EOD** turns a day of commits, meetings and documents into an update for a manager. It kept **30 of 30** details and made up nothing. But when I read the updates myself, only 5 of 10 days were right. The AI had upgraded the verbs: "shipped the fix" when the code was only committed, "sent the draft" when there was only a note to send it. No automatic check caught it, because every fact was true. Two new rules fixed it on the same 10 days. The full story: [My AI Passed Every Test. Then I Read What It Wrote.](https://avigaurav.substack.com/p/my-ai-passed-every-test-then-i-read)

**PM-OS** drafts PM documents. Its test counted figures stated as fact that came from nowhere: **28 before the fix and 2 after**. Drafts too long for the app's limit went from 7 of 10 to 0. I report the two that remain rather than tweak the prompt until they disappear.

## Where the code was the weak part

**Form & Flow** is a fitness coach for Indian users. Code works out the plan, calories and protein; the AI only words the coach's replies. On 27 cases, 7 in Hinglish, run 3 times, it wrote **0 wrong numbers** in 54 AI replies.

The biggest problems were in my code. Two of the 7 crisis messages slipped past my safety word list: it knew "hurt myself" but not "hurting myself", and "after eating" but not "after dinner". My own checker wrongly rejected **23 of 47** good replies, almost all because of one rule about "reps in reserve". After the fixes, **all 9 safety cases were handled correctly** and wrong rejections fell to **4 of 47**. But those are the cases I fixed against.

**Kill the Quote** caught me too. My README said prices the AI was unsure of were "routed to a person". They were not: "unsure" only marks a price on screen. On a low-light photo, the AI read only 7 of 21 prices exactly and marked every wrong one as unsure. The AI knew, but nothing stopped those prices. (In the app, a second read of photo prices keeps disputed ones out of the totals; it was off in this test.) I corrected the README.

Writing down what the code guarantees, then testing it, is how you find out which guarantees were really just hopes.

## Where fixing one thing broke another

- **FollowThrough** pulls client promises out of call transcripts. The first prompt padded the list: only 71% of what it flagged was right. A tighter prompt made **100%** right, but found fewer real items. A person reviews the output anyway, so I accept that trade for now.
- **Kill the Quote**'s fixes probably made the AI stricter about units, and its confidence on one supplier's correct prices fell from 0.87 to 0.40. A person now checks them for no reason.
- **PM-OS**: a later prompt change left the user stories without their INVEST check (a standard checklist for user stories) in all 4 runs after it, 3 of them on new cases. Asking for that section by name fixed it in 6 of 6 reruns.

## What happened on new cases

Fixes written while looking at the misses always look good on those same cases. So for all seven products I wrote new cases with new people, products and wording, saved the answers before the AI saw them, changed no app code or prompts, and ran each set 3 times (ClientLens's pitch test once). Two scoring steps did change after a first look, and I say so: I rewrote the instructions for Kokum's AI reviewer once after its first pass (and checked them on sentences with known answers), and I adjusted EOD's rules after the first scoring.

| Product | What I measured | Cases I fixed against | New cases |
|---|---|---|---|
| Kill the Quote | Prices read exactly | 107 of 107 | **93%** (77 prices) |
| Kill the Quote | Made-up prices | 0 | **0** |
| FollowThrough | Flagged items that were right | 100% | **100%** |
| FollowThrough | Real items it found | 88 to 92% | **76%** |
| ClientLens | Knowledge-base answers right, Gemini search | 66 of 69 | **67 of 72** |
| ClientLens | Same, keyword search | 57 of 69 | **60 of 72** |
| ClientLens | Writable pitch decks matching a false-claim pattern | 0 of 13 | **0 of 5** (1 run) |
| Kokum | Grocery lists read fully correctly | 20 of 20 | **20 of 20** (49 of 49 lines) |
| Kokum | Look-alike or unknown items wrongly matched | 0 of 5 | **0 of 11** |
| Kokum | Top dish is one you can cook now | 10 of 10 | **9 of 9** |
| EOD | Days that claimed more than happened | 0 of 10 (my read) | **0 of 10** (fixed rules) |
| EOD | Facts kept / work items kept | 30 of 30 / 29 of 29 | **38 of 38 / 32 of 32** |
| EOD | Weekly summaries that claimed more | not tested | **0 of 2** |
| PM-OS | Required sections present | 87 of 88 | **87 of 88** |
| PM-OS | Made-up figures stated as fact | 2 or 3 in 10 documents | **5 in 30 documents** (first #97 prompt, before the INVEST rewording) |
| Form & Flow | Wrong numbers in coach replies | 0 of 54 | **0 of 60** |
| Form & Flow | Real safety messages the old word list caught (before broadening) | 7 of 7 | **1 of 14** |

New-case results were the same in every run except Kill the Quote (70 to 72 of 77), FollowThrough (71 to 79%), ClientLens with Gemini (22, 22 and 23 right per run), PM-OS (0, 3 and 2 figures) and Form & Flow's coach replies (3, 3 and 5 of 20 rejected; 4 of 20 cases changed between runs). Every Kill the Quote miss was on one very hard low-light photo; without it, it read 62 of 62. FollowThrough's misses were mostly two promises by one person merged into one. ClientLens's 67 of 72 is the same as 66 of 69 at this size, not a drop.

One new miss, and one change to disclose. Kokum's dish sentences were graded by another AI, which I first checked on 20 sentences with known answers (10 of 10 bad ones caught, 0 of 10 good ones flagged). When an item was in the kitchen but not enough of it, the AI sometimes argued with the app's missing list ("your pantry shows you have some"): 1, 4 and 0 of 80 sentences in the 3 runs. On EOD, my hand check for upgraded verbs is now a set of automatic rules; I adjusted them once after the first scoring, because they flagged fine lines such as "to be sent once legal checks".

Form & Flow dropped furthest. On new wordings, such as "ending it all" and Hinglish phrasings, the old word list caught 0 of 8 crisis messages. A word list only knows the words it was given. So I broadened it and added an AI safety screen: a small model that may only name a category, while code sends anything other than a clear "none", including an error, to the fixed safe reply. On a fresh hidden set written before the fix (24 safety messages, 16 harmless lookalikes), all 24 got the right fixed reply and 0 reached the coach, with Claude Haiku as the screen. But 17 of the 24 share wording with what the fix was tuned on. On the 7 fresh messages with wording the fix had not seen, the broader word list caught 1, and the screen caught the other 6.

That screen is not live. Today the app runs on the phone only, with no backend, so there is no AI coach and no AI screen. The broader word list is live, and users rely on it alone.

## What these numbers are not

- **Small, made-up test sets.** Four calls, ten days, eighteen decks. One item going the other way moves some numbers by 3 to 5 points.
- **New cases, but not real users.** I or Claude wrote every new case, so they share our habits.
- **Not the way the live apps run.** I ran the tests through the Claude Code command-line tool with each app's own prompts, or called the AI directly. EOD is the exception: that tool is how EOD really runs.
- **Mostly one company's AI.** Claude answered almost every test. Gemini was tested only for ClientLens's search.
- **Some scores came from another AI.** I checked EOD, Kokum and Form & Flow's first runs by hand. Another AI graded PM-OS's figures and FollowThrough's matches, and on the new cases also Kokum's sentences, EOD's updates and some Form & Flow replies. I have not checked those labels by hand yet.

Every miss, method and raw result is in each repo's `EVALS.md`.

## What I would do next

1. Give Form & Flow a backend, so the AI safety screen can go live, and test it there, including how fast it answers.
2. Rerun every test the way the live apps run.
3. Check by hand the labels another AI graded.
4. Fix in code the gaps the tests found, starting with making "unsure" keep a price out of Kill the Quote's totals, and FollowThrough merging two promises into one.

The rule still holds: let the model read, let the code decide. What testing added is a third step I had been skipping. Write down what the code is supposed to guarantee, then go and check it, on cases it has never seen.
