# Computer-Use Agents: Tools/Models Comparison (as of 2026-10-08)

Method note: direct page fetching was blocked by the sandbox egress proxy (os-world.github.io, llm-stats.com, leaderboard.steel.dev all refused), so every finding below comes from web-search result summaries of the cited pages. Treat exact decimals as "as reported by the cited page". **SR** = vendor self-reported; **IND** = independent / third-party run. Benchmark versions differ (OSWorld original, OSWorld-Verified, OSWorld 2.0, OSWorld 2.1) and **must not be compared across versions**. Step budgets (15/50/100/500) also change scores.

## 1. What are the current top OSWorld(-Verified) scores and human baseline, with dates?

### Takeaway
On OSWorld-Verified, frontier models are now well above the ~72% human baseline: the top listed score is Qwen3.8 Max at 86.1% (Alibaba, SR, Aug 2026), with Claude Mythos 5 / Fable 5 at 85.0% and Claude Mythos Preview at 85.4% (Anthropic system-card, SR, revised harness). Since OSWorld-Verified is close to saturated, the harder benchmark is now **OSWorld 2.0** (long-horizon tasks, June 2026). There the best agent finishes only about 20–31% of tasks under binary scoring.

### Cited Findings
**Human baseline**
- OSWorld human baseline is 72.36%. Simular's press material cites it ([Simular Agent S3](https://www.simular.ai/articles/agent-s3); [Simular "outperforms humans"](https://www.simular.ai/articles/simulars-computer-use-agent-outperforms-humans)). Secondary sources round it to 72.4% ([Wikipedia GPT-5.4](https://en.wikipedia.org/wiki/GPT-5.4)). I could not open the OSWorld paper itself to confirm.
- OSWorld 2.0 gives no human success rate. It reports that the median skilled human needs about 1.6 h of active work per task, roughly 48x the OSWorld 1.0 median, and that 69.6% of tasks take a skilled human more than 1 h ([arXiv 2606.29537](https://arxiv.org/abs/2606.29537)).

**OSWorld-Verified top scores (late Sep / early Oct 2026, aggregator boards)**
- Qwen3.8 Max (Alibaba): 86.1%, the top entry, dated Aug 2026 and SR by Alibaba ([Steel leaderboard](https://leaderboard.steel.dev/leaderboards/osworld/); [BenchLM, Sep 29 2026](https://benchlm.ai/benchmarks/osworld-verified); [LLM Stats](https://llm-stats.com/benchmarks/osworld-verified)).
- Claude Mythos Preview 85.4%; Claude Mythos 5 85.0%; Claude Fable 5 85.0%. All are SR in the Claude 5 system card on a "revised harness" that is not directly comparable to older entries ([Steel leaderboard](https://leaderboard.steel.dev/leaderboards/osworld/); [BenchLM](https://benchlm.ai/benchmarks/osworld-verified)).
- Qwen3.8-27B (open-weight) 84.3%, rank #3 on LLM Stats and the top open-source entry ([LLM Stats](https://llm-stats.com/benchmarks/osworld-verified)).
- Claude Opus 4.8: 83.4% pass@1 on the revised harness, SR in the Opus 4.8 system card (May 2026) ([Steel leaderboard](https://leaderboard.steel.dev/leaderboards/osworld/)).
- OpenAI GPT-5.5: 78.7% OSWorld-Verified, SR in OpenAI's launch post, used with Codex computer-use skills ([OpenAI, Introducing GPT-5.5](https://openai.com/index/introducing-gpt-5-5/)). Vellum's table puts Claude Opus 4.7 at 78.0% and GPT-5.4 at 75.0% ([Vellum](https://www.vellum.ai/blog/everything-you-need-to-know-about-gpt-5-5)).
- Gemini 3.5 Flash (computer use built-in, June 24 2026): 78.4% OSWorld-Verified, reported by a third party as SR. I did not find this number in Google's own post ([Digital Applied](https://www.digitalapplied.com/blog/gemini-3-5-flash-computer-use-agent-automation-2026); [Google blog](https://blog.google/innovation-and-ai/models-and-research/gemini-models/introducing-computer-use-gemini-3-5-flash/)).
- GPT-5.4: 75.0% OSWorld-Verified (OpenAI, March 2026), above the 72.4% human figure. GPT-5.2 scored 47.3% ([Wikipedia GPT-5.4](https://en.wikipedia.org/wiki/GPT-5.4)).
- Claude Sonnet 4.6 72.5% and Opus 4.6 72.7% (Feb 2026). Both are now superseded and were cited via a vendor blog ([Coasty blog, low reliability](https://coasty.ai/blog/osworld-benchmark-2026-ai-computer-use-agents-ranked)).
- The first agentic framework to claim human-level performance was Simular Agent S3 + Behavior Best-of-N, at 72.6% (Dec 2025, SR). This uses best-of-N selection over several rollouts, not a single run ([Simular](https://www.simular.ai/articles/agent-s3); [arXiv 2510.02250](https://arxiv.org/pdf/2510.02250)).
- Low-reliability claim: Coasty, a vendor, claims 82–85.6% in its own blogs. Its numbers are internally inconsistent and do not appear on independent boards ([Coasty](https://coasty.ai/blog/osworld-benchmark-2026-computer-use-results)).

**OSWorld 2.0 / 2.1 (harder successor, released June 2026)**
- The paper (arXiv 2606.29537) was submitted 28 Jun 2026 and revised 13 Jul 2026. Best result: Claude Opus 4.8 at max thinking completes 20.6% of tasks (binary), with 54.8% partial score, at 500 steps. GPT-5.5 plateaus near 14% ([arXiv abs](https://arxiv.org/abs/2606.29537)). Other summaries give 18.5% binary / 49.3% partial ([Emergent Mind](https://www.emergentmind.com/papers/2606.29537)), which suggests a version or metric difference.
- Binary completion drops to zero for every model on tasks longer than 163 minutes. In the 137–163 min bin, no model exceeds 10% ([arXiv 2606.29537](https://arxiv.org/abs/2606.29537)).
- OSWorld-Verified 2.0 board: Claude Opus 5 leads at 31.4% as of 7 Oct 2026 ([BenchLeader](https://www.benchleader.com/benchmarks/osworld_2)). OSWorld 2.1 partial-score metric: Claude Opus 5.5 leads at 81.8 ([LLM Learner OSWorld 2.1](https://llmlearner.com/rankings/osworld-2-1)). A third-party table lists Opus 5.5 at 81.8% and Opus 5 at 74.0% on OSWorld 2.0 (partial) ([GitHub PR citing Opus 5.5 system card](https://github.com/JacobBrooke95/General/pull/46)). The Claude Opus 5 system card is dated July 24, 2026 ([Anthropic](https://www-cdn.anthropic.com/c5fbac3f0b1280a933ebd26d3cb8bb9f5bdeaf48/Claude%20Opus%205%20System%20Card.pdf)) and the Opus 5.5 system card September 22, 2026 ([Anthropic](https://www-cdn.anthropic.com/fc1b44717c85dc068bc6ba5024219938094694bd/Claude%20Opus%205.5%20System%20Card.pdf)).
- "GPT-6 Astra" is reported at 72.6% on OSWorld 2.0 by a blog ([Miraflow](https://miraflow.ai/blog/osworld-2-explained-computer-use-agent-benchmark-2026)). The source is unverified, and the metric (partial or binary) is unclear.

**Historical anchors (dated)**
- OpenAI CUA/Operator: 38.1% OSWorld at launch (Jan 2025) ([localaimaster, secondary](https://localaimaster.com/blog/ui-tars-desktop-automation)).
- UI-TARS-1.5: 42.5% OSWorld at 100 steps (Apr 2025, SR). In the same table, OpenAI CUA scored 36.4 and Claude 3.7 scored 28 ([UI-TARS GitHub](https://github.com/bytedance/ui-tars); [Seed blog](https://seed.bytedance.com/en/blog/bytedance-seed-agent-model-ui-tars-1-5-open-source-achieving-sota-performance-in-various-benchmarks)).
- UI-TARS-2: 47.5% OSWorld (Sep 2025, SR) ([arXiv 2509.02544](https://arxiv.org/abs/2509.02544)).

### Inferences
- OSWorld-Verified is effectively saturated at the frontier: at least 6 models score 78–86%, above the 72.36% human figure. The useful discriminator in late 2026 is OSWorld 2.0/2.1 (long-horizon). There even the best models complete roughly 20–31% of tasks binary.
- Almost every frontier number is SR, often on vendor-modified harnesses (Anthropic's "revised harness"). I found no independent re-run of the 85%+ claims.

### Gaps
- I could not load the official os-world.github.io leaderboard, which was blocked, so I could not confirm the "verified" status, step limits or exact dates of entries.
- I found no OSWorld-Verified number for Claude Opus 5 / 5.5 specifically; Anthropic appears to report OSWorld 2.0 for these models.
- I found no confirmed OSWorld-Verified score for "ChatGPT agent" as a product, as distinct from the underlying GPT model.
- "Qwen3.8", "Claude Mythos/Fable 5" and "GPT-6 Astra" appear only on aggregator boards in my results. I did not see the primary vendor posts.

## 2. Per-tool comparison table and details

### Takeaway
The field has split into three layers:
1. **Frontier closed models with a built-in computer-use tool** (Claude, GPT-5.x/Codex, Gemini 3.5 Flash). These are cloud-only and screenshot-based, and they have the highest scores.
2. **Open-weight GUI models** (UI-TARS-2, Qwen3.8-27B, EvoCUA, OpenCUA, Fara1.5). These can run locally and score lower, apart from Qwen3.8-27B, which is reported near the frontier.
3. **Agent frameworks/harnesses** (Agent S3, browser-use, Stagehand, Skyvern, AskUI, Open Interpreter, UI-TARS Desktop). These wrap any of the models above.

### Cited Findings

| Tool | Open/closed | Local/cloud | Input modality | OS / scope | Key published accuracy (date, SR/IND) | Latency |
|---|---|---|---|---|---|---|
| Anthropic Claude computer use (Opus 4.8 / Opus 5 / 5.5, Mythos/Fable 5) | Closed | Cloud API | Screenshot + mouse/keyboard actions | Any OS via the harness | OSWorld-Verified 83.4% (Opus 4.8, May 2026, SR, revised harness); 85.0–85.4% (Claude 5 family, SR); OSWorld 2.0 20.6% binary (Opus 4.8, paper authors); ScreenSpot-Pro 87.9% (Opus 4.8) | n/a found |
| OpenAI CUA → Operator → ChatGPT agent / GPT-5.4 / GPT-5.5 + Codex | Closed | Cloud | Screenshot (CUA) | Browser (Operator); desktop via Codex | CUA 38.1% OSWorld (Jan 2025); GPT-5.4 75.0% OSWorld-V (Mar 2026); GPT-5.5 78.7% OSWorld-V (SR); ScreenSpot-Pro: GPT-5.4 85.4%, GPT-6 Astra 92.7% | n/a found |
| Google Gemini 2.5 Computer Use → Gemini 3.5 Flash computer use; Project Mariner | Closed | Cloud | Screenshot | 2.5: browser-focused; 3.5 Flash: browser + mobile + desktop | Gemini 2.5 CU: Online-Mind2Web 70%+ (Google, Browserbase harness), 65.7% / 69.0% elsewhere; 3.5 Flash 78.4% OSWorld-V (third-party-cited SR) | Gemini 2.5 CU about 225 s per task on Online-Mind2Web (Google chart) |
| ByteDance UI-TARS-1.5 / UI-TARS-2 / UI-TARS Desktop | Open weights (1.5-7B on HF; UI-TARS-2 reportedly on GH/HF) | Local (vLLM/Ollama) or cloud | Pure screenshot (native VLM agent) | Win/macOS desktop + browser (Desktop app) | 1.5: OSWorld 42.5 (100 steps), 7B 27.5; ScreenSpot-Pro 61.6 (7B 49.6) (Apr 2025, SR); UI-TARS-2: OSWorld 47.5 (Sep 2025, SR) | n/a |
| Microsoft OmniParser (V2) | Open (screen parser, not an agent) | Local | Screenshot → structured elements, which then feed any LLM | Any | Evaluated on ScreenSpot-Pro per its authors; no score found | n/a |
| Microsoft Fara-7B / Fara1.5 (4B/9B/27B) | Open weights (HF, Azure Foundry) | Local, on-device | Screenshot only; predicts coordinates | Web/browser tasks | Fara-7B: WebVoyager 73.5%, Online-Mind2Web 34.1%, DeepShop 26.2%, WebTailBench 38.4% (Nov 2025, SR); Fara1.5-9B +29.3 Online-Mind2Web, +13.1 WebVoyager vs Fara-7B (SR) | about 16.5 actions/task; about $0.025/task |
| Simular Agent S / S2 / S3 | Open framework (GitHub) | Framework runs locally; uses cloud or local models | Screenshot-based, with a grounding model | Linux/Win/macOS | S3: 62.6% → 69.9% with bBoN (Oct 2025), 72.6% with bBoN (Dec 2025), above human (SR) | n/a |
| Skyvern | Open source + cloud | Self-host or cloud | Vision LLM + DOM; planner/validator | Browser | WebVoyager 85.85% (Skyvern 2.0, SR); Web Bench 64.4% (co-published by Skyvern) | n/a |
| browser-use | Open source (+ cloud) | Local, or any LLM | DOM / accessibility extraction + optional vision | Browser (Chromium) | WebVoyager 89.1% (586 tasks, SR); 88.3% in a third-party same-model eval | n/a |
| Stagehand (Browserbase) | Open source SDK | Local or Browserbase cloud | DOM/a11y + LLM ("act/extract/observe") | Browser | No published WebVoyager score; v3 claims 44% faster than prior versions | Speed claim only |
| Open Interpreter (OS mode) | Open source | Local; model is any LLM | Screenshot via multimodal LLM | Desktop; no multi-display | No benchmark found; docs call OS mode "highly experimental" | n/a |
| AskUI Vision Agent | Open-source SDK + commercial | Local controller; model is AskUI API / Claude / self-hosted UI-TARS | Screenshot/vision | Desktop, Android, HMI | AndroidWorld 94.8% pass@1, 2nd place (SR, with Claude Sonnet 4.5); claims OSWorld 66.2 (Nov 2025, SR) | n/a |
| Meituan EvoCUA (2026 entrant) | Open weights | Local | Screenshot (Qwen3-VL based) | Desktop (OSWorld, WindowsAgentArena) | EvoCUA-32B 56.7% OSWorld; 8B 46.1% (Jan 2026, SR); 56.48% WindowsAgentArena (Mar 2026, SR) | n/a |
| xlang OpenCUA (7B/32B/72B) | Fully open (data + infra + weights) | Local (vLLM supported from 2026-01-17) | Screenshot | Desktop | OpenCUA-72B 45.0% OSWorld-Verified; 32B 34.8%; 7B 26.6% (100 steps) | n/a |
| Alibaba Qwen3.8 (Max / 27B) (2026 entrant) | Max: reported as "open weight" by BenchLM; 27B open | 27B can run locally | Screenshot (VLM) | Desktop | Max 86.1% OSWorld-V (Aug 2026, SR); 27B 84.3%; ScreenSpot-Pro (Max) 84.5% | n/a |

Sources by row:
- Claude: [Steel](https://leaderboard.steel.dev/leaderboards/osworld/); [arXiv 2606.29537](https://arxiv.org/abs/2606.29537); [BenchLM ScreenSpot-Pro](https://benchlm.ai/benchmarks/screenspot-pro). The Opus 5 card notes the largest gains in prompt-injection robustness across computer use and browser use ([Opus 5 system card](https://www-cdn.anthropic.com/c5fbac3f0b1280a933ebd26d3cb8bb9f5bdeaf48/Claude%20Opus%205%20System%20Card.pdf)).
- OpenAI: [OpenAI GPT-5.5](https://openai.com/index/introducing-gpt-5-5/); [Wikipedia GPT-5.4](https://en.wikipedia.org/wiki/GPT-5.4); [BenchLM ScreenSpot-Pro](https://benchlm.ai/benchmarks/screenspot-pro).
- Google: [Google blog, Gemini 2.5 CU](https://blog.google/innovation-and-ai/models-and-research/google-deepmind/gemini-computer-use-model/); [GigaNectar](https://giganectar.com/gemini-2-5-computer-use-public-preview-70-accuracy-225s-latency/); [Google blog, 3.5 Flash CU](https://blog.google/innovation-and-ai/models-and-research/gemini-models/introducing-computer-use-gemini-3-5-flash/); [Digital Applied](https://www.digitalapplied.com/blog/gemini-3-5-flash-computer-use-agent-automation-2026). 3.5 Flash added "simplified actions with intents, built-in support for browser, mobile, and desktop, configurable safety policies, prompt-injection detection" (June 24 2026). **Project Mariner was shut down on May 4, 2026**, and its technology was folded into Gemini Agent and Chrome Auto Browse ([Android Headlines](https://www.androidheadlines.com/2026/05/google-shuts-down-project-mariner-ai-agent.html); [Wikipedia](https://en.wikipedia.org/wiki/Project_Mariner)).
- UI-TARS: [UI-TARS GitHub](https://github.com/bytedance/ui-tars); [HF UI-TARS-1.5-7B](https://huggingface.co/ByteDance-Seed/UI-TARS-1.5-7B); [arXiv 2509.02544](https://arxiv.org/abs/2509.02544); [UI-TARS-desktop GitHub](https://github.com/bytedance/UI-TARS-desktop). A reproduction issue reports about 40 vs the claimed 48 on ScreenSpot-Pro for a 7B variant ([GH issue #215](https://github.com/bytedance/UI-TARS/issues/215)).
- OmniParser: [ScreenSpot-Pro repo](https://github.com/likaixin2000/ScreenSpot-Pro-GUI-Grounding).
- Fara: [MSR paper](https://www.microsoft.com/en-us/research/publication/fara-7b-an-efficient-agentic-model-for-computer-use/); [microsoft/fara GitHub](https://github.com/microsoft/fara); [MarkTechPost](https://www.marktechpost.com/2025/11/24/microsoft-ai-releases-fara-7b-an-efficient-agentic-model-for-computer-use/).
- Agent S: [Simular](https://www.simular.ai/articles/agent-s3); [GitHub](https://github.com/simular-ai/agent-s); [arXiv 2510.02250](https://arxiv.org/pdf/2510.02250).
- Skyvern, browser-use and Stagehand: [TinyFish](https://www.tinyfish.ai/blog/ai-browser-agents); [ByteTunnels](https://bytetunnels.com/posts/browser-agent-frameworks-compared-browser-use-vs-stagehand-vs-skyvern/); [Firecrawl](https://www.firecrawl.dev/blog/best-browser-agents).
- Open Interpreter: [OS mode docs](https://docs.openinterpreter.com/guides/os-mode); [openinterpreter.com](https://www.openinterpreter.com/). The homepage now presents the product as a terminal coding agent, so OS mode may be legacy.
- AskUI: [AskUI benchmarks](https://www.askui.com/benchmarks); [AskUI AndroidWorld post](https://www.askui.com/blog-posts/askui-vision-agent-achieves-2nd-place-on-androidworld-benchmark/index.html); [DeepWiki](https://deepwiki.com/askui/vision-agent).
- EvoCUA: [EvoCUA GitHub](https://github.com/meituan/EvoCUA).
- OpenCUA: [OpenCUA GitHub](https://github.com/xlang-ai/OpenCUA); [arXiv 2508.09123](https://arxiv.org/html/2508.09123v3).
- Qwen3.8: [LLM Stats](https://llm-stats.com/benchmarks/osworld-verified); [BenchLM](https://benchlm.ai/benchmarks/screenspot-pro).

**ScreenSpot-Pro (GUI grounding) context**
- Leaders as of October 2026: GPT-6 Astra 92.7%, Claude Opus 4.8 87.9%, GPT-5.4 85.4%, Qwen3.8 Max 84.5% (BenchLM, verified Oct 2, 2026) ([BenchLM](https://benchlm.ai/benchmarks/screenspot-pro)).
- When the benchmark was released (2025), the best model scored 18.9% ([ScreenSpot-Pro paper](https://arxiv.org/html/2504.07981v1)).

**WebVoyager caveat**: this benchmark is saturated, and every 85%+ claim on it is SR. Magnitude reports 93.9%, Browserable 90.4% and OpenAI CUA 87% ([TinyFish](https://www.tinyfish.ai/blog/ai-browser-agents)).

**Latency**
- The OSWorld-Human study (MLSys 2026) finds that LLM calls for planning, reflection and judging take more than 90% of end-to-end latency, while screenshots and clicks add little.
- Later steps can take 3x longer than early ones as the context grows.
- Example: an agent took 12 minutes to double-space two paragraphs, a task a human does in under 30 s.
- The best agents take 1.4–2.7x (arXiv v1) or 2.7–4.3x (MLSys version) more steps than necessary.
- Sources: [arXiv 2506.16042](https://arxiv.org/pdf/2506.16042); [MLSys paper](https://proceedings.mlsys.org/paper_files/paper/2026/file/5edb57c05c81d04beb716ef1d542fe9e-Paper-Conference.pdf).

### Inferences
- Modality trend: native screenshot VLMs (Claude, OpenAI, Gemini, UI-TARS, Fara, EvoCUA, OpenCUA, Qwen) now dominate full-desktop control. DOM/a11y-tree approaches (browser-use, Stagehand, Skyvern hybrid) remain the cheaper and more reliable choice for browser-only work.
- Per-task wall-clock time for screenshot agents is on the order of minutes, not seconds. Gemini 2.5 CU took about 225 s per task, and the OSWorld-Human example took 12 min. This is the main obstacle to a fast "Jarvis".

### Gaps
- I found no per-step latency in seconds for Claude, GPT-5.5 or Gemini 3.5 Flash computer use, and no 2026 independent latency benchmark.
- I found no OmniParser V2 ScreenSpot-Pro score, and no 2026 OmniParser update.
- I found no OSWorld or WebArena numbers for browser-use, Stagehand or Skyvern, and no WebArena numbers in 2026 for any tool.
- Fara1.5 absolute scores were not visible; only deltas versus Fara-7B were.
- I could not confirm whether the OpenAI Operator brand still exists separately or what the ChatGPT agent product's current benchmark numbers are.
- For UI-TARS-2, the open-weight release scope and license were not verified from primary sources (third parties claim Apache 2.0).

## 3. Which tools can run fully locally, and on what hardware? Which are practical for a personal "Jarvis"-style desktop assistant today?

### Takeaway
Several stacks run fully locally:
- UI-TARS-1.5-7B / UI-TARS Desktop
- Fara-7B / Fara1.5
- OpenCUA-7B/32B
- EvoCUA-8B/32B
- Qwen3.8-27B
- Agent S3 or Open Interpreter paired with a local model

An 7–9B model is the realistic size for a single consumer GPU. The quoted figure is about 16 GB VRAM for UI-TARS 7B (unverified third-party). Local models are weaker than the frontier (27–57% on original OSWorld), apart from Qwen3.8-27B, which is reported near the frontier. For a practical Jarvis today, the best accuracy comes from a cloud frontier model with a local harness. The best speed and privacy come from a hybrid: API/DOM/a11y tools for known apps and a screenshot model only as a fallback.

### Cited Findings
- **Fara-7B** is designed for on-device use: screenshots only, coordinate prediction, "small enough to run on-device", weights on HF/Azure Foundry ([MSR](https://www.microsoft.com/en-us/research/publication/fara-7b-an-efficient-agentic-model-for-computer-use/); [TechRepublic](https://www.techrepublic.com/article/news-microsoft-fara-7b-launch/)). The successor Fara1.5 comes in 4B/9B/27B and is built on Qwen3.5 ([GitHub](https://github.com/microsoft/fara)).
- **UI-TARS Desktop** is described as a "native GUI agent for your local computer". It can run fully locally by deploying the model with vLLM/Ollama, or use cloud and remote operators (v0.2.0, June 2025) ([UI-TARS-desktop GitHub](https://github.com/bytedance/UI-TARS-desktop); [localaimaster](https://localaimaster.com/blog/ui-tars-desktop-automation)).
- **UI-TARS hardware**: the 7B variant reportedly needs about 16 GB VRAM, and quantized versions fit in 4–8 GB, reportedly dropping OSWorld from 47.5% to about 44%. These are unverified third-party claims ([localaimaster](https://localaimaster.com/blog/ui-tars-desktop-automation)).
- **OpenCUA**: vLLM supports the 7B/32B/72B models (2026-01-17). The 7B is listed as 8B params in BF16 ([OpenCUA GitHub](https://github.com/xlang-ai/OpenCUA); [HF](https://huggingface.co/xlangai/OpenCUA-7B)).
- **EvoCUA-8B** scores 46.1% OSWorld, claimed competitive with 72B-class models (SR, Jan 2026) ([EvoCUA GitHub](https://github.com/meituan/EvoCUA)).
- **Qwen3.8-27B** scores 84.3% OSWorld-Verified and is the top open-source model on LLM Stats ([LLM Stats](https://llm-stats.com/benchmarks/osworld-verified)).
- **AskUI** can use a self-hosted UI-TARS backend, and its OS controller is installed on the host ([DeepWiki AskUI](https://deepwiki.com/askui/vision-agent)).
- **Open Interpreter `--os`** needs a screen-recording permission and does not support multiple displays ([OI docs](https://docs.openinterpreter.com/guides/os-mode)).
- **Cost**: Fara-7B costs about $0.025/task vs about $0.30/task for SoM agents built on GPT-5/o3 (estimate) ([MarkTechPost](https://www.marktechpost.com/2025/11/24/microsoft-ai-releases-fara-7b-an-efficient-agentic-model-for-computer-use/)).
- **Gemini 3.5 Flash** computer use is positioned at about one-third of GPT-5.5's cost, per a third party ([Digital Applied](https://www.digitalapplied.com/blog/gemini-3-5-flash-computer-use-agent-automation-2026)).
- **Speed bottleneck**: more than 90% of latency is LLM planning, reflection and judging calls ([OSWorld-Human](https://arxiv.org/pdf/2506.16042)).
- **Safety**: Gemini 3.5 Flash adds configurable safety policies and prompt-injection detection ([Google](https://blog.google/innovation-and-ai/models-and-research/gemini-models/introducing-computer-use-gemini-3-5-flash/)). Claude Opus 5 reports gains in prompt-injection robustness for computer and browser use ([system card](https://www-cdn.anthropic.com/c5fbac3f0b1280a933ebd26d3cb8bb9f5bdeaf48/Claude%20Opus%205%20System%20Card.pdf)).
- **Long tasks remain unreliable**: on OSWorld 2.0, the best completion is about 20% on tasks lasting 1h+ ([arXiv 2606.29537](https://arxiv.org/abs/2606.29537)).

### Inferences
Practical Jarvis options, ranked:
1. **Highest capability (cloud)**: Claude (Opus 5.x / Sonnet tier) computer-use tool or GPT-5.5 with Codex computer use, driven by a local harness (Agent S3, AskUI, or a custom loop). Gemini 3.5 Flash is the cheaper and probably faster cloud option and also covers mobile and desktop.
2. **Fully local / private**: UI-TARS Desktop with UI-TARS-1.5-7B or 2, or Fara1.5-9B for web. Qwen3.8-27B is the highest-scoring local option if it fits in VRAM: a 27B model likely needs a 24 GB+ GPU when quantized (my estimate, unsourced).
3. **Browser-only tasks**: browser-use or Stagehand (DOM/a11y). These are faster and cheaper than pixel agents and can use local LLMs.

Design implications:
- For "fast like Jarvis", avoid screenshot loops where possible. Route to app APIs, CLI tools or the accessibility tree first, use a small local VLM for grounding (UI-TARS/Fara/OmniParser), and keep a frontier model only as the planner. This follows from the OSWorld-Human finding that planner LLM calls dominate latency.
- Expect minute-scale tasks and roughly 80% or lower reliability on multi-step desktop work. Keep a human confirmation step for risky actions.

### Gaps
- I found no primary-source VRAM or hardware requirements for UI-TARS-2, Fara1.5, EvoCUA, OpenCUA or Qwen3.8-27B. The 16 GB figure for 7B is third-party.
- I found no benchmark of local-model latency per step on consumer hardware (for example an RTX 4090 or Apple Silicon).
- macOS/Windows support details for each frontier vendor's desktop agent products (e.g., Claude desktop computer use, the ChatGPT agent desktop app, Gemini Desktop computer use, which [TestingCatalog](https://www.testingcatalog.com/google-tests-computer-use-on-gemini-desktop/) reports Google is testing) were not confirmed from primary sources.
