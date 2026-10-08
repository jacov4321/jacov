# Latency in Computer-Use Agents and Techniques That Measurably Speed Them Up

Method note: arxiv.org, github.com, huggingface.co and proceedings.mlsys.org were unreachable from this sandbox (DNS and proxy blocked), so every finding below comes from search-engine extracts of the primary pages (arXiv abs/HTML/PDF, project pages, vendor docs). The URLs cited are the primary sources. Numbers were not checked against full-text tables, so verify them before quoting exactly. Status labels: [PEER] = peer-reviewed venue, [PREPRINT] = arXiv only, [VENDOR] = company-reported.

## Q1. What do OSWorld-Human and "Step-level Optimization for Efficient Computer-use Agents" (arXiv 2604.27151) find about the latency breakdown and step inefficiency?

### Takeaway
Most CUA latency comes from LLM calls, not from the environment. In agentic frameworks (Agent S2, GTA1), planning plus reflection takes about 75-94% of wall-clock time. Per-step latency grows about 3x over a task as the prompt grows, and agents take about 1.4-4.3x more steps than humans. Routing most steps to a small model and escalating to a large one only when needed cut latency by up to 45.8% and cost by up to 74.6% while keeping near-large-model success.

### Cited Findings
- OSWorld-Human (Abhyankar, Qi, Zhang; UCSD WukLab) [PEER: MLSys 2026, oral in the "Agentic AI 1" session]: CUAs are "practically unusable" because end-to-end latency for tasks humans finish in minutes runs to tens of minutes. On one 50-step OS task, total latency exceeded 40 minutes, mostly planning and reflection. — [arXiv 2506.16042](https://arxiv.org/abs/2506.16042); [MLSys 2026 PDF](https://proceedings.mlsys.org/paper_files/paper/2026/file/5edb57c05c81d04beb716ef1d542fe9e-Paper-Conference.pdf); [MLSys session](https://mlsys.org/virtual/2026/session/3668)
- LLM calls for planning, reflection and judging take "often over 90%" of end-to-end latency. Planning plus reflection is 75-94% of task latency across applications (an earlier version said 76-96% for Agent S2). Planning alone is more than half and sometimes close to 75%, because it runs every step and again after each subtask, while reflection runs only once per step. Screenshot capture and click execution contribute minimally. — [arXiv HTML v2](https://arxiv.org/html/2506.16042v2); [WukLab blog](https://mlsys.wuklab.io/posts/oshuman/)
- Setup: grounding used UI-TARS-7B-DPO (for S2) and GTA1-7B (for GTA1), served on a single NVIDIA A6000 with SGLang. Grounding was a small share of time. — [arXiv HTML v2](https://arxiv.org/html/2506.16042v2)
- Each later step can take about 3x longer than early steps, because the prompt and history grow. — [arXiv 2506.16042](https://arxiv.org/abs/2506.16042)
- Step inefficiency: the best agents take 1.4-2.7x more steps than the human trajectory (v1 abstract and GitHub README) or 2.7-4.3x (later version). The figures changed between versions. — [arXiv](https://arxiv.org/abs/2506.16042); [GitHub WukLab/osworld-human](https://github.com/WukLab/osworld-human)
- 23% of errors come from poor visual grounding, which costs up to 30 extra steps on a single task. — [arXiv HTML v2](https://arxiv.org/html/2506.16042v2)
- The Weighted Efficiency Score (WES) rewards a success by the human/agent step ratio and penalizes failures by steps consumed (WES+ / WES-). The best publicly-traced OSWorld agent has a 41.4% success rate but only 15.6% WES. The leaderboard reports both single-action and grouped-action WES. — [arXiv](https://arxiv.org/abs/2506.16042); [GitHub](https://github.com/WukLab/osworld-human)
- Recommendation: predict action groups so that one plan/reflect/judge call covers several actions. — [arXiv](https://arxiv.org/abs/2506.16042)
- Step-level Optimization (Wei, Ni, Zhao, Gan, Cohan) [PREPRINT 2604.27151]: a small policy runs every step by default. A Stuck Monitor (stalled progress in recent reasoning and actions) escalates to a strong model, and a Milestone Monitor triggers sparse verification by the strong model at checkpoints. It targets two failure types: progress stalls and "silent semantic drift". Across OSWorld and WebArena it reduces inference cost by up to 74.6% and latency by up to 45.8% versus always-large agents, at comparable success. It works as a layer on existing agents without retraining the large model. — [arXiv abs](https://arxiv.org/abs/2604.27151); [arXiv HTML](https://arxiv.org/html/2604.27151)
- Example row from the efficiency table: EvoCUA-8B routed with Claude Sonnet 4.5 escalation costs $0.224/task at 55.4% accuracy and 4.1 s per request. Always-Sonnet-4.5 costs $0.881/task at 58.1% and 6.4 s per request. That is about 75% cheaper and 36% lower per-request latency for -2.7 points. — [arXiv HTML](https://arxiv.org/html/2604.27151)

### Inferences
- The latency budget is dominated by (number of steps) x (planner tokens per step), and both grow over a trajectory. The biggest levers are fewer LLM calls (grouping, caching, replay) and cheaper calls (small-model routing, context compression). A faster screenshot or grounder alone gives only small gains.
- The per-step 3x growth implies that history truncation and compression (see Q5) pays off most on long tasks.

### Gaps
- "OSWorld-Gold": I found no paper or benchmark by this name. It may be a confusion with OSWorld-Human (human "gold" trajectories) or OSWorld-Verified.
- I could not extract absolute per-step seconds by component (for example, planner s/step versus grounder s/step) from Table 1 of OSWorld-Human, or per-benchmark breakdowns for 2604.27151.

## Q2. Planner plus small fast grounder (Agent S2, GTA1, UI-TARS as grounder, Jedi): latency/accuracy tradeoffs

### Takeaway
Splitting a large planner from a small 7B grounder is now the standard high-accuracy design. With an o3 planner, a 7B grounder reaches about 45-50% on OSWorld. The grounder is cheap (about 7B, one GPU), but the planner still dominates latency, so the split helps accuracy more than speed unless the planner is also shrunk or called less often.

### Cited Findings
- Agent S2 [PREPRINT, also on OpenReview] uses generalist Manager/Worker planners plus a "Mixture of Grounding" that routes each action to visual, textual or structural specialist grounders. It works from screenshots only, with no accessibility tree or HTML. It reports 18.9% and 32.7% relative improvement over Claude Computer Use and UI-TARS on OSWorld 15-step and 50-step evaluations. — [arXiv 2504.00906](https://arxiv.org/pdf/2504.00906); [OpenReview](https://openreview.net/pdf?id=zg5is4GJ3R)
- GTA1 (Salesforce) [listed as ICLR 2026 by papernotes]: o3 planner plus GTA1-7B grounder scores 45.2% on OSWorld at 100 steps, versus OpenAI CUA o3 at 42.9% using 200 steps. The model card gives 53.1 on OSWorld-Verified. The GTA1-7B grounder scores 50.1 / 92.4 / 67.7 on ScreenSpot-Pro / ScreenSpot-V2 / OSWorld-G (later release: 55.5 / 93.4 / 60.1). — [arXiv 2507.05791](https://arxiv.org/html/2507.05791v1); [GitHub Yan98/GTA1](https://github.com/Yan98/GTA1); [HF model card](https://huggingface.co/Salesforce/GTA1-7B); [paper note ICLR2026](https://en.papernotes.org/ICLR2026/llm_agent/gta1_gui_test-time_scaling_agent/)
- Jedi (OSWorld-G / Jedi dataset): Jedi-7B with a GPT-4o planner scores 27.0 ± 1.81 at 100 steps, versus GPT-4o alone at 5.0 (15 steps). Jedi-7B with an o3 planner scores 50.2 at 100 steps. Third-party tables give Jedi-7B+o3 at 50.65 and GTA1-7B+o3 at 48.59. — [Jedi project page](https://osworld-grounding.github.io/); [arXiv 2505.13227](https://www.alphaxiv.org/abs/2505.13227)
- OSWorld-Human measured S2 with a UI-TARS-7B-DPO grounder and GTA1 with a GTA1-7B grounder on an A6000 via SGLang. Planning and reflection, not grounding, dominated latency. — [arXiv HTML v2](https://arxiv.org/html/2506.16042v2)

### Inferences
- The accuracy gain comes from the planner quality (GPT-4o at 27% versus o3 at about 50% with the same Jedi-7B). The latency cost is also in the planner, so the practical speed recipe is small-model-by-default with escalation (Q1, 2604.27151) plus a 7B grounder.

### Gaps
- I found no published head-to-head of wall-clock latency for planner+grounder versus a single native model (for example UI-TARS-1.5 end-to-end versus o3+GTA1). No per-call latency numbers for the 7B grounders surfaced.

## Q3. Multi-action per step, action batching, hybrid GUI+API actions, code-as-action

### Takeaway
Replacing many GUI primitives with one programmatic tool call improves both success and speed. UltraCUA reports +22% relative success and about 11% faster execution on average. OSWorld-Human argues that action grouping is the main route to fewer LLM calls.

### Cited Findings
- UltraCUA (Apple + HKU) [PREPRINT 2510.17790]: hybrid action space that unifies GUI primitives with high-level programmatic tool calls. It uses a synthetic engine with 17,000+ verifiable tasks and SFT followed by online RL that learns when to choose GUI versus API. On OSWorld it reports +22% average relative improvement over base models while being 11% faster. On WindowsAgentArena (out of domain) it reaches 21.7% success. The authors say hybrid actions reduce error propagation. — [arXiv 2510.17790](https://arxiv.org/pdf/2510.17790); [deeplearn.org abstract](https://deeplearn.org/arxiv/671453/ultracua:-a-foundation-model-for-computer-use-agents-with-hybrid-action); [MarkTechPost](https://www.marktechpost.com/2025/10/23/ultracua-a-foundation-computer-use-agents-model-that-bridges-the-gap-between-general-purpose-gui-agents-and-specialized-api-based-agents/)
- CUA-Universe (GUI+CLI hybrid) reports that a 9B model reduces step counts and token usage while improving success. No numbers were extracted. — [summary page](https://book.st-hakky.com/en/news/cua-universe-gui-cli-hybrid-agent-scalable-dynamic-environment)
- OSWorld-Human: when an agent predicts grouped actions, a single plan/reflect/judge pass can serve the whole group. Its leaderboard has a grouped-action WES column. — [arXiv](https://arxiv.org/abs/2506.16042); [GitHub](https://github.com/WukLab/osworld-human)
- The Efficient GUI Agents survey [workshop paper, EMNLP 2026 GLM workshop] lists "verification-aware action control" and "hybrid runtimes" as recurring efficiency mechanisms. It warns that savings can be offset by new parser, verifier, retriever or orchestration costs. — [arXiv 2609.02309](https://arxiv.org/abs/2609.02309)

### Inferences
- An 11% speedup is modest compared with caching or replay (Q4). Hybrid actions mainly cut steps on tasks with available APIs or CLIs, so expect large gains for file and office operations and small gains for pure-GUI apps.

### Gaps
- I could not extract UltraCUA's absolute OSWorld numbers (7B/32B) or its step counts.
- I found no quantitative 2025-26 measurement of how much multi-action-per-step reduces latency (for example seconds saved per grouped call).

## Q4. Caching and replay of trajectories (PreAct, Skyvern, AWM, skill libraries)

### Takeaway
For repeated tasks, compiling a successful run into a program and replaying it without per-step LLM calls gives the largest speedups found: about 2-13x. The cost is a one-time verification overhead and brittleness when the UI changes, which needs screen-state checks and a fallback to the agent.

### Cited Findings
- PreAct (Bojie Li, Pine AI) [PREPRINT 2606.17929]: compiles successful runs into verified state-machine programs. Warm replays are 8.5-13x faster in wall-clock time with no per-step LLM calls. Before each action the replay checks that the screen matches the program's expectation and hands control back to the agent if it does not. Store-time verification (an independent evaluator re-runs the program from a clean state) is what makes repeated runs improve rather than degrade, worth 1.75-2.6 tasks per benchmark. Verification overhead is +162% wall time (Android) and +217% (OSWorld) on the first, cold run. — [arXiv abs](https://arxiv.org/abs/2606.17929); [arXiv HTML](https://arxiv.org/html/2606.17929); [GitHub 19PINE-AI/PreAct](https://github.com/19PINE-AI/PreAct); [Pith review](https://pith.science/paper/2606.17929)
- PreAct evaluation sets are small: the AndroidWorld "official-15" subset, the OSWorld "test_tiny" subset (6 tasks) and a 12-task WebArena shopping_admin subset. On WebArena the gate alone gives a warm-minus-cold success change of -4.0, versus -0.75 for the Muscle-Mem record-and-replay baseline, because the gate can empty the corpus. With a cache-miss fallback, PreAct is statistically indistinguishable from Muscle-Mem (p ≈ 0.84). Pith's third-party review says the gains rest on sparse experimental detail. — [arXiv HTML](https://arxiv.org/html/2606.17929); [Pith](https://pith.science/paper/2606.17929)
- Skyvern code caching [VENDOR]: records successful agent actions as Playwright code (`run_with="agent"`) and replays it (`run_with="code"`). Caching is per block. When the page changes it falls back to the agent and regenerates the code. — [Skyvern docs](https://www.skyvern.com/docs/developers/features/code-caching)
  - Product page: repeat runs "skip the LLM entirely — 3–5× faster, up to 70% cheaper". — [Skyvern products](https://www.skyvern.com/products)
  - Blog measurement on scraper tests: average run 278.95 s to 119.92 s (2.3x), cost $0.11 to $0.04 (2.7x). It admits that failure insights are not yet aggregated across runs. — [Skyvern blog](https://www.skyvern.com/blog/asking-ai-to-build-scrapers-should-be-easy-right/)
  - Competitors claim the cache breaks on UI changes and is keyed on a parameter hash. These are rival-vendor claims. — [anakin.io](https://anakin.io/blog/wire-vs-skyvern); [twin-browser](https://twin-browser.com/compare/skyvern)
- Agent Workflow Memory (Wang et al.) [PEER: ICML 2025]: induces reusable workflows from past trajectories. It reports +51.1% relative success on WebArena (and +7.9% over methods using human-written workflows), +24.6% relative step-wise success on Mind2Web, and fewer steps on successful WebArena tasks (not quantified in the extracts). Online AWM beats baselines by 8.9-14.0 absolute points as the train/test gap widens. — [PMLR v267](https://proceedings.mlr.press/v267/wang25bx.html); [arXiv 2409.07429](https://arxiv.org/html/2409.07429v1)
- Related 2026 work on executable or state-machine memory exists: "Executable Agentic Memory for GUI Agent" (2605.12294) and ActionEngine (reactive to programmatic GUI agents via state-machine memory). I extracted no numbers. — [arXiv 2605.12294](https://arxiv.org/html/2605.12294v1); [Pith citations](https://pith.science/citations/61064077-5867-4600-94ab-3bbad7c98787)

### Inferences
- Replay is the dominant speed lever for recurring personal-assistant tasks, an order of magnitude versus tens of percent for other methods. It needs (a) precondition and screen checks at each step, (b) a verified store, and (c) an agent fallback on cache misses. Without the fallback, success can drop (PreAct -4.0 points).
- Workflow memory such as AWM helps accuracy and step count but still calls the LLM every step, so its speedup is bounded by the steps it removes.

### Gaps
- PreAct's absolute replay seconds were not extracted. Its benchmarks are tiny (6 OSWorld tasks).
- I found no independent replication of the Skyvern or PreAct speedups.

## Q5. Prompt caching / KV cache, image downscaling, screen-diff, token reduction; local grounding inference speed

### Takeaway
Context and visual-token compression cuts compute by about 40-50% with equal or better accuracy. Provider prompt caching can cut time-to-first-token by about 80% on long, stable prefixes (text benchmark). Local parsers and grounders run in under a second per frame on datacenter GPUs.

### Cited Findings
- GUI-KV [listed as ICLR 2026 / OpenReview; arXiv 2510.00536]: training-free KV-cache compression. It combines spatial saliency (L2 norm of hidden states) with temporal redundancy, projecting earlier frames' keys onto the current frame's subspace to prune redundant history. Attention sparsity is above 0.99 across all layers, so it uses a uniform per-layer budget. It recovers near-full-cache accuracy at 10-20% budgets on six benchmarks. With 5 screenshots on AgentNetBench it cuts decoding FLOPs by 38.9% while raising step accuracy by +4.1%. No wall-clock numbers. — [arXiv 2510.00536](https://arxiv.org/abs/2510.00536); [OpenReview PDF](https://openreview.net/pdf/6c3fd69dc6cafc71cd3404963bfedce535aca8b4.pdf)
- ReVision (Microsoft Research et al.) [PREPRINT 2605.11212]: a learned patch selector drops temporally redundant screenshot patches (a learned "screen-diff"), and the model is trained on the filtered trajectories. With 5 history screenshots on Qwen2.5-VL-7B it uses 46% fewer tokens on average with +3% success across OSWorld, WebTailBench and AgentNetBench. With redundancy removed, more history keeps helping. — [arXiv 2605.11212](https://arxiv.org/abs/2605.11212); [MSR page](https://www.microsoft.com/en-us/research/publication/revision-scaling-computer-use-agents-via-temporal-visual-redundancy-reduction/)
- ST-Lite (2603.00188) is another spatio-trajectory KV compression method for GUI agents. No numbers were extracted. — [arXiv 2603.00188](https://arxiv.org/html/2603.00188)
- Anthropic prompt caching [VENDOR]: up to 85% latency reduction for long prompts. Example: 100K cached tokens give 2.4 s versus 11.5 s uncached (-79%). This is a text benchmark, not screenshots. — [Claude blog](https://claude.com/it/blog/prompt-caching); [SD Times](https://sdtimes.com/ai/anthropic-introduces-prompt-caching-to-reduce-latency-and-costs)
- Agent implementations: Cua marks messages `cache_control: ephemeral`. Magnitude keeps 1 screenshot plus screenshots already in a cache write, caches the system prompt with one breakpoint, and cycles the other 3 breakpoints over screenshots. It notes prompt caching is unavailable on Bedrock. — [Cua docs](https://cua.ai/docs/cua/guide/advanced/prompt-caching); [Magnitude docs](https://docs.magnitude.run/advanced/memory)
- OmniParser V2 [VENDOR, Microsoft model card]: 0.6 s/frame on an A100 and 0.8 s on a single RTX 4090, 60% lower latency than V1. It scores 39.6 on ScreenSpot-Pro with GPT-4o, versus 0.8 for GPT-4o alone. It pairs a YOLOv8 detector with a Florence-2 captioner. Hosted latency differs: Replicate quotes about 6 s on a T4. — [Azure AI Labs](https://labs.ai.azure.com/innovations/omniparserv2/); [HF mirror](https://huggingface.co/yuxin111222/OmniParser-v2.0); [Replicate](https://replicate.com/lucataco/omniparser-v2)

### Inferences
- In a stack with a cached prefix and pruned history, the growing-context effect from Q1 (about 3x per-step growth) should be largely flattened. Trained redundancy removal (ReVision) beats naive dropping because it also improves accuracy.

### Gaps
- I found no rigorous 2025-26 study of plain image downscaling versus accuracy and latency for CUAs. No prompt-caching benchmark specific to screenshots was found. Inference speeds for 7B grounders (UI-TARS-7B, GTA1-7B, Jedi-7B) in s/call were not found.

## Q6. Speculative / parallel execution

### Takeaway
Speculation gives about 20-50% latency reduction in general agent settings. For GUIs, a 2026 study shows that moving decoding off the critical path (pre-compiled policy trees prepared during idle time) lets agents hit sub-second event windows they otherwise miss.

### Cited Findings
- Speculative Actions (Ye et al.) [PEER: ICLR 2026]: a fast model predicts next actions that run in parallel and are committed only on a match. Up to 55% next-action accuracy (54.7% with 3 branches) gives up to 20% latency reduction (19.5% average) in games, e-commerce and web search, with a lossy extension for OS settings. The theoretical maximum is 50% when predictions are always correct. — [arXiv 2510.04371](https://arxiv.org/pdf/2510.04371); [mlanthology ICLR 2026](https://mlanthology.org/iclr/2026/ye2026iclr-speculative/)
- PASTE (pattern-aware speculative tool execution) [PREPRINT 2603.18897]: up to 48.5% lower average latency. Speculative jobs are scheduled so they never slow real calls. — [arXiv 2603.18897](https://arxiv.org/html/2603.18897v1)
- Other speculation work, no GUI numbers: speculative search-agent co-design (2511.20048), Dynamic Speculative Agent Planning (2509.01920), Speculate with Memory (2607.12236). — [2511.20048](https://arxiv.org/pdf/2511.20048); [2509.01920](https://arxiv.org/pdf/2509.01920); [2607.12236](https://arxiv.org/pdf/2607.12236)
- "Why Are GUI Agents Correct but Late?" (Dong et al.) [PREPRINT 2607.28399]: CUAs fail on transient GUI events because autoregressive decoding sits on the decision-time critical path. AAPT builds a bounded conditional policy tree during idle screen time, with guards, pre-authorized actions and deadlines. A lightweight observer matches change-gated frames to a branch and acts without generating text. Success in a contested window of about 600 ms rose from 0.50 to 0.79 (McNemar p = 1.8e-3) with no incorrect actions. Open-loop and predict-and-replan baselines scored 0. Replicated on a second MLLM over 126 paired trials. Branch routing is the bottleneck, and the method requires enumerable candidate actions. — [arXiv 2607.28399](https://arxiv.org/abs/2607.28399); [Pith](https://pith.science/paper/2607.28399)

### Inferences
- Speculation works best for predictable, reversible actions. With about 55% hit rates, wasted compute is significant, and irreversible GUI actions (send, delete, pay) cannot be executed speculatively. Pre-computing the decision (AAPT) is safer than pre-executing it.

### Gaps
- I found no end-to-end OSWorld wall-clock result for speculative execution in desktop CUAs.
