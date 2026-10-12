# Frontier model strengths per task type: OpenAI vs Google vs Anthropic (as of 2026-10-12)

> **Method and reliability note (read first).** These notes were compiled on 2026-10-12 using web search only. Direct page fetches were blocked by the network proxy (artificialanalysis.ai, wikipedia.org and openai.com all failed), so **no primary page could be opened in full**. Every number below comes from search-result summaries of the cited pages. Many cited pages are third-party aggregators (BenchLM, Morphllm, Suprmind, Codersera, Apidog and similar). Their numbers often **conflict** with each other and with the vendors. Treat all figures as provisional, and check the primary page before quoting anything in a final deliverable.
> Labels: **[VENDOR]** = self-reported by the lab. **[INDEP]** = independent evaluator (Artificial Analysis, LMArena, METR, OSWorld/Terminal-Bench maintainers, Scale). **[AGG]** = third-party aggregator or blog that relays figures of uncertain origin.
>
> **Important premise correction:** the model lineup has moved beyond the names in the brief ("GPT-5.x / Gemini 3.x / Opus-Sonnet-Haiku"). Secondary sources report these flagships as of October 2026:
> - **Anthropic:** Claude Fable 5 (2026-06-09), Opus 5 (mid-2026), Opus 5.5 (2026-09-22), Sonnet 5.5 (late September 2026). Some sources also mention "Fable 5.1", which could not be verified. Mythos 5 is restricted.
> - **OpenAI:** GPT-5.5 (2026-04-23); GPT-5.6 Sol/Terra/Luna (date not found); GPT-6 Astra (2026-09-03, staged rollout).
> - **Google:** Gemini 3.1 Pro (still Google's newest "Pro" per several sources); Gemini 3.5 Flash; Gemini 3.7/3.8 Flash (3.8 Flash ~2026-09-02); Gemini 4 Argon (announced 2026-09-30, restricted to the "Fairwind" program).
> - **Not found:** Gemini 3.5 Pro had not shipped as of a 2026-09-20 report.
>
> Several of these names appear mainly on aggregator or SEO sites. Some (for example "Fable 5.1" and "Gemini 4 Argon") could not be confirmed against a vendor page.

## 1. Current lineup and overall independent rankings

### Takeaway
On the independent composite indices, Anthropic (Opus 5.5 / Fable 5) and OpenAI (GPT-5.6 Sol / GPT-6 Astra) are roughly tied at the top. Google trails on the overall indices because its newest broadly available models are Flash-tier, and its new flagship (Gemini 4 Argon) is restricted. Sources disagree on who is #1.

### Cited Findings
- **Opus 5.5 release, cost and SWE-bench Verified status:**
  - [AGG] Anthropic released Claude Opus 5.5 on 2026-09-22, under two months after Opus 5 — [Vellum](https://www.vellum.ai/blog/claude-opus-5-5-benchmarks-explained).
  - [AGG] Vellum reports it cuts execution cost ~40% vs Opus 5 — [Vellum](https://www.vellum.ai/blog/claude-opus-5-5-benchmarks-explained).
  - [AGG] Anthropic reportedly did not report a SWE-bench Verified score for Opus 5.5 in its announcement or system card — [Codersera](https://codersera.com/blog/claude-opus-5-5-complete-guide-2026/amp/).
- **Opus 5.5 pricing conflicts across sources:**
  - [AGG] Codersera gives $4/$20 per M tokens — [Codersera](https://codersera.com/blog/claude-opus-5-5-complete-guide-2026/amp/).
  - [AGG] Morphllm lists Opus 5 at $5/$25 — [Morphllm](https://www.morphllm.com/claude-benchmarks).
- **Claude Fable 5 launch and access:**
  - [AGG] Launched 2026-06-09 as a public "Mythos-class" model. Mythos 5 (same underlying model, fewer safeguards) is restricted to Project Glasswing partners — [NYU Shanghai RITS](https://rits.shanghai.nyu.edu/ai/anthropic-launches-claude-fable-5-its-first-public-mythos-class-model/); [Techmeme 2026-06-09](https://www.techmeme.com/260609/p37).
  - [AGG] Sensitive cyber/bio requests are rerouted to an older model (reportedly Opus 4.8). Anthropic says the safeguards trigger in under 5% of sessions — [NYU Shanghai RITS](https://rits.shanghai.nyu.edu/ai/anthropic-launches-claude-fable-5-its-first-public-mythos-class-model/).
  - [AGG] Pricing reportedly $10/$50 per M tokens — [NYU Shanghai RITS](https://rits.shanghai.nyu.edu/ai/anthropic-launches-claude-fable-5-its-first-public-mythos-class-model/).
- **GPT-5.5 launch and vendor numbers:**
  - [VENDOR] Launched 2026-04-23 — [OpenAI](https://openai.com/index/introducing-gpt-5-5/).
  - [VENDOR] Terminal-Bench 2.0 82.7%; FrontierMath Tier 1–3 51.7% and Tier 4 35.4% — [OpenAI](https://openai.com/index/introducing-gpt-5-5/).
  - [VENDOR] 400K context in Codex, plus a Fast mode — [OpenAI](https://openai.com/index/introducing-gpt-5-5/).
- **GPT-5.6 on the Artificial Analysis index:**
  - [INDEP] GPT-5.6 Sol (max) scores 59, 1 point below Claude Fable 5 (max), at about one third of the cost — [Artificial Analysis "GPT-5.6 has landed"](https://artificialanalysis.ai/articles/gpt-5-6-has-landed).
  - [INDEP] GPT-5.6 Terra (max) scores 55 and Luna (max) 51 — [Artificial Analysis "GPT-5.6 has landed"](https://artificialanalysis.ai/articles/gpt-5-6-has-landed).
  - [INDEP] GPT-5.6 leads AA's Coding Agent Index in the Codex harness — [Artificial Analysis "GPT-5.6 has landed"](https://artificialanalysis.ai/articles/gpt-5-6-has-landed).
- **GPT-6 Astra (secondary coverage):**
  - [AGG] Released 2026-09-03 at $10/$50 per M tokens, with a staged rollout (Daybreak program first, then Plus/Pro/Business/Enterprise and the API) — [DataNorth](https://datanorth.ai/news/openai-launches-gpt-6-astra); [Pillitteri](https://pasqualepillitteri.it/en/news/14246/openai-launches-gpt-6-astra-pricing-benchmarks).
  - [AGG] Reported AA-index scores: Astra ~61, GPT-5.6 Sol 61, Fable 5.1 66. Figures vary slightly between sources — [Pillitteri](https://pasqualepillitteri.it/en/news/14246/openai-launches-gpt-6-astra-pricing-benchmarks).
- **Who leads the Artificial Analysis Intelligence Index (search summary of the AA pages, ~October 2026):**
  - [INDEP] Claude Opus 5.5 (max) is #1 at 58 out of 205 models — [Artificial Analysis leaderboard](https://artificialanalysis.ai/leaderboards/models).
  - [INDEP] A late-September AA headline says Sonnet 5.5 reached #2 — [AA articles](https://artificialanalysis.ai/articles).
  - **Conflict:** BenchLM (updated 2026-10-10) puts GPT-5.6 Sol first at 58.9, ahead of Opus 5.5 (57.6) and Sonnet 5.5 (56.0) — [BenchLM](https://benchlm.ai/benchmarks/artificialanalysis).
  - **Conflict:** an "AA Index v4.2" (2026-09-04) write-up names Fable 5.1, then GPT-6 Astra, as leaders — [ExplainX](https://www.explainx.ai/blog/artificial-analysis-intelligence-index-v4-2-september-2026).
  - Likely cause: different index versions and default effort settings.
- **Google: Gemini 3.5 Pro status:**
  - [AGG] As of 2026-09-20, Gemini 3.5 Pro had not shipped. Only 3.5 Flash shipped from the 3.5 family, which was announced at I/O on 2026-05-19 — [AIToolsReview](https://aitoolsreview.co.uk/insights/gemini-3-5-pro).
  - [AGG] Wikipedia's infobox reportedly lists 3.1 Pro, 3 Deep Think and 3.8 Flash as current — [Wikipedia Google Gemini](https://en.wikipedia.org/wiki/Google_Gemini).
- **Google: Gemini 4 Argon:**
  - [AGG] Announced 2026-09-30 and limited to trusted cyber defenders in the "Fairwind" program, with no general-availability date — [OfficeChai](https://officechai.com/ai/gemini-is-back-to-the-frontier-with-gemini-4-argon/).
  - [AGG] Up to 1M output tokens; introductory price $2/$10 — [OfficeChai](https://officechai.com/ai/gemini-is-back-to-the-frontier-with-gemini-4-argon/).
  - [AGG] Reportedly AA index 53 — [OfficeChai](https://officechai.com/ai/gemini-is-back-to-the-frontier-with-gemini-4-argon/).
  - [AGG] Bloomberg-cited employees doubted its real-world coding; Google disputed this — [ifeng](https://tech.ifeng.com/c/8wrT9WNEyX9).
- [AGG] Gemini 3.8 Flash is generally available (~2026-09-02), with coding and agent improvements plus a Cyber variant — [Apidog](https://apidog.com/vi/blog/gemini-4-argon-vs-gemini-3-8-flash).
- **[INDEP] LMArena Text (Overall) on 2026-08-12 (relayed by a Manifold market):**
  - Anthropic held all three top spots: claude-fable-5 1507±5, claude-opus-4-6-thinking 1505±4, claude-opus-4-7-thinking 1502±4.
  - Meta muse-spark-1.2 was 4th at 1499±10.
  - Source: [Manifold](https://manifold.markets/Terminator2/will-anthropic-hold-all-3-top-spots).
  - No October 2026 snapshot was found.

### Inferences
- For a user with all three subscriptions, the default "top brain" in October 2026 is a near tie between Claude Opus 5.5 / Fable 5 and GPT-5.6 Sol / GPT-6 Astra. Google's consumer-available frontier is Gemini 3.1 Pro plus 3.8 Flash.
- Google competes on price, speed and multimodality rather than on the composite-intelligence crown.
- Arena gaps under ~10 Elo are within noise, so "Claude #1 on LMArena" means "tied at the top".

### Gaps
- Primary vendor announcement pages (Anthropic Opus 5.5 / Sonnet 5.5 / Fable 5; OpenAI GPT-5.6 / GPT-6 Astra; Google Gemini 4 Argon) could not be fetched. The existence of "Fable 5.1" is unconfirmed.
- No information was found on the current Haiku version or on Gemini 3 Deep Think successors.
- No exact GPT-5.6 release date was found.

## 2. Agentic coding (SWE-bench, Terminal-Bench, developer sentiment): Codex vs Claude Code vs Gemini CLI / Antigravity

### Takeaway
Codex (GPT-5.6 Sol / GPT-5.5) and Claude Code (Opus 5 / 5.5) are statistically tied on the independent Artificial Analysis Coding Agent Index (67 each). Codex leads on terminal-heavy and long-horizon SWE tasks per OpenAI-side numbers. Claude Code leads on developer satisfaction. Google's Antigravity is clearly behind (56), and Gemini CLI's free individual tier was cut in June 2026.

### Cited Findings
- **[INDEP] Artificial Analysis Coding Agent Index v1.3** (composite of DeepSWE, Terminal-Bench v2 and SWE-Atlas-QnA):

  | Agent and model | Index | DeepSWE | Terminal-Bench v2 |
  |---|---|---|---|
  | Codex + GPT-5.6 Sol (max) | 67 | 69% | 88% |
  | Claude Code + Opus 5 (xhigh) | 67 | 60% | 85% |
  | Antigravity SDK + Gemini 3.7 Flash (high) | 56 | 56% | 84% |

  Sources: [AA Antigravity vs Codex](https://artificialanalysis.ai/agents/coding-agents/comparisons/antigravity-sdk-vs-codex); [AA Antigravity vs Claude Code](https://artificialanalysis.ai/ko/agents/coding-agents/comparisons/antigravity-sdk-vs-claude-code).
- **Terminal-Bench 2.0:**
  - [AGG mirroring INDEP] BenchLM (October 2026, 45 models): GPT-5.5 82%, GPT-5.3 Codex 77.3%, Claude Opus 4.7 (Adaptive) 69.4% — [BenchLM Terminal-Bench 2.0](https://benchlm.ai/benchmarks/terminal-bench-2).
  - Note: newer Claude models (Opus 5 / 5.5) appear in the AA agent table above but not in this snippet.
  - Some papers have moved to Terminal-Bench 2.1, which is not comparable with 2.0 — [BenchLM](https://benchlm.ai/benchmarks/terminal-bench-2).
- **SWE-bench Pro (all vendor-reported or aggregator figures):**
  - [VENDOR via AGG] Opus 5.5 89.9% (Opus 5: 79.2%); SWE-bench Multilingual 93.9% — [Codersera](https://codersera.com/blog/claude-opus-5-5-complete-guide-2026/amp/); [llm-stats](https://llm-stats.com/blog/research/claude-opus-5-5-launch).
  - [VENDOR via AGG] Fable 5 ~80.0–80.3% — [Morphllm SWE-bench Pro](https://www.morphllm.com/swe-bench-pro); [NYU Shanghai RITS](https://rits.shanghai.nyu.edu/ai/anthropic-launches-claude-fable-5-its-first-public-mythos-class-model/).
  - [AGG] A BenchLM mirror of Scale's V2 leaderboard shows Opus 5 at 99.4% (snapshot 2026-09-23). This is an outlier and should be treated as suspect until checked on Scale's own page — [BenchLM](https://benchlm.ai/benchmarks/scale-swe-bench-pro-public-v2-full).
- **SWE-bench Verified:** it is saturated and losing usefulness.
  - [VENDOR via AGG] Opus 5 96.0% — [SeaWork](https://seawork.ai/en/blogs/claude-opus-5-for-coding/).
  - [VENDOR] Gemini 3.1 Pro 80.6% (Google-reported) — [BenchLM frontier page](https://benchlm.ai/frontier-ai-models).
  - [AGG] GPT-5.5 82.6% vs Opus 4.7 82.0% — [Appwrite](https://appwrite.io/blog/post/gpt-5-5-launch).
- [VENDOR via AGG] GPT-6 Astra on DeepSWE v1.1: 74.1%, vs Opus 5 73.7% and Gemini 3.8 Flash 73.8%. Essentially level — [Pillitteri](https://pasqualepillitteri.it/en/news/14246/openai-launches-gpt-6-astra-pricing-benchmarks).
- [AGG] FrontierSWE v2 public snapshot: GPT-6 Astra 65.5%, Opus 5.5 62.3%, Sonnet 5.5 61.9%, Gemini 4 Argon 55.0% — [BenchLM FrontierSWE v2](https://benchlm.ai/benchmarks/frontierswev2); [Kingy AI](https://kingy.ai/blog/frontier-ai-models-gemini-4-argon-gpt-6-astra-sol-claude/).
- **Developer sentiment:**
  - [AGG, unverified] A JetBrains April 2026 survey reportedly gave Claude Code the highest satisfaction among AI coding tools (46%) — [Fungies](https://fungies.io/best-ai-coding-cli-tools-2026-comparison-2/).
  - [AGG] A Codex-focused blog cites Claude Code at 91% CSAT / NPS 54 and argues the gap comes from workflow and context management, not model quality — [D. Vaughan](https://codex.danielvaughan.com/2026/04/26/ai-coding-agent-adoption-2026-survey-data-codex-cli-positioning/).
  - [AGG] 59% of developers use three or more AI tools in parallel (JetBrains, January 2026) — [D. Vaughan](https://codex.danielvaughan.com/2026/04/26/ai-coding-agent-adoption-2026-survey-data-codex-cli-positioning/).
- [AGG] On 2026-06-18 Google cut free Gemini CLI access for individual developers and pointed users to Antigravity CLI — [Fungies](https://fungies.io/best-ai-coding-cli-tools-2026-comparison-2/).

### Inferences
- **Route terminal-heavy or long autonomous runs** (DevOps scripting, multi-hour background tasks) to **Codex (GPT-5.6 Sol / GPT-5.5)**. It leads Terminal-Bench and DeepSWE.
- **Route interactive pair-programming, repo-wide edits and work where code quality or taste matters** to **Claude Code (Opus 5.5)**. It leads developer satisfaction and Anthropic's SWE-bench Pro numbers.
- **Use Gemini/Antigravity** mainly as a cheap or fast second opinion, or where its 1M+ context helps. On independent agentic numbers it is ~10 points behind.
- The independent index cannot separate Codex and Claude Code. Running both on hard tasks and comparing outputs is a rational strategy with all three subscriptions.

### Gaps
- No independent head-to-head of Jules, or of Antigravity with Gemini 3.1 Pro or Argon, was found.
- The Scale SWE-bench Pro official leaderboard and tbench.ai could not be accessed directly.
- No Aider Polyglot data for the October 2026 models was found.

## 3. Code review / bug finding, large refactors, long-context codebase understanding

### Takeaway
Claude has the best independent evidence for reliable recall at 1M tokens. Gemini has the largest and longest-standing context window (1M–2M) but degrades more at the extreme end. GPT offers 400K in Codex and a premium 1M tier. No independent 2026 code-review or bug-finding leaderboard comparing all three was found.

### Cited Findings
- **[AGG] Long-context retrieval (MRCR v2, 8-needle, early 2026 models):**
  - Claude Opus 4.6: 76% at 1M.
  - GPT-5.4: 36.6% in the 512K–1M range.
  - Gemini 3 Pro: 24.5% at 1M.
  - The same source says Claude Opus 4.6 was "the only model that maintains high reliability at 1M".
  - Source: [yage.ai long-context benchmark, 2026-03-15](https://yage.ai/share/long-context-benchmark-en-20260315.html).
- [AGG] A different MRCR 1M listing: Opus 4.6 (max) 92.9%, DeepSeek V4 Pro 83.5%, and Gemini 3.1 Pro High 16.6 points below Opus 4.6. This is a different variant, so it is not comparable with the figures above — [BenchmarkList MRCR 1M](https://benchmarklist.com/benchmarks/mrcr_1m/).
- **Context windows:**
  - [AGG] Gemini 3.1 Pro offers 1M–2M, the largest standard window, with a 2x price surcharge above 200K — [ropewalk.ai](https://ropewalk.ai/blog/chatgpt-vs-claude-vs-gemini-2026).
  - [AGG] Claude 1M context went GA in March 2026 — [ropewalk.ai](https://ropewalk.ai/blog/chatgpt-vs-claude-vs-gemini-2026).
  - [AGG] GPT-5.4 restored 1M as a premium tier — [ropewalk.ai](https://ropewalk.ai/blog/chatgpt-vs-claude-vs-gemini-2026).
  - [VENDOR] GPT-5.5 in Codex: 400K — [Appwrite](https://appwrite.io/blog/post/gpt-5-5-launch).
- [AGG] Gemini 4 Argon can reportedly emit up to 1M output tokens (vs 64K previously). This matters for whole-codebase rewrites, but it is restricted access — [OfficeChai](https://officechai.com/ai/gemini-is-back-to-the-frontier-with-gemini-4-argon/).
- [INDEP] AA's Coding Agent Index includes SWE-Atlas-QnA, a codebase question-answering component. Its per-component scores were not surfaced in search — [AA comparison](https://artificialanalysis.ai/agents/coding-agents/comparisons/antigravity-sdk-vs-codex).

### Inferences
- **"Read this whole monorepo and explain or find X"**: use Claude (Opus 5.5, 1M) for accuracy, or Gemini 3.1 Pro when the input exceeds 1M or cost matters.
- **Large mechanical refactors**: Claude Code or Codex. Gemini Argon's 1M-output capability is promising but unavailable to most users.
- The MRCR figures are for early-2026 models (Opus 4.6, GPT-5.4, Gemini 3 Pro). Newer-model long-context data was not found.

### Gaps
- No 2026 independent code-review / bug-finding benchmark was found (for example SWE-Atlas component scores or bug-detection evals).
- Fiction.LiveBench has no 2026 data at 1M.

## 4. Deep research / web research quality

### Takeaway
No credible 2026 independent head-to-head of ChatGPT Deep Research vs Gemini Deep Research vs Claude Research was found. Evidence is weak, and this should be reported as a gap rather than ranked.

### Cited Findings
- [VENDOR / AGG, 2025, outdated] Parallel's BrowseComp posts compare older systems: GPT-4 browsing 1%, Claude search 6%, Perplexity 8%, Parallel up to 48%. Parallel labels its own page outdated — [Parallel](https://parallel.ai/blog/deep-research-browsecomp).
- [INDEP academic, 2025] BrowseComp-Plus (fixed corpus): GPT-5 55.9%, rising to 70.1% with a better retriever. Only GPT-5 was tested, with no Gemini or Claude — [arXiv 2508.06600](https://arxiv.org/abs/2508.06600).
- [INDEP academic] DeepResearch Bench consists of 100 PhD-level tasks, open-sourced in June 2025 — [arXiv 2506.11763](https://arxiv.org/abs/2506.11763v1).
- [VENDOR via AGG] Humanity's Last Exam with tools (a proxy for tool-augmented research): Claude Fable 5.1 65.0% vs GPT-6 Astra 57.2% — [Pillitteri](https://pasqualepillitteri.it/en/news/14246/openai-launches-gpt-6-astra-pricing-benchmarks).

### Inferences
- With no current data, practical routing has to rest on product features rather than benchmarks. ChatGPT Deep Research and Gemini Deep Research are long-running, report-style products. Gemini integrates with Google Search and Workspace. Claude Research is strong at synthesis.
- The writer should present this as "no independent ranking exists" rather than imply one.

### Gaps
- No 2026 BrowseComp or DeepResearch Bench leaderboard entries for the current ChatGPT, Gemini or Claude research products were found.
- No independent user studies were found.

## 5. Hard reasoning / math / science (GPT Pro, Gemini Deep Think, Claude extended thinking)

### Takeaway
OpenAI is reported (vendor and secondary) as strongest on hard mathematics (FrontierMath). Anthropic's Fable 5.x leads Humanity's Last Exam with tools per secondary reports. Google's Deep Think brand has the strongest older competition results (IMO gold, ARC-AGI-2), but no new Deep Think model was confirmed.

### Cited Findings
- [VENDOR] GPT-5.5: FrontierMath Tier 1–3 51.7%, Tier 4 35.4% — [OpenAI](https://openai.com/index/introducing-gpt-5-5/).
- [AGG] One analysis says GPT-6 Astra's edge is "clearly ahead on operating a computer, on cybersecurity and on hard mathematics", while it is level with the field on general reasoning and everyday coding — [Pillitteri](https://pasqualepillitteri.it/en/news/14246/openai-launches-gpt-6-astra-pricing-benchmarks).
- **GPT-6 Astra ARC-AGI-3 caveat:** [VENDOR via AGG] the reported 99.9% used a custom harness; the standard harness gives 62.7% — [Pillitteri](https://pasqualepillitteri.it/en/news/14246/openai-launches-gpt-6-astra-pricing-benchmarks).
- [VENDOR via AGG] HLE with tools: Claude Fable 5.1 65.0% vs GPT-6 Astra 57.2% — [Pillitteri](https://pasqualepillitteri.it/en/news/14246/openai-launches-gpt-6-astra-pricing-benchmarks).
- [VENDOR] Gemini 3.1 Pro GPQA Diamond 94.3% (Google-reported) — [BenchLM](https://benchlm.ai/frontier-ai-models).
- [AGG] Gemini 4 Argon vendor claims: LABBench 2 88.8 vs 85.4 for GPT-6 Astra (science/biology lab tasks) — [Kingy AI](https://kingy.ai/blog/frontier-ai-models-gemini-4-argon-gpt-6-astra-sol-claude/).
- [AGG] The circulating Deep Think figures (ARC-AGI-2 84.6%, IMO 2025 gold) are from the earlier Gemini 3 Deep Think line, not a new model — [AIToolsReview](https://aitoolsreview.co.uk/insights/gemini-3-5-pro).

### Inferences
- **Competition-style math or proofs**: GPT Pro-tier / GPT-6 Astra first, with Gemini Deep Think as a cross-check.
- **Broad expert Q&A with tools**: Claude Fable / Opus 5.5.
- **Science / lab-protocol reasoning**: Gemini 4 Argon claims an edge, but it is restricted access and vendor-only.

### Gaps
- No independent 2026 FrontierMath (Epoch) table for current models was found.
- No current GPT-5.x Pro / GPT-6 Pro identification was found.
- No Gemini Deep Think update after 3.x was found.

## 6. Multimodal: vision, video understanding, image/video generation, voice

### Takeaway
Image generation: OpenAI's GPT-Image-2 reportedly leads the LMArena image board by a wide margin over Google's Nano Banana 2 (early 2026). Video generation: leadership rotated in early 2026 from Google Veo 3.1 (first in January) to non-US models (Seedance 2.0, happyhorse-1.0, May 2026), with Sora 2 Pro close behind Veo. No October 2026 data was found.

### Cited Findings
- [INDEP via AGG] LMArena January–April 2026: GPT-Image-2 at 1,512, 242 points ahead of Nano Banana 2 — [TraeAI summary](https://www.traeai.com/articles/b457b78d-59b7-47b6-ae1a-8393598839df). Unverified on lmarena.ai.
- [INDEP via AGG] LMArena text-to-video, January 2026: veo-3.1-audio #1, sora-2-pro #2, about 2 points apart — [DataLearner video leaderboard](https://www.datalearner.com/en/leaderboards/external/video-generation).
- [INDEP via AGG] By May 2026, happyhorse-1.0 led text-to-video and Seedance 2.0 led image-to-video — [DataLearner](https://www.datalearner.com/en/leaderboards/external/image-to-video); [LMArena image-to-video](https://lmarena.ai/leaderboard/image-to-video).
- [AGG] Claude Fable 5 demonstrated vision-only gameplay at launch, a vision/agentic capability — [LetsDataScience](https://letsdatascience.com/news/anthropic-releases-claude-fable-5-demonstrates-vision-only-g-9a9b9185).

### Inferences
- **Image generation or editing**: ChatGPT (GPT-Image-2) first; Gemini Nano Banana as a strong alternative for edits.
- **Video generation**: Gemini (Veo) or Sora; check the current arena before committing.
- Claude has no native image or video generation, so it is not a choice for generative media.

### Gaps
- No October 2026 arena snapshots were found.
- No independent 2026 data was found on video understanding (Gemini traditionally strongest), voice-mode quality, or Veo 4 / Sora 3 existence.

## 7. Computer use / browser agents

### Takeaway
On independent OSWorld-Verified, Claude (Fable 5 / Mythos 5 ~85–86%, Opus 5 83.4%) is at or near the top among Western models, tied with Qwen3.8 Max. OpenAI vendor-claims leadership on the newer OSWorld 2.0 with GPT-6 Astra. The benchmark is near saturation, above the ~72% human baseline.

### Cited Findings
- [INDEP via AGG] OSWorld-Verified (2026-09-10): Qwen3.8 Max 86.1%, Claude Fable 5 85%, Claude Mythos 5 85% — [BenchLM OSWorld-Verified](https://benchlm.ai/benchmarks/os-world-verified).
- [INDEP reproduction] Yutori (August 2026), among general models without extra channels: claude-fable-5 86.0%, claude-opus-5 83.4%. A full-system agent leads at 90.2% — [Yutori](https://yutori.com/leaderboards/osworld-verified.md).
- [AGG] Gemini 3.6 Flash ~83% — [AnotherWrapper](https://anotherwrapper.com/tools/llm-pricing/leaderboards/osworld).
- [AGG] An undated older BenchLM snapshot had GPT-5.3 Codex at 86 — [BenchLM](https://www.benchlm.ai/benchmarks/osworld-verified).
- [VENDOR via AGG] GPT-6 Astra: 72.6% on an offline subset of OSWorld 2.0, vs 65.7% for GPT-5.6 Sol. Not comparable with OSWorld-Verified — [Pillitteri](https://pasqualepillitteri.it/en/news/14246/openai-launches-gpt-6-astra-pricing-benchmarks).
- [AGG] The human baseline on OSWorld is ~72%, so the benchmark is near saturation — [CryptoBriefing](https://cryptobriefing.com/computer-use-agents-85-percent-osworld-benchmark/).

### Inferences
- The model gap is now small. Product factors (browser integration, safety prompts, logged-in sessions) matter more than benchmark points.
- Reasonable defaults: Claude (Claude for Chrome / computer use) or ChatGPT Agent / Atlas with GPT-6 Astra. Gemini Flash models are competitive and cheap.

### Gaps
- No independent WebArena / Online-Mind2Web 2026 comparisons of the consumer products (ChatGPT Atlas, Claude for Chrome, Gemini / Project Mariner) were found.

## 8. Writing quality and Hebrew

### Takeaway
Claude leads most writing evaluations (EQ-Bench Creative Writing v3 in June 2026; arena creative writing in August 2026), but LLM-judged and human-vote leaderboards disagree sharply. No Hebrew benchmark covering GPT, Claude or Gemini was found.

### Cited Findings
- [INDEP, LLM-judged] EQ-Bench Creative Writing v3 (June 2026): Claude Opus 4.7 #1 at 2050.8 Elo vs GPT-5.4 1906.0 — [AI Benchmark Digest 2026-06-01](https://buttondown.com/theaggregate/archive/ai-benchmark-digest-2026-06-01/).
- [AGG] As of 2026-08-31, claude-fable-5 is #1 in arena.ai creative writing (human votes) but only #6 on EQ-Bench (LLM judges). kimi-k3 is #2 on EQ-Bench but #23 on arena — [Digital Applied](https://www.digitalapplied.com/blog/which-ai-model-writes-best-leaderboards-disagree).
- [AGG] Gemini 4 Argon is described in Chinese tech press as "a frontier model whose writing is stronger than its coding" — [ifeng](https://tech.ifeng.com/c/8wrT9WNEyX9).
- [INDEP academic] **Hebrew:**
  - The Hebrew Open LLM Leaderboard (Mafat/DICTA) covers open models only — [HF blog](https://github.com/huggingface/blog/blob/main/leaderboard-hebrew.md).
  - Dicta-LM 3.0 (February 2026) introduced a Hebrew chat benchmark suite (translation, summarization, Winograd, Israeli trivia, nikud) — [arXiv 2602.02104](https://arxiv.org/abs/2602.02104v1).
  - No frontier closed-model scores were surfaced.

### Inferences
- **Long-form prose, editing and tone**: default to Claude. Use GPT as an alternative; it is often preferred for structured or business writing, but that is anecdotal and unsourced here.
- **Hebrew**: no evidence-based ranking exists. Recommend a quick personal A/B test on the user's own Hebrew tasks. The Dicta-LM 3.0 paper may contain frontier-model comparisons (worth checking the PDF).

### Gaps
- No Hebrew-language quality data for GPT-5.x/6, Claude 5.x or Gemini 3.x/4 was found.

## 9. Speed, cost-per-task, hallucination rates, and which leaderboards are independent

### Takeaway
OpenAI GPT-5.6 Sol delivers near-top intelligence at about one third of Claude Fable 5's cost (per AA). Claude Opus 5.5 reportedly cut cost ~40% vs Opus 5. Gemini Flash models and Argon's introductory pricing are the cheapest. On AA-Omniscience, Anthropic models hallucinate least among frontier models, and GPT-5.6 reportedly hallucinates heavily when it doesn't know.

### Cited Findings
- **Cost:**
  - [INDEP] GPT-5.6 Sol is ~1/3 the cost of Fable 5 at similar index score — [AA](https://artificialanalysis.ai/articles/gpt-5-6-has-landed).
  - [AGG] Price list (per M tokens):

    | Model | Input / output price |
    |---|---|
    | Fable 5 | $10 / $50 |
    | GPT-6 Astra | $10 / $50 |
    | Opus 5.5 | ~$4 / $20 |
    | GPT-5.5 | $5 input |
    | Gemini 4 Argon (introductory) | $2 / $10 |

    Sources: [NYU RITS](https://rits.shanghai.nyu.edu/ai/anthropic-launches-claude-fable-5-its-first-public-mythos-class-model/); [DataNorth](https://datanorth.ai/news/openai-launches-gpt-6-astra); [Codersera](https://codersera.com/blog/claude-opus-5-5-complete-guide-2026/amp/); [tech-insider](https://tech-insider.org/gpt-5-5-launch-openai-april-23-terminal-bench-2026/); [OfficeChai](https://officechai.com/ai/gemini-is-back-to-the-frontier-with-gemini-4-argon/).
- **Hallucination (AA-Omniscience):**
  - [INDEP via AGG] AA-Omniscience (launched 2025-11-16) penalizes guessing over abstaining — [AA](https://artificialanalysis.ai/articles/aa-omniscience-knowledge-hallucination-benchmark).
  - Per Suprmind (2026-08-27), [Suprmind](https://suprmind.ai/hub/ai-hallucination-rates-benchmarks-in-2026/):

    | Model | Omniscience Index | Accuracy | Hallucination rate |
    |---|---|---|---|
    | Claude Fable 5 | 43 | 65% | 63.6% |
    | Claude Opus 5 | 37 | 61% | not given |
    | GPT-5.6 Sol | not given | not given | 92.2% |
    | GPT-5.6 Terra | not given | not given | 87.9% |
    | GPT-5.6 Luna | not given | not given | 92.6% |

  - Gemini 3.1 Pro was 2nd on the index at 33 (July 2026) — [Suprmind](https://suprmind.ai/hub/ai-hallucination-rates-benchmarks-in-2026/).
  - **Conflict:** an earlier Suprmind version said AA had not published a GPT-5.6 hallucination rate.
- **METR time horizons:**
  - [INDEP] Claude Mythos 50%-time horizon ≥16 hours, 80%-horizon 3h06m. METR says measurements above 16h are unreliable — [Wikipedia METR](https://en.wikipedia.org/wiki/METR).
  - [INDEP via AGG] v1.1 figures: Mythos Preview ~1,045 min, Gemini 3.1 Pro ~384 min, GPT-5.2 ~352 min — [DataLearner](https://www.datalearner.com/en/benchmarks/metr-time-horizons-v1-1).
  - [INDEP] METR has not reported horizons for Opus 4.7 or GPT-5.5, citing saturation — [Manifold](https://manifold.markets/Bayesian/best-metr-time-horizons-in-2026).
  - [AGG] A GPT-5.6 Sol horizon falls from more than 270h to 11.3h when "cheats" are scored as failures — [FutureSearch](https://futuresearch.ai/app/p/a/rsi-metr-time-horizon).
- **Independence of leaderboards:**
  - **Independent:**
    - Artificial Analysis: own evaluations; Intelligence, Coding Agent and Omniscience indices.
    - LMArena / arena.ai: human preference votes; raised money at a $1.7B valuation in January 2026 — [TechCrunch](https://techcrunch.com/2026/01/06/lmarena-lands-1.7b-valuation-four-months-after-launching-its-product).
    - METR.
    - Terminal-Bench (tbench.ai).
    - OSWorld-Verified.
    - Scale SWE-bench Pro.
    - EQ-Bench (LLM-judged).
    - Epoch (FrontierMath).
  - **Partly independent:** SWE-bench Verified. The harness is public, but submissions are often vendor-run and the benchmark is now saturated.
  - **Vendor-reported:** most launch-table numbers (Opus 5.5's SWE-bench Pro 89.9%, GPT-6 Astra's OSWorld 2.0, Argon's LABBench).
  - **Aggregators that mix vendor and independent numbers:** BenchLM, Morphllm, llm-stats. They often conflict — [BenchLM caveat](https://benchlm.ai/benchmarks/os-world-verified); [Morphllm](https://www.morphllm.com/swe-bench-pro).

### Inferences
- **Cost-sensitive bulk work**: GPT-5.6 Terra/Luna or Gemini Flash.
- **Factual Q&A where wrong answers are costly**: Claude (lowest hallucination per AA-Omniscience). Always verify GPT-5.6 factual claims.
- **Draft task-routing map** (synthesis; low-to-moderate confidence given source quality):

  | Task | First choice | Second choice | Also / notes |
  |---|---|---|---|
  | Agentic coding, terminal or long autonomous runs | Codex (GPT-5.6 Sol / GPT-5.5) | Claude Code | |
  | Interactive coding, refactors, code quality | Claude Code (Opus 5.5) | Codex | |
  | Long-context codebase / document understanding | Claude (1M, best recall) | Gemini 3.1 Pro (largest window, cheaper) | |
  | Hard math | GPT Pro / GPT-6 Astra | Gemini Deep Think | |
  | Expert Q&A with tools | Claude Fable/Opus | | |
  | Image generation | ChatGPT (GPT-Image-2) | | |
  | Video generation | Gemini Veo / Sora | | Check the current arena |
  | Computer use | Claude, ChatGPT Agent (GPT-6 Astra) | | Roughly tied |
  | Writing | Claude | | |
  | Hebrew | Unknown | | Test personally |
  | Low-hallucination factual answers | Claude | | |
  | Cheapest capable | Gemini Flash / GPT-5.6 Luna | | |
  | Deep research | No independent data | | Choose on product features |

### Gaps
- No independent speed (tokens/sec) or cost-per-task figures for the October 2026 flagships were found; the AA pages could not be fetched.
- The Vectara hallucination leaderboard was not checked.
- Consumer-subscription usage limits (which drive practical routing for subscription users) were not researched here.
