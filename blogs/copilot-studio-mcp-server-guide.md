---
# ===== Rank Math SEO fields (paste into the Rank Math panel) =====
focus_keyword: "Copilot Studio MCP"
additional_keywords:
  - "connect MCP server to Copilot Studio"
  - "Model Context Protocol Copilot Studio"
  - "Copilot Studio agent tools"
  - "Copilot Studio MCP server tutorial"
seo_title: "Copilot Studio MCP: Connect an MCP Server Step by Step"
slug: "copilot-studio-mcp"
meta_description: "Copilot Studio MCP guide: learn how to connect an MCP server to your agent, set up authentication, test tools and avoid common mistakes. Step by step."
category: "Copilot Studio"
tags: ["Copilot Studio", "MCP", "AI Agents", "Power Platform"]
author: "WR Vishnu"
canonical: "https://www.wrvishnu.com/copilot-studio-mcp/"
featured_image_alt: "Copilot Studio MCP server connected to an agent as a tool"
---

# Copilot Studio MCP: How to Connect an MCP Server to Your Agent (Step by Step)

**Copilot Studio MCP support lets your agent use tools and data from any Model Context Protocol (MCP) server, without you building a custom connector for every action.** In this guide you will learn what MCP is, why it matters for Copilot Studio agents, and exactly how to connect an MCP server, secure it, test it and troubleshoot it.

> **Quick answer (TL;DR):** In Copilot Studio, open your agent, go to **Tools → Add a tool → Model Context Protocol**, choose an existing MCP server or add your own server URL, pick the authentication type, and add the tools you want the agent to call. Then test in the test pane and publish.

