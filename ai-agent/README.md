# ai-agent/

This folder is the **operating manual for AI coding agents** working in Playground. It is also where I log how those agents perform over time so I can recalibrate trust.

## Files

- [`instructions.md`](instructions.md) — what an AI agent should read and do before making changes.
- [`coding-agent-rules.md`](coding-agent-rules.md) — hard rules the agent must follow.
- [`prompts/`](prompts/) — reusable prompts I've found work well.
- [`review-logs/`](review-logs/) — logs of agent runs and what I changed.
- [`accepted-suggestions/`](accepted-suggestions/) — patterns the agent proposed that I keep using.
- [`rejected-suggestions/`](rejected-suggestions/) — patterns I actively reject and why.
- [`hallucinations/`](hallucinations/) — fabrications worth remembering.
- [`architecture-critiques/`](architecture-critiques/) — substantive design pushback worth saving.

## Why this folder exists

AI agents accelerate output. They do not, by default, improve _judgment_. The judgment is the point. This folder is how I keep score on my own AI use:

- Catalog reusable wins so I don't reinvent good prompts.
- Catalog reusable losses so I don't trip on the same mistakes.
- Catalog hallucinations to recalibrate trust.
- Force myself to articulate _why_ I disagreed when I overrode an agent.

## Logging cadence

After any non-trivial agent task, fill in [`templates/ai-agent-review-template.md`](../templates/ai-agent-review-template.md) and place it under the appropriate subfolder.
