### Human-in-the-loop, for now.

[Portfolio](https://avinash-portfolio-six-puce.vercel.app/hi/github) · [LinkedIn](https://www.linkedin.com/in/avinashgaurav) · [Substack](https://avigaurav.substack.com)

**How I build with AI:** models extract, summarise and suggest. Plain, tested code does the counting, the state changes and the final call. Anything the model cannot back with a source gets flagged or dropped, not shipped.

I tested all seven of these projects and wrote up the misses as well as the wins: [The model reads, the code decides](ESSAY.md).

#### Projects

| Project | Problem | How the AI is kept in check |
|---|---|---|
| [Bidline](https://github.com/avinashgaurav/bidline) · [live](https://bidline-app.vercel.app) | Buyers retype supplier quotes from five formats, and a per-piece price can make the wrong supplier win | No AI in the app: the buyer's own Claude reads quotes through 58 MCP tools and must cite the supplier's words. Code does every conversion and logs each edit. 150 of 150 demo prices match to the rupee. |
| [form-and-flow](https://github.com/avinashgaurav/form-and-flow) · [live](https://form-and-flow.vercel.app) | Fitness apps give everyone the same plan and guess the numbers for Indian meals | Tested code works out the plan, calories and protein; the AI only words the coach's replies (0 wrong numbers in 60 on new test cases). The live app runs on the phone only, so its safety check is a word list in code: on new safety wordings it had not seen, it caught 1 of 7. An AI safety screen caught 24 of 24 in a fresh hidden test, but stays off until the app has a backend. India helplines only. |
| [followthrough](https://github.com/avinashgaurav/followthrough) · [live](https://followthrough-eight.vercel.app) | Promises made to clients on calls get lost between the call, the ship and telling the client. **In pilot with a sales team (October 2026).** | Every point it pulls from a call must quote the transcript word for word, or it is thrown out. A person sorts and checks the items before anything goes out. |
| [kokum](https://github.com/avinashgaurav/kokum) · [live](https://kokum-six.vercel.app) | Cooking apps suggest dishes you cannot make with what is in your kitchen | Dish availability is computed in code; the model only ranks and explains, and made-up recipe ids are dropped. |
| [ClientLens](https://github.com/avinashgaurav/clientlens) · [live](https://clientlens-ai.vercel.app) | Sales reps pitch from memory and misquote the product | A checker flags any claim it cannot trace to your knowledge base. Flagged claims are removed or the draft is held, and held drafts cannot be written to a doc. A test of its knowledge-base search, and a code review, found 9 bugs. After the fixes, on new questions it was never tuned on, Gemini search got 67 of 72 answers right and keyword search 60 of 72 (66 and 57 of 69 on the questions the fixes were written against). |
| [EOD](https://github.com/avinashgaurav/EOD) · [live](https://eodreceipt.vercel.app) | Writing the end-of-day update from memory. **In daily use by 5 people across a product and marketing team since mid-2026.** | Local-first. Every claim in its README is tied to a test. If the two stop matching, the build fails. |
| [pm-os](https://github.com/avinashgaurav/pm-os) · [live](https://pm-os-nine.vercel.app) | PMs rebuild the same PRD, roadmap and OKR documents from scratch | 82 modules. The prompts tell the model to mark what it does not know instead of making it up. Scored with a checklist on 10 modules, made-up figures fell from 28 to 2. |

#### Background

- **ZopDev Technologies**, Technical PM (Jul 2025 to now): multi-cloud FinOps platform
- **Jubilant Motorworks**, Associate PM (May 2024 to May 2025): built internal lead and sales tools from zero to one
- PGDM E-Business, Welingkar; B.E. Computer Science, VTU