> **Note on freshness:** Copilot Studio ships updates frequently. Menu names and options below reflect the documented experience; always compare with the official [Copilot Studio documentation](https://learn.microsoft.com/microsoft-copilot-studio/) and its *What's new* page before publishing, and add any October 2026 changes in the "Updates" section at the end.

## Table of Contents
1. What is Copilot Studio MCP?
2. Why use MCP in Copilot Studio?
3. Prerequisites
4. Step-by-step: connect an MCP server
5. Authentication options
6. Test and publish your agent
7. Best practices and security
8. Troubleshooting
9. Copilot Studio MCP vs custom connectors
10. FAQ
11. Conclusion

## What is Copilot Studio MCP?

The **Model Context Protocol (MCP)** is an open standard that defines how AI applications discover and call external tools and data sources. An MCP server exposes a set of **tools** (actions), and sometimes **resources** (data), in a consistent format that any MCP-aware client can understand.

**Copilot Studio MCP** is Microsoft Copilot Studio's support for acting as an MCP client. Your agent connects to an MCP server, reads the tools it publishes (names, descriptions and input schema) and decides when to call them during a conversation.

## Why use MCP in Copilot Studio?

- **Reuse, don't rebuild:** one MCP server can serve many agents and many AI platforms.
- **Automatic tool discovery:** when the server's tools change, the agent can pick up the updated tool list instead of you re-creating each action by hand.
- **Faster delivery:** makers add capabilities by connecting a server rather than writing flows or connectors.
- **Ecosystem access:** a growing number of vendors and open-source projects publish MCP servers.
- **Works with Foundry:** the same MCP approach is used by Azure AI Foundry agents, so skills transfer. See our companion post: [Build your first Azure AI Foundry agent](https://www.wrvishnu.com/azure-ai-foundry-agent-service/).

## Prerequisites

- A Copilot Studio license and access to a Power Platform environment where you can create agents.
- An existing agent (or create a new one).
- The **URL of a remote MCP server** that you are allowed to use, plus its authentication details (API key or OAuth settings).
- Your admin has not blocked MCP or the related connector in your environment's data policies (DLP).

## Step-by-step: connect an MCP server to Copilot Studio

### Step 1: Open your agent
Sign in to [Copilot Studio](https://copilotstudio.microsoft.com), select your environment, and open the agent you want to extend.

### Step 2: Add a tool
Go to the **Tools** page and select **Add a tool**.

### Step 3: Choose Model Context Protocol
In the tool picker, choose **Model Context Protocol**. You will see available MCP servers (for example, ones provided by Microsoft or your organization) and the option to add your own.

### Step 4: Add or select the MCP server
- Select a listed server, **or**
- Create a new connection by providing the **server name, description and server URL**.

Write a clear description. The agent's orchestrator uses names and descriptions to decide when to call a tool.

### Step 5: Configure authentication
Pick the authentication method your server requires (see the next section) and create the connection.

### Step 6: Select the tools
After the connection is created, Copilot Studio lists the tools the server exposes. Add the ones your agent needs. **Only enable what the agent truly requires** (least privilege).

### Step 7: Tune tool descriptions and instructions
In the agent's **Instructions**, tell the agent when to use the tool, for example: *"Use the inventory tool whenever the user asks about stock levels."* Good instructions reduce wrong tool calls.

## Authentication options

| Method | When to use it |
|---|---|
| **None** | Public, read-only servers only (never for sensitive data). |
| **API key** | Simple servers that accept a key in a header or query parameter. Store keys securely and rotate them. |
| **OAuth 2.0** | Servers acting on behalf of a user or app. Preferred for enterprise scenarios. |

Prefer OAuth 2.0 with least-privilege scopes for anything touching business data.

## Test and publish your agent

1. Open the **test pane** and ask a question that should trigger the tool.
2. Use the activity/tracking view to confirm which tool was called and with which inputs.
3. Fix descriptions or instructions if the wrong tool (or no tool) was selected.
4. **Publish** and add channels such as Microsoft Teams or a website.

## Best practices and security

- **Least privilege:** enable only needed tools and scopes.
- **Trust the server:** connect only to MCP servers you own or have vetted; tool descriptions and outputs are inputs to your agent and can be abused (prompt injection).
- **Human approval:** require confirmation before tools that write, delete or send data.
- **Govern with DLP:** use Power Platform admin center data policies to control who can connect what.
- **Monitor:** review analytics and conversation transcripts regularly.
- **Version and document:** keep a record of which MCP servers each agent uses.

## Troubleshooting

| Problem | Likely cause | Fix |
|---|---|---|
| Server not connecting | Wrong URL or unsupported transport | Confirm the remote endpoint URL and that the server supports streamable HTTP as required by Copilot Studio. |
| 401/403 errors | Bad credentials or scopes | Recreate the connection; check key or OAuth scopes. |
| Tool never called | Weak description/instructions | Rewrite descriptions; add explicit instructions. |
| Tools missing | Server not exposing them, or connection cached | Refresh the tool list; verify on the server side. |
| Blocked by policy | DLP | Ask your admin to review the data policy. |

## Copilot Studio MCP vs custom connectors

| | MCP server | Custom connector / flow |
|---|---|---|
| Setup | Connect once, tools discovered | Define each operation |
| Reusability | Works across many AI clients | Power Platform focused |
| Best for | Tool-rich, evolving APIs | Stable, well-defined REST APIs |

## FAQ

**What is Copilot Studio MCP?**
It is Copilot Studio's ability to connect an agent to Model Context Protocol servers so the agent can discover and call their tools.

**Do I need to write code to use MCP in Copilot Studio?**
No, to connect an existing server you only configure it in the Tools page. You need code only if you build your own MCP server.

**Is Copilot Studio MCP secure?**
It can be, if you use OAuth, least-privilege tools, trusted servers and DLP policies.

**Can I use the same MCP server in Azure AI Foundry?**
Yes. MCP is an open standard, so one server can serve both Copilot Studio and Foundry agents.

**Where do I find MCP servers?**
Check the servers listed in the Copilot Studio tool picker, your organization's catalog, and vendor documentation.

## Conclusion

Copilot Studio MCP is the fastest way to give your agent real capabilities: connect a server, secure it, select tools, write clear instructions, test and publish. Start with one low-risk, read-only server, then expand.

**Next step:** read how to build the pro-code side in [Azure AI Foundry Agent Service with tracing](https://www.wrvishnu.com/azure-ai-foundry-agent-service/), and share your results in the comments.

## Updates (October 2026 onwards)
*Add verified changes from the Copilot Studio What's New page here, with dates.*

## References
- [Microsoft Copilot Studio documentation](https://learn.microsoft.com/microsoft-copilot-studio/)
- [Model Context Protocol specification](https://modelcontextprotocol.io)

---
<!-- JSON-LD for Rank Math / schema (paste into Schema → Custom or use Article + FAQ) -->
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {"@type":"Question","name":"What is Copilot Studio MCP?","acceptedAnswer":{"@type":"Answer","text":"It is Copilot Studio's ability to connect an agent to Model Context Protocol servers so the agent can discover and call their tools."}},
    {"@type":"Question","name":"Do I need to write code to use MCP in Copilot Studio?","acceptedAnswer":{"@type":"Answer","text":"No. To connect an existing server you only configure it on the Tools page. Code is needed only to build your own MCP server."}},
    {"@type":"Question","name":"Can I use the same MCP server in Azure AI Foundry?","acceptedAnswer":{"@type":"Answer","text":"Yes. MCP is an open standard, so one server can serve both Copilot Studio and Foundry agents."}}
  ]
}
</script>
