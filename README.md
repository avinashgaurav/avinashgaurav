### Human-in-the-loop, for now.

[Portfolio](https://avinash-portfolio-six-puce.vercel.app/hi/github) · [LinkedIn](https://www.linkedin.com/in/avinashgaurav) · [Substack](https://avigaurav.substack.com)

**How I build with AI:** models extract, summarise and suggest. Plain, tested code does the counting, the state changes and the final call. Anything the model cannot back with a source gets flagged or dropped, not shipped.

I tested six of these projects in one essay and wrote up the misses as well as the wins (Form & Flow's tests are in its own repo): [The model reads, the code decides](ESSAY.md).

#### Projects

| Project | Problem | How the AI is kept in check |
|---|---|---|
| [kill-the-quote-spreadsheet](https://github.com/avinashgaurav/kill-the-quote-spreadsheet) | Procurement buyers retype five supplier quotes (Excel, PDF, photos, email) into one comparison sheet | The model reads values; code computes landed cost and cross-checks it. Disputed photo prices are excluded, not averaged. |
| [form-and-flow](https://github.com/avinashgaurav/form-and-flow) | Fitness apps give everyone the same plan and guess the numbers for Indian meals | Tested code works out the plan, calories, protein and safety checks. The AI only writes the coach's replies in friendly words. Crisis messages get a fixed, safe reply instead of an AI one (9 of 9 in testing, India helplines only). |
| [followthrough](https://github.com/avinashgaurav/followthrough) | Promises made to clients on calls get lost between the call, the ship and telling the client. **In pilot with a sales team (October 2026).** | Every point it pulls from a call must quote the transcript word for word, or it is thrown out. A person sorts and checks the items before anything goes out. |
| [kokum](https://github.com/avinashgaurav/kokum) | Cooking apps suggest dishes you cannot make with what is in your kitchen | Dish availability is computed in code; the model only ranks and explains, and made-up recipe ids are dropped. |
| [ClientLens](https://github.com/avinashgaurav/clientlens) | Sales reps pitch from memory and misquote the product | A checker flags any claim it cannot trace to your knowledge base. Flagged claims are removed or the draft is held, and held drafts cannot be written to a doc. |
| [EOD](https://github.com/avinashgaurav/EOD) | Writing the end-of-day update from memory. **In daily use by 5 people across a product and marketing team since mid-2026.** | Local-first. Every claim in its README is tied to a test. If the two stop matching, the build fails. |
| [pm-os](https://github.com/avinashgaurav/pm-os) | PMs rebuild the same PRD, roadmap and OKR documents from scratch | 82 modules. The prompts tell the model to mark what it does not know instead of making it up. Scored with a checklist on 10 modules, made-up figures fell from 28 to 2. |

#### Background

- **ZopDev Technologies**, Technical PM (Jul 2025 to now): multi-cloud FinOps platform
- **Jubilant Motorworks**, Associate PM (May 2024 to May 2025): built internal lead and sales tools from zero to one
- PGDM E-Business, Welingkar; B.E. Computer Science, VTU
