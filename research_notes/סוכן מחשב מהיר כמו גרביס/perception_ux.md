# Perception/Control Layer and UX Layer for a Fast "Jarvis"-like Desktop Agent

Research date: 2026-10-08. Note: WebFetch could not resolve most domains in this session (DNS failure), so findings come from search-result extracts of the primary sources (arXiv, official docs). Figures should be spot-checked against the originals before you publish them.

## 1. Accessibility APIs per OS and browser DOM/CDP: speed, coverage, gaps, hybrid accessibility+vision

### Takeaway
Accessibility trees (Windows UIA, macOS AX, Linux AT-SPI, browser a11y/DOM) are the cheapest and most precise perception channel, but their coverage is uneven. On macOS only about a third of apps expose complete AX trees. Electron, Chromium and custom-drawn controls often expose sparse trees or none. Measured results consistently favor hybrid a11y+vision: UIA markers improved pixel-only detectors by 15-57% relative (Navi/WAA), and UFO2 fuses UIA with vision parsing.

### Cited Findings
- **macOS AX coverage gap:** Screen2AX (MacPaw / Ukrainian Catholic University, arXiv 2507.16704, July 2025) found that only about 33% of macOS apps offer full accessibility support. Many apps remain largely inaccessible because developer-provided metadata is incomplete or missing. — [HF Papers: Screen2AX](https://huggingface.co/papers/2507.16704)
- Screen2AX generates real-time, tree-structured AX metadata from a single screenshot using VLM and object-detection models. arXiv v1 reports 77% F1 for tree reconstruction across 112 apps. MacPaw's later page reports 302 apps, 79% F1, and 2.2x better grounding accuracy than native AX metadata. The numbers differ between versions. — [MacPaw Research](https://research.macpaw.com/publications/screen2axvisionbasedapproachautomatic); [arXiv PDF](https://web3.arxiv.org/pdf/2507.16704)
- **Windows UIA + vision (Navi / Windows Agent Arena, Microsoft, Sept 2024, NeurIPS 2024):** Navi's best configuration reached 19.5% success on WAA (humans: 74.5%). It used Set-of-Marks prompting with the UIA tree plus pixel-based element detectors. Adding UIA markers to pixel detectors raised performance by 57% (OmniParser), 52% (open-source models) and 15% (proprietary pixel models), all relative, using GPT-4V-1106. OmniParser combined with UIA was the strongest pairing. WAA has more than 150 Windows tasks and can run in parallel on Azure in about 20 minutes. — [arXiv 2409.08264](https://arxiv.org/html/2409.08264v1); [MSR page](https://www.microsoft.com/en-us/research/?p=1140568)
- **UFO2 (Microsoft, arXiv 2504.14603, April 2025):** UFO2 uses a hybrid control-detection pipeline that fuses Windows UIA with vision-based parsing, so the agent can handle both standard and custom UI components. — [arXiv 2504.14603](https://arxiv.org/html/2504.14603v2); [MSR publication](https://www.microsoft.com/en-us/research/publication/ufo2-the-desktop-agentos/)
- **UIA gaps:** Microsoft's docs say custom controls are accessible only if the developer implements UIA providers for them. Without providers, a UIA client may see nothing inside the control. — [MS Learn: UI Automation Providers Overview](https://learn.microsoft.com/en-us/windows/win32/WinAuto/uiauto-providersoverview)
- Electron disables accessibility by default for performance reasons and enables it automatically when assistive tech is detected. `app.setAccessibilitySupportEnabled` can force it on. — [Electron accessibility docs](https://www.electronjs.org/de/docs/latest/tutorial/barrierefreiheit)
- The pywinauto docs say Chrome needs `--force-renderer-accessibility` before it exposes its tree. The pywinauto UIA backend does not support custom properties and controls because of comtypes limits. — [pywinauto Getting Started](https://pywinauto.readthedocs.io/en/latest/getting_started.html)
- Chromium's native UIA provider is still described as "under development" and is enabled via `--enable-features=UiaProvider`. — [Chromium UIA doc](https://chromium.googlesource.com/chromium/src/+/HEAD/docs/accessibility/browser/uiautomation.md)
- pywinauto maintainers report that UIA calls can hang (for example `invoke()`) and COM errors can come from UIAutomationCore.dll. Users run calls in a separate thread with a timeout as a workaround. — [Lightrun/pywinauto issue](https://lightrun.com/answers/pywinauto-pywinauto-problem-printing-element)
- **Linux AT-SPI:** AT-SPI is the screen-reader accessibility bus. It finds elements by name and role, without synthetic input. One open-source Linux desktop MCP orders its backends as AT-SPI first, then ydotool, then xdotool. — [linux-desktop-mcp input_backends](https://glama.ai/mcp/servers/@BeckhamLabsLLC/linux-desktop-mcp/blob/1aad34cc6ae1aa216ae4382f8df8902268ec5a9f/src/linux_desktop_mcp/input_backends.py)
- **Browser (Playwright MCP):** The official Playwright MCP returns accessibility snapshots instead of screenshots. Each interactive element gets a ref the model acts on. A vision mode (`--caps=vision`) covers canvas, games and poorly marked-up pages. — [playwright.dev MCP snapshots](https://playwright.dev/mcp/snapshots)
- Token-cost claims for a snapshot versus a screenshot vary widely across blogs: about 120 vs 1,500 tokens (roughly 12x), or 500-5,000 vs 10,000-50,000. None of these are controlled benchmarks. One source measured Playwright MCP at 114K tokens per task against 27K for Playwright CLI. — [dev.to](https://dev.to/pointchecknote/browser-automation-with-claude-playwright-mcp-why-accessibility-snapshots-beat-screenshots-2pke); [morphllm](https://morphllm.com/playwright-mcp)

### Inferences
- A Jarvis agent should treat the a11y tree as its primary "fast eyes". It should fall back to a vision parser (OmniParser or a Screen2AX-like model) only when the tree is sparse. The fallback trigger can be few nodes or nodes without names or bounds.
- On macOS, expect vision fallback in most third-party apps, given the roughly one-third full-coverage figure. For Electron apps on any OS, force accessibility on.
- Text snapshots are probably faster per step mostly because they put less into context. This is an inference; I found no end-to-end latency benchmarks.

### Gaps
- I found no published latency benchmarks (ms per tree walk) for UIA, AX or AT-SPI. Tree size and cross-process cost are known qualitatively but not measured in the sources I found.
- I found no primary data on macOS AX API performance or on TCC permission friction beyond Screen2AX's coverage figures.
- I could not open the UFO2 ablation table that isolates UIA-only versus UIA+vision detection.

## 2. Direct integrations instead of GUI: API-first, GUI-fallback

### Takeaway
Evidence consistently favors hybrid action spaces: GUI actions plus programmatic tool or API calls. UFO2's API-equipped AppAgents roughly doubled Operator's success rate on OSWorld-W. UltraCUA's hybrid actions gave about 22% relative improvement with 11% fewer steps. API actions in UFO2 added about 8% success.

### Cited Findings
- **UFO2 architecture:** A HostAgent decomposes tasks and app-specialized AppAgents work in each application. The AppAgents have native APIs, domain knowledge, and a unified GUI-API action layer. UFO2 was evaluated on more than 20 real Windows apps. — [arXiv 2504.14603](https://arxiv.org/html/2504.14603v2)
- **UFO2 results:** Full UFO2 scored 27.9% on WAA and 28.6% on OSWorld-W with GPT-4o, and 30.5% / 32.7% with o1. The base variant (UIA + GUI actions only, GPT-4o) scored 23.4% WAA and 16.3% OSWorld-W. OpenAI Operator scored 20.8% WAA and 14.3% OSWorld-W. The paper says this is roughly double on OSWorld-W. The samples are small (154 WAA tasks, 49 OSWorld-W tasks). — [arXiv HTML](https://arxiv.org/html/2504.14603); [Lacuna summary](https://lacuna.tiptreesystems.com/work/ufo2-the-desktop-agentos/wrk_9b36074fe0e448cf330ad8a31df30040); [AlphaXiv](https://www.alphaxiv.org/abs/2504.14603)
- **UFO2 API gains:** The paper text reports API integration raising completion rate by more than 8%. A secondary summary claims a 6-8% gain and up to about 60% fewer steps in some scenarios; the 60% figure is unverified. — [arXiv PDF](https://arxiv.org/pdf/2504.14603); [Lacuna](https://lacuna.tiptreesystems.com/work/ufo2-the-desktop-agentos/wrk_9b36074fe0e448cf330ad8a31df30040)
- **UFO2 speculative multi-action:** One inference proposes several likely next actions. Each is validated against UIA at runtime, and the agent re-plans if validation fails. Reported savings are up to 51.5% fewer LLM inference steps on OSWorld-W and up to about 10% fewer completion steps on WAA, with comparable success rates. — [AlphaXiv](https://www.alphaxiv.org/abs/2504.14603); [Dataconomy](https://dataconomy.com/2025/04/22/ufo2-turns-your-desktop-into-an-agent-playground/)
- **UltraCUA (Apple + HKU, arXiv 2510.17790, Oct 2025):** UltraCUA is a foundation model that mixes GUI primitives with programmatic tool calls ("hybrid action"). Its tools are scaled automatically from software documentation, open-source repos and code generation, and it was trained on more than 17,000 synthetic verifiable tasks with SFT followed by online RL. On OSWorld, the 7B and 32B models improved 22% relative on average over their base models and needed 11% fewer steps. On WindowsAgentArena, UltraCUA-7B reached 21.7% with no Windows-specific training. — [arXiv 2510.17790](https://arxiv.org/abs/2510.17790)
- **Navi/WAA** (see section 1) shows that structured UIA signals improve pixel-based perception. That supports "structured first" as a principle. — [arXiv 2409.08264](https://arxiv.org/html/2409.08264v1)

### Inferences
- Recommended layering for speed and reliability:
  1. MCP servers or native app APIs and CLIs (for example the Office COM/Graph APIs, the filesystem, git, a calendar API).
  2. Browser via Playwright/CDP a11y snapshot.
  3. OS a11y tree (UIA/AX/AT-SPI) with semantic actions (Invoke, SetValue).
  4. Vision grounding plus synthetic mouse and keyboard as the last resort.
- Each step down this ladder costs more latency and fragility.
- Speculative batching of actions, validated against the a11y tree, is a cheap latency win: it cuts LLM round trips roughly in half on complex tasks.

### Gaps
- I found no independent benchmark of MCP-tool-first agents versus GUI agents on the same tasks.
- I could not verify UltraCUA's v3 revisions or the dual-agent (Planner/Grounder) description, which appears only on a third-party summary site.

## 3. Input control libraries and their limits

### Takeaway
Prefer semantic a11y actions over synthetic input. Use xdotool on X11 only. ydotool works on Wayland via kernel uinput but needs root or udev rules, ignores focus and window management, and emits relative motion that compositor acceleration distorts. Newer portal/libei tools such as wdotool respect compositor permissions. On Windows, pywinauto's UIA backend gives semantic control but can hang on COM calls.

### Cited Findings
- **xdotool:** X11-only. It sends X events straight to the X server and does not work on Wayland. — [ydotool README (Debian)](https://sources.debian.org/src/ydotool/1.0.4-2/README.md); [lemmy discussion](https://lemmy.manganiello.tech/post/209232/82113)
- **ydotool:** Emulates an input device through the kernel uinput framework, so it works on Wayland. It needs access to `/dev/uinput`, which usually means root. If the startup delay is too short, the virtual device may not register with the graphical environment in time. — [ydotool README](https://sources.debian.org/src/ydotool/1.0.4-3/README.md)
- ydotool cannot manipulate windows. KDE's KdeEcoTest paired it with kdotool for window control. — [KDE Eco blog, Feb 2024](https://eco.kde.org/blog/2024-02-20-sok24-wayland_support_kdeecotest)
- uinput injection is not focus-aware and has no background targeting: the real cursor moves, and the window must be focused first. Relative motion plus compositor acceleration forces closed-loop cursor correction. — [wgaf (GitHub)](https://github.com/Ranrar/wgaf); [linux-desktop-mcp backends](https://glama.ai/mcp/servers/@BeckhamLabsLLC/linux-desktop-mcp/blob/1aad34cc6ae1aa216ae4382f8df8902268ec5a9f/src/linux_desktop_mcp/input_backends.py)
- **Security:** Unlike xdotool, uinput-generated input can be seen by any user on the system. — [lemmy discussion](https://lemmy.manganiello.tech/post/209232/82113)
- **wdotool:** An early Wayland xdotool successor that uses libei via the XDG RemoteDesktop portal, wlr virtual-keyboard and virtual-pointer protocols, KWin scripting, or a GNOME Shell extension. It respects compositor focus and permissions, but is not a drop-in replacement yet. — [wdotool (GitHub)](https://github.com/cushycush/wdotool)
- **pywinauto:** Two backends, win32 and uia. Custom controls and properties are unsupported in uia because of comtypes. Some UIA calls (`invoke()`) can hang, so they need timeouts. — [pywinauto docs](https://pywinauto.readthedocs.io/en/latest/getting_started.html); [pywinauto issue](https://lightrun.com/answers/pywinauto-pywinauto-problem-printing-element)

### Inferences
- pyautogui works through screen coordinates and needs the target visible and focused. It takes over the user's real cursor, which conflicts with letting the user keep control (see section 4).
- AppleScript/JXA (scriptable apps) and the macOS AX API are the semantic route on macOS. I could not source their limits in this session.

### Gaps
- I found no sourced material on pyautogui limits (multi-monitor, HiDPI scaling, failsafe), AppleScript coverage, or macOS TCC/Accessibility permission prompts. These remain unverified.
- I found no latency figures for input injection per library.

## 4. Running in the background (virtual desktops, VMs, sandboxes) and security

### Takeaway
To keep the user in control, the agent should act in an isolated surface. UFO2's Picture-in-Picture mode runs automation in an isolated virtual desktop alongside the user's session. Cloud agents use VMs or containers. Prompt injection from on-screen content is the main risk. Vendors use layered mitigations: injection classifiers on screenshots, confirmation before sensitive actions, adversarial training, sandboxing and least privilege. Research says none of these are complete.

### Cited Findings
- **UFO2 PiP:** A Picture-in-Picture interface runs automation inside an isolated virtual desktop, so agents and users work at the same time without interfering. — [arXiv 2504.14603](https://arxiv.org/html/2504.14603v2); [HF Papers](https://huggingface.co/papers/2504.14603)
- **Anthropic computer use:** When the computer-use tool runs, Anthropic adds classifiers that detect potential prompt injections in screenshots and steer Claude to ask the user for confirmation before acting (details and opt-out are on a separate page). Anthropic's guidance also says to treat tool and document content as untrusted data and to monitor outputs for injection. — [Claude Docs: mitigate jailbreaks / prompt injections](https://platform.claude.com/docs/en/docs/mitigating-jailbreaks-prompt-injections)
- Anthropic's original 2024 computer-use warning said instructions on webpages or in images may override user instructions. It recommended isolating Claude from sensitive data and actions. — [Simon Willison, Oct 2024](https://simonwillison.net/2024/Oct/22/computer-use)
- A secondary deployment guide recommends a dedicated VM or container, no direct credential access, domain allowlisting, and human confirmation for consequential actions. — [decryptiondigest](https://www.decryptiondigest.com/blog/governing-claude-computer-use-autonomous-desktop-agents)
- **OpenAI Operator:** Defenses included confirmation prompts before sensitive actions (for example form submission), prompt-injection detection, and fine-tuning to reduce susceptibility. These come at the cost of autonomy and usability. — ["Mind the Web" arXiv 2506.07153](https://arxiv.org/pdf/2506.07153); [OpenAI: prompt injections](https://openai.com/index/prompt-injections/)
- Text-only defenses such as LlamaFirewall and Meta SecAlign are "neither fully suitable nor efficient" for multimodal computer-use scenarios. — [RedTeamCUA arXiv 2505.21936](https://arxiv.org/pdf/2505.21936)
- A 2026 preprint argues that classifier and training defenses fail when attacks work through context rather than syntax, and that they hurt utility. — [arXiv 2605.17634 "AI Agents May Always Fall for Prompt Injections"](https://arxiv.org/pdf/2605.17634)
- An academic review (March 2026) reports that real-world exploits persist in browser and desktop integrations despite improved robustness in Claude Opus 4.5. — [arXiv 2603.21642](https://arxiv.org/pdf/2603.21642)

### Inferences
- Recommended design:
  - Run GUI actions in a secondary desktop, session or VM (on Windows: a PiP-like RDP loopback or virtual desktop; on Linux: a nested Wayland/Xvfb session; on macOS: a VM). This lets the agent act without moving the user's cursor.
  - Use API and MCP actions directly where possible, since they need no visible surface.
  - Gate irreversible actions (send, pay, delete) behind voice or click confirmation.
  - Treat all screen and web text as untrusted.

### Gaps
- I could not confirm the PiP implementation mechanism (RDP loopback vs. Windows virtual desktop) from the primary text.
- I found no measured performance overhead for VM or virtual-desktop isolation.

## 5. Real-time voice for the "Jarvis" feel

### Takeaway
Native speech-to-speech is needed for conversational feel. The commonly cited target is under about 500 ms. Measured cloud S2S time-to-first-audio in 2026 is roughly 0.8-1.1 s, and reasoning-heavy settings are slower (up to about 2.3 s). Full-duplex research models such as Moshi reach about 200 ms. Interruptibility depends on server VAD or semantic VAD plus client-side playback truncation.

### Cited Findings
- **OpenAI GPT-Realtime-2 (May 2026):** First audio arrives in 1.12 s at minimal reasoning and 2.33 s at high reasoning. Reasoning effort is an API parameter and defaults to low. The source notes that real-time interaction benefits from latency under 500 ms. — [DeepLearning.AI The Batch](https://www.deeplearning.ai/the-batch/openai-challenges-speech-to-speech-leaders)
- **Artificial Analysis (via Softcery, April 2026):** gpt-realtime-1.5 measured 0.82 s time-to-first-audio. Network latency makes real-world numbers higher. — [Softcery](https://softcery.com/lab/ai-voice-agents-real-time-vs-turn-based-tts-stt-architecture)
- Lower claims (about 250-600 ms, "gpt-live-1" about 0.8 s turn latency, July 2026) come from less established sites with inconsistent model naming. Treat them as low confidence. — [Silicon Report](https://www.siliconreport.com/openai-rolls-out-gpt-live-targeting-300ms-latency-for-real-time-voice-53903ae9); [tech-insider](https://tech-insider.org/openai-realtime-api-gpt-live-1-setup-2026/)
- **OpenAI semantic VAD:** A semantic classifier estimates whether the user has finished speaking. When confidence is high, there is no need to wait. `eagerness` takes low, medium, high or auto (auto = medium) and tunes the maximum wait. Semantic VAD reduces the model interrupting the user. — [OpenAI Realtime VAD guide](https://developers.openai.com/api/docs/guides/realtime-vad)
- **OpenAI interruption pattern:** On barge-in, the client sends `response.cancel` plus `conversation.item.truncate` with the played-audio offset, so the server's context matches what the user actually heard. Developers report that the transcript was sometimes not trimmed properly (anecdotal). — [OpenAI community thread](https://community.openai.com/t/realtime-api-interruptions-dont-properly-trim-the-transcript/1000703)
- **Gemini Live API:** Uses a stateful WebSocket with 16-bit PCM input at 16 kHz and output at 24 kHz. It supports barge-in (users can interrupt at any time), VAD, affective dialog, proactive audio and tool use. The recommended GA model is `gemini-live-2.5-flash-native-audio`. Google gives no official latency number. Vapi claims "hundreds of milliseconds", which is a vendor claim. — [Google Cloud Live API docs](https://docs.cloud.google.com/vertex-ai/generative-ai/docs/live-api); [Vapi](https://vapi.ai/partnerships/directory/partner/google-gemini)
- **Kyutai Moshi (open-source, full-duplex):** 160 ms theoretical latency, about 200 ms in practice. It models user and system speech as parallel streams, so it can listen while speaking. Cascaded STT→LLM→TTS pipelines are described as inducing "several seconds" of latency. — [Moshi arXiv 2410.00037](https://arxiv.org/html/2410.00037v1)
- **Production budget:** One 2026 guide budgets about 800 ms total voice-to-voice for a Realtime API agent. — [Forasoft](https://www.forasoft.com/blog/article/openai-realtime-api-voice-agent-production-guide-2026)
- **Turn-detection frameworks:** LiveKit Agents documents turn detection and interruption handling for voice agents. — [LiveKit docs](https://docs.livekit.io/agents/v1/build/turn-detection)

### Inferences
- For a Jarvis feel, decouple the voice loop from the computer-use loop. The S2S model answers immediately ("on it, opening your calendar") and makes a tool call. The slower perception/action agent runs in the background or PiP desktop and streams progress back as short spoken updates.
- Use low reasoning effort on the voice model and delegate heavy reasoning to the agent.
- Implement barge-in: stop local playback immediately on VAD, then cancel and truncate the server response.

### Gaps
- I found no official OpenAI or Google first-party latency SLAs.
- I found no independent head-to-head of Gemini Live vs OpenAI Realtime vs others (Amazon Nova Sonic, ElevenLabs, Hume) with dated figures from this period. The Artificial Analysis leaderboard is the likely source but could not be fetched.
