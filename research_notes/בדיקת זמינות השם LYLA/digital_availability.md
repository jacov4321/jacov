# Digital Availability of "LYLA" and Variants (as of 2026-10-12)

## Environment and method (read this first)

### Takeaway
The sandbox's network was almost fully locked down. Nearly every live check (DNS, RDAP/WHOIS, registries, social sites) failed at the network layer. Only the GitHub search API (through the GitHub MCP connector) and server-side WebSearch worked. Most domain and handle rows below are therefore **UNKNOWN (blocked)**, not "available".

### Cited Findings
- `dig`, `nslookup`, `host` and `whois` binaries: **not installed** (`which dig nslookup whois host` returned nothing).
- `/etc/resolv.conf` points to 8.8.8.8/8.8.4.4, but direct DNS does not work. `python3 -c "socket.gethostbyname('pypi.org')"` returned `gaierror: Name or service not known`, and `getent hosts lyla.com github.com` returned nothing.
- The HTTPS proxy (`$HTTPS_PROXY` = http://127.0.0.1:44853) returned **`CONNECT tunnel failed, response 403`** (an org egress policy denial) for every one of these:
  - dns.google
  - cloudflare-dns.com
  - rdap.org
  - rdap.verisign.com
  - rdap.identitydigital.services (.ai)
  - rdap.nic.io
  - pubapi.registry.google (.dev)
  - rdap.isoc.org.il
  - whois.isoc.org.il
  - crates.io
  - hub.docker.com
  - formulae.brew.sh
  - packages.debian.org
  - archlinux.org
  - repology.org
  - www.reddit.com
  - x.com
  - www.instagram.com
  - www.youtube.com
  - itunes.apple.com
  - play.google.com
  - discord.com
  - lyla.com
  - www.lyla.ai
  - web.archive.org
- registry.npmjs.org and pypi.org are on the `NO_PROXY` list, but DNS can't resolve them:
  - `curl` gave `Could not resolve host`.
  - `npm view lyla` hung until it was killed (timeout 30s).
  - Forcing the requests through the proxy returned 403.
- `curl https://api.github.com/users/lyla` and `gh api users/lyla` returned HTTP 403: "This GitHub API path is not available: sessions are bound to their configured repositories."
- WebFetch failed with `getaddrinfo ENOTFOUND` for rdap.org and registry.npmjs.org.
- What worked: the GitHub search API through the MCP connector (`search_users`, `search_repositories`), and WebSearch, which is indirect evidence only.

### Inferences
- Every row marked UNKNOWN has to be re-run from a machine with normal internet access. The table at the end lists the exact commands to use.

### Gaps
- No DNS, RDAP or WHOIS evidence could be collected for any domain.

## Domains

### Takeaway
None of the 12 requested domains could be checked live. All of them are **UNKNOWN**. WebSearch did not surface any live site at lyla.com, lyla.ai or lyla.io. There is no `.os` TLD, so "lyla.os" cannot be registered; the IANA root zone has no .os.

### Cited Findings
- **Attempted:** `curl "https://dns.google/resolve?name=<d>&type=A|NS"` for lyla.com, lyla.ai, lyla.io, lyla.dev, lylaos.com, lylaos.io, lylaos.dev, lyla-os.com, getlyla.com, lyla.co.il, lylaos.co.il (plus lyla.app, lyla.org and lylaos.org).
  - **Result:** proxy 403 for every domain.
- **Attempted:** `curl https://rdap.org/domain/lyla.com`, plus the registry RDAP endpoints for .com, .ai, .io, .dev and .co.il.
  - **Result:** proxy 403. The WebFetch fallback failed with ENOTFOUND.
- **Attempted:** `curl https://lyla.com` and `curl https://www.lyla.ai` (to see whether a site is live).
  - **Result:** proxy 403.
- **WebSearch `"lyla.com"`:** returned no result hosted on lyla.com. It did return unrelated businesses using the name:
  - Lyla Lash (cosmetics) — [Product Hunt](https://www.producthunt.com/@lyla_lash)
  - Lyla Comserv SRL (Romania) — [targetare.ro](https://targetare.ro/9871644/lyla-comserv-srl)
  - Lyla Blanc — [oncosmetics](https://oncosmetics.com/company/lyla-blanc/)
  - "Lyla (fka Errand Solutions)" (Chicago) — [Datanyze](https://www.datanyze.com/people/Micky-York/450063199)
- **WebSearch `lyla.com OR lyla.ai OR lyla.io`:** none of the three domains appeared. The results were dominated by the similarly named "Layla" AI apps — [App Store id6456886656](https://apps.apple.com/app/id6456886656), [parse.gl](https://parse.gl/brands/layla-network-ai).

### Inferences
- **lyla.com:** a four-letter, pronounceable given name in .com is very likely registered, possibly parked or for sale. One possibility, not verified, is the Chicago "Lyla (fka Errand Solutions)" company. Treat it as "probably taken" until RDAP confirms.
- **lyla.ai and lyla.io:** short and attractive, so likely registered. This is unverified.
- **lylaos.\*, lyla-os.com and lylaos.co.il:** more likely to be free, but this is unverified.

### Gaps
- Registration status, registrant, creation date and live-site content for every domain. All of these were blocked by the network.

## Code hosting (GitHub)

### Takeaway
The GitHub username **"Lyla" is TAKEN**. **"lylaos" and "lyla-os" show no matching users**, so they are likely available. No repo named "lylaos" exists. Repos named "lyla" do exist, including an active TypeScript HTTP client and a Hack Club project.

### Cited Findings
- **`search_users` query `lyla in:login`:** 1,843 results. An exact login **`Lyla`** exists (id 293938, a low id that suggests an old account) — [github.com/Lyla](https://github.com/Lyla).
  - `search_repositories` with `user:Lyla` returned "Validation Failed … cannot be searched", so the account has no visible repos or is restricted.
  - Related logins also exist: `Lyla-Dev`, `lyla-vela`, `Lyla-M`.
- **`search_users` queries `lylaos in:login` and `lyla-os in:login`:** `total_count: 0`.
- **`search_repositories` query `lylaos in:name`:** `total_count: 0`.
- **`search_repositories` query `lyla in:name` (sorted by stars):** 533 repos. Notable ones:
  - [07akioni/lyla](https://github.com/07akioni/lyla): "A fully typed HTTP client…", TypeScript, 71 stars, updated 2026-06-09.
  - [Billy-Ellis/Lyla-Debugger](https://github.com/Billy-Ellis/Lyla-Debugger): an iOS debugger in C, 42 stars.
  - [hackclub/lyla](https://github.com/hackclub/lyla): Hack Club's moderation bot, updated 2026-09-27.
  - [jotajunior/lyla](https://github.com/jotajunior/lyla)
- **Direct checks blocked:** `api.github.com/users/<name>` returned 403 because the session is scoped to its repositories.
- **Nearby OS-name collision:** [jimx-/lyos](https://github.com/jimx-/lyos), "Lyos", an open-source microkernel OS.

### Inferences
- The user search is scoped to type:user, so an *organization* named lylaos or lyla-os would not show up here. Confirm by opening github.com/lylaos directly.
- "Lyos" is close in spelling and is in the same domain (an OS), so it is a minor confusion risk.

### Gaps
- Organization accounts were not checked directly.
- Other code hosts (GitLab, Codeberg) were not checked.

## Package registries

### Takeaway
- **PyPI `lyla`: TAKEN.** Versions 0.2.1 and 0.2.2 exist.
- **npm `lyla`: very likely TAKEN.** It is the 07akioni HTTP client, which also has an `@lylajs/*` scope.
- **crates.io, Docker Hub, Homebrew, Debian/Ubuntu and Arch: UNKNOWN** (blocked). WebSearch found no `lyla` crate.
- **`lylaos` on every registry: UNKNOWN.**

### Cited Findings
- **PyPI:** release pages exist at [pypi.org/project/lyla/0.2.2](https://pypi.org/project/lyla/0.2.2/) and [0.2.1](https://pypi.org/project/lyla/0.2.1/). The package has no description and was built with Hatch.
  - The direct check `curl https://pypi.org/pypi/lyla/json` failed with `Could not resolve host`.
- **npm:** the 07akioni/lyla README describes an unscoped `lyla` package plus `@lylajs/web` and other scoped packages — [github.com/07akioni/lyla](https://github.com/07akioni/lyla), via WebSearch.
  - The direct checks `curl https://registry.npmjs.org/lyla` and `npm view lyla` / `npm view lylaos` / `npm view lyla-os` could not resolve DNS or timed out.
- **crates.io:** `curl https://crates.io/api/v1/crates/lyla` returned a proxy 403. WebSearch found only lyd, lyt and le, with no "lyla" crate — [docs.rs/lyd](https://docs.rs/lyd).
- **Docker Hub** (`hub.docker.com/v2/...`), **Homebrew** (`formulae.brew.sh/api/formula/lyla.json`), **Debian** (`packages.debian.org/search?keywords=lyla`), **Arch** (`archlinux.org/packages/search/json/?name=lyla`) and **Repology:** all returned proxy 403.

### Inferences
- The npm claim relies on the README, not a registry response. It is high confidence but not directly verified.
- The `lylaos` package names are probably free everywhere, because no project by that name was found on GitHub or the web. This is unverified.

### Gaps
- Direct registry responses for every registry and every name.

## Social and app handles

### Takeaway
Every social handle check (X, Instagram, YouTube, Reddit, Discord) is **UNKNOWN** because the sites were blocked. App stores could only be searched indirectly. They show **"LYLA – Love Your Life App"** (iOS), a **"Lyla – Personal Style Assistant"** Android developer, and many heavily marketed **"Layla"** AI apps, which are a phonetic and visual confusion risk.

### Cited Findings
- **Attempted:** `curl https://x.com/lyla`, `https://www.instagram.com/lyla/`, `https://www.youtube.com/@lyla`, `https://www.reddit.com/r/lyla/about.json`, `https://discord.com/api/v9/invites/lyla`.
  - **Result:** proxy 403 for all of them.
- **Attempted:** `curl "https://itunes.apple.com/search?term=lyla&entity=software"` and `https://play.google.com/store/search?q=lyla`.
  - **Result:** proxy 403.
- **iOS:** "LYLA – Love Your Life App", a personal concierge app — [App Store id1264370883](https://apps.apple.com/cn/app/id1264370883).
- **Android:** the developer "Lyla – Personal Style Assistant" has 3 apps — [AppBrain](https://www.appbrain.com/dev/Lyla+-+Personal+Style+Assistant/).
- **"Layla" AI apps (similar name):**
  - "Layla", an on-device AI companion by Layla Network Pty Ltd (App Store id6456886656, Play package com.layla) — [App Store](https://apps.apple.com/app/id6456886656), [PromptQuorum review](https://www.promptquorum.com/power-local-llm/layla-review).
  - A separate "Layla AI" call-screening app — [mwm.ai](https://mwm.ai/apps/layla-ai/6751909122).
  - Layla AI, a Berlin AI travel agent — [vcbacked](https://www.vcbacked.co/company/layla-ai).
- **WebSearch for an official "Lyla" brand on Instagram or Twitter:** found only individual creators ("Lyla.fit", "Life With Lyla") — [Collabstr](https://collabstr.com/lifewithlyla). No startup brand was found.
- **WebSearch `"Lyla OS" OR "LylaOS" operating system`:** found no existing project with that name. The closest names were Lyos, LyraeOS and LyarOS — [osdev.org](https://f.osdev.org/viewtopic.php?p=348678), [zff.dev LyraeOS](https://zff.dev/LyraeOS/lyraeos_oxidized), [itch.io LyarOS](https://areds.itch.io/lyaros/devlog/685003/lyaros-01).

### Inferences
- @lyla on X, Instagram and YouTube is almost certainly held by an individual, because it is a common first name. Plan around @lylaos or @lyla_os instead. This is unverified.
- "LylaOS" appears to be unused as an OS name, which is a positive signal.

### Gaps
- Direct handle status on every platform.
- Exact app-store search counts.

## Summary availability table

| Asset | Status | Evidence / command |
|---|---|---|
| lyla.com | UNKNOWN (likely taken) | dns.google + rdap.org returned proxy 403; no site found via WebSearch |
| lyla.ai / lyla.io / lyla.dev | UNKNOWN | RDAP endpoints returned proxy 403 |
| lyla.os | N/A | no .os TLD exists |
| lylaos.com / .io / .dev, lyla-os.com, getlyla.com | UNKNOWN | DNS/RDAP returned proxy 403 |
| lyla.co.il / lylaos.co.il | UNKNOWN | rdap.isoc.org.il and whois.isoc.org.il returned proxy 403 |
| GitHub user `lyla` | TAKEN | search_users finds login `Lyla` (id 293938) |
| GitHub user `lylaos` / `lyla-os` | LIKELY AVAILABLE (users only; orgs not checked) | search_users total_count 0 |
| GitHub repo name `lylaos` | AVAILABLE (no repos) | search_repositories total_count 0 |
| GitHub repos named `lyla` | many exist (533) | 07akioni/lyla, hackclub/lyla, etc. |
| npm `lyla` | TAKEN (high confidence, indirect) | 07akioni README; registry not resolvable |
| npm `lylaos` | UNKNOWN | registry not resolvable |
| PyPI `lyla` | TAKEN | pypi.org/project/lyla/0.2.2 |
| PyPI `lylaos` | UNKNOWN | DNS failed |
| crates.io `lyla` | UNKNOWN (none found via search) | proxy 403 |
| Docker Hub / Homebrew / Debian / Arch `lyla` | UNKNOWN | proxy 403 |
| X / Instagram / YouTube @lyla | UNKNOWN (likely taken) | proxy 403 |
| Reddit r/lyla, Discord | UNKNOWN | proxy 403 |
| App stores "Lyla" | name in use | LYLA – Love Your Life App (iOS); Lyla Personal Style Assistant (Android); many "Layla" AI apps |

**Re-run commands for an unrestricted machine:**
- `whois lyla.com`
- `curl -s https://rdap.org/domain/<d>` for each domain
- `dig +short NS <d>`
- `curl -s https://api.github.com/users/lylaos` (and `/orgs/lylaos`)
- `npm view lylaos`
- `curl -s https://pypi.org/pypi/lylaos/json`
- `curl -s https://crates.io/api/v1/crates/lyla`
- `curl -s https://hub.docker.com/v2/namespaces/lyla/repositories`
- `brew info lyla`
- `apt-cache search lyla`
- `curl -s "https://archlinux.org/packages/search/json/?name=lyla"`
- `curl -s https://www.reddit.com/r/lyla/about.json`
