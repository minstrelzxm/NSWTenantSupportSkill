# NSW Tenant Support skill

A skill for Anthropic's Claude that helps renters in New South Wales, Australia work through tenancy problems — repairs that never happen, agent harassment, termination notices, bond disputes, rent increases — by routing them to the right pathway, helping them build a written record, and always connecting them with **free specialist advice services**.

> **If you need help right now, you don't need this repository.**
> Find your free local Tenants' Advice and Advocacy Service: **https://www.tenants.org.au/get-advice**
> General legal triage (LawAccess NSW): **1300 888 529**
> Immediate danger: **000**

## What this is — and is not

This is **legal information, not legal advice**. The skill is built around one core principle: an AI's most useful role in a tenancy dispute is making the tenant *better documented, better prepared, and better connected to the free human advocates who already exist* — not replacing them. Its most repeated instruction is "call your Tenants' Advice and Advocacy Service before acting."

It will not tell you to withhold rent, vacate, or serve a termination notice without specialist advice, because those moves carry asymmetric legal risk.

## What it does

When installed, Claude gains a structured workflow for NSW tenancy matters:

1. **Put it in writing** — itemised, dated, deadline-bearing requests (linked to Tenants' Union sample letters), and same-day confirming emails after hostile calls.
2. **Free specialist advice** — routing by the rental property's local government area to the correct Tenants' Advice and Advocacy Service, with call-preparation help.
3. **Escalation down the right branch** — repairs → NSW Fair Trading complaint and rectification-order pathway; agent conduct → Fair Trading professional-conduct complaint; orders needed → NCAT.
4. **NCAT** — mapping facts to Residential Tenancies Act 2010 sections for the application form, respondent-naming traps, fee concessions, duty advocacy.

Built-in guards: deadline-first triage (retaliation and revocation windows expire in days), an urgency bypass for essential-service failures, a jurisdiction guard (NSW only), a coverage guard (boarders/lodgers, land lease communities, retirement villages and short-term stays fall outside the Act), and safety routing (violence → police; domestic violence → the dedicated s 105A pathway and specialist services).

## Repository contents

```
NSWTenantSupportSkill/
├── README.md                   # This file
├── LICENSE                     # MIT
└── nsw-tenant-support/         # The skill (Agent Skills open standard)
    ├── SKILL.md                # Workflow, ground rules, routing logic
    └── references/
        ├── legislation.md      # Situation → section map of the RTA 2010, deadlines table,
        │                       #   live-verification protocol, agent conduct rules
        ├── resources.md        # Verified links & phone numbers for every service, by stage
        └── letters.md          # Letter anatomy, confirming-email pattern, drafting variants
```

The packaged, installable build (`nsw-tenant-support.skill`) is attached to each [Release](../../releases) rather than committed to the repository.

## Installation

The skill follows the **Agent Skills open standard** — a folder containing `SKILL.md` plus reference files. It works in Claude products natively and in a growing set of other agents (Codex, Cursor, VS Code Copilot, and others) with a folder copy.

> Whichever platform you use, keep the folder structure intact: `SKILL.md` refers to the `references/` files by relative path. A flat pile of files breaks the skill.

### Claude.ai (web, desktop, and mobile apps)

Two routes:

1. **From the packaged file:** download `nsw-tenant-support.skill` from [Releases](../../releases), attach it to any chat, and click **Save skill** on the file card.
2. **From Settings:** zip the `nsw-tenant-support/` folder and upload it via **Settings → Features** (custom skills require a Pro, Max, Team, or Enterprise plan with code execution enabled; skills are individual to each user, not organisation-wide).

### Claude Code

```bash
# Personal (all projects)
git clone https://github.com/minstrelzxm/NSWTenantSupportSkill.git
cp -r NSWTenantSupportSkill/nsw-tenant-support ~/.claude/skills/

# Or project-scoped
cp -r NSWTenantSupportSkill/nsw-tenant-support .claude/skills/
```

Start a new session and run `/skills` to confirm it loaded. It triggers automatically on matching questions, or invoke it directly with `/nsw-tenant-support`.

### Claude Cowork

Upload the `.skill` file from [Releases](../../releases) through the skills/plugin interface. If your Cowork setup includes a GitHub skill-installer helper, you can also point it at this repository directly.

### Claude API / Agent SDK

For developers embedding this in an application: place the folder under your project's `.claude/skills/`, enable the `Skill` tool in `allowedTools`, and include `setting_sources` so skills load from the filesystem. See Anthropic's Agent Skills documentation (platform.claude.com → Agents and tools → Agent Skills) for the full pattern, including uploading skills via the Skills API on managed platforms.

**If you embed this in your own product, read [Porting responsibilities](#porting-responsibilities) below — it is not optional.**

### OpenAI Codex, Cursor, VS Code, and other agents

Several agents have adopted the same standard:

- **Codex CLI** reads skills from `.agents/skills/` by default — copy the folder there.
- **Cursor** supports skills through its plugin system; `.agents/skills/` is also recognised.
- **VS Code (Copilot)** recognises both `.claude/skills/` and `.agents/skills/`, with extra paths configurable via `chat.agentSkillsLocations`.
- Community tooling exists for cross-agent installs (e.g. `npx agent-skills-cli add minstrelzxm/NSWTenantSupportSkill --agent codex`), though a manual folder copy is always sufficient.

Paths and support levels change quickly — check your agent's own documentation if the skill doesn't appear.

**Fallback for agents without skill support:** paste the body of `SKILL.md` into the agent's system prompt / custom instructions / `AGENTS.md`, and keep the `references/` folder in the workspace where the agent can read files. This loses progressive disclosure (everything loads up front) but preserves behaviour.

### Smoke test (any platform, 30 seconds)

Ask: *"My landlord in Newcastle hasn't fixed our hot water for two weeks, what do I do?"*

You should see the skill engage, treat it as an urgent repair, ask about your rental suburb, and point you to a Tenants' Advice and Advocacy Service — with a note that it's providing information, not legal advice. If it doesn't, the skill hasn't loaded (check the folder structure and restart your session).

## Porting responsibilities

This is a legal-information skill for people in stressful situations. If you embed it anywhere — especially in your own app serving other users — three things must survive the port:

1. **The guardrails travel with the skill.** The ground rules in `SKILL.md` (not-legal-advice framing, routing to free Tenants' Advice and Advocacy Services, the never-advise-vacating rule, safety and domestic-violence routing, the jurisdiction and coverage guards) are the safety architecture, not boilerplate. Do not strip or summarise them away.
2. **Web access matters.** The skill's live-verification protocol has the model fetch current legislation before quoting sections, amounts, or time limits. If your agent cannot browse, it must not state `(verify)`-tagged specifics as fact — configure it to tell users to check the linked sources instead.
3. **Your users, your disclaimer.** If you serve this to others, carry a visible "legal information, not legal advice" notice in your own interface, and keep the crisis shortcuts (tenants.org.au/get-advice, LawAccess 1300 888 529, 000) reachable.

The skill's usefulness comes from making tenants better-documented and better-connected to free human advocates. Any embedding that turns it into a substitute for those advocates is a worse product and a riskier one.

## Design principles

- **Route to humans.** The Tenants' Advice and Advocacy Services are free, specialist, and effective. The skill treats them as the destination, not the fallback.
- **Map + live fetch, not a snapshot of the law.** The legislation reference is a curated map of which sections matter for which situations. Before quoting any section's words, amounts, or time periods, the skill instructs the model to fetch the current text from legislation.nsw.gov.au / AustLII. Anything not fully verified is explicitly tagged `(verify)` — the file admits where its certainty ends.
- **Deadlines before substance.** Several tenant rights expire within days of a notice being served. The skill computes clocks first and discusses merits second.
- **Fail closed on coverage.** Wrong-state and wrong-Act questions get redirected early rather than answered confidently and wrongly.

## Maintenance

NSW tenancy law changed substantially on 19 May 2025, and continues to move. The skill mitigates staleness through its live-verification protocol, but two things benefit from periodic human attention:

- **Link rot** — `references/resources.md` should be spot-checked against tenants.org.au and nsw.gov.au every few months.
- **Law changes** — watch the Tenants' Union law-change page (https://www.tenants.org.au/resource/law-change) and update `references/legislation.md` when the banner moves.

Issues and pull requests for corrections — especially from tenant advocates and community legal workers — are very welcome. If you work in this sector and spot an error, that feedback is worth more than any code contribution.

## Credits

The substantive knowledge here belongs to the people who built it over decades: the **Tenants' Union of NSW** and the **Tenants' Advice and Advocacy Services** (factsheets, sample letters, and the advice network this skill routes to), **NSW Fair Trading**, and **NCAT**. This skill links to their current materials rather than reproducing them, so users always land on the authoritative, up-to-date source. This project is not affiliated with or endorsed by any of these organisations.

## Disclaimer

This project provides general legal information about New South Wales tenancy law. It is not legal advice, it is not a substitute for advice from a qualified professional or a Tenants' Advice and Advocacy Service, and no responsibility is accepted for actions taken in reliance on it. AI models can make mistakes; verify anything important against the current legislation and with a human adviser before acting on it.

## License

MIT — see `LICENSE`. (The linked third-party materials remain under their own terms.)
