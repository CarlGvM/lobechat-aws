AI tool used: Claude (claude.ai, Sonnet 4.6)
Date: 2026-06-02

## Full AI transcript

---

**Me:** I have to do Q4, a build vs. buy memo to the CTO. The case scenario involves a European business/academic institution with 200 users. What kind of structure would be best?

**Claude:** Use a real executive memo format, not academic prose — the rubric explicitly asks for this. Structure: header (To/From/Date/Subject), TL;DR as three bullets at the very top, then body sections covering 3-year TCO comparison, lock-in analysis across four dimensions, data sovereignty and compliance, capability gap table, a conditional recommendation, and a reversal trigger. Close with a cost sensitivity appendix. Keep it punchy — this is an exec memo, not an essay.

**Me:** What numbers can I use for the TCO calculations? In the rubric, there are mentions of ChatGPT Enterprise being $60/user/monthly and Copilot being $30/user/monthly.

**Claude:** Those are the rubric's own reference prices so use them as your baseline. For self-hosted, take your Q3 infrastructure cost (roughly $1,400/month at 200 MAU) and add a dedicated engineer — budget around $80,000/year fully loaded for a junior-to-mid DevOps engineer in Europe. That gives approximately $97,000/year self-hosted vs $144,000/year for ChatGPT Enterprise vs $72,000/year for Copilot. The key insight worth highlighting: if you already have a DevOps engineer on staff, your marginal cost is just the infrastructure — about $16,800/year — which makes self-hosting roughly 4x cheaper than either SaaS option. That's worth an appendix row.

**Me:** What are the most persuasive lock-in considerations?

**Claude:** Cover four dimensions: (1) Data lock-in — conversation history and knowledge base embeddings live on the vendor's platform; export tools exist but are limited and labour-intensive; (2) Identity lock-in — Copilot is deeply tied to Azure AD/Entra ID, ChatGPT requires OpenAI account management; our Casdoor SSO works with any OIDC provider; (3) Model lock-in — both SaaS options tie you to OpenAI models; with OpenRouter you can switch between 200+ models from 20+ providers with a single config change; (4) Tooling lock-in — vendor plugin ecosystems require approval to expose internal APIs; MCP lets you connect any internal HTTP API as a callable tool with zero vendor permission required.

**Me:** For compliance, what industry should I use?

**Claude:** Education is the most natural fit given the university context and it stays consistent with Q1. Key points to make: GDPR Article 28 requires a Data Processing Agreement for any third-party processor; both OpenAI and Microsoft offer DPAs but process data in the US by default, with EU residency only on specific higher-cost tiers; self-hosted in eu-west-1 is a clean GDPR posture with no grey areas. Also flag the case study publisher licence issue specifically — pasting licensed HBP or INSEAD content into any external LLM may breach the school's licence agreements; our RAG pipeline sends only retrieved chunks as context, not full documents, which is a defensible legal position.

**Me:** How does the reversal trigger sound?

**Claude:** Frame it as "this recommendation flips if any two of the following occur simultaneously" — that's stronger than a single trigger because it shows the recommendation is robust to individual changes but acknowledges it's not unconditional. The three conditions: (1) a vendor offers contractually guaranteed EU-only data residency at current pricing with explicit prohibition on telemetry leaving the EU; (2) your engineering team drops below one capable person with no budget to backfill; (3) a hosted platform offers native open MCP tool integration, eliminating the extensibility advantage of self-hosting. If conditions 1 and 2 both hit together, the Copilot case becomes compelling.

**Me:** Can you write out the entire memo for me based on everything we've discussed above?

**Claude:** [produced the complete a4.md executive memo with full header, TL;DR bullets, TCO comparison table across all three options, four-dimension lock-in analysis, GDPR and education sector compliance section, capability gap table comparing self-hosted vs SaaS features, conditional recommendation with explicit "self-host if / buy if" framing, reversal trigger, and cost sensitivity appendix — approximately 1,350 words]

**Me:** Are the Copilot and ChatGPT prices mentioned current?

**Claude:** The rubric itself gives ~$60/user/mo for ChatGPT Enterprise and ~$30/user/mo for Copilot as reference figures, so they are the expected baseline for this exercise. I've flagged in the text that both should be verified against current vendor pricing pages before final submission, since enterprise pricing can change. I'd recommend checking openai.com/enterprise and microsoft.com/copilot to confirm before you submit.

## What I kept / changed

- Kept: full memo structure, all TCO tables, lock-in analysis across four dimensions, GDPR arguments, capability gap table, conditional recommendation, and reversal trigger.
- Kept: the multi-condition reversal trigger framing — I agree the logic is sound.
- Modified: sharpened the recommendation section with my own judgment on which conditions matter most for this institution type.
- Flagged: vendor pricing figures need verification against current pricing pages before submission.
- The university/business school context was my own choice, kept consistent with Q1.
