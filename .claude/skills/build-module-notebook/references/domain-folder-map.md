# Domain → Folder Map

Source: [claudecertificationguide.com/learn](https://claudecertificationguide.com/learn),
as fetched when this repo was set up. Match a module URL's domain-slug by its
**leading number only** — the rest of the slug's wording isn't load-bearing
and could drift without notice.

| Leading digit in domain-slug | Repo folder | Domain name | Weight | Modules |
|---|---|---|---|---|
| `1-...` | `01-agentic-architecture-orchestration` | Agentic Architecture & Orchestration | 27% | 7 |
| `2-...` | `02-tool-design-mcp-integration` | Tool Design & MCP Integration | 18% | 5 |
| `3-...` | `03-claude-code-configuration-workflows` | Claude Code Configuration & Workflows | 20% | 6 |
| `4-...` | `04-prompt-engineering-structured-output` | Prompt Engineering & Structured Output | 20% | 6 |
| `5-...` | `05-context-management-reliability` | Context Management & Reliability | 15% | 6 |

## Verified example

- URL: `https://claudecertificationguide.com/learn/1-agentic-architecture/1-1-agentic-loops`
- Domain-slug: `1-agentic-architecture` → leading digit `1` → `01-agentic-architecture-orchestration`
- Module-slug: `1-1-agentic-loops` → filename `1_1_agentic_loops.ipynb`
- Full target path: `01-agentic-architecture-orchestration/src/1_1_agentic_loops.ipynb`

## If the pattern doesn't match

The exact domain-slug text for domains 2–5 hasn't been directly observed yet
(only domain 1's slug has been confirmed against a real URL). If a URL's
leading digit is outside 1–5, or something about the URL structure looks
different from the example above, don't guess — re-fetch
`https://claudecertificationguide.com/learn` to check whether the site's
domain structure or URL scheme changed, and confirm with the user before
creating a new folder or deviating from this map.
