---
name: maf-fetch-docs
description: Fetches live Microsoft Agent Framework documentation using Microsoft Learn MCP as the primary source, with web fetch as fallback. Use this skill when the static migration KB doesn't cover a question, when current package versions are needed, or when official samples or release notes must be retrieved.
---

# MAF Fetch Docs Skill

Retrieves live MAF documentation using a resilient two-source strategy.
Always try sources in order and stop at the first successful result.

> **Currency note:** prefer the latest stable 1.x patch information when package
> feeds or release pages have moved beyond the initial `1.0.0` GA release.

---

## Source Priority

### 1. Microsoft Learn MCP (Primary)

Use the host's Microsoft Learn search/fetch tooling if available for structured,
queryable documentation retrieval. It handles versioning better than hard-coded
URLs and avoids unnecessary URL fragility.

If the current host does not expose Microsoft Learn tooling, skip directly to
source 2.

**Query patterns by user need:**

| User need | MCP query |
|-----------|-----------|
| Python breaking changes | `agent framework python 2026 significant changes upgrade` |
| .NET breaking changes | `agent framework dotnet significant changes upgrade` |
| Foundry namespace | `agent framework foundry namespace migration` |
| Session/thread API | `agent framework session agentsession migration` |
| Package install | `agent framework package install nuget pypi 1.0` |
| RC migration guide | `semantic kernel agent framework RC migration guide` |
| Latest samples | `agent framework samples dotnet python foundry` |

If MCP returns no results or errors -> proceed to source 2.

---

### 2. Direct URL Fetch (Fallback)

Use the host's generic web fetch capability on these URLs in priority order
based on user need.

**Python migration:**
1. `https://learn.microsoft.com/en-us/agent-framework/support/upgrade/python-2026-significant-changes`
2. `https://github.com/microsoft/agent-framework/releases`
3. `https://pypi.org/project/agent-framework-foundry/`

**.NET / C# migration:**
1. `https://learn.microsoft.com/en-us/agent-framework/overview/`
2. `https://github.com/microsoft/agent-framework/releases/tag/dotnet-1.0.0`
3. `https://www.nuget.org/packages/Microsoft.Agents.AI.Foundry`

**Cross-language / general:**
1. `https://devblogs.microsoft.com/agent-framework/microsoft-agent-framework-version-1-0/`
2. `https://learn.microsoft.com/en-us/agent-framework/`
3. `https://github.com/microsoft/agent-framework/releases`

**Current patch / release verification:**
1. `https://github.com/microsoft/agent-framework/releases`
2. `https://www.nuget.org/packages/Microsoft.Agents.AI.Foundry`
3. `https://pypi.org/project/agent-framework-foundry/`

**RC history (rc1-rc6):**
1. `https://devblogs.microsoft.com/foundry/microsoft-agent-framework-reaches-release-candidate/`
2. `https://devblogs.microsoft.com/foundry/whats-new-in-microsoft-foundry-feb-2026/`
3. `https://learn.microsoft.com/en-us/semantic-kernel/support/migration/agent-framework-rc-migration-guide`

If URL fetch also fails -> fall back to `/maf-migration-kb` static knowledge
and inform the user: *"Live docs unavailable - using embedded KB. Verify
against official docs when connectivity is restored."*

---

## Response Rules After a Successful Fetch

1. **Scope to the question** - extract only the relevant section, not full page dumps
2. **Lead with code** - if before/after examples exist in the fetched content, surface those first
3. **Call out versions explicitly** - always state which rc or release a sample applies to
4. **Flag preview content** - if content is marked Preview or Beta, tell the user
5. **Cite the source** - always state which URL or MCP query produced the result
6. **Call out stale pins** - if a prompt or sample hard-codes `1.0.0` but a newer stable `1.x` patch exists, say so explicitly
7. **Prefer authoritative pages** - for current versions, package feeds and official release pages outrank blog posts
