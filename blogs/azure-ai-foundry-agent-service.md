---
# ===== Rank Math SEO fields (paste into the Rank Math panel) =====
focus_keyword: "Azure AI Foundry Agent Service"
additional_keywords:
  - "build AI agent in Azure AI Foundry"
  - "Foundry agent tracing"
  - "Azure AI Foundry tutorial"
  - "Application Insights agent tracing"
seo_title: "Azure AI Foundry Agent Service: Build Your First Agent"
slug: "azure-ai-foundry-agent-service"
meta_description: "Azure AI Foundry Agent Service tutorial: create a project, deploy a model, build and test your first AI agent, and turn on tracing. Beginner friendly."
category: "Azure AI Foundry"
tags: ["Azure AI Foundry", "AI Agents", "Azure", "Observability"]
author: "WR Vishnu"
canonical: "https://www.wrvishnu.com/azure-ai-foundry-agent-service/"
featured_image_alt: "Azure AI Foundry Agent Service playground with tracing enabled"
---

# Azure AI Foundry Agent Service: Build and Deploy Your First Agent with Tracing

**Azure AI Foundry Agent Service is Microsoft's managed service for building, deploying and operating AI agents that use models, tools and your own data.** This tutorial shows you how to create a project, deploy a model, build your first agent, test it and turn on tracing so you can see exactly what it does.

> **Quick answer (TL;DR):** Create a Foundry project → deploy a model → create an agent with clear instructions → add a tool (such as file search) → test in the playground → connect Application Insights and review traces.

> **Note on naming and freshness:** Microsoft has been renaming and evolving this product (Azure AI Foundry, now also referred to as Microsoft Foundry) and the portal changes often. Compare each step with the official [Foundry documentation](https://learn.microsoft.com/azure/ai-foundry/) and its *What's new* page, and record October 2026 changes in the "Updates" section.

## Table of Contents
1. What is Azure AI Foundry Agent Service?
2. When to use it (and when Copilot Studio is better)
3. Prerequisites
4. Step-by-step: build your first agent
5. Turn on tracing
6. Evaluate and improve
7. Security and cost tips
8. Troubleshooting
9. FAQ
10. Conclusion

## What is Azure AI Foundry Agent Service?

**Azure AI Foundry Agent Service** hosts agents for you. An agent combines:

- **A model** (for example, a GPT-family model deployed in your project),
- **Instructions** that define its role and behavior,
- **Tools** such as file search, Azure AI Search grounding, code interpreter, function calling or MCP servers,
- **Threads and runs** that manage conversation state.

Because the service manages orchestration and state, you focus on behavior and data rather than infrastructure.

## When to use it (and when Copilot Studio is better)

| Need | Choose |
|---|---|
| Low-code, makers, Microsoft 365 and Teams channels | **Copilot Studio** |
| Pro-code control, custom tools, model choice, deep observability | **Azure AI Foundry Agent Service** |
| Both | Build in Foundry and connect it with Copilot Studio |

Related: [Copilot Studio MCP guide](https://www.wrvishnu.com/copilot-studio-mcp/).

## Prerequisites

- An Azure subscription with permission to create resources.
- Access to a Foundry project in a region that offers the model you want.
- Quota for a chat model deployment.
- Optional: an Application Insights resource for tracing.
- Python 3.9+ if you later move from the portal to the SDK.

## Step-by-step: build your first agent

### Step 1: Create a Foundry project
Open the [Azure AI Foundry portal](https://ai.azure.com), select **Create new project**, name it and choose a region. Confirm the resource group and hub/resource settings.

### Step 2: Deploy a model
Go to the **Model catalog**, choose a chat model suitable for agents, and **Deploy** it. Note the deployment name; you will select it for the agent.

### Step 3: Create the agent
Open **Agents** and select **New agent**. Set:
- **Name:** something descriptive, e.g. `hr-policy-helper`.
- **Deployment:** the model you just deployed.
- **Instructions:** be specific. Example: *"You answer questions about company HR policy using only the attached documents. If the answer is not in the documents, say you don't know."*

### Step 4: Add a tool or knowledge
Add **file search** and upload a few documents, or connect **Azure AI Search** for an existing index. Grounding in your own data reduces hallucinations.

### Step 5: Test in the playground
Ask questions with known answers and questions with no answer. Check that the agent cites your data and declines unsupported questions.

### Step 6: Iterate
Refine instructions, tool selection and temperature/parameters until responses are reliable.

## Turn on tracing

Tracing records each step: prompts, tool calls, latency and token usage.

1. In your project, open the **Tracing** area.
2. **Connect an Application Insights** resource.
3. Run a few test conversations.
4. Open the traces to inspect the sequence of model calls and tools, errors and timings.

Use traces to answer: *Which tool was called? How long did it take? How many tokens were used? Where did it fail?*

## Evaluate and improve

- Build a small test set of real questions with expected answers.
- Use Foundry's evaluation features to score quality and safety.
- Re-run after each change to catch regressions.

## Security and cost tips

- Use **Microsoft Entra ID / managed identity** instead of keys where possible.
- Apply **RBAC** with least privilege to the project.
- Configure **content safety** filters.
- Don't put sensitive data in instructions or logs; review what tracing stores.
- Set quotas, monitor token usage and delete unused deployments to control cost.

## Troubleshooting

| Problem | Likely cause | Fix |
|---|---|---|
| Can't deploy model | No quota/region | Request quota or change region. |
| Agent ignores documents | File search not attached or poor instructions | Verify tool is enabled; tighten instructions. |
| No traces | App Insights not connected | Reconnect the resource and rerun. |
| Authorization errors | Missing role | Assign the required Azure RBAC role. |

## FAQ

**What is Azure AI Foundry Agent Service?**
A managed service to build, deploy and run AI agents with models, tools and your data.

**Do I need to code to build an agent?**
No. You can create and test agents in the portal; the SDK is for automation and production.

**How is it different from Copilot Studio?**
Copilot Studio is low-code and Microsoft 365 oriented; Foundry Agent Service is pro-code with more control and observability.

**How do I see what my agent did?**
Enable tracing with Application Insights and review the traces.

**Can Foundry agents use MCP servers?**
Yes, agents can use tools exposed through MCP; check the current docs for supported configuration.

## Conclusion

Azure AI Foundry Agent Service gives you a fast path from idea to a working, observable agent: project, model, instructions, tools, test, trace. Start small with one grounded agent and add evaluation before going to production.

**Next step:** compare the low-code route in our [Copilot Studio MCP guide](https://www.wrvishnu.com/copilot-studio-mcp/) and share what you build.

## Updates (October 2026 onwards)
*Add verified changes from the Foundry What's New page here, with dates.*

## References
- [Azure AI Foundry documentation](https://learn.microsoft.com/azure/ai-foundry/)
- [Azure Monitor Application Insights](https://learn.microsoft.com/azure/azure-monitor/app/app-insights-overview)

---
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {"@type":"Question","name":"What is Azure AI Foundry Agent Service?","acceptedAnswer":{"@type":"Answer","text":"A managed service to build, deploy and run AI agents with models, tools and your own data."}},
    {"@type":"Question","name":"Do I need to code to build an agent?","acceptedAnswer":{"@type":"Answer","text":"No. You can create and test agents in the portal; the SDK is for automation and production."}},
    {"@type":"Question","name":"How do I see what my agent did?","acceptedAnswer":{"@type":"Answer","text":"Enable tracing with Application Insights and review the traces."}}
  ]
}
</script>
