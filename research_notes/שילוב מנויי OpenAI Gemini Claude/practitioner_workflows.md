# How Practitioners Combine OpenAI (ChatGPT/Codex), Google Gemini and Anthropic Claude Subscriptions (2025 to Oct 2026)

Method note (for the report writer): research done 2026-10-12. WebSearch worked, but WebFetch failed with DNS errors (ENOTFOUND) for arxiv.org, news.ycombinator.com, mejba.me and buildmvpfast.com. Most findings below therefore come from search-result summaries of the cited pages, not full-text reads. Exact quotes and numbers should be treated as "reported by the search snippet". Evidence types are labeled: [MEASURED] means a paper or experiment with numbers, [VENDOR-INTERNAL] means a company's own unverified data, and [ANECDOTE] means individual practitioner reports, blogs or forum comments. Dates are publication dates where known. "~" means the date was inferred from context, such as an HN item ID range or the model names mentioned.

## Q1. Which role splits are reported most often, and why?

### Takeaway
By late 2026 the most common and best-documented split is **Claude (Claude Code) as primary planner/builder, with OpenAI Codex (GPT-5.x-Codex) as a skeptical reviewer, debugger or second opinion**. OpenAI now ships an official Codex plugin for Claude Code that supports this. Gemini usually plays a narrower role: long-context reading or orchestration, Google-ecosystem or Workspace tasks, and research. Practitioners disagree about which model should take which role, and the stated reasons are anecdotal.

