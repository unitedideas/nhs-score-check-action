# NHS Agentic Readiness Check

GitHub Action that fetches your site's *agentic readiness score* from [Not Human Search](https://nothumansearch.ai) and optionally fails the build if it drops below a threshold.

Use this in CI to protect against regressions in your site's machine-readable surface — if someone accidentally ships a deploy that breaks your `llms.txt` or `OpenAPI` spec, the action flags it before it hits production.

[![NHS Score Check](https://github.com/unitedideas/nhs-score-check-action/workflows/self-test/badge.svg)](https://github.com/unitedideas/nhs-score-check-action/actions)
[![SafeSkill 92/100](https://img.shields.io/badge/SafeSkill-92%2F100_Verified%20Safe-brightgreen)](https://safeskill.dev/scan/unitedideas-nhs-score-check-action)

## What does it check?

NHS scores sites across 7 machine-readable signals:

| Signal | Points |
|---|---|
| llms.txt | 25 |
| ai-plugin.json | 20 |
| OpenAPI spec | 20 |
| Structured API | 15 |
| MCP server (verified via live JSON-RPC) | 10 |
| robots.txt AI rules | 5 |
| Schema.org | 5 |

Max score: **100/100**.

## Usage

```yaml
name: Agentic Readiness
on:
  schedule:
    - cron: '0 6 * * *'   # daily at 06:00 UTC
  workflow_dispatch:

jobs:
  check:
    runs-on: ubuntu-latest
    steps:
      - uses: unitedideas/nhs-score-check-action@v1
        with:
          domain: example.com
          min-score: 75
```

### Inputs

| Name | Required | Default | Description |
|---|---|---|---|
| `domain` | yes | — | Domain to score. Just the host, no `https://`. |
| `min-score` | no | `50` | Minimum acceptable score (0–100). Build fails if below. |
| `recrawl` | no | `false` | Request a fresh crawl first (~30s latency added). |

### Outputs

| Name | Description |
|---|---|
| `score` | Numeric score, 0–100. |
| `signals` | JSON map of which 7 signals were detected. |
| `report-url` | Human-readable NHS report page for the domain. |

## Badge

Once you're in NHS's index, embed your live score badge anywhere:

```markdown
[![NHS Agentic Readiness](https://nothumansearch.ai/badge/example.com.svg)](https://nothumansearch.ai/site/example.com)
```

## Example step summary

When the action runs, a readable summary is posted to `$GITHUB_STEP_SUMMARY` with the score, pass/fail status, and a per-signal breakdown.

## Related tools

- [Verify any MCP server in 3 curls](https://gist.github.com/unitedideas/ce709323717b95eb56f7be7392a0a557) — one-line shell probes to check whether a site advertising MCP support actually responds to `tools/list`. Pairs with the Q2 2026 MCP Ecosystem Health data showing only 10.3% of sites claiming MCP support actually answer a live probe.
- [Q2 2026 MCP Ecosystem Health](https://8bitconcepts.com/research/q2-2026-mcp-ecosystem-health.html) — methodology + raw CSV for the verification-gap numbers.
- [NHS MCP server one-line install](https://nothumansearch.ai/install) — wire the NHS search engine itself into Claude Code, Cursor, Cline, or Continue:

  ```bash
  curl -fsSL https://nothumansearch.ai/install | sh
  ```

## License

MIT
