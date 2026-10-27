# Credits

These agent profiles were written from scratch for a one-owner team that uses spec-driven development ([github/spec-kit](https://github.com/github/spec-kit)). Ideas that shaped them:

- Role split with explicit "must not" limits and reviewers without write access: our own design, refined with [garrytan/gstack](https://github.com/garrytan/gstack) (role-based skills and release hygiene) and [shanraisshan/claude-code-best-practice](https://github.com/shanraisshan/claude-code-best-practice) (review in a separate context, subagents returning only conclusions).
- Moving repeated review findings into linters and architecture tests: [dmmulroy/anti-slop](https://github.com/dmmulroy/anti-slop).
- Security reviewer workflow: [cloudflare/security-audit-skill](https://github.com/cloudflare/security-audit-skill) (verdicts and coverage), [NVIDIA/SkillSpector](https://github.com/NVIDIA/SkillSpector) (vetting agent code before installing).
- Browser testing limits for QA and frontend: [microsoft/playwright-cli](https://github.com/microsoft/playwright-cli) and [vercel-labs/agent-browser](https://github.com/vercel-labs/agent-browser) documentation.
- Picking the lightest piece (skill, subagent, hook…): [Hainrixz/claude-anatomy](https://github.com/Hainrixz/claude-anatomy).