### Cited Findings
- **Claude builds, Codex reviews (most frequent pattern).** A 2026 developer write-up uses Claude Code as the primary builder and Codex as a "skeptical reviewer": Claude drafts a detailed plan, and Codex critiques it before implementation. The author reports that the first `/codex:review` "came back with three issues I would have shipped without thinking twice" [ANECDOTE, 2026] — [mejba.me](https://www.mejba.me/claude-code-codex-plugin-dynamic-duo-workflow-2026)
- **Official OpenAI Codex plugin for Claude Code (2026).** Commands include `/codex:review` (reviews uncommitted changes), `/codex:adversarial-review` (challenges designs), `/codex:rescue` (background or whole-codebase tasks), and `/codex:status`, `/codex:result` and `/codex:cancel` for async runs. An optional "review gate" makes Codex review every Claude turn, which one author calls "powerful but expensive" — [buildmvpfast.com](https://www.buildmvpfast.com/blog/codex-plugin-claude-code-gpt-5-5-opus-multi-model-2026); [mejba.me](https://www.mejba.me/claude-code-codex-plugin-dynamic-duo-workflow-2026); [To Data & Beyond](https://todatabeyond.substack.com/p/use-codex-inside-claude-code-instead)
- **Stated rationale for the split.** One guide says Claude Code "excels at deep reasoning across large codebases and architecture decisions", while "Codex CLI dominates autonomous execution, background tasks, and code review" [ANECDOTE, 2026-04-18] — [Daniel Vaughan, codex.danielvaughan.com](https://codex.danielvaughan.com/2026/04/18/codex-cli-claude-code-working-together/)
- **Codex as bug-finder (HN).** In the GPT-5.2-Codex thread (~Dec 2025), one user had two fresh instances review a large task and said "Codex finds more issues in ~9 out of 10 cases". Another said Codex is "absolutely killer at analyzing flows and finding subtle bugs" while Claude Code stays their main tool [ANECDOTE] — [HN 46316367](https://news.ycombinator.com/item?id=46316367)
- **The reverse direction also exists.** One HN commenter said they "always have claude review codex output" [ANECDOTE] — [HN 45960789](https://news.ycombinator.com/item?id=45960789)
- **Three-model orchestration (Gemini on top).** The open-source repo "claude-codex-gemini" makes Gemini the orchestrator that "makes decisions, delegates work". Claude Code handles requirements, planning and implementation, and Codex is the final gate reviewer. The pipeline runs sequential plan and code reviews through Sonnet, then Opus, then Codex. The repo was last updated about 298 days before the search, so roughly late 2025 [ANECDOTE/tooling] — [GitHub Z-M-Huang/claude-codex-gemini](https://github.com/Z-M-Huang/claude-codex-gemini)
- **Tembo's split.** Claude Code writes the change, Codex reviews the diff, and a third agent acts as a contrarian check on the spec. The stated reason is that "a reviewer who didn't write the code catches mistakes the author rationalizes" [ANECDOTE, 2026] — [Tembo.io](https://www.tembo.io/blog/codex-vs-claude-code-vs-gemini-cli)
- **A different split.** Designveloper suggests Gemini verifies Google Cloud-specific configuration, Claude reviews architecture, and Codex implements scoped changes [ANECDOTE, 2026] — [Designveloper](https://www.designveloper.com/blog/claude-vs-chatgpt-vs-gemini-for-coding/)
- **Personal stack example (2025).** A blog titled "My 2025 AI Development Workflow: Claude, Codex, Gemini & Google AI Studio" describes combining all three plus AI Studio [ANECDOTE, 2025; details not read] — [Context Studios](https://www.contextstudios.ai/blog/my-ai-development-workflow-2025-how-i-combine-claude-code-codex-gemini-and-google-ai-studio)
- **Popular "Claude plans, Codex reviews" meme.** A router vendor's post is titled "Rebuilding the Viral Two-Agent Coding Workflow", which suggests the pattern circulated widely. It comes from a vendor with a commercial interest — [Crazyrouter](https://crazyrouter.com/en/blog/claude-codex-two-agent-review-workflow-crazyrouter)
- **Consumer-level roles.** 2026 comparison guides consistently describe Claude as strongest for writing, coding and document analysis, ChatGPT as the broadest feature set (voice, images, Deep Research, GPTs), and Gemini as most valuable for Google Workspace users [ANECDOTE/opinion] — [ClickForest](https://clickforest.com/en/blog/ai-tools-superpowers); [aivario](https://aivario.com/blog/chatgpt-vs-claude-vs-gemini); [toolindex](https://toolindex.net/blog/chatgpt-claude-gemini-subscription-cost-2026)

### Inferences
- Ranked by how often the sources mention them: (1) Claude implements, Codex reviews or debugs; (2) Codex as second opinion on hard bugs; (3) Gemini for long-context reading, Google-ecosystem work or orchestration; (4) "GPT Pro for the hardest reasoning". The fourth is plausible but I found no specific 2026 source for it.
- The split seems driven mainly by (a) perceived temperament (Claude reported as better at interactive building and planning, Codex as more thorough or literal at finding bugs), (b) tooling (the official plugin removes switching friction), and (c) quota (see Q2).
- Practitioners agree that roles should be split but disagree on which model does what. That points to the value coming from independent review rather than from any particular model being the reviewer.

### Gaps
- I found no survey or poll that quantifies how common each split is. Frequency is inferred from how many sources mention it.
- I found no 2026 primary source on the "GPT-5.x Pro for hard reasoning" pattern.
- Posts by Steve Yegge, Simon Willison and Armin Ronacher focused mostly on single-tool agent use (see Q5). I found no post from them describing a deliberate three-subscription workflow.

## Q2. Workflow patterns: planner/executor, cross-model review, parallel attempts, triangulated research, second opinion, and routing by quota

### Takeaway
The patterns with the clearest practitioner support are cross-model review (author model plus reviewer model in a fresh context) and second opinions on hard bugs. Parallel attempts are done with git worktrees and carry known merge pitfalls. Quota-driven routing (switching tools when one runs out) is mentioned often in passing. I found almost no practitioner documentation of triangulating Deep Research across all three vendors.

### Cited Findings
- **Fresh-context review beats same-session self-review [VENDOR-INTERNAL].** A Hacker News discussion of Anthropic's Code Review launch (~Mar 2026) cited internal figures: review by a separate context caught more critical errors than a second pass in the same session (40% vs 29%). A commenter argued that "reviewing twice in the same session doesn't help much". This figure comes from a search summary of the HN thread, and I could not fetch the thread to verify it — [HN 47313787](https://news.ycombinator.com/item?id=47313787)
- **Anthropic's Code Review product (launched 2026-03-09, research preview).** Multiple agents review a PR in parallel, each looking for a different class of issue, and a verification step filters false positives. Anthropic's internal figures [VENDOR-INTERNAL]:
  - PRs with substantive review comments rose from 16% to 54%.
  - Fewer than 1% of findings were marked incorrect.
  - For PRs over 1,000 lines changed, 84% got findings (7.5 issues on average). For PRs under 50 lines, 31% got findings (0.5 on average).
  - A review takes about 20 minutes and costs about $15–25 per PR.

  — [InfoWorld](https://www.infoworld.com/article/4143297/claude-code-adds-code-reviews.html); [TechRadar](https://www.techradar.com/pro/anthropic-launches-a-new-code-review-tool-to-check-ai-generated-content-but-it-might-cost-you-more-than-youd-hope); [InfoQ, 2026-04](https://infoq.com/news/2026/04/claude-code-review)
- **Second-pass cost in time.** A blogger runs every finished Claude Code build through Codex for a second pass and reports it "adds maybe 5-10 mins to your process" [ANECDOTE] — [Little Might](https://www.littlemight.com/claude-code-second-opinion-codex-skill/)
- **Second opinion on hard bugs, routed by quota.** An HN commenter said Codex was the stronger debugger, but its quota drained quickly, "so Claude Code remains my main driver" [ANECDOTE, ~2025–26] — [HN 46391391 "Codex vs. Claude Code (today)"](https://news.ycombinator.com/item?id=46391391)
- **Shifts in primary tool.** HN threads titled "Codex has arguably been better than Claude Code for months now" and "A week of using Codex more than Claude" (~mid/late 2026) suggest practitioners move their primary driver between vendors as models change [ANECDOTE] — [HN 48849401](https://news.ycombinator.com/item?id=48849401); [HN 49393051](https://news.ycombinator.com/item?id=49393051)
- **A personal two-tool account** ("A Heated Rivalry: Claude Code + OpenAI Codex") describes using both side by side [ANECDOTE; details not read] — [Pablo Stanley, Substack](https://pablostanley.substack.com/p/a-heated-rivalry-claude-code-openai)
- **Parallel attempts with worktrees.** The standard setup is one git worktree and branch per agent, with a different agent CLI in each directory. Guides show Claude Code and Codex running side by side this way — [agentsroom.dev](https://agentsroom.dev/blog/git-worktrees-multiple-ai-agents); [MindStudio](https://www.mindstudio.ai/blog/git-worktrees-parallel-ai-coding-agents)
- **Async vs sync tools.** Comparison pieces say Codex's asynchronous, isolated-container model suits parallel backlog work, while Claude Code's synchronous model needs more of the user's attention [ANECDOTE, 2026] — [SitePoint](https://www.sitepoint.com/claude-code-vs-codex-2026/); [MindStudio](https://mindstudio.ai/blog/codex-vs-claude-code-2026-2)
- **Triangulating Deep Research.** I found no practitioner write-up that systematically runs the same question through ChatGPT, Gemini and Claude research modes and compares the results. 2026 guides disagree on which tool is best for research. One says ChatGPT's Deep Research synthesis beats Gemini's; another favors Gemini for its context window and Google search — [aiagentrank](https://aiagentrank.io/blog/claude-vs-chatgpt-vs-gemini); [radar.firstaimovers](https://radar.firstaimovers.com/ai-platform-guide-2026-chatgpt-claude-gemini)

### Inferences
- Within the coding pattern, the active ingredient appears to be **context independence**: the reviewer did not write the code. Model diversity may add to that, but no source isolates the two effects (see Q3).
- Quota routing is real but works mostly as a convenience: a second subscription extends usable hours.

### Gaps
- I found no controlled comparison of the "Claude writes, Codex reviews" pipeline against "Claude writes, fresh Claude reviews".
- I found no measured data on the Deep Research triangulation workflow.

## Q3. Evidence: do ensembles, mixture-of-agents (MoA) and cross-model review beat a single model? Correlated errors and judge self-preference bias

### Takeaway
The measured evidence is mixed. Original MoA showed that combining models can beat a single frontier model. A later study found that repeatedly sampling the single best model (Self-MoA) usually beats mixing different models, because weaker models dilute quality. Errors across LLMs are strongly correlated, which limits what diversity can add. LLM judges show self-preference bias, though how large that bias is remains disputed. Together these support using a different-vendor model as reviewer or judge, while warning against expecting large ensemble gains.

### Cited Findings
- **Original MoA [MEASURED].** Wang et al., "Mixture-of-Agents Enhances LLM Capabilities" (arXiv 2406.04692, June 2024; ICLR 2025). A layered MoA built only from open-source models scored **65.1% on AlpacaEval 2.0**, beating GPT-4o at 57.5%. The cost is a slower time to first token and growing token use. A typo in v1 gives 65.8% — [arXiv](https://arxiv.org/html/2406.04692v1); [Together AI blog](https://together.ai/blog/together-moa)
- **Self-MoA [MEASURED].** Li, Lin, Xia and Jin, "Rethinking Mixture-of-Agents: Is Mixing Different LLMs Beneficial?" (arXiv 2502.00674, Feb 2025; TMLR 2026). Aggregating samples from only the top model scored **6.6% higher than MoA on AlpacaEval 2.0** and **3.8% higher on average across MMLU, CRUX and MATH**. The authors report that MoA "is rather sensitive to the quality, and mixing different LLMs often lowers the average quality". They identify a few scenarios where mixing still helps — [arXiv](https://arxiv.org/pdf/2502.00674); [TMLR 2026](https://mlanthology.org/tmlr/2026/li2026tmlr-rethinking/)
- **Correlated errors [MEASURED].** Kim, Garg, Peng and Garg, "Correlated Errors in Large Language Models" (arXiv 2506.07962, ICML 2025) evaluated more than 350 LLMs. On one leaderboard dataset, when two models both erred, they agreed on the same wrong answer **about 60% of the time**. Shared provider or base architecture increases the correlation, and more accurate models are correlated even across different providers. The authors discuss what this means for LLM-as-judge setups — [arXiv](https://arxiv.org/pdf/2506.07962)
- **Ensemble ceiling [MEASURED/theory].** Turkmen et al., "Don't Always Pick the Highest-Performing Model" (arXiv 2602.08003, Feb 2026), model correlated errors with a Gaussian copula and derive an error floor. This explains why ensemble accuracy plateaus as models are added. They select models by mutual information rather than individual accuracy — [arXiv](https://arxiv.org/html/2602.08003v1)
- **Family bias [MEASURED, vision-language models].** "Hidden Clones" (arXiv 2603.17111, 2026) studied 17 vision-language models from 8 families. Same-family models make correlated errors, so the ensemble behaves like only about 2.5–3.6 effective independent voters — [arXiv](https://arxiv.org/pdf/2603.17111)
- **Shared wrong answers [MEASURED].** Bradley, "LLMs and the Madness of Crowds" (arXiv 2411.01539) finds that LLM errors on multiple-choice evaluations are systematically correlated across models rather than random — [arXiv](https://www.arxiv.org/pdf/2411.01539)
- **Self-preference bias [MEASURED].** Wataoka et al. (arXiv 2410.21819, 2024) found that GPT-4 shows significant self-preference. The bias is largely explained by perplexity: judges favor low-perplexity, familiar-style text — [arXiv](https://arxiv.org/html/2410.21819v1)
- **Self-preference measurement [MEASURED].** Chen et al., "Beyond the Surface" (arXiv 2506.02592, 2025) proposed the DBG score and found that response style and post-training data affect the bias — [arXiv](https://arxiv.org/pdf/2506.02592)
- **Counter-evidence [MEASURED].** Roytburg et al., "Are LLM Evaluators Really Narcissists?" (arXiv 2601.22548, 2026) found that part of the measured self-preference is a confound: judges also err on hard queries. With an Evaluator Quality Baseline, only about **51%** of the initial self-preference findings stayed significant — [arXiv](https://arxiv.org/pdf/2601.22548v3)
- **Bias vs capability [MEASURED].** Yang et al. (arXiv 2604.22891, 2026) tested 20 LLMs on equal-quality response pairs. Stronger capability is often uncorrelated, or even negatively correlated, with low self-preference bias — [arXiv](https://arxiv.org/abs/2604.22891)

### Inferences
- **Using a different-vendor model as reviewer** has two supports: reduced self-preference (different perplexity profile) and somewhat lower error correlation across providers. Kim et al. show cross-provider errors are still substantially correlated, so the gain is real but bounded.
- **"Ask all three and pick the best"** is weakly supported: Self-MoA says that if one model is clearly stronger on a task, more samples of that model can beat mixing. Mixing pays off mainly when the models are roughly equal in strength and have complementary weaknesses.
- The vendor-internal finding on fresh-context review (40% vs 29%) suggests a large share of the review benefit comes from a fresh context alone, which one subscription can provide.

### Gaps
- I found no peer-reviewed study of cross-vendor code review (for example, Codex reviewing Claude's diffs) on a real-world software engineering benchmark versus same-model review.
- I found no measurement of how often Codex's review findings are false positives. It is only reported anecdotally (see Q4).

## Q4. Pitfalls: context switching, inconsistent instructions, cost, duplicated work, security, merge conflicts

### Takeaway
The pitfalls practitioners report most often are quota or cost drain from review loops, false positives from the reviewer, merge and isolation problems with parallel agents, and harness lock-in (each vendor's model works best in its own tool). I found no documented incidents of secrets leaking across tools; that risk is inferred rather than reported.

### Cited Findings
- **Review loops are expensive.** The Codex plugin's "review gate" (Codex reviews every Claude turn) is described as "powerful but expensive" — [mejba.me](https://www.mejba.me/claude-code-codex-plugin-dynamic-duo-workflow-2026). Anthropic's own multi-agent review costs about $15–25 per PR [VENDOR] — [TechRadar](https://www.techradar.com/pro/anthropic-launches-a-new-code-review-tool-to-check-ai-generated-content-but-it-might-cost-you-more-than-youd-hope)
- **Reviewer false positives.** An HN commenter said Codex sometimes flags nonexistent bugs in Claude's code [ANECDOTE] — [HN 46316367](https://news.ycombinator.com/item?id=46316367)
- **Quota drain.** Codex was the better debugger, but its quota drained quickly [ANECDOTE] — [HN 46391391](https://news.ycombinator.com/item?id=46391391)
- **Harness lock-in.** Armin Ronacher (May 2026, via Simon Willison) argues that recent Anthropic models were trained on Claude Code's own edit tool, so other harnesses' edit tools are more error-prone. Ronacher also said Codex is effectively unusable outside its original harness. So running a vendor's model through another vendor's tool can hurt quality [EXPERT OPINION] — [Simon Willison, armin-ronacher tag](https://simonwillison.net/tags/armin-ronacher/)
- **Silent overwrites in shared checkouts.** Parallel agents in one checkout overwrite each other's edits with no error — "an isolation problem, not a parallelism problem" — [geggleto Substack](https://geggleto.substack.com/p/build-your-agenticos-worktrees-for)
- **Merges still need sequencing.** "Parallel building is okay. Parallel merging is not." — [Cursor forum](https://forum.cursor.com/t/how-do-you-avoid-messy-diffs-when-running-parallel-cursor-ai-agent-tasks/160928)
- **Integration-time conflicts.** Merge conflicts surface at integration, along with shared ports and services, index-lock failures and test-port contention. Running many agents can be a net loss even for tasks that look parallelizable — [DEV, dublecc](https://dev.to/dublecc/parallel-ai-agent-workflows-with-git-worktrees-the-concrete-pattern-59lp); [MindStudio](https://www.mindstudio.ai/blog/git-worktrees-parallel-ai-coding-agents)
- **Contradictory fixes.** Two agents "fixed" the same bug in opposite directions before the user moved them to worktrees [ANECDOTE] — [DEV, yureki_lab](https://dev.to/yureki_lab/how-i-run-multiple-claude-code-agents-in-parallel-without-collisions-3pbk)
- **Mitigation.** Each agent runs `git merge main --no-edit` before a new task, and conflicting merges are aborted and sent back to the agent — [Marcus docs](https://marcus.readthedocs.io/en/stable/_sources/systems/infrastructure/15-git-worktree-isolation.md.txt)

### Inferences
- **Inconsistent instructions.** Each CLI reads its own instruction file (Claude Code: CLAUDE.md; Codex: AGENTS.md; Gemini CLI: GEMINI.md). Without one shared source of truth, the agents drift apart. This comes from general knowledge of the tools; I did not find a 2026 source on it in this session.
- **Security.** Running three CLIs means three sets of credentials and three vendors' data-retention policies applied to the same repo and secrets. I found no specific incident reports.

### Gaps
- I found no measured data on context-switching overhead beyond the "5–10 min per second pass" anecdote.
- I found no documented cross-tool secrets incidents.

## Q5. Is paying for all three worth it? Consolidating vs spreading spend

### Takeaway
2026 pricing guides agree that the three $20/month tiers cost nearly the same, so choose by fit. They generally advise one primary paid tool plus at most one complement. Heavy coders commonly pair a primary coding subscription with a second for review or overflow. Paying for three is rarely recommended outside specific needs, such as Google Workspace integration for Gemini.

### Cited Findings
- **Price parity.** ChatGPT Plus, Claude Pro and Google AI Pro cost within about a dollar of each other. Cheaper tiers exist: Google AI Plus at $7.99 and ChatGPT Go at about $8. Higher tiers run $100–$200. Sources conflict on details such as ChatGPT's $100 vs $200 Pro tier and Gemini storage of 2TB vs 5TB — [toolindex](https://toolindex.net/blog/chatgpt-claude-gemini-subscription-cost-2026); [aizolo](https://aizolo.com/blog/chatgpt-vs-claude-vs-gemini-cost-2026/); [explainx](https://explainx.ai/blog/ai-subscription-true-cost-chatgpt-claude-gemini-2026)
- **Running two is common.** One guide notes "many researchers run both twenties" [opinion] — [toolindex](https://toolindex.net/blog/chatgpt-claude-gemini-subscription-cost-2026)
- **When Gemini earns its place.** It is worth paying for mainly if you live in Google Workspace; "the integration is worth more than a marginal capability difference" [opinion] — [aivario](https://aivario.com/blog/chatgpt-vs-claude-vs-gemini)
- **Primary tool shifts.** HN threads in mid/late 2026 show practitioners moving their primary subscription between Claude and Codex as models change. That argues for month-to-month flexibility over long commitments [ANECDOTE] — [HN 48849401](https://news.ycombinator.com/item?id=48849401); [HN 49393051](https://news.ycombinator.com/item?id=49393051)
- **Expert interest in harness economics.** Simon Willison (Jan 2026) amplified Ronacher's point that Anthropic's unit economics depend on cache use and harness design. This is relevant to why each vendor's subscription is cheapest inside its own tool — [X/@simonw](https://x.com/simonw/status/2009714613441745027)

### Inferences
- A defensible heuristic: keep the subscription you build in daily, add a second from a different vendor for review, debugging and overflow, and drop the third unless it provides a capability you use weekly (Workspace or Gemini long context, or ChatGPT's voice, images and Deep Research). Self-MoA and the correlated-error research suggest diminishing returns from a third model in quality terms, though not necessarily in quota terms.
- Signals that it is time to drop a subscription: it has not been the primary or reviewer tool for a month, or another tool's quota already covers your overflow.

### Gaps
- I found no rigorous cost-benefit analysis or survey of people paying for all three.
- Consumer pricing sources are low-quality aggregators with conflicting figures, so check vendor pricing pages.

## Q6. Non-coding project work: documents, research, presentations and data analysis

### Takeaway
Guides repeat a consistent heuristic, mostly as opinion rather than measurement:
- **Claude** for writing quality and long-document analysis.
- **ChatGPT** for breadth: Deep Research, image generation, voice, data analysis and GPTs.
- **Gemini** for Google Workspace (Docs, Sheets, Slides, Gmail), very long context and Google-search-grounded research.

Sources disagree on which tool is best for deep research.

### Cited Findings
- Claude is "the strongest tool for writing, coding, and document analysis" — [ClickForest](https://clickforest.com/en/blog/ai-tools-superpowers). Prose quality is called "the most reliably reported distinction" between the tools [opinion] — [toolindex](https://toolindex.net/blog/chatgpt-claude-gemini-subscription-cost-2026)
- ChatGPT Plus offers the broadest set of features (GPT-5.4, voice, image generation, browsing, GPT Store) — [aivario](https://aivario.com/blog/chatgpt-vs-claude-vs-gemini)
- **Research, conflicting views.** One guide rates ChatGPT's Deep Research synthesis above Gemini's — [aiagentrank](https://aiagentrank.io/blog/claude-vs-chatgpt-vs-gemini). Another rates Gemini stronger for research and document work because of its context window and Google's structural advantages — [radar.firstaimovers](https://radar.firstaimovers.com/ai-platform-guide-2026-chatgpt-claude-gemini)
- Gemini Deep Research has a free tier of 5 reports per month and full access with Google AI Pro at $19.99 per month — [AI Agent Index](https://theaiagentindex.com/compare/gemini-deep-research-vs-claude)
- Gemini is recommended mainly for Workspace users — [aivario](https://aivario.com/blog/chatgpt-vs-claude-vs-gemini); [dev.to platform guide](https://dev.to/dr_hernani_costa/ai-platform-selection-guide-2026-chatgpt-vs-claude-vs-gemini-51hb)

### Inferences
- A common non-coding pipeline consistent with these sources: gather sources with Deep Research (ChatGPT or Gemini), synthesize and write with Claude, then produce Workspace or Slides output with Gemini. This is inferred from the role descriptions; I found no practitioner write-up that documents it end to end.

### Gaps
- I found no head-to-head measured tests of the Deep Research products, presentation tools or data-analysis modes from independent testers in 2026. Most sources are SEO comparison blogs with inconsistent model names and dates.
