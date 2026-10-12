# Integration and Orchestration Tooling: Codex, Claude Code and Gemini (as of October 12, 2026)

> Scope note: About 17 searches/fetches were run. Direct fetches of code.claude.com and codex.danielvaughan.com failed (DNS), so several facts rest on search-result summaries of those pages and on secondary blogs. Each claim is tagged with its source. **A major landscape change:** Gemini CLI stopped serving consumer accounts (free, Google AI Pro, Ultra) on June 18, 2026. Those users now get the closed-source **Antigravity CLI (`agy`)**. Any "Gemini CLI" integration below has to be read with that in mind.

## Q1. Shared standards: MCP, instruction files (AGENTS.md / CLAUDE.md / GEMINI.md), Agent Skills, A2A

### Takeaway
All three ecosystems now share three layers: MCP for tools, AGENTS.md for instructions, and Agent Skills (SKILL.md, increasingly in `.agents/skills/`) for packaged capabilities. MCP and AGENTS.md are governed by the Linux Foundation's Agentic AI Foundation (AAIF), and A2A joined AAIF in August 2026. As of Claude Code v2.1.277 (Sept 18, 2026), a single AGENTS.md can serve all three CLIs with little glue.

### Cited Findings
**Governance**
- The Linux Foundation launched the Agentic AI Foundation (AAIF) on Dec 9, 2025. Its founding contributions were Anthropic's MCP, Block's Goose and OpenAI's AGENTS.md. — [baeseokjae summary](https://baeseokjae.github.io/posts/linux-foundation-agentic-ai-foundation-2026/); [FlowHunt](https://www.flowhunt.io/blog/agentic-ai-foundation-a2a-mcp-standards/)
- Google's A2A (Agent2Agent) protocol joined AAIF in August 2026. Sources give the date as Aug 17, 19 or 20. A2A had already been donated to the Linux Foundation in 2025, so the move changed the host, not who controls it. — [A2A official blog, 2026-08-27](https://a2a-protocol.org/latest/blog/2026/08/27/a-new-chapter-for-a2a-joining-the-agentic-ai-foundation/); [Forbes 2026-08-19](https://www.forbes.com/sites/janakirammsv/2026/08/19/agent2agent-joins-the-agentic-ai-foundation-alongside-mcp/)
- Layering as commonly described: MCP connects agents to tools and data, A2A connects agents to each other, and AGENTS.md carries instructions and context. A2A is supported by LangGraph, CrewAI, Pydantic AI, AG2 and IBM BeeAI. — [FlowHunt](https://www.flowhunt.io/blog/agentic-ai-foundation-a2a-mcp-standards/); [salmanq](https://www.salmanq.com/blog/agent-protocols-mcp-a2a/)
- The Linux Foundation launched an MCPA (MCP) certification on Sept 14, 2026. — [Linux Foundation press](https://www.linuxfoundation.org/press/agentic-ai-foundation-launches-mcpa-certification-to-validate-mcp-expertise)

**MCP in the CLIs**
- Codex CLI acts as an MCP *client* through `codex mcp` / config.toml. Its MCP *server* mode (`codex mcp-server`) was deprecated on Aug 24, 2026 (v0.149.x) and removed in v0.154.0 on Sept 9, 2026. Official replacements are the Codex **app-server** protocol and, for Claude Code, the **Codex plugin for Claude Code**. — [Codex Releases on X (deprecation)](https://x.com/CodexReleases/status/2091929016194289782); [Codex Releases on X (removal)](https://x.com/CodexReleases/status/2097817398040551429); [sterling issue #18](https://github.com/Chulf58/sterling/issues/18); [Daniel Vaughan migration post](https://codex.danielvaughan.com/2026/08/25/codex-mcp-server-deprecated-app-server-migration-claude-code-plugin-v0149/)
- Claude Code has mature MCP client support (local stdio and remote servers). Gemini CLI also supports adding HTTP MCP servers. — [truthifi](https://truthifi.com/education/state-of-mcp-2026-ai-agents-custom-connectors); [pasqualepillitteri](https://pasqualepillitteri.it/en/news/8923/mcp-servers-connect-claude-chatgpt-gemini)

**MCP in the chat apps**
- **ChatGPT:** custom MCP connectors are added through Developer Mode on the web, and the settings were recently renamed to "Plugins". A public HTTPS endpoint is required because ChatGPT cannot launch local processes. Sources disagree on plan access (paid plans vs Plus/Pro/Business/Enterprise/Edu), and Plus/Pro users may be limited to read-only tools while write-capable tools are limited to Business, Enterprise and Edu. Users report reliability problems such as connectors vanishing or OAuth completing without the connector appearing. — [Peliqan](https://peliqan.io/blog/chatgpt-mcp/); [coworker.ai](https://coworker.ai/blog/chatgpt-mcp); [truthifi](https://truthifi.com/education/state-of-mcp-2026-ai-agents-custom-connectors)
- **Claude apps:** custom connectors live under Customize → Connectors. Plan availability conflicts across sources: Free with one custom connector, or Pro and above. — [truthifi](https://truthifi.com/education/state-of-mcp-2026-ai-agents-custom-connectors); [fieldproxy](https://www.fieldproxy.ai/guides/how-to-use-mcp-with-chatgpt-claude-copilot)
- **Gemini app:** Settings → Connected Apps → Custom apps, reportedly limited to US personal accounts, 18+, in English. The Gemini API/SDK gained native MCP support in March 2026. — [truthifi](https://truthifi.com/education/state-of-mcp-2026-ai-agents-custom-connectors)
- MCP Apps (interactive UI) is described as the first official MCP extension and is reported as adopted by Claude and ChatGPT. — [pasqualepillitteri](https://pasqualepillitteri.it/en/news/8923/mcp-servers-connect-claude-chatgpt-gemini)

**Instruction files**
- Claude Code changelog v2.1.277 (Sept 18, 2026): if a project has no CLAUDE.md, Claude Code reads AGENTS.md. Precedence is CLAUDE.md only → CLAUDE.md; AGENTS.md only → AGENTS.md; both → **CLAUDE.md only** (a fallback, not a merge). The behavior is configurable in `/config` → "Project instructions" with four modes: CLAUDE.md only, AGENTS.md fallback, load both, or managed-only. Bedrock, Vertex and Foundry support followed in v2.1.281 (Sept 23). There is an unverified report that the feature did nothing for users with telemetry off. — [Classmethod v2.1.277-278](https://dev.classmethod.jp/en/articles/20260919-cc-updates-v2-1-278/); [DevOps.com](https://devops.com/claude-code-adds-agents-md-fallback-cutting-instruction-file-sprawl/); [runtimewire](https://runtimewire.com/article/claude-code-adds-agents-md-support); [rohitraj notes](https://rohitraj.tech/notes/claude-code-agents-md-vs-claude-md-precedence-2026)
- The classic sharing workarounds still apply and are needed if you keep a CLAUDE.md. Either put a one-line `@AGENTS.md` import in CLAUDE.md, or run `ln -s AGENTS.md CLAUDE.md`. The import is preferred on Windows, where symlinks need extra permissions. — [inventivehq](https://inventivehq.com/blog/claude-md-vs-agents-md-vs-gemini-md); [spec-weave](https://spec-weave.com/docs/guides/agents-md-vs-claude-md/)
- Gemini CLI: `context.fileName` in `.gemini/settings.json` accepts an array, e.g. `{"context":{"fileName":["AGENTS.md","GEMINI.md"]}}`, so Gemini loads AGENTS.md. — [inventivehq](https://inventivehq.com/blog/claude-md-vs-agents-md-vs-gemini-md); [thepromptshelf](https://thepromptshelf.dev/blog/agents-md-vs-claude-md-vs-gemini-md-2026/)
- Codex reads AGENTS.md natively; it is OpenAI's own convention. — [Daniel Vaughan portability post](https://codex.danielvaughan.com/2026/05/27/agent-instruction-files-agents-md-claude-md-cross-tool-portability-codex-cli/)

**Agent Skills**
- Anthropic published the Agent Skills specification (SKILL.md) on Dec 18, 2025, hosted at agentskills.io. "Open" here means openly licensed, not independently governed. — [Laurent Kempé](https://laurentkempe.com/2026/01/27/Agent-Skills-From-Claude-to-Open-Standard/); [paperclipped](https://www.paperclipped.de/en/blog/agent-skills-open-standard-interoperability/)
- Codex launched Skills experimentally in Dec 2025 (`~/.codex/skills`, then repo `.agents/skills`). Gemini CLI added native skills as a preview in v0.23.0 (Jan 7, 2026), on by default in January. `.agents/skills/` is reported as read by Codex, Gemini CLI, Antigravity, Cursor, VS Code Copilot and others. Adoption counts vary: "over 25 tools" in one source, 32 tools as of March 2026 in another. — [cookiesforai](https://cookiesforai.app/compare/how-codex-claude-code-and-gemini-cli-load-skills); [termdock](https://www.termdock.com/en/blog/agent-skills-guide); [inference.sh](https://inference.sh/blog/skills/agent-skills-overview)
- When migrating from Gemini CLI to Antigravity CLI, `agy plugin import gemini` imports extensions and skills move from `.gemini/skills/` to `.agents/skills/`. — [openclaw issue #84527](https://github.com/openclaw/openclaw/issues/84527); [aibuilderclub](https://www.aibuilderclub.com/blog/google-kills-gemini-cli-june-18-2026)

### Inferences
- The most portable repo layout as of October 2026:
  - AGENTS.md is the canonical instruction file.
  - Either no CLAUDE.md, or a CLAUDE.md containing `@AGENTS.md` plus Claude-specific extras.
  - `.gemini/settings.json` with `context.fileName` including AGENTS.md.
  - Skills live in `.agents/skills/`. Claude Code uses `.claude/skills/`; I did not confirm whether it also reads `.agents/skills/`, so a symlink may be needed.
  - MCP servers are configured separately in each tool, because there is no shared MCP config file format.
- A2A is the right standard for enterprise agent meshes, but I found no evidence that any of the three coding CLIs is commonly orchestrated through A2A in practice. MCP and subprocess calls dominate.

### Gaps
- Could not fetch the official Claude Code memory docs (DNS failure), so the AGENTS.md behavior relies on changelog coverage by Classmethod, DevOps.com and others.
- Did not confirm whether Claude Code reads `.agents/skills/` or only `.claude/skills/`.
- Did not find whether Gemini CLI or Antigravity CLI exposes or consumes A2A natively.
- ChatGPT and Claude connector plan matrices conflict across secondary sources; check the vendor help centers.

## Q2. Calling one agent from another (subagents, MCP servers, bridges)

### Takeaway
Since September 2026 the **official** route from Claude Code into Codex is OpenAI's `codex-plugin-cc`, because `codex mcp-server` has been removed. For anything else, the robust pattern is shelling out to the other CLI's headless mode (`codex exec`, `claude -p`, `gemini -p` / `agy`), directly or through PAL MCP's `clink`. Several popular Gemini bridges have broken or been abandoned since the June 2026 Gemini CLI consumer cutoff.

### Cited Findings
- **openai/codex-plugin-cc** is an official OpenAI plugin running inside Claude Code. Reported publish date is Mar 30, 2026; star counts range from about 3.7k to over 20k across sources.
  - Install: `/plugin marketplace add openai/codex-plugin-cc`, then `/plugin install codex@openai-codex`, then `/codex:setup`.
  - Commands: `/codex:review` (read-only), `/codex:adversarial-review`, `/codex:rescue`, `/codex:transfer`, and `/codex:status`, `/codex:result`, `/codex:cancel` for background jobs.
  - An optional "review gate" stops Claude from finishing until Codex has reviewed its changes. It can trigger long Claude↔Codex loops that burn tokens.
  - Requirements: Node 18.18+ and a ChatGPT subscription (Free included) or an OpenAI API key. It wraps the local Codex CLI and uses the same config and auth.
  - — [GitHub openai/codex-plugin-cc](https://github.com/openai/codex-plugin-cc); [The Decoder](https://the-decoder.com/openai-launches-a-codex-plugin-that-runs-inside-anthropics-claude-code/); [Daniel Vaughan](https://codex.danielvaughan.com/2026/04/12/codex-plugin-cc-cross-provider-bridge/)
- **Removal of `codex mcp-server`:** deprecated Aug 24, 2026 and removed in v0.154.0 on Sept 9, 2026. The standalone `codex-mcp-server` binary was also removed. Downstream tools broke; for example, Sterling's Codex lanes fail on codex ≥0.154.0. — [Codex Releases X](https://x.com/CodexReleases/status/2097817398040551429); [sterling issue #18](https://github.com/Chulf58/sterling/issues/18). **Any guide recommending `claude mcp add codex -- codex mcp-server` is outdated.**
- **Third-party Codex bridges:**
  - `codex-as-mcp` (kky42) wraps `codex exec`. Latest PyPI release is June 29, 2026. It runs with `--dangerously-bypass-approvals-and-sandbox` by default. — [PyPI](https://pypi.org/project/codex-as-mcp/); [GitHub](https://github.com/kky42/codex-as-mcp)
  - `codex-subagent-for-claude` does async dispatch into a long-lived `codex app-server`, with results pushed back into Claude through hooks. It was listed in a Sept 2026 awesome-mcp PR and is unverified. — [awesome-mcp-servers PR #15256](https://github.com/punkpeye/awesome-mcp-servers/pull/15256)
  - `codex-subagents-mcp` (leonardsellem) adds Claude-style persona subagents inside Codex, running `codex exec --profile`. — [GitHub](https://github.com/leonardsellem/codex-subagents-mcp)
- **Reverse direction (Claude from Codex):**
  - `claude-subagents-mcp` (jordankzf) lets Codex delegate to Claude subagents, with up to 4 running in parallel. — [GitHub](https://github.com/jordankzf/claude-subagents-mcp)
  - `subagent-mcp` (ntoscano) is a stdio MCP server that turns Claude Code, Codex or Gemini CLI into a manager of local claude and codex CLI subagents. — [GitHub](https://github.com/ntoscano/subagent-mcp)
- **PAL MCP Server** (BeehiveInnovations, formerly Zen MCP) has a `clink` tool that launches isolated Gemini CLI, Codex CLI, Claude Code (also Qwen and Cursor) subagents with roles such as planner or codereviewer, returning only the final output.
  - It also offers multi-model consensus and review tools through APIs (Gemini, OpenAI, OpenRouter, Azure, Grok, Ollama).
  - Caveat: clink launches CLIs with relaxed permission flags, which gives them real write access.
  - Activity: PRs through about July 2026 (Windows shims, nvm codex path, a proposed Antigravity `agy` client). A community fork (ramarivera) exists because upstream maintainers were slow to respond.
  - — [GitHub pal-mcp-server](https://github.com/BeehiveInnovations/pal-mcp-server); [clink docs](https://github.com/BeehiveInnovations/pal-mcp-server/blob/main/docs/tools/clink.md); [PR list](https://github.com/BeehiveInnovations/pal-mcp-server/pulls); [ramarivera fork](https://github.com/ramarivera/pal-mcp-server)
- **gemini-mcp-tool** (jamubc), the classic "Claude Code calls Gemini CLI for its large context" bridge:
  - Its README now says Google retired Gemini CLI for free, Pro and Ultra users on 2026-06-18. It now defaults to Antigravity CLI (`agy`), switchable with `GEMINI_MCP_BACKEND`.
  - A competitor claims the last main commit was 2025-07-23 with unanswered 2026 issues. npm, however, shows v1.1.8 published about 3 months ago. Status is conflicting, at best low-maintenance.
  - Open issues include a deprecated `-p` flag error (#48).
  - — [GitHub](https://github.com/jamubc/gemini-mcp-tool); [npm](https://www.npmjs.com/package/gemini-mcp-tool); [qmediat comparison (vendor)](https://github.com/qmediat/gemini-code-context-mcp)
- `qmediat/gemini-code-context-mcp` is an SDK/API-key-based alternative using the Files API and context caching. It is self-described as a "drop-in upgrade over the abandoned gemini-mcp-tool". — [GitHub](https://github.com/qmediat/gemini-code-context-mcp)
- **Gemini CLI → Antigravity CLI:**
  - Announced at Google I/O on May 19, 2026. On June 18, 2026, Gemini CLI stopped serving consumer accounts (free, Google AI Pro, Ultra); those tiers moved to Antigravity CLI.
  - `agy` is a Go binary, reportedly closed source (Gemini CLI was Apache-2.0), and ships with Antigravity 2.0 defaulting to Gemini 3.5 Flash.
  - Gemini Code Assist Standard and Enterprise licenses and API-key use of Gemini CLI are unaffected.
  - — [google-gemini/gemini-cli Discussion #27274 (official)](https://github.com/google-gemini/gemini-cli/discussions/27274); [avinashsangle](https://avinashsangle.com/blog/gemini-cli-to-antigravity-cli-guide); [stacknovahq](https://stacknovahq.com/ai-tools-for-developers/antigravity-cli-tested-real-project); [gemini-cli issue #28229](https://github.com/google-gemini/gemini-cli/issues/28229)
- Codex has its own native subagents in the TUI, while Claude Code uses its Task/subagent tool. — [blakecrosley Codex guide](https://blakecrosley.com/guides/codex)

### Inferences
- The simplest durable pattern is a Claude Code subagent or skill that runs `codex exec "<task>"` or `agy`/`gemini -p "<prompt>"` through Bash and reads stdout or a result file. It depends only on stable headless CLI flags, not on bridge projects that keep breaking. Wrapper MCPs (PAL clink, codex-as-mcp) add role prompts and context isolation but inherit the same flag-churn risk.
- Expect bridge breakage after each vendor CLI release. Two examples: the Codex mcp-server removal, and the Gemini `-p` deprecation / Antigravity switch.

### Gaps
- claude-code-router (musistudent): no search results on its 2026 status. It routes Claude Code to non-Anthropic models through API keys, so it is not a subscription bridge.
- Did not verify exact current headless flags for `agy` (equivalent of `gemini -p`), or the official Codex app-server API docs.
- Did not get PAL MCP's current star count or last commit date.

## Q3. Multi-agent orchestrators and parallel harnesses (worktrees)

### Takeaway
Worktree-per-agent desktop and TUI managers are the standard way to run Codex, Claude Code and Gemini side by side. Several of them died or pivoted in 2026: Crystal was deprecated in favor of Nimbalyst, and the company behind Vibe Kanban shut down. GitHub Agent HQ is the main hosted multi-vendor option, but it has no Gemini agent yet.

### Cited Findings
- **Conductor** (Melty Labs, macOS) runs agents in separate git worktrees. It supports Claude Code, Codex, Cursor and OpenCode; no Gemini support was listed. The free plan covers running agents in parallel in local workspaces on your Mac. — [nimbalyst comparison](https://nimbalyst.com/blog/best-multi-agent-coding-tools-2026/); [abralo alternatives](https://abralo.com/alternatives)
- **Claude Squad** is a tmux-based TUI that works with Claude Code, Codex, Gemini and Aider. License is AGPL-3.0. — [nimbalyst](https://nimbalyst.com/blog/best-agent-management-tools-2026/); [dev.to](https://dev.to/stravukarl/best-tools-for-managing-parallel-ai-coding-agents-in-2026-14l8)
- **Vibe Kanban:** the company shut down on Apr 10, 2026. The project continues as community-maintained Apache-2.0 open source, and its paid cloud and remote services were sunset. Supported agents are Claude Code, Codex, Gemini CLI, Copilot CLI, Amp, Cursor, OpenCode, Droid, Aider and Qwen. **Flag: maintenance risk.** — [nimbalyst](https://nimbalyst.com/blog/best-git-worktree-tools-ai-coding-2026/); [agentsroom](https://agentsroom.dev/blog/best-multi-agent-coding-tools)
- **Crystal** was deprecated in Feb 2026 and replaced by **Nimbalyst** (Claude Code and Codex; Gemini support not confirmed). **Flag: abandoned.** — [nimbalyst](https://nimbalyst.com/blog/best-agent-management-tools-2026/) (note: vendor's own blog)
- **Codeman** runs Claude Code, Codex, Gemini CLI and others side by side. — [getcodeman](https://getcodeman.com/guides/claude-code-codex-gemini-side-by-side)
- **GitHub Agent HQ:**
  - Claude and Codex became available as third-party coding agents on Feb 4, 2026 (Copilot Pro+ and Enterprise), extended to Pro and Business on Feb 26. Still public preview as of August 2026.
  - You can assign an issue to Copilot, Claude, Codex, or several at once to compare results. Agents open draft PRs, and you can request follow-ups with `@Claude` / `@Codex` mentions.
  - Each session uses one premium request. Org-level policy controls which agents are allowed.
  - Google, Cognition and xAI agents were "in the works".
  - — [GitHub Blog](https://github.blog/news-insights/company-news/pick-your-agent-use-claude-and-codex-on-agent-hq/); [Help Net Security](https://www.helpnetsecurity.com/2026/02/05/github-enables-coding-agents/)
- **Worktree caveat:** worktrees isolate files, not the environment. A runaway `npm install` or a rogue migration still hits your real machine and databases. — [nimbalyst worktree tools](https://nimbalyst.com/blog/best-git-worktree-tools-ai-coding-2026/)
- Addy Osmani's "Code Agent Orchestra" essay discusses what makes multi-agent coding work. — [addyosmani.com](https://addyosmani.com/blog/code-agent-orchestra/)

### Inferences
- If you need Gemini/Antigravity in the same board, Claude Squad, Vibe Kanban (community) and Codeman are the reported options. Conductor and Agent HQ currently cover only Claude and Codex.
- Most comparison sources are vendor blogs (nimbalyst, abralo, agentsroom), so they are biased toward their own product.

### Gaps
- No primary-source verification of the current Conductor, Claude Squad or Sculptor feature lists or last commit dates. Sculptor, OpenCode multi-provider, Cline/Kilo/Roo, Cursor multi-model and Copilot CLI were not researched within the tool budget.
- Whether Agent HQ added a Google/Gemini agent after August 2026 is unknown.

## Q4. Cross-model code review on the same PR

### Takeaway
Yes, all three can review the same GitHub PR at the same time, because each runs as an independent GitHub App or Action posting its own comments. The options are Codex cloud review (`@codex review`), Claude (Code Review managed product, Claude Code GitHub Action, or `@Claude` via Agent HQ) and Gemini (the Gemini Code Assist GitHub App with `/gemini review`, or the Gemini CLI GitHub Action). They differ in cost and plan requirements.

### Cited Findings
- **Codex:** reviews PRs automatically when they move from draft to ready, or on demand with an `@codex review` PR comment. — [Tembo](https://www.tembo.io/blog/codex-vs-claude-code-vs-gemini-cli)
- **Claude Code Review (managed):** multi-agent analysis, severity-tagged comments, `REVIEW.md` customization. Reported cost is about $15–25 per PR, on Team plan ($25/seat) or higher, billed as extra usage. — [Tembo](https://www.tembo.io/blog/codex-vs-claude-code-vs-gemini-cli)
- **Gemini Code Assist GitHub App:**
  - Reviews new PRs automatically, with `gemini-code-assist[bot]` added as a reviewer.
  - Manual commands `/gemini review`, `/gemini summary` and `/gemini help` go in issue comments.
  - Severity labels run from Critical to Low, with a configurable minimum threshold. Config is `config.yaml` plus `styleguide.md`; the path is reported as `.gemini/` vs `.github/gemini.yml`, which conflicts across sources.
  - — [Google Cloud docs](https://docs.cloud.google.com/gemini/docs/code-review/use-code-assist-github); [Google docs (review)](https://docs.cloud.google.com/gemini/docs/code-review/review-repo-code); [GitHub App page](https://github.com/apps/gemini-code-assist)
- **Gemini CLI GitHub Action:** `@gemini-cli /review` with auto-trigger on `pull_request: [opened, synchronize]`. Reported as free on the free tier, though this predates the June 2026 consumer cutoff, so re-check auth (API key). — [Tembo](https://www.tembo.io/blog/codex-vs-claude-code-vs-gemini-cli)
- **Agent HQ:** `@Claude` / `@Codex` mentions in PR comments trigger follow-up work. — [GitHub Blog](https://github.blog/news-insights/company-news/pick-your-agent-use-claude-and-codex-on-agent-hq/)
- A common pattern is to have Claude Code write and Codex review. One blog benchmark (about early 2026, single source) reported raw bug detection of Claude 53%, Codex 33% and Gemini 13%. Milvus reports that multi-model "debate" review improves results. — [Tembo](https://www.tembo.io/blog/codex-vs-claude-code-vs-gemini-cli); [Milvus blog](https://milvus.io/blog/ai-code-review-gets-better-when-models-debate-claude-vs-gemini-vs-codex-vs-qwen-vs-minimax.md)
- Locally, `/codex:review` and `/codex:adversarial-review` inside Claude Code give a second-model review before you push. — [openai/codex-plugin-cc](https://github.com/openai/codex-plugin-cc)

### Inferences
- A practical triple-review setup:
  - Install the Codex GitHub integration (auto-review on ready-for-review).
  - Install the Gemini Code Assist app (auto-review on open).
  - Add the Claude Code GitHub Action, or Claude Code Review if on Team/Enterprise.
  - Expect comment noise. Severity thresholds (Gemini) and REVIEW.md / AGENTS.md review guidelines help.

### Gaps
- Did not fetch the official docs for Codex cloud code review or the Claude Code GitHub Action (`anthropics/claude-code-action`), so plan requirements and pricing come from Tembo (secondary).
- The Gemini Code Assist GitHub app's consumer/free status after June 2026 was not confirmed.

## Q5. Subscription login vs API key; ToS on routing subscriptions through third-party tools

### Takeaway
The three vendors now differ sharply.
- **OpenAI** officially allows "Sign in with ChatGPT" in partner third-party tools such as OpenCode, with per-app caps.
- **Anthropic** restricts Claude Pro/Max OAuth to Claude Code and Claude.ai. Third-party harnesses need API keys, and the status of a possible 2026 reversal is unclear.
- **Google** forbids using Gemini CLI OAuth from third-party software, and shut consumer OAuth on Gemini CLI entirely on June 18, 2026 in favor of Antigravity CLI.

Running each vendor's *own* CLI with its own subscription, and orchestrating them via subprocess, is the ToS-safe pattern.

### Cited Findings
**OpenAI**
- Codex CLI and codex-plugin-cc accept ChatGPT sign-in (Free included) or an API key. — [openai/codex-plugin-cc](https://github.com/openai/codex-plugin-cc)
- At DevDay, OpenAI announced "Sign in with ChatGPT" for third-party developer tools. Plus and Pro subscribers can use plan allowance in partner tools; 16 launch partners include OpenCode. Users can set weekly caps per app, and usage stops at the cap. The announcement date was not clear in results. — [daily.dev](https://daily.dev/posts/openai-makes-sign-in-with-chatgpt-a-way-to-use-your-subscription-in-third-party-developer-tools-7ewaoo6ze); [Developers Digest](https://www.developersdigest.tech/blog/sign-in-with-chatgpt-coding-agents-plan-usage-2026)
- Pro allowance change: existing Pro subscribers move to a smaller allowance after Oct 29, 2026, and third-party traffic draws from the shared Work/Codex pool. This comes from a third-party guide and is unverified. — [Developers Digest](https://www.developersdigest.tech/blog/sign-in-with-chatgpt-coding-agents-plan-usage-2026)
- Community OAuth plugins (e.g. `opencode-openai-codex-auth`) are unofficial and for personal use only. — [plugin site](https://numman-ali.github.io/opencode-openai-codex-auth/)

**Anthropic**
- Anthropic's terms restrict subscription OAuth tokens to Claude Code and Claude.ai; the Agent SDK is also covered. Server-side rejection of third-party OAuth tokens began Jan 9, 2026, and the Feb 2026 clarification made the policy explicit. — [The Register 2026-02-20](https://www.theregister.com/software/2026/02/20/anthropic-clarifies-ban-on-third-party-tool-access-to-claude/5014546); [WinBuzzer](https://winbuzzer.com/2026/02/19/anthropic-bans-claude-subscription-oauth-in-third-party-apps-xcxwbn/); [Kersai](https://kersai.com/anthropic-killed-third-party-claude-access-heres-every-workaround-that-still-works/)
- OpenCode removed Claude Pro/Max support, with a commit citing "anthropic legal requests". — [The Register](https://www.theregister.com/software/2026/02/20/anthropic-clarifies-ban-on-third-party-tool-access-to-claude/5014546)
- An April 4, 2026 extension blocked third-party harnesses from subscription billing; it affected, for example, claude-mem's default path. — [claude-mem issue #1826](https://github.com/thedotmack/claude-mem/issues/1826)
- **Conflict:** BetterClaw claims a May 13, 2026 reversal with a planned "Agent SDK credit", paused June 15. Other recent sources still say the ban is in force. Treat it as unverified. — [BetterClaw](https://www.betterclaw.io/blog/openclaw-anthropic-subscription-ban), contradicted by [Kersai](https://kersai.com/anthropic-killed-third-party-claude-access-heres-every-workaround-that-still-works/)
- Calling the official `claude` CLI as a subprocess, rather than reusing its OAuth token, is the commonly recommended compliant workaround. — [vincentvandeth](https://vincentvandeth.nl/blog/best-openclaw-alternative-cli-subprocess-oauth-ban)

**Google**
- Gemini CLI's own ToS doc says that accessing the services behind Gemini CLI (e.g. Code Assist) with third-party software, such as OpenClaw with Gemini CLI OAuth, violates the terms and may lead to suspension. — [gemini-cli tos-privacy.md](https://github.com/google-gemini/gemini-cli/blob/main/docs/resources/tos-privacy.md)
- Google added "more robust detection" of policy-violating OAuth use in March 2026. Antigravity bans in early 2026 also cut users' Gemini CLI and Code Assist access, because of a shared backend. — [Discussion #22970](https://github.com/google-gemini/gemini-cli/discussions/22970); [Discussion #20632](https://github.com/google-gemini/gemini-cli/discussions/20632)
- June 18, 2026: Gemini CLI stopped consumer OAuth (free, AI Pro, Ultra), with the error "This client is no longer supported for Gemini Code Assist for individuals". Code Assist Standard/Enterprise and API keys (Gemini API / Vertex) still work with Gemini CLI. Consumer subscribers use Antigravity CLI. — [issue #28229](https://github.com/google-gemini/gemini-cli/issues/28229); [Discussion #27274](https://github.com/google-gemini/gemini-cli/discussions/27274); [syntackle](https://syntackle.com/blog/google-gemini-ai-subscription-with-opencode/)

### Inferences
- For a user holding all three consumer subscriptions, a compliant multi-agent setup is:
  - Codex CLI signed in with ChatGPT.
  - Claude Code signed in with Claude Pro/Max.
  - Antigravity CLI (`agy`) signed in with Google AI Pro/Ultra.
  - Each invoked as its own binary, either by a human-facing orchestrator (worktree manager) or by another agent through subprocess or plugin.
- Avoid tools that extract OAuth tokens and call vendor backends directly, especially for Anthropic and Google.
- Through PAL clink-style wrappers, each CLI still authenticates itself, which is likely compliant. API-model features of PAL need API keys.

### Gaps
- Did not read Anthropic's current Consumer Terms or OpenAI's official "Sign in with ChatGPT" doc directly; the exact date of the DevDay announcement is not confirmed.
- Whether Antigravity CLI's terms allow being driven as a subprocess by another agent is not confirmed.

## Q6. Context sharing: shared memory, handoff docs, repo-based state

### Takeaway
There is no cross-vendor memory protocol. In practice, shared context is repo-based: AGENTS.md for durable rules, shared `.agents/skills/`, and plain markdown handoff or plan files plus git history and PRs. Some bridges add explicit transfer commands, such as `/codex:transfer`.

### Cited Findings
- AGENTS.md as a single instruction source, plus imports or symlinks, is the main shared-context mechanism (see Q1). — [spec-weave](https://spec-weave.com/docs/guides/agents-md-vs-claude-md/); [parallelcode](https://parallelcode.app/blog/agents-md-vs-claude-md/)
- codex-plugin-cc provides `/codex:transfer` and `/codex:rescue` to hand the current task or context from Claude Code to Codex, with background job status and result retrieval. — [openai/codex-plugin-cc](https://github.com/openai/codex-plugin-cc)
- PAL MCP's clink returns only the subagent's final result to the parent, which keeps the parent context clean. PAL also supports conversation continuity across models. — [clink docs](https://github.com/BeehiveInnovations/pal-mcp-server/blob/main/docs/tools/clink.md)
- `gemini-code-context-mcp` gives Claude Code "persistent memory of your codebase" by using Gemini's context cache (API-key based). — [GitHub](https://github.com/qmediat/gemini-code-context-mcp)
- `codex-subagents-mcp` injects persona and context per call through a temp-dir AGENTS.md. — [GitHub](https://github.com/leonardsellem/codex-subagents-mcp)

### Inferences
- A recommended handoff pattern:
  - Keep a `docs/plan.md` or `HANDOFF.md` (task, decisions, open questions, files touched) in the worktree or branch.
  - Have each agent update it before finishing, and reference it from AGENTS.md.
  - Use commits and PR descriptions as the durable log.
  - This works across all three CLIs because each reads AGENTS.md and can read and write repo files.
- Vendor memories (Claude memory, ChatGPT memory, Gemini saved info) do not sync with each other. Chat-app context moves between ecosystems only through exported files or MCP-connected shared stores (e.g. a Notion or Drive MCP server connected to all three apps).

### Gaps
- No primary sources found on standardized cross-agent memory (no MCP memory standard adopted by all three). Handoff-file conventions are community practice, and I found no authoritative source describing them.
