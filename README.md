# NSW Tenant Support — a Claude skill

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
nsw-tenant-support/
├── SKILL.md                    # Workflow, ground rules, routing logic
├── references/
│   ├── legislation.md          # Situation → section map of the RTA 2010, deadlines table,
│   │                           #   live-verification protocol, agent conduct rules
│   ├── resources.md            # Verified links & phone numbers for every service, by stage
│   └── letters.md              # Letter anatomy, confirming-email pattern, drafting variants
├── evals/evals.json            # Test prompts used during development
└── nsw-tenant-support.skill    # Packaged, installable build (see Releases)
```

## Installation

**Claude.ai / Claude apps:** attach the `.skill` file from Releases to a chat and click **Save skill** on the file card. It will then trigger automatically on NSW tenancy questions.

**Claude Code:** copy the `nsw-tenant-support/` folder into your skills directory (for example `~/.claude/skills/`).

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
