---
name: maf-migration-kb
description: Microsoft Agent Framework rc5 -> 1.0 breaking change catalog for C# and Python. Use this skill when migrating MAF projects, looking up renamed types, fixing build errors after upgrading packages, or understanding what changed between rc5 and GA.
---

# MAF Migration Knowledge Base: rc5 -> 1.x

> **Scope:** User must be on rc5 or higher. If on rc4 or lower, direct them
> to upgrade to rc5 first via the RC migration guide before continuing.
>
> **Currency:** This KB is a static quick-reference. Prefer the latest stable
> `1.x` patch guidance from official release pages and package feeds when
> recommending concrete package versions.
>
> **Coverage boundary:** This KB focuses on the highest-value breaking changes
> for real migrations. It is not a complete changelog for every rc1-rc6 change.

---

## 1. Package & Installation Changes (Both Languages)

| Change | rc5 | 1.x |
|--------|-----|-----|
| Core packages | Required `--pre` flag | Stable - no `--pre` needed |
| Foundry package (Python) | `agent-framework-azure-ai` | `agent-framework-foundry` |
| Foundry namespace (.NET) | `Microsoft.Agents.AI.AzureAI` | `Microsoft.Agents.AI.Foundry` |
| Dependency floor | Loose RC compatibility | `>=1.0.0,<2` - breaks older RC installs |
| Python provider dependencies | Often arrived transitively | Install provider packages explicitly when importing `agent_framework.openai` or `agent_framework.foundry` |
| Still-beta connectors | Mixed | `ag-ui`, `azurefunctions`, `copilotstudio`, `foundry-local`, `github-copilot`, `mem0`, `ollama` - still need `--pre` |

For package commands, prefer the latest stable `1.x` patch instead of assuming
`1.0.0` is still the newest available release.

---

## 2. Foundry Namespace & Class Renames (Highest Customer Pain)

### .NET

| rc5 | 1.x |
|-----|-----|
| `Microsoft.Agents.AI.AzureAI` | `Microsoft.Agents.AI.Foundry` |
| `ServiceStoredSimulatingChatClient` | `PerServiceCallChatHistoryPersistingChatClient` |
| `AzureAIAgent` | `FoundryAgent` |
| Separate `FoundryMemory` package | Consolidated into `Microsoft.Agents.AI.Foundry` |

### Python

| rc5 | 1.x |
|-----|-----|
| `agent_framework_azure_ai` (package removed) | `agent_framework.foundry` |
| `AzureAIAgentClient` / `AzureAIClient` | `FoundryChatClient` or `FoundryAgent` |
| `AzureAIProjectAgentProvider` / `AzureAIAgentsProvider` | `FoundryAgent` |
| `OpenAIAssistantsClient` / `OpenAIResponsesClient` | `OpenAIChatClient` |

---

## 3. Session / Thread API (Compile-Breaking for Named Argument Users)

### .NET
```csharp
// rc5
var thread = agent.GetNewThread();
var result = await agent.RunAsync("Hello", thread: thread);

// 1.x
var session = agent.CreateSession();
var result = await agent.RunAsync("Hello", session: session);
```

### Python
```python
# rc5
thread = agent.get_new_thread()
result = await agent.run("Hello", thread=thread)

# 1.x
session = await agent.create_session()
result = await agent.run("Hello", session=session)
```

---

## 4. Credential & Auth Changes

### Python
```python
# rc5
from agent_framework_azure_ai import get_entra_auth_token
agent = AzureAIAgent(ad_token=get_entra_auth_token(), ad_token_provider=my_provider)

# 1.x
from azure.identity import DefaultAzureCredential
agent = AzureAIAgent(credential=DefaultAzureCredential())
```

### .NET
```csharp
// rc5
var client = new AzureAIProjectClient(endpoint, apiKey);

// 1.x
var client = new AIProjectClient(new Uri(endpoint), new DefaultAzureCredential());
```

> **Silent breaking change (Python):** Generic OpenAI clients no longer
> auto-switch to Azure just because `AZURE_OPENAI_*` env vars are present.
> Must pass explicit `credential=` or `azure_endpoint=` to use Azure OpenAI.

If both `OPENAI_*` and `AZURE_OPENAI_*` settings are present, audit every
generic OpenAI client construction and make provider routing explicit.

---

## 5. Instructions Placement (.NET)

```csharp
// rc5
var options = new ChatClientAgentOptions { Instructions = "..." };
var agent = new ChatClientAgent(client, options);

// 1.x
var agent = new ChatClientAgent(client, instructions: "...");
```

---

## 6. Streaming API (Python)

```python
# rc5
async for chunk in agent.run_stream("Analyze this"):
    print(chunk, end="")

# 1.x - stream= flag on run()
async for chunk in agent.run("Analyze this", stream=True):
    print(chunk, end="")
```

---

## 7. Model Parameter Unification (Python)

```python
# rc5 - inconsistent per provider
client = SomeClient(model_id="gpt-4o")
client = AzureClient(deployment_name="gpt-4o")

# 1.x - unified everywhere
client = OpenAIChatClient(model="gpt-4o")
client = FoundryChatClient(model="gpt-4o", ...)
```

**Environment variable renames:**

| rc5 | 1.0 |
|-----|-----|
| `OPENAI_MODEL_ID` | `OPENAI_MODEL` / `OPENAI_CHAT_MODEL` |
| `AZURE_OPENAI_DEPLOYMENT` | `AZURE_OPENAI_MODEL` / `AZURE_OPENAI_CHAT_MODEL` |
| *(none)* | `ANTHROPIC_CHAT_MODEL` |
| *(none)* | `FOUNDRY_LOCAL_MODEL` |

---

## 8. Message Construction (Python)

```python
# rc5
msg = Message(role="user", text="Hello")

# 1.x - text= removed
msg = Message(role="user", contents=["Hello"])
```

Applies everywhere: workflow requests, middleware, orchestration helpers.

---

## 9. Workflow Runtime Kwargs (Python)

```python
# rc5 and earlier compatibility paths
await workflow.run(
  "Draft the report",
  db_config={"connection_string": "..."},
  user_preferences={"format": "markdown"},
)

# 1.x - explicit buckets
await workflow.run(
  "Draft the report",
  function_invocation_kwargs={
    "researcher": {"db_config": {"connection_string": "..."}},
    "writer": {"user_preferences": {"format": "markdown"}},
  },
)
```

For agent and workflow code that still depends on blanket forwarded `**kwargs`,
move runtime data into `function_invocation_kwargs` and `client_kwargs`.

---

## 10. Removed Base Provider Aliases (Python)

```python
# rc5 / rc6 compatibility alias
from agent_framework import BaseHistoryProvider

# 1.x
from agent_framework import HistoryProvider
```

`BaseContextProvider` and `BaseHistoryProvider` compatibility aliases were
removed in the stable 1.0 release.

---

## 11. Microsoft.Extensions.AI Version (.NET)

```xml
<!-- rc5 -->
<PackageReference Include="Microsoft.Extensions.AI" Version="9.*-preview.*" />

<!-- 1.x -->
<PackageReference Include="Microsoft.Extensions.AI" Version="10.4.1" />
```

Treat `10.4.1` as the minimum known-good GA floor and prefer the latest stable
compatible patch when issuing final package commands.

---

## 12. Known rc6 Gotcha

`RawFoundryAgentChatClient` may fail when tools contain schemas the Foundry
service doesn't expect. Validate tool schemas against the Foundry endpoint
after migrating if this client is in use.

---

## Official Doc URLs (for live fetch)

| Resource | URL |
|----------|-----|
| Python 2026 Significant Changes | `https://learn.microsoft.com/en-us/agent-framework/support/upgrade/python-2026-significant-changes` |
| RC Migration Guide | `https://learn.microsoft.com/en-us/semantic-kernel/support/migration/agent-framework-rc-migration-guide` |
| Agent Framework Overview | `https://learn.microsoft.com/en-us/agent-framework/overview/` |
| GitHub Releases (all) | `https://github.com/microsoft/agent-framework/releases` |
| .NET 1.0.0 Release Tag | `https://github.com/microsoft/agent-framework/releases/tag/dotnet-1.0.0` |
| 1.0 GA Blog | `https://devblogs.microsoft.com/agent-framework/microsoft-agent-framework-version-1-0/` |
| NuGet - Agents.AI.Foundry | `https://www.nuget.org/packages/Microsoft.Agents.AI.Foundry` |
| PyPI - agent-framework-foundry | `https://pypi.org/project/agent-framework-foundry/` |
| .NET Samples | `https://github.com/microsoft/agent-framework/tree/main/dotnet/samples` |
| Python Samples | `https://github.com/microsoft/agent-framework/tree/main/python/samples` |

Use GitHub releases plus the NuGet/PyPI package pages to confirm whether a newer
stable patch than `1.0.0` is already available.

---

## Escalation Path

If the KB and live docs don't resolve the issue:
1. Check the official release pages first: `https://github.com/microsoft/agent-framework/releases`
2. Check current package feeds: NuGet for `.NET`, PyPI for Python
3. GitHub Issues (filter: `breaking-change`, `foundry`): `https://github.com/microsoft/agent-framework/issues`
4. GitHub Discussions: `https://github.com/microsoft/agent-framework/discussions`
5. If pinning is required, pin to the latest verified stable `1.x` patch rather than assuming `1.0.0`
