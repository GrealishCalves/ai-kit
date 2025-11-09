# MCP Connector Documentation

Scraped from docs.claude.com/en/docs/agents-and-tools/mcp-connector

Total pages: 17

---

## 1. Remote MCP servers

**URL:** https://docs.claude.com/en/docs/agents-and-tools/remote-mcp-servers

# Remote MCP servers

export const MCPServersTable = ({platform = "all"}) => {
const generateClaudeCodeCommand = server => {
if (server.customCommands && server.customCommands.claudeCode) {
return server.customCommands.claudeCode;
}
if (server.urls.http) {
return `claude mcp add --transport http ${server.name.toLowerCase().replace(/[^a-z0-9]/g, '-')} ${server.urls.http}`;
}
if (server.urls.sse) {
return `claude mcp add --transport sse ${server.name.toLowerCase().replace(/[^a-z0-9]/g, '-')} ${server.urls.sse}`;
}
if (server.urls.stdio) {
const envFlags = server.authentication && server.authentication.envVars ? server.authentication.envVars.map(v => `--env ${v}=YOUR_${v.split('_').pop()}`).join(' ') : '';
const baseCommand = `claude mcp add --transport stdio ${server.name.toLowerCase().replace(/[^a-z0-9]/g, '-')}`;
return envFlags ? `${baseCommand} ${envFlags} -- ${server.urls.stdio}` : `${baseCommand} -- ${server.urls.stdio}`;
}
return null;
};
const servers = [{
name: "Airtable",
category: "Databases & Data Management",
description: "Read/write records, manage bases and tables",
documentation: "https://github.com/domdomegg/airtable-mcp-server",
urls: {
stdio: "npx -y airtable-mcp-server"
},
authentication: {
type: "api_key",
envVars: ["AIRTABLE_API_KEY"]
},
availability: {
claudeCode: true,
mcpConnector: false,
claudeDesktop: true
}
}, {
name: "Figma",
category: "Design & Media",
description: "Generate better code by bringing in full Figma context",
documentation: "https://developers.figma.com",
urls: {
http: "https://mcp.figma.com/mcp"
},
customCommands: {
claudeCode: "claude mcp add --transport http figma-remote-mcp https://mcp.figma.com/mcp"
},
availability: {
claudeCode: true,
mcpConnector: false,
claudeDesktop: false
},
notes: "Visit developers.figma.com for local server setup."
}, {
name: "Asana",
category: "Project Management & Documentation",
description: "Interact with your Asana workspace to keep projects on track",
documentation: "https://developers.asana.com/docs/using-asanas-model-control-protocol-mcp-server",
urls: {
sse: "https://mcp.asana.com/sse"
},
authentication: {
type: "oauth"
},
availability: {
claudeCode: true,
mcpConnector: true,
claudeDesktop: false
}
}, {
name: "Atlassian",
category: "Project Management & Documentation",
description: "Manage your Jira tickets and Confluence docs",
documentation: "https://www.atlassian.com/platform/remote-mcp-server",
urls: {
sse: "https://mcp.atlassian.com/v1/sse"
},
authentication: {
type: "oauth"
},
availability: {
claudeCode: true,
mcpConnector: true,
claudeDesktop: false
}
}, {
name: "ClickUp",
category: "Project Management & Documentation",
description: "Task management, project tracking",
documentation: "https://github.com/hauptsacheNet/clickup-mcp",
urls: {
stdio: "npx -y @hauptsache.net/clickup-mcp"
},
authentication: {
type: "api_key",
envVars: ["CLICKUP_API_KEY", "CLICKUP_TEAM_ID"]
},
availability: {
claudeCode: true,
mcpConnector: false,
claudeDesktop: true
}
}, {
name: "Cloudflare",
category: "Infrastructure & DevOps",
description: "Build applications, analyze traffic, monitor performance, and manage security settings through Cloudflare",
documentation: "https://developers.cloudflare.com/agents/model-context-protocol/mcp-servers-for-cloudflare/",
urls: {},
availability: {
claudeCode: true,
mcpConnector: true,
claudeDesktop: false
},
notes: "Multiple services available. See documentation for specific server URLs. Claude Code can use the Cloudflare CLI if installed."
}, {
name: "Cloudinary",
category: "Design & Media",
description: "Upload, manage, transform, and analyze your media assets",
documentation: "https://cloudinary.com/documentation/cloudinary_llm_mcp#mcp_servers",
urls: {},
authentication: {
type: "oauth"
},
availability: {
claudeCode: true,
mcpConnector: true,
claudeDesktop: false
},
notes: "Multiple services available. See documentation for specific server URLs."
}, {
name: "Intercom",
category: "Project Management & Documentation",
description: "Access real-time customer conversations, tickets, and user data",
documentation: "https://developers.intercom.com/docs/guides/mcp",
urls: {
http: "https://mcp.intercom.com/mcp"
},
authentication: {
type: "oauth"
},
availability: {
claudeCode: true,
mcpConnector: true,
claudeDesktop: false
}
}, {
name: "invideo",
category: "Design & Media",
description: "Build video creation capabilities into your applications",
documentation: "https://invideo.io/ai/mcp",
urls: {
sse: "https://mcp.invideo.io/sse"
},
authentication: {
type: "oauth"
},
availability: {
claudeCode: true,
mcpConnector: true,
claudeDesktop: false
}
}, {
name: "Linear",
category: "Project Management & Documentation",
description: "Integrate with Linear's issue tracking and project management",
documentation: "https://linear.app/docs/mcp",
urls: {
http: "https://mcp.linear.app/mcp"
},
authentication: {
type: "oauth"
},
availability: {
claudeCode: true,
mcpConnector: true,
claudeDesktop: false
}
}, {
name: "Notion",
category: "Project Management & Documentation",
description: "Read docs, update pages, manage tasks",
documentation: "https://developers.notion.com/docs/mcp",
urls: {
http: "https://mcp.notion.com/mcp"
},
authentication: {
type: "oauth"
},
availability: {
claudeCode: true,
mcpConnector: false,
claudeDesktop: false
}
}, {
name: "PayPal",
category: "Payments & Commerce",
description: "Integrate PayPal commerce capabilities, payment processing, transaction management",
documentation: "https://www.paypal.ai/",
urls: {
http: "https://mcp.paypal.com/mcp"
},
authentication: {
type: "oauth"
},
availability: {
claudeCode: true,
mcpConnector: true,
claudeDesktop: false
}
}, {
name: "Plaid",
category: "Payments & Commerce",
description: "Analyze, troubleshoot, and optimize Plaid integrations. Banking data, financial account linking",
documentation: "https://plaid.com/blog/plaid-mcp-ai-assistant-claude/",
urls: {
sse: "https://api.dashboard.plaid.com/mcp/sse"
},
authentication: {
type: "oauth"
},
availability: {
claudeCode: true,
mcpConnector: true,
claudeDesktop: false
}
}, {
name: "Sentry",
category: "Development & Testing Tools",
description: "Monitor errors, debug production issues",
documentation: "https://docs.sentry.io/product/sentry-mcp/",
urls: {
http: "https://mcp.sentry.dev/mcp"
},
authentication: {
type: "oauth"
},
availability: {
claudeCode: true,
mcpConnector: false,
claudeDesktop: false
}
}, {
name: "Square",
category: "Payments & Commerce",
description: "Use an agent to build on Square APIs. Payments, inventory, orders, and more",
documentation: "https://developer.squareup.com/docs/mcp",
urls: {
sse: "https://mcp.squareup.com/sse"
},
authentication: {
type: "oauth"
},
availability: {
claudeCode: true,
mcpConnector: true,
claudeDesktop: false
}
}, {
name: "Socket",
category: "Development & Testing Tools",
description: "Security analysis for dependencies",
documentation: "https://github.com/SocketDev/socket-mcp",
urls: {
http: "https://mcp.socket.dev/"
},
authentication: {
type: "oauth"
},
availability: {
claudeCode: true,
mcpConnector: false,
claudeDesktop: false
}
}, {
name: "Stripe",
category: "Payments & Commerce",
description: "Payment processing, subscription management, and financial transactions",
documentation: "https://docs.stripe.com/mcp",
urls: {
http: "https://mcp.stripe.com"
},
authentication: {
type: "oauth"
},
availability: {
claudeCode: true,
mcpConnector: true,
claudeDesktop: false
}
}, {
name: "Workato",
category: "Automation & Integration",
description: "Access any application, workflows or data via Workato, made accessible for AI",
documentation: "https://docs.workato.com/mcp.html",
urls: {},
availability: {
claudeCode: true,
mcpConnector: true,
claudeDesktop: false
},
notes: "MCP servers are programmatically generated"
}, {
name: "Zapier",
category: "Automation & Integration",
description: "Connect to nearly 8,000 apps through Zapier's automation platform",
documentation: "https://help.zapier.com/hc/en-us/articles/36265392843917",
urls: {},
availability: {
claudeCode: true,
mcpConnector: true,
claudeDesktop: false
},
notes: "Generate a user-specific URL at mcp.zapier.com"
}, {
name: "Box",
category: "Project Management & Documentation",
description: "Ask questions about your enterprise content, get insights from unstructured data, automate content workflows",
documentation: "https://box.dev/guides/box-mcp/remote/",
urls: {
http: "https://mcp.box.com/"
},
authentication: {
type: "oauth"
},
availability: {
claudeCode: true,
mcpConnector: true,
claudeDesktop: false
}
}, {
name: "Canva",
category: "Design & Media",
description: "Browse, summarize, autofill, and even generate new Canva designs directly from Claude",
documentation: "https://www.canva.dev/docs/connect/canva-mcp-server-setup/",
urls: {
http: "https://mcp.canva.com/mcp"
},
authentication: {
type: "oauth"
},
availability: {
claudeCode: true,
mcpConnector: true,
claudeDesktop: false
}
}, {
name: "Daloopa",
category: "Databases & Data Management",
description: "Supplies high quality fundamental financial data sourced from SEC Filings, investor presentations",
documentation: "https://docs.daloopa.com/docs/daloopa-mcp",
urls: {
http: "https://mcp.daloopa.com/server/mcp"
},
authentication: {
type: "oauth"
},
availability: {
claudeCode: true,
mcpConnector: true,
claudeDesktop: false
}
}, {
name: "Fireflies",
category: "Project Management & Documentation",
description: "Extract valuable insights from meeting transcripts and summaries",
documentation: "https://guide.fireflies.ai/articles/8272956938-learn-about-the-fireflies-mcp-server-model-context-protocol",
urls: {
http: "https://api.fireflies.ai/mcp"
},
authentication: {
type: "oauth"
},
availability: {
claudeCode: true,
mcpConnector: true,
claudeDesktop: false
}
}, {
name: "HubSpot",
category: "Databases & Data Management",
description: "Access and manage HubSpot CRM data by fetching contacts, companies, and deals, and creating and updating records",
documentation: "https://developers.hubspot.com/mcp",
urls: {
http: "https://mcp.hubspot.com/anthropic"
},
authentication: {
type: "oauth"
},
availability: {
claudeCode: true,
mcpConnector: true,
claudeDesktop: false
}
}, {
name: "Hugging Face",
category: "Development & Testing Tools",
description: "Provides access to Hugging Face Hub information and Gradio AI Applications",
documentation: "https://huggingface.co/settings/mcp",
urls: {
http: "https://huggingface.co/mcp"
},
authentication: {
type: "oauth"
},
availability: {
claudeCode: true,
mcpConnector: true,
claudeDesktop: false
}
}, {
name: "Jam",
category: "Development & Testing Tools",
description: "Debug faster with AI agents that can access Jam recordings like video, console logs, network requests, and errors",
documentation: "https://jam.dev/docs/debug-a-jam/mcp",
urls: {
http: "https://mcp.jam.dev/mcp"
},
authentication: {
type: "oauth"
},
availability: {
claudeCode: true,
mcpConnector: true,
claudeDesktop: false
}
}, {
name: "Monday",
category: "Project Management & Documentation",
description: "Manage monday.com boards by creating items, updating columns, assigning owners, setting timelines, adding CRM activities, and writing summaries",
documentation: "https://developer.monday.com/apps/docs/mondaycom-mcp-integration",
urls: {
http: "https://mcp.monday.com/mcp"
},
authentication: {
type: "oauth"
},
availability: {
claudeCode: true,
mcpConnector: true,
claudeDesktop: false
}
}, {
name: "Netlify",
category: "Infrastructure & DevOps",
description: "Create, deploy, and manage websites on Netlify. Control all aspects of your site from creating secrets to enforcing access controls to aggregating form submissions",
documentation: "https://docs.netlify.com/build/build-with-ai/netlify-mcp-server/",
urls: {
http: "https://netlify-mcp.netlify.app/mcp"
},
authentication: {
type: "oauth"
},
availability: {
claudeCode: true,
mcpConnector: true,
claudeDesktop: false
}
}, {
name: "Stytch",
category: "Infrastructure & DevOps",
description: "Configure and manage Stytch authentication services, redirect URLs, email templates, and workspace settings",
documentation: "https://stytch.com/docs/workspace-management/stytch-mcp",
urls: {
http: "http://mcp.stytch.dev/mcp"
},
authentication: {
type: "oauth"
},
availability: {
claudeCode: true,
mcpConnector: true,
claudeDesktop: false
}
}, {
name: "Vercel",
category: "Infrastructure & DevOps",
description: "Vercel's official MCP server, allowing you to search and navigate documentation, manage projects and deployments, and analyze deployment logs—all in one place",
documentation: "https://vercel.com/docs/mcp/vercel-mcp",
urls: {
http: "https://mcp.vercel.com/"
},
authentication: {
type: "oauth"
},
availability: {
claudeCode: true,
mcpConnector: true,
claudeDesktop: false
}
}];
const filteredServers = servers.filter(server => {
if (platform === "claudeCode") {
return server.availability.claudeCode;
} else if (platform === "mcpConnector") {
return server.availability.mcpConnector;
} else if (platform === "claudeDesktop") {
return server.availability.claudeDesktop;
} else if (platform === "all") {
return true;
} else {
throw new Error(`Unknown platform: ${platform}`);
}
});
const serversByCategory = filteredServers.reduce((acc, server) => {
if (!acc[server.category]) {
acc[server.category] = [];
}
acc[server.category].push(server);
return acc;
}, {});
const categoryOrder = ["Development & Testing Tools", "Project Management & Documentation", "Databases & Data Management", "Payments & Commerce", "Design & Media", "Infrastructure & DevOps", "Automation & Integration"];
return <>

<style jsx>{`         .cards-container {
          display: grid;
          gap: 1rem;
          margin-bottom: 2rem;
        }
        .server-card {
          border: 1px solid var(--border-color, #e5e7eb);
          border-radius: 6px;
          padding: 1rem;
        }
        .command-row {
          display: flex;
          align-items: center;
          gap: 0.25rem;
        }
        .command-row code {
          font-size: 0.75rem;
          overflow-x: auto;
        }
      `}</style>

      {categoryOrder.map(category => {
    if (!serversByCategory[category]) return null;
    return <div key={category}>
            <h3>{category}</h3>
            <div className="cards-container">
              {serversByCategory[category].map(server => {
      const claudeCodeCommand = generateClaudeCodeCommand(server);
      const mcpUrl = server.urls.http || server.urls.sse;
      const commandToShow = platform === "claudeCode" ? claudeCodeCommand : mcpUrl;
      return <div key={server.name} className="server-card">
                    <div>
                      {server.documentation ? <a href={server.documentation}>
                          <strong>{server.name}</strong>
                        </a> : <strong>{server.name}</strong>}
                    </div>

                    <p style={{
        margin: '0.5rem 0',
        fontSize: '0.9rem'
      }}>
                      {server.description}
                      {server.notes && <span style={{
        display: 'block',
        marginTop: '0.25rem',
        fontSize: '0.8rem',
        fontStyle: 'italic',
        opacity: 0.7
      }}>
                          {server.notes}
                        </span>}
                    </p>

                    {commandToShow && <>
                      <p style={{
        display: 'block',
        fontSize: '0.75rem',
        fontWeight: 500,
        minWidth: 'fit-content',
        marginTop: '0.5rem',
        marginBottom: 0
      }}>
                        {platform === "claudeCode" ? "Command" : "URL"}
                      </p>
                      <div className="command-row">
                        <code>
                          {commandToShow}
                        </code>
                      </div>
                    </>}
                  </div>;
    })}
            </div>
          </div>;

})}
</>;
};

Several companies have deployed remote MCP servers that developers can connect to via the Anthropic MCP connector API. These servers expand the capabilities available to developers and end users by providing remote access to various services and tools through the MCP protocol.

<Note>
  The remote MCP servers listed below are third-party services designed to work with the Claude API. These servers
  are not owned, operated, or endorsed by Anthropic. Users should only connect to remote MCP servers they trust and
  should review each server's security practices and terms before connecting.
</Note>

## Connecting to remote MCP servers

To connect to a remote MCP server:

1. Review the documentation for the specific server you want to use.
2. Ensure you have the necessary authentication credentials.
3. Follow the server-specific connection instructions provided by each company.

For more information about using remote MCP servers with the Claude API, see the [MCP connector docs](/en/docs/agents-and-tools/mcp-connector).

## Remote MCP server examples

<MCPServersTable platform="mcpConnector" />

<Note>
  **Looking for more?** [Find hundreds more MCP servers on GitHub](https://github.com/modelcontextprotocol/servers).
</Note>

---

## 2. MCP connector

**URL:** https://docs.claude.com/en/docs/agents-and-tools/mcp-connector

# MCP connector

Claude's Model Context Protocol (MCP) connector feature enables you to connect to remote MCP servers directly from the Messages API without a separate MCP client.

<Note>
  This feature requires the beta header: `"anthropic-beta": "mcp-client-2025-04-04"`
</Note>

## Key features

- **Direct API integration**: Connect to MCP servers without implementing an MCP client
- **Tool calling support**: Access MCP tools through the Messages API
- **OAuth authentication**: Support for OAuth Bearer tokens for authenticated servers
- **Multiple servers**: Connect to multiple MCP servers in a single request

## Limitations

- Of the feature set of the [MCP specification](https://modelcontextprotocol.io/introduction#explore-mcp), only [tool calls](https://modelcontextprotocol.io/docs/concepts/tools) are currently supported.
- The server must be publicly exposed through HTTP (supports both Streamable HTTP and SSE transports). Local STDIO servers cannot be connected directly.
- The MCP connector is currently not supported on Amazon Bedrock and Google Vertex.

## Using the MCP connector in the Messages API

To connect to a remote MCP server, include the `mcp_servers` parameter in your Messages API request:

<CodeGroup>
  ```bash cURL theme={null}
  curl https://api.anthropic.com/v1/messages \
    -H "Content-Type: application/json" \
    -H "X-API-Key: $ANTHROPIC_API_KEY" \
    -H "anthropic-version: 2023-06-01" \
    -H "anthropic-beta: mcp-client-2025-04-04" \
    -d '{
      "model": "claude-sonnet-4-5",
      "max_tokens": 1000,
      "messages": [{"role": "user", "content": "What tools do you have available?"}],
      "mcp_servers": [
        {
          "type": "url",
          "url": "https://example-server.modelcontextprotocol.io/sse",
          "name": "example-mcp",
          "authorization_token": "YOUR_TOKEN"
        }
      ]
    }'
  ```

```typescript TypeScript theme={null}
import { Anthropic } from "@anthropic-ai/sdk";

const anthropic = new Anthropic();

const response = await anthropic.beta.messages.create({
  model: "claude-sonnet-4-5",
  max_tokens: 1000,
  messages: [
    {
      role: "user",
      content: "What tools do you have available?",
    },
  ],
  mcp_servers: [
    {
      type: "url",
      url: "https://example-server.modelcontextprotocol.io/sse",
      name: "example-mcp",
      authorization_token: "YOUR_TOKEN",
    },
  ],
  betas: ["mcp-client-2025-04-04"],
});
```

```python Python theme={null}
import anthropic

client = anthropic.Anthropic()

response = client.beta.messages.create(
    model="claude-sonnet-4-5",
    max_tokens=1000,
    messages=[{
        "role": "user",
        "content": "What tools do you have available?"
    }],
    mcp_servers=[{
        "type": "url",
        "url": "https://mcp.example.com/sse",
        "name": "example-mcp",
        "authorization_token": "YOUR_TOKEN"
    }],
    betas=["mcp-client-2025-04-04"]
)
```

</CodeGroup>

## MCP server configuration

Each MCP server in the `mcp_servers` array supports the following configuration:

```json theme={null}
{
  "type": "url",
  "url": "https://example-server.modelcontextprotocol.io/sse",
  "name": "example-mcp",
  "tool_configuration": {
    "enabled": true,
    "allowed_tools": ["example_tool_1", "example_tool_2"]
  },
  "authorization_token": "YOUR_TOKEN"
}
```

### Field descriptions

| Property                           | Type    | Required | Description                                                                                                                                                     |
| ---------------------------------- | ------- | -------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `type`                             | string  | Yes      | Currently only "url" is supported                                                                                                                               |
| `url`                              | string  | Yes      | The URL of the MCP server. Must start with https\://                                                                                                            |
| `name`                             | string  | Yes      | A unique identifier for this MCP server. It will be used in `mcp_tool_call` blocks to identify the server and to disambiguate tools to the model.               |
| `tool_configuration`               | object  | No       | Configure tool usage                                                                                                                                            |
| `tool_configuration.enabled`       | boolean | No       | Whether to enable tools from this server (default: true)                                                                                                        |
| `tool_configuration.allowed_tools` | array   | No       | List to restrict the tools to allow (by default, all tools are allowed)                                                                                         |
| `authorization_token`              | string  | No       | OAuth authorization token if required by the MCP server. See [MCP specification](https://modelcontextprotocol.io/specification/2025-03-26/basic/authorization). |

## Response content types

When Claude uses MCP tools, the response will include two new content block types:

### MCP Tool Use Block

```json theme={null}
{
  "type": "mcp_tool_use",
  "id": "mcptoolu_014Q35RayjACSWkSj4X2yov1",
  "name": "echo",
  "server_name": "example-mcp",
  "input": { "param1": "value1", "param2": "value2" }
}
```

### MCP Tool Result Block

```json theme={null}
{
  "type": "mcp_tool_result",
  "tool_use_id": "mcptoolu_014Q35RayjACSWkSj4X2yov1",
  "is_error": false,
  "content": [
    {
      "type": "text",
      "text": "Hello"
    }
  ]
}
```

## Multiple MCP servers

You can connect to multiple MCP servers by including multiple objects in the `mcp_servers` array:

```json theme={null}
{
  "model": "claude-sonnet-4-5",
  "max_tokens": 1000,
  "messages": [
    {
      "role": "user",
      "content": "Use tools from both mcp-server-1 and mcp-server-2 to complete this task"
    }
  ],
  "mcp_servers": [
    {
      "type": "url",
      "url": "https://mcp.example1.com/sse",
      "name": "mcp-server-1",
      "authorization_token": "TOKEN1"
    },
    {
      "type": "url",
      "url": "https://mcp.example2.com/sse",
      "name": "mcp-server-2",
      "authorization_token": "TOKEN2"
    }
  ]
}
```

## Authentication

For MCP servers that require OAuth authentication, you'll need to obtain an access token. The MCP connector beta supports passing an `authorization_token` parameter in the MCP server definition.
API consumers are expected to handle the OAuth flow and obtain the access token prior to making the API call, as well as refreshing the token as needed.

### Obtaining an access token for testing

The MCP inspector can guide you through the process of obtaining an access token for testing purposes.

1. Run the inspector with the following command. You need Node.js installed on your machine.

   ```bash theme={null}
   npx @modelcontextprotocol/inspector
   ```

2. In the sidebar on the left, for "Transport type", select either "SSE" or "Streamable HTTP".

3. Enter the URL of the MCP server.

4. In the right area, click on the "Open Auth Settings" button after "Need to configure authentication?".

5. Click "Quick OAuth Flow" and authorize on the OAuth screen.

6. Follow the steps in the "OAuth Flow Progress" section of the inspector and click "Continue" until you reach "Authentication complete".

7. Copy the `access_token` value.

8. Paste it into the `authorization_token` field in your MCP server configuration.

### Using the access token

Once you've obtained an access token using either OAuth flow above, you can use it in your MCP server configuration:

```json theme={null}
{
  "mcp_servers": [
    {
      "type": "url",
      "url": "https://example-server.modelcontextprotocol.io/sse",
      "name": "authenticated-server",
      "authorization_token": "YOUR_ACCESS_TOKEN_HERE"
    }
  ]
}
```

For detailed explanations of the OAuth flow, refer to the [Authorization section](https://modelcontextprotocol.io/docs/concepts/authentication) in the MCP specification.

---

## 3. Skill authoring best practices

**URL:** https://docs.claude.com/en/docs/agents-and-tools/agent-skills/best-practices

# Skill authoring best practices

> Learn how to write effective Skills that Claude can discover and use successfully.

Good Skills are concise, well-structured, and tested with real usage. This guide provides practical authoring decisions to help you write Skills that Claude can discover and use effectively.

For conceptual background on how Skills work, see the [Skills overview](/en/docs/agents-and-tools/agent-skills/overview).

## Core principles

### Concise is key

The [context window](/en/docs/build-with-claude/context-windows) is a public good. Your Skill shares the context window with everything else Claude needs to know, including:

- The system prompt
- Conversation history
- Other Skills' metadata
- Your actual request

Not every token in your Skill has an immediate cost. At startup, only the metadata (name and description) from all Skills is pre-loaded. Claude reads SKILL.md only when the Skill becomes relevant, and reads additional files only as needed. However, being concise in SKILL.md still matters: once Claude loads it, every token competes with conversation history and other context.

**Default assumption**: Claude is already very smart

Only add context Claude doesn't already have. Challenge each piece of information:

- "Does Claude really need this explanation?"
- "Can I assume Claude knows this?"
- "Does this paragraph justify its token cost?"

**Good example: Concise** (approximately 50 tokens):

````markdown theme={null}
## Extract PDF text

Use pdfplumber for text extraction:

```python
import pdfplumber

with pdfplumber.open("file.pdf") as pdf:
    text = pdf.pages[0].extract_text()
```
````

**Bad example: Too verbose** (approximately 150 tokens):

```markdown theme={null}
## Extract PDF text

PDF (Portable Document Format) files are a common file format that contains
text, images, and other content. To extract text from a PDF, you'll need to
use a library. There are many libraries available for PDF processing, but we
recommend pdfplumber because it's easy to use and handles most cases well.
First, you'll need to install it using pip. Then you can use the code below...
```

The concise version assumes Claude knows what PDFs are and how libraries work.

### Set appropriate degrees of freedom

Match the level of specificity to the task's fragility and variability.

**High freedom** (text-based instructions):

Use when:

- Multiple approaches are valid
- Decisions depend on context
- Heuristics guide the approach

Example:

```markdown theme={null}
## Code review process

1. Analyze the code structure and organization
2. Check for potential bugs or edge cases
3. Suggest improvements for readability and maintainability
4. Verify adherence to project conventions
```

**Medium freedom** (pseudocode or scripts with parameters):

Use when:

- A preferred pattern exists
- Some variation is acceptable
- Configuration affects behavior

Example:

````markdown theme={null}
## Generate report

Use this template and customize as needed:

```python
def generate_report(data, format="markdown", include_charts=True):
    # Process data
    # Generate output in specified format
    # Optionally include visualizations
```
````

**Low freedom** (specific scripts, few or no parameters):

Use when:

- Operations are fragile and error-prone
- Consistency is critical
- A specific sequence must be followed

Example:

````markdown theme={null}
## Database migration

Run exactly this script:

```bash
python scripts/migrate.py --verify --backup
```

Do not modify the command or add additional flags.
````

**Analogy**: Think of Claude as a robot exploring a path:

- **Narrow bridge with cliffs on both sides**: There's only one safe way forward. Provide specific guardrails and exact instructions (low freedom). Example: database migrations that must run in exact sequence.
- **Open field with no hazards**: Many paths lead to success. Give general direction and trust Claude to find the best route (high freedom). Example: code reviews where context determines the best approach.

### Test with all models you plan to use

Skills act as additions to models, so effectiveness depends on the underlying model. Test your Skill with all the models you plan to use it with.

**Testing considerations by model**:

- **Claude Haiku** (fast, economical): Does the Skill provide enough guidance?
- **Claude Sonnet** (balanced): Is the Skill clear and efficient?
- **Claude Opus** (powerful reasoning): Does the Skill avoid over-explaining?

What works perfectly for Opus might need more detail for Haiku. If you plan to use your Skill across multiple models, aim for instructions that work well with all of them.

## Skill structure

<Note>
  **YAML Frontmatter**: The SKILL.md frontmatter requires two fields:

`name`:

- Maximum 64 characters
- Must contain only lowercase letters, numbers, and hyphens
- Cannot contain XML tags
- Cannot contain reserved words: "anthropic", "claude"

`description`:

- Must be non-empty
- Maximum 1024 characters
- Cannot contain XML tags
- Should describe what the Skill does and when to use it

For complete Skill structure details, see the [Skills overview](/en/docs/agents-and-tools/agent-skills/overview#skill-structure).
</Note>

### Naming conventions

Use consistent naming patterns to make Skills easier to reference and discuss. We recommend using **gerund form** (verb + -ing) for Skill names, as this clearly describes the activity or capability the Skill provides.

Remember that the `name` field must use lowercase letters, numbers, and hyphens only.

**Good naming examples (gerund form)**:

- `processing-pdfs`
- `analyzing-spreadsheets`
- `managing-databases`
- `testing-code`
- `writing-documentation`

**Acceptable alternatives**:

- Noun phrases: `pdf-processing`, `spreadsheet-analysis`
- Action-oriented: `process-pdfs`, `analyze-spreadsheets`

**Avoid**:

- Vague names: `helper`, `utils`, `tools`
- Overly generic: `documents`, `data`, `files`
- Reserved words: `anthropic-helper`, `claude-tools`
- Inconsistent patterns within your skill collection

Consistent naming makes it easier to:

- Reference Skills in documentation and conversations
- Understand what a Skill does at a glance
- Organize and search through multiple Skills
- Maintain a professional, cohesive skill library

### Writing effective descriptions

The `description` field enables Skill discovery and should include both what the Skill does and when to use it.

<Warning>
  **Always write in third person**. The description is injected into the system prompt, and inconsistent point-of-view can cause discovery problems.

- **Good:** "Processes Excel files and generates reports"
- **Avoid:** "I can help you process Excel files"
- **Avoid:** "You can use this to process Excel files"
  </Warning>

**Be specific and include key terms**. Include both what the Skill does and specific triggers/contexts for when to use it.

Each Skill has exactly one description field. The description is critical for skill selection: Claude uses it to choose the right Skill from potentially 100+ available Skills. Your description must provide enough detail for Claude to know when to select this Skill, while the rest of SKILL.md provides the implementation details.

Effective examples:

**PDF Processing skill:**

```yaml theme={null}
description: Extract text and tables from PDF files, fill forms, merge documents. Use when working with PDF files or when the user mentions PDFs, forms, or document extraction.
```

**Excel Analysis skill:**

```yaml theme={null}
description: Analyze Excel spreadsheets, create pivot tables, generate charts. Use when analyzing Excel files, spreadsheets, tabular data, or .xlsx files.
```

**Git Commit Helper skill:**

```yaml theme={null}
description: Generate descriptive commit messages by analyzing git diffs. Use when the user asks for help writing commit messages or reviewing staged changes.
```

Avoid vague descriptions like these:

```yaml theme={null}
description: Helps with documents
```

```yaml theme={null}
description: Processes data
```

```yaml theme={null}
description: Does stuff with files
```

### Progressive disclosure patterns

SKILL.md serves as an overview that points Claude to detailed materials as needed, like a table of contents in an onboarding guide. For an explanation of how progressive disclosure works, see [How Skills work](/en/docs/agents-and-tools/agent-skills/overview#how-skills-work) in the overview.

**Practical guidance:**

- Keep SKILL.md body under 500 lines for optimal performance
- Split content into separate files when approaching this limit
- Use the patterns below to organize instructions, code, and resources effectively

#### Visual overview: From simple to complex

A basic Skill starts with just a SKILL.md file containing metadata and instructions:

<img src="https://mintcdn.com/anthropic-claude-docs/4Bny2bjzuGBK7o00/images/agent-skills-simple-file.png?fit=max&auto=format&n=4Bny2bjzuGBK7o00&q=85&s=87782ff239b297d9a9e8e1b72ed72db9" alt="Simple SKILL.md file showing YAML frontmatter and markdown body" data-og-width="2048" width="2048" data-og-height="1153" height="1153" data-path="images/agent-skills-simple-file.png" data-optimize="true" data-opv="3" srcset="https://mintcdn.com/anthropic-claude-docs/4Bny2bjzuGBK7o00/images/agent-skills-simple-file.png?w=280&fit=max&auto=format&n=4Bny2bjzuGBK7o00&q=85&s=c61cc33b6f5855809907f7fda94cd80e 280w, https://mintcdn.com/anthropic-claude-docs/4Bny2bjzuGBK7o00/images/agent-skills-simple-file.png?w=560&fit=max&auto=format&n=4Bny2bjzuGBK7o00&q=85&s=90d2c0c1c76b36e8d485f49e0810dbfd 560w, https://mintcdn.com/anthropic-claude-docs/4Bny2bjzuGBK7o00/images/agent-skills-simple-file.png?w=840&fit=max&auto=format&n=4Bny2bjzuGBK7o00&q=85&s=ad17d231ac7b0bea7e5b4d58fb4aeabb 840w, https://mintcdn.com/anthropic-claude-docs/4Bny2bjzuGBK7o00/images/agent-skills-simple-file.png?w=1100&fit=max&auto=format&n=4Bny2bjzuGBK7o00&q=85&s=f5d0a7a3c668435bb0aee9a3a8f8c329 1100w, https://mintcdn.com/anthropic-claude-docs/4Bny2bjzuGBK7o00/images/agent-skills-simple-file.png?w=1650&fit=max&auto=format&n=4Bny2bjzuGBK7o00&q=85&s=0e927c1af9de5799cfe557d12249f6e6 1650w, https://mintcdn.com/anthropic-claude-docs/4Bny2bjzuGBK7o00/images/agent-skills-simple-file.png?w=2500&fit=max&auto=format&n=4Bny2bjzuGBK7o00&q=85&s=46bbb1a51dd4c8202a470ac8c80a893d 2500w" />

As your Skill grows, you can bundle additional content that Claude loads only when needed:

<img src="https://mintcdn.com/anthropic-claude-docs/4Bny2bjzuGBK7o00/images/agent-skills-bundling-content.png?fit=max&auto=format&n=4Bny2bjzuGBK7o00&q=85&s=a5e0aa41e3d53985a7e3e43668a33ea3" alt="Bundling additional reference files like reference.md and forms.md." data-og-width="2048" width="2048" data-og-height="1327" height="1327" data-path="images/agent-skills-bundling-content.png" data-optimize="true" data-opv="3" srcset="https://mintcdn.com/anthropic-claude-docs/4Bny2bjzuGBK7o00/images/agent-skills-bundling-content.png?w=280&fit=max&auto=format&n=4Bny2bjzuGBK7o00&q=85&s=f8a0e73783e99b4a643d79eac86b70a2 280w, https://mintcdn.com/anthropic-claude-docs/4Bny2bjzuGBK7o00/images/agent-skills-bundling-content.png?w=560&fit=max&auto=format&n=4Bny2bjzuGBK7o00&q=85&s=dc510a2a9d3f14359416b706f067904a 560w, https://mintcdn.com/anthropic-claude-docs/4Bny2bjzuGBK7o00/images/agent-skills-bundling-content.png?w=840&fit=max&auto=format&n=4Bny2bjzuGBK7o00&q=85&s=82cd6286c966303f7dd914c28170e385 840w, https://mintcdn.com/anthropic-claude-docs/4Bny2bjzuGBK7o00/images/agent-skills-bundling-content.png?w=1100&fit=max&auto=format&n=4Bny2bjzuGBK7o00&q=85&s=56f3be36c77e4fe4b523df209a6824c6 1100w, https://mintcdn.com/anthropic-claude-docs/4Bny2bjzuGBK7o00/images/agent-skills-bundling-content.png?w=1650&fit=max&auto=format&n=4Bny2bjzuGBK7o00&q=85&s=d22b5161b2075656417d56f41a74f3dd 1650w, https://mintcdn.com/anthropic-claude-docs/4Bny2bjzuGBK7o00/images/agent-skills-bundling-content.png?w=2500&fit=max&auto=format&n=4Bny2bjzuGBK7o00&q=85&s=3dd4bdd6850ffcc96c6c45fcb0acd6eb 2500w" />

The complete Skill directory structure might look like this:

```
pdf/
├── SKILL.md              # Main instructions (loaded when triggered)
├── FORMS.md              # Form-filling guide (loaded as needed)
├── reference.md          # API reference (loaded as needed)
├── examples.md           # Usage examples (loaded as needed)
└── scripts/
    ├── analyze_form.py   # Utility script (executed, not loaded)
    ├── fill_form.py      # Form filling script
    └── validate.py       # Validation script
```

#### Pattern 1: High-level guide with references

````markdown theme={null}
---
name: pdf-processing
description: Extracts text and tables from PDF files, fills forms, and merges documents. Use when working with PDF files or when the user mentions PDFs, forms, or document extraction.
---

# PDF Processing

## Quick start

Extract text with pdfplumber:

```python
import pdfplumber
with pdfplumber.open("file.pdf") as pdf:
    text = pdf.pages[0].extract_text()
```

## Advanced features

**Form filling**: See [FORMS.md](FORMS.md) for complete guide
**API reference**: See [REFERENCE.md](REFERENCE.md) for all methods
**Examples**: See [EXAMPLES.md](EXAMPLES.md) for common patterns
````

Claude loads FORMS.md, REFERENCE.md, or EXAMPLES.md only when needed.

#### Pattern 2: Domain-specific organization

For Skills with multiple domains, organize content by domain to avoid loading irrelevant context. When a user asks about sales metrics, Claude only needs to read sales-related schemas, not finance or marketing data. This keeps token usage low and context focused.

```
bigquery-skill/
├── SKILL.md (overview and navigation)
└── reference/
    ├── finance.md (revenue, billing metrics)
    ├── sales.md (opportunities, pipeline)
    ├── product.md (API usage, features)
    └── marketing.md (campaigns, attribution)
```

````markdown SKILL.md theme={null}
# BigQuery Data Analysis

## Available datasets

**Finance**: Revenue, ARR, billing → See [reference/finance.md](reference/finance.md)
**Sales**: Opportunities, pipeline, accounts → See [reference/sales.md](reference/sales.md)
**Product**: API usage, features, adoption → See [reference/product.md](reference/product.md)
**Marketing**: Campaigns, attribution, email → See [reference/marketing.md](reference/marketing.md)

## Quick search

Find specific metrics using grep:

```bash
grep -i "revenue" reference/finance.md
grep -i "pipeline" reference/sales.md
grep -i "api usage" reference/product.md
```
````

#### Pattern 3: Conditional details

Show basic content, link to advanced content:

```markdown theme={null}
# DOCX Processing

## Creating documents

Use docx-js for new documents. See [DOCX-JS.md](DOCX-JS.md).

## Editing documents

For simple edits, modify the XML directly.

**For tracked changes**: See [REDLINING.md](REDLINING.md)
**For OOXML details**: See [OOXML.md](OOXML.md)
```

Claude reads REDLINING.md or OOXML.md only when the user needs those features.

### Avoid deeply nested references

Claude may partially read files when they're referenced from other referenced files. When encountering nested references, Claude might use commands like `head -100` to preview content rather than reading entire files, resulting in incomplete information.

**Keep references one level deep from SKILL.md**. All reference files should link directly from SKILL.md to ensure Claude reads complete files when needed.

**Bad example: Too deep**:

```markdown theme={null}
# SKILL.md

See [advanced.md](advanced.md)...

# advanced.md

See [details.md](details.md)...

# details.md

Here's the actual information...
```

**Good example: One level deep**:

```markdown theme={null}
# SKILL.md

**Basic usage**: [instructions in SKILL.md]
**Advanced features**: See [advanced.md](advanced.md)
**API reference**: See [reference.md](reference.md)
**Examples**: See [examples.md](examples.md)
```

### Structure longer reference files with table of contents

For reference files longer than 100 lines, include a table of contents at the top. This ensures Claude can see the full scope of available information even when previewing with partial reads.

**Example**:

```markdown theme={null}
# API Reference

## Contents

- Authentication and setup
- Core methods (create, read, update, delete)
- Advanced features (batch operations, webhooks)
- Error handling patterns
- Code examples

## Authentication and setup

...

## Core methods

...
```

Claude can then read the complete file or jump to specific sections as needed.

For details on how this filesystem-based architecture enables progressive disclosure, see the [Runtime environment](#runtime-environment) section in the Advanced section below.

## Workflows and feedback loops

### Use workflows for complex tasks

Break complex operations into clear, sequential steps. For particularly complex workflows, provide a checklist that Claude can copy into its response and check off as it progresses.

**Example 1: Research synthesis workflow** (for Skills without code):

````markdown theme={null}
## Research synthesis workflow

Copy this checklist and track your progress:

```
Research Progress:
- [ ] Step 1: Read all source documents
- [ ] Step 2: Identify key themes
- [ ] Step 3: Cross-reference claims
- [ ] Step 4: Create structured summary
- [ ] Step 5: Verify citations
```

**Step 1: Read all source documents**

Review each document in the `sources/` directory. Note the main arguments and supporting evidence.

**Step 2: Identify key themes**

Look for patterns across sources. What themes appear repeatedly? Where do sources agree or disagree?

**Step 3: Cross-reference claims**

For each major claim, verify it appears in the source material. Note which source supports each point.

**Step 4: Create structured summary**

Organize findings by theme. Include:

- Main claim
- Supporting evidence from sources
- Conflicting viewpoints (if any)

**Step 5: Verify citations**

Check that every claim references the correct source document. If citations are incomplete, return to Step 3.
````

This example shows how workflows apply to analysis tasks that don't require code. The checklist pattern works for any complex, multi-step process.

**Example 2: PDF form filling workflow** (for Skills with code):

````markdown theme={null}
## PDF form filling workflow

Copy this checklist and check off items as you complete them:

```
Task Progress:
- [ ] Step 1: Analyze the form (run analyze_form.py)
- [ ] Step 2: Create field mapping (edit fields.json)
- [ ] Step 3: Validate mapping (run validate_fields.py)
- [ ] Step 4: Fill the form (run fill_form.py)
- [ ] Step 5: Verify output (run verify_output.py)
```

**Step 1: Analyze the form**

Run: `python scripts/analyze_form.py input.pdf`

This extracts form fields and their locations, saving to `fields.json`.

**Step 2: Create field mapping**

Edit `fields.json` to add values for each field.

**Step 3: Validate mapping**

Run: `python scripts/validate_fields.py fields.json`

Fix any validation errors before continuing.

**Step 4: Fill the form**

Run: `python scripts/fill_form.py input.pdf fields.json output.pdf`

**Step 5: Verify output**

Run: `python scripts/verify_output.py output.pdf`

If verification fails, return to Step 2.
````

Clear steps prevent Claude from skipping critical validation. The checklist helps both Claude and you track progress through multi-step workflows.

### Implement feedback loops

**Common pattern**: Run validator → fix errors → repeat

This pattern greatly improves output quality.

**Example 1: Style guide compliance** (for Skills without code):

```markdown theme={null}
## Content review process

1. Draft your content following the guidelines in STYLE_GUIDE.md
2. Review against the checklist:
   - Check terminology consistency
   - Verify examples follow the standard format
   - Confirm all required sections are present
3. If issues found:
   - Note each issue with specific section reference
   - Revise the content
   - Review the checklist again
4. Only proceed when all requirements are met
5. Finalize and save the document
```

This shows the validation loop pattern using reference documents instead of scripts. The "validator" is STYLE_GUIDE.md, and Claude performs the check by reading and comparing.

**Example 2: Document editing process** (for Skills with code):

```markdown theme={null}
## Document editing process

1. Make your edits to `word/document.xml`
2. **Validate immediately**: `python ooxml/scripts/validate.py unpacked_dir/`
3. If validation fails:
   - Review the error message carefully
   - Fix the issues in the XML
   - Run validation again
4. **Only proceed when validation passes**
5. Rebuild: `python ooxml/scripts/pack.py unpacked_dir/ output.docx`
6. Test the output document
```

The validation loop catches errors early.

## Content guidelines

### Avoid time-sensitive information

Don't include information that will become outdated:

**Bad example: Time-sensitive** (will become wrong):

```markdown theme={null}
If you're doing this before August 2025, use the old API.
After August 2025, use the new API.
```

**Good example** (use "old patterns" section):

```markdown theme={null}
## Current method

Use the v2 API endpoint: `api.example.com/v2/messages`

## Old patterns

<details>
<summary>Legacy v1 API (deprecated 2025-08)</summary>

The v1 API used: `api.example.com/v1/messages`

This endpoint is no longer supported.

</details>
```

The old patterns section provides historical context without cluttering the main content.

### Use consistent terminology

Choose one term and use it throughout the Skill:

**Good - Consistent**:

- Always "API endpoint"
- Always "field"
- Always "extract"

**Bad - Inconsistent**:

- Mix "API endpoint", "URL", "API route", "path"
- Mix "field", "box", "element", "control"
- Mix "extract", "pull", "get", "retrieve"

Consistency helps Claude understand and follow instructions.

## Common patterns

### Template pattern

Provide templates for output format. Match the level of strictness to your needs.

**For strict requirements** (like API responses or data formats):

````markdown theme={null}
## Report structure

ALWAYS use this exact template structure:

```markdown
# [Analysis Title]

## Executive summary

[One-paragraph overview of key findings]

## Key findings

- Finding 1 with supporting data
- Finding 2 with supporting data
- Finding 3 with supporting data

## Recommendations

1. Specific actionable recommendation
2. Specific actionable recommendation
```
````

**For flexible guidance** (when adaptation is useful):

````markdown theme={null}
## Report structure

Here is a sensible default format, but use your best judgment based on the analysis:

```markdown
# [Analysis Title]

## Executive summary

[Overview]

## Key findings

[Adapt sections based on what you discover]

## Recommendations

[Tailor to the specific context]
```

Adjust sections as needed for the specific analysis type.
````

### Examples pattern

For Skills where output quality depends on seeing examples, provide input/output pairs just like in regular prompting:

````markdown theme={null}
## Commit message format

Generate commit messages following these examples:

**Example 1:**
Input: Added user authentication with JWT tokens
Output:

```
feat(auth): implement JWT-based authentication

Add login endpoint and token validation middleware
```

**Example 2:**
Input: Fixed bug where dates displayed incorrectly in reports
Output:

```
fix(reports): correct date formatting in timezone conversion

Use UTC timestamps consistently across report generation
```

**Example 3:**
Input: Updated dependencies and refactored error handling
Output:

```
chore: update dependencies and refactor error handling

- Upgrade lodash to 4.17.21
- Standardize error response format across endpoints
```

Follow this style: type(scope): brief description, then detailed explanation.
````

Examples help Claude understand the desired style and level of detail more clearly than descriptions alone.

### Conditional workflow pattern

Guide Claude through decision points:

```markdown theme={null}
## Document modification workflow

1. Determine the modification type:

   **Creating new content?** → Follow "Creation workflow" below
   **Editing existing content?** → Follow "Editing workflow" below

2. Creation workflow:
   - Use docx-js library
   - Build document from scratch
   - Export to .docx format

3. Editing workflow:
   - Unpack existing document
   - Modify XML directly
   - Validate after each change
   - Repack when complete
```

<Tip>
  If workflows become large or complicated with many steps, consider pushing them into separate files and tell Claude to read the appropriate file based on the task at hand.
</Tip>

## Evaluation and iteration

### Build evaluations first

**Create evaluations BEFORE writing extensive documentation.** This ensures your Skill solves real problems rather than documenting imagined ones.

**Evaluation-driven development:**

1. **Identify gaps**: Run Claude on representative tasks without a Skill. Document specific failures or missing context
2. **Create evaluations**: Build three scenarios that test these gaps
3. **Establish baseline**: Measure Claude's performance without the Skill
4. **Write minimal instructions**: Create just enough content to address the gaps and pass evaluations
5. **Iterate**: Execute evaluations, compare against baseline, and refine

This approach ensures you're solving actual problems rather than anticipating requirements that may never materialize.

**Evaluation structure**:

```json theme={null}
{
  "skills": ["pdf-processing"],
  "query": "Extract all text from this PDF file and save it to output.txt",
  "files": ["test-files/document.pdf"],
  "expected_behavior": ["Successfully reads the PDF file using an appropriate PDF processing library or command-line tool", "Extracts text content from all pages in the document without missing any pages", "Saves the extracted text to a file named output.txt in a clear, readable format"]
}
```

<Note>
  This example demonstrates a data-driven evaluation with a simple testing rubric. We do not currently provide a built-in way to run these evaluations. Users can create their own evaluation system. Evaluations are your source of truth for measuring Skill effectiveness.
</Note>

### Develop Skills iteratively with Claude

The most effective Skill development process involves Claude itself. Work with one instance of Claude ("Claude A") to create a Skill that will be used by other instances ("Claude B"). Claude A helps you design and refine instructions, while Claude B tests them in real tasks. This works because Claude models understand both how to write effective agent instructions and what information agents need.

**Creating a new Skill:**

1. **Complete a task without a Skill**: Work through a problem with Claude A using normal prompting. As you work, you'll naturally provide context, explain preferences, and share procedural knowledge. Notice what information you repeatedly provide.

2. **Identify the reusable pattern**: After completing the task, identify what context you provided that would be useful for similar future tasks.

   **Example**: If you worked through a BigQuery analysis, you might have provided table names, field definitions, filtering rules (like "always exclude test accounts"), and common query patterns.

3. **Ask Claude A to create a Skill**: "Create a Skill that captures this BigQuery analysis pattern we just used. Include the table schemas, naming conventions, and the rule about filtering test accounts."

   <Tip>
     Claude models understand the Skill format and structure natively. You don't need special system prompts or a "writing skills" skill to get Claude to help create Skills. Simply ask Claude to create a Skill and it will generate properly structured SKILL.md content with appropriate frontmatter and body content.
   </Tip>

4. **Review for conciseness**: Check that Claude A hasn't added unnecessary explanations. Ask: "Remove the explanation about what win rate means - Claude already knows that."

5. **Improve information architecture**: Ask Claude A to organize the content more effectively. For example: "Organize this so the table schema is in a separate reference file. We might add more tables later."

6. **Test on similar tasks**: Use the Skill with Claude B (a fresh instance with the Skill loaded) on related use cases. Observe whether Claude B finds the right information, applies rules correctly, and handles the task successfully.

7. **Iterate based on observation**: If Claude B struggles or misses something, return to Claude A with specifics: "When Claude used this Skill, it forgot to filter by date for Q4. Should we add a section about date filtering patterns?"

**Iterating on existing Skills:**

The same hierarchical pattern continues when improving Skills. You alternate between:

- **Working with Claude A** (the expert who helps refine the Skill)
- **Testing with Claude B** (the agent using the Skill to perform real work)
- **Observing Claude B's behavior** and bringing insights back to Claude A

1. **Use the Skill in real workflows**: Give Claude B (with the Skill loaded) actual tasks, not test scenarios

2. **Observe Claude B's behavior**: Note where it struggles, succeeds, or makes unexpected choices

   **Example observation**: "When I asked Claude B for a regional sales report, it wrote the query but forgot to filter out test accounts, even though the Skill mentions this rule."

3. **Return to Claude A for improvements**: Share the current SKILL.md and describe what you observed. Ask: "I noticed Claude B forgot to filter test accounts when I asked for a regional report. The Skill mentions filtering, but maybe it's not prominent enough?"

4. **Review Claude A's suggestions**: Claude A might suggest reorganizing to make rules more prominent, using stronger language like "MUST filter" instead of "always filter", or restructuring the workflow section.

5. **Apply and test changes**: Update the Skill with Claude A's refinements, then test again with Claude B on similar requests

6. **Repeat based on usage**: Continue this observe-refine-test cycle as you encounter new scenarios. Each iteration improves the Skill based on real agent behavior, not assumptions.

**Gathering team feedback:**

1. Share Skills with teammates and observe their usage
2. Ask: Does the Skill activate when expected? Are instructions clear? What's missing?
3. Incorporate feedback to address blind spots in your own usage patterns

**Why this approach works**: Claude A understands agent needs, you provide domain expertise, Claude B reveals gaps through real usage, and iterative refinement improves Skills based on observed behavior rather than assumptions.

### Observe how Claude navigates Skills

As you iterate on Skills, pay attention to how Claude actually uses them in practice. Watch for:

- **Unexpected exploration paths**: Does Claude read files in an order you didn't anticipate? This might indicate your structure isn't as intuitive as you thought
- **Missed connections**: Does Claude fail to follow references to important files? Your links might need to be more explicit or prominent
- **Overreliance on certain sections**: If Claude repeatedly reads the same file, consider whether that content should be in the main SKILL.md instead
- **Ignored content**: If Claude never accesses a bundled file, it might be unnecessary or poorly signaled in the main instructions

Iterate based on these observations rather than assumptions. The 'name' and 'description' in your Skill's metadata are particularly critical. Claude uses these when deciding whether to trigger the Skill in response to the current task. Make sure they clearly describe what the Skill does and when it should be used.

## Anti-patterns to avoid

### Avoid Windows-style paths

Always use forward slashes in file paths, even on Windows:

- ✓ **Good**: `scripts/helper.py`, `reference/guide.md`
- ✗ **Avoid**: `scripts\helper.py`, `reference\guide.md`

Unix-style paths work across all platforms, while Windows-style paths cause errors on Unix systems.

### Avoid offering too many options

Don't present multiple approaches unless necessary:

````markdown theme={null}
**Bad example: Too many choices** (confusing):
"You can use pypdf, or pdfplumber, or PyMuPDF, or pdf2image, or..."

**Good example: Provide a default** (with escape hatch):
"Use pdfplumber for text extraction:

```python
import pdfplumber
```

For scanned PDFs requiring OCR, use pdf2image with pytesseract instead."
````

## Advanced: Skills with executable code

The sections below focus on Skills that include executable scripts. If your Skill uses only markdown instructions, skip to [Checklist for effective Skills](#checklist-for-effective-skills).

### Solve, don't punt

When writing scripts for Skills, handle error conditions rather than punting to Claude.

**Good example: Handle errors explicitly**:

```python theme={null}
def process_file(path):
    """Process a file, creating it if it doesn't exist."""
    try:
        with open(path) as f:
            return f.read()
    except FileNotFoundError:
        # Create file with default content instead of failing
        print(f"File {path} not found, creating default")
        with open(path, 'w') as f:
            f.write('')
        return ''
    except PermissionError:
        # Provide alternative instead of failing
        print(f"Cannot access {path}, using default")
        return ''
```

**Bad example: Punt to Claude**:

```python theme={null}
def process_file(path):
    # Just fail and let Claude figure it out
    return open(path).read()
```

Configuration parameters should also be justified and documented to avoid "voodoo constants" (Ousterhout's law). If you don't know the right value, how will Claude determine it?

**Good example: Self-documenting**:

```python theme={null}
# HTTP requests typically complete within 30 seconds
# Longer timeout accounts for slow connections
REQUEST_TIMEOUT = 30

# Three retries balances reliability vs speed
# Most intermittent failures resolve by the second retry
MAX_RETRIES = 3
```

**Bad example: Magic numbers**:

```python theme={null}
TIMEOUT = 47  # Why 47?
RETRIES = 5   # Why 5?
```

### Provide utility scripts

Even if Claude could write a script, pre-made scripts offer advantages:

**Benefits of utility scripts**:

- More reliable than generated code
- Save tokens (no need to include code in context)
- Save time (no code generation required)
- Ensure consistency across uses

<img src="https://mintcdn.com/anthropic-claude-docs/4Bny2bjzuGBK7o00/images/agent-skills-executable-scripts.png?fit=max&auto=format&n=4Bny2bjzuGBK7o00&q=85&s=4bbc45f2c2e0bee9f2f0d5da669bad00" alt="Bundling executable scripts alongside instruction files" data-og-width="2048" width="2048" data-og-height="1154" height="1154" data-path="images/agent-skills-executable-scripts.png" data-optimize="true" data-opv="3" srcset="https://mintcdn.com/anthropic-claude-docs/4Bny2bjzuGBK7o00/images/agent-skills-executable-scripts.png?w=280&fit=max&auto=format&n=4Bny2bjzuGBK7o00&q=85&s=9a04e6535a8467bfeea492e517de389f 280w, https://mintcdn.com/anthropic-claude-docs/4Bny2bjzuGBK7o00/images/agent-skills-executable-scripts.png?w=560&fit=max&auto=format&n=4Bny2bjzuGBK7o00&q=85&s=e49333ad90141af17c0d7651cca7216b 560w, https://mintcdn.com/anthropic-claude-docs/4Bny2bjzuGBK7o00/images/agent-skills-executable-scripts.png?w=840&fit=max&auto=format&n=4Bny2bjzuGBK7o00&q=85&s=954265a5df52223d6572b6214168c428 840w, https://mintcdn.com/anthropic-claude-docs/4Bny2bjzuGBK7o00/images/agent-skills-executable-scripts.png?w=1100&fit=max&auto=format&n=4Bny2bjzuGBK7o00&q=85&s=2ff7a2d8f2a83ee8af132b29f10150fd 1100w, https://mintcdn.com/anthropic-claude-docs/4Bny2bjzuGBK7o00/images/agent-skills-executable-scripts.png?w=1650&fit=max&auto=format&n=4Bny2bjzuGBK7o00&q=85&s=48ab96245e04077f4d15e9170e081cfb 1650w, https://mintcdn.com/anthropic-claude-docs/4Bny2bjzuGBK7o00/images/agent-skills-executable-scripts.png?w=2500&fit=max&auto=format&n=4Bny2bjzuGBK7o00&q=85&s=0301a6c8b3ee879497cc5b5483177c90 2500w" />

The diagram above shows how executable scripts work alongside instruction files. The instruction file (forms.md) references the script, and Claude can execute it without loading its contents into context.

**Important distinction**: Make clear in your instructions whether Claude should:

- **Execute the script** (most common): "Run `analyze_form.py` to extract fields"
- **Read it as reference** (for complex logic): "See `analyze_form.py` for the field extraction algorithm"

For most utility scripts, execution is preferred because it's more reliable and efficient. See the [Runtime environment](#runtime-environment) section below for details on how script execution works.

**Example**:

````markdown theme={null}
## Utility scripts

**analyze_form.py**: Extract all form fields from PDF

```bash
python scripts/analyze_form.py input.pdf > fields.json
```

Output format:

```json
{
  "field_name": { "type": "text", "x": 100, "y": 200 },
  "signature": { "type": "sig", "x": 150, "y": 500 }
}
```

**validate_boxes.py**: Check for overlapping bounding boxes

```bash
python scripts/validate_boxes.py fields.json
# Returns: "OK" or lists conflicts
```

**fill_form.py**: Apply field values to PDF

```bash
python scripts/fill_form.py input.pdf fields.json output.pdf
```
````

### Use visual analysis

When inputs can be rendered as images, have Claude analyze them:

````markdown theme={null}
## Form layout analysis

1. Convert PDF to images:

   ```bash
   python scripts/pdf_to_images.py form.pdf
   ```

2. Analyze each page image to identify form fields
3. Claude can see field locations and types visually
````

<Note>
  In this example, you'd need to write the `pdf_to_images.py` script.
</Note>

Claude's vision capabilities help understand layouts and structures.

### Create verifiable intermediate outputs

When Claude performs complex, open-ended tasks, it can make mistakes. The "plan-validate-execute" pattern catches errors early by having Claude first create a plan in a structured format, then validate that plan with a script before executing it.

**Example**: Imagine asking Claude to update 50 form fields in a PDF based on a spreadsheet. Without validation, Claude might reference non-existent fields, create conflicting values, miss required fields, or apply updates incorrectly.

**Solution**: Use the workflow pattern shown above (PDF form filling), but add an intermediate `changes.json` file that gets validated before applying changes. The workflow becomes: analyze → **create plan file** → **validate plan** → execute → verify.

**Why this pattern works:**

- **Catches errors early**: Validation finds problems before changes are applied
- **Machine-verifiable**: Scripts provide objective verification
- **Reversible planning**: Claude can iterate on the plan without touching originals
- **Clear debugging**: Error messages point to specific problems

**When to use**: Batch operations, destructive changes, complex validation rules, high-stakes operations.

**Implementation tip**: Make validation scripts verbose with specific error messages like "Field 'signature_date' not found. Available fields: customer_name, order_total, signature_date_signed" to help Claude fix issues.

### Package dependencies

Skills run in the code execution environment with platform-specific limitations:

- **claude.ai**: Can install packages from npm and PyPI and pull from GitHub repositories
- **Anthropic API**: Has no network access and no runtime package installation

List required packages in your SKILL.md and verify they're available in the [code execution tool documentation](/en/docs/agents-and-tools/tool-use/code-execution-tool).

### Runtime environment

Skills run in a code execution environment with filesystem access, bash commands, and code execution capabilities. For the conceptual explanation of this architecture, see [The Skills architecture](/en/docs/agents-and-tools/agent-skills/overview#the-skills-architecture) in the overview.

**How this affects your authoring:**

**How Claude accesses Skills:**

1. **Metadata pre-loaded**: At startup, the name and description from all Skills' YAML frontmatter are loaded into the system prompt
2. **Files read on-demand**: Claude uses bash Read tools to access SKILL.md and other files from the filesystem when needed
3. **Scripts executed efficiently**: Utility scripts can be executed via bash without loading their full contents into context. Only the script's output consumes tokens
4. **No context penalty for large files**: Reference files, data, or documentation don't consume context tokens until actually read

- **File paths matter**: Claude navigates your skill directory like a filesystem. Use forward slashes (`reference/guide.md`), not backslashes
- **Name files descriptively**: Use names that indicate content: `form_validation_rules.md`, not `doc2.md`
- **Organize for discovery**: Structure directories by domain or feature
  - Good: `reference/finance.md`, `reference/sales.md`
  - Bad: `docs/file1.md`, `docs/file2.md`
- **Bundle comprehensive resources**: Include complete API docs, extensive examples, large datasets; no context penalty until accessed
- **Prefer scripts for deterministic operations**: Write `validate_form.py` rather than asking Claude to generate validation code
- **Make execution intent clear**:
  - "Run `analyze_form.py` to extract fields" (execute)
  - "See `analyze_form.py` for the extraction algorithm" (read as reference)
- **Test file access patterns**: Verify Claude can navigate your directory structure by testing with real requests

**Example:**

```
bigquery-skill/
├── SKILL.md (overview, points to reference files)
└── reference/
    ├── finance.md (revenue metrics)
    ├── sales.md (pipeline data)
    └── product.md (usage analytics)
```

When the user asks about revenue, Claude reads SKILL.md, sees the reference to `reference/finance.md`, and invokes bash to read just that file. The sales.md and product.md files remain on the filesystem, consuming zero context tokens until needed. This filesystem-based model is what enables progressive disclosure. Claude can navigate and selectively load exactly what each task requires.

For complete details on the technical architecture, see [How Skills work](/en/docs/agents-and-tools/agent-skills/overview#how-skills-work) in the Skills overview.

### MCP tool references

If your Skill uses MCP (Model Context Protocol) tools, always use fully qualified tool names to avoid "tool not found" errors.

**Format**: `ServerName:tool_name`

**Example**:

```markdown theme={null}
Use the BigQuery:bigquery_schema tool to retrieve table schemas.
Use the GitHub:create_issue tool to create issues.
```

Where:

- `BigQuery` and `GitHub` are MCP server names
- `bigquery_schema` and `create_issue` are the tool names within those servers

Without the server prefix, Claude may fail to locate the tool, especially when multiple MCP servers are available.

### Avoid assuming tools are installed

Don't assume packages are available:

`````markdown theme={null}
**Bad example: Assumes installation**:
"Use the pdf library to process the file."

**Good example: Explicit about dependencies**:
"Install required package: `pip install pypdf`

Then use it:

````python
from pypdf import PdfReader
reader = PdfReader("file.pdf")
```"
````
`````

````

## Technical notes

### YAML frontmatter requirements

The SKILL.md frontmatter requires `name` and `description` fields with specific validation rules:

- `name`: Maximum 64 characters, lowercase letters/numbers/hyphens only, no XML tags, no reserved words
- `description`: Maximum 1024 characters, non-empty, no XML tags

See the [Skills overview](/en/docs/agents-and-tools/agent-skills/overview#skill-structure) for complete structure details.

### Token budgets

Keep SKILL.md body under 500 lines for optimal performance. If your content exceeds this, split it into separate files using the progressive disclosure patterns described earlier. For architectural details, see the [Skills overview](/en/docs/agents-and-tools/agent-skills/overview#how-skills-work).

## Checklist for effective Skills

Before sharing a Skill, verify:

### Core quality

- [ ] Description is specific and includes key terms
- [ ] Description includes both what the Skill does and when to use it
- [ ] SKILL.md body is under 500 lines
- [ ] Additional details are in separate files (if needed)
- [ ] No time-sensitive information (or in "old patterns" section)
- [ ] Consistent terminology throughout
- [ ] Examples are concrete, not abstract
- [ ] File references are one level deep
- [ ] Progressive disclosure used appropriately
- [ ] Workflows have clear steps

### Code and scripts

- [ ] Scripts solve problems rather than punt to Claude
- [ ] Error handling is explicit and helpful
- [ ] No "voodoo constants" (all values justified)
- [ ] Required packages listed in instructions and verified as available
- [ ] Scripts have clear documentation
- [ ] No Windows-style paths (all forward slashes)
- [ ] Validation/verification steps for critical operations
- [ ] Feedback loops included for quality-critical tasks

### Testing

- [ ] At least three evaluations created
- [ ] Tested with Haiku, Sonnet, and Opus
- [ ] Tested with real usage scenarios
- [ ] Team feedback incorporated (if applicable)

## Next steps

<CardGroup cols={2}>
  <Card title="Get started with Agent Skills" icon="rocket" href="/en/docs/agents-and-tools/agent-skills/quickstart">
    Create your first Skill
  </Card>

  <Card title="Use Skills in Claude Code" icon="terminal" href="/en/docs/claude-code/skills">
    Create and manage Skills in Claude Code
  </Card>

  <Card title="Use Skills in the Agent SDK" icon="cube" href="/en/api/agent-sdk/skills">
    Use Skills programmatically in TypeScript and Python
  </Card>

  <Card title="Use Skills with the API" icon="code" href="/en/api/skills-guide">
    Upload and use Skills programmatically
  </Card>
</CardGroup>

---

## 4. Get started with Agent Skills in the API

**URL:** https://docs.claude.com/en/docs/agents-and-tools/agent-skills/quickstart

# Get started with Agent Skills in the API

> Learn how to use Agent Skills to create documents with the Claude API in under 10 minutes.

This tutorial shows you how to use Agent Skills to create a PowerPoint presentation. You'll learn how to enable Skills, make a simple request, and access the generated file.

## Prerequisites

- [Anthropic API key](https://console.anthropic.com/settings/keys)
- Python 3.7+ or curl installed
- Basic familiarity with making API requests

## What are Agent Skills?

Pre-built Agent Skills extend Claude's capabilities with specialized expertise for tasks like creating documents, analyzing data, and processing files. Anthropic provides the following pre-built Agent Skills in the API:

- **PowerPoint (pptx)**: Create and edit presentations
- **Excel (xlsx)**: Create and analyze spreadsheets
- **Word (docx)**: Create and edit documents
- **PDF (pdf)**: Generate PDF documents

<Note>
  **Want to create custom Skills?** See the [Agent Skills Cookbook](https://github.com/anthropics/claude-cookbooks/tree/main/skills) for examples of building your own Skills with domain-specific expertise.
</Note>

## Step 1: List available Skills

First, let's see what Skills are available. We'll use the Skills API to list all Anthropic-managed Skills:

<CodeGroup>
  ```python Python theme={null}
  import anthropic

client = anthropic.Anthropic()

# List Anthropic-managed Skills

skills = client.beta.skills.list(
source="anthropic",
betas=["skills-2025-10-02"]
)

for skill in skills.data:
print(f"{skill.id}: {skill.display_title}")

````

```typescript TypeScript theme={null}
import Anthropic from "@anthropic-ai/sdk";

const client = new Anthropic();

// List Anthropic-managed Skills
const skills = await client.beta.skills.list({
  source: "anthropic",
  betas: ["skills-2025-10-02"],
});

for (const skill of skills.data) {
  console.log(`${skill.id}: ${skill.display_title}`);
}
```

```bash Shell theme={null}
curl "https://api.anthropic.com/v1/skills?source=anthropic" \
  -H "x-api-key: $ANTHROPIC_API_KEY" \
  -H "anthropic-version: 2023-06-01" \
  -H "anthropic-beta: skills-2025-10-02"
```

</CodeGroup>

You see the following Skills: `pptx`, `xlsx`, `docx`, and `pdf`.

This API returns each Skill's metadata: its name and description. Claude loads this metadata at startup to know what Skills are available. This is the first level of **progressive disclosure**, where Claude discovers Skills without loading their full instructions yet.

## Step 2: Create a presentation

Now we'll use the PowerPoint Skill to create a presentation about renewable energy. We specify Skills using the `container` parameter in the Messages API:

<CodeGroup>
  ```python Python theme={null}
  import anthropic

client = anthropic.Anthropic()

# Create a message with the PowerPoint Skill

response = client.beta.messages.create(
model="claude-sonnet-4-5-20250929",
max_tokens=4096,
betas=["code-execution-2025-08-25", "skills-2025-10-02"],
container={
"skills": [
{
"type": "anthropic",
"skill_id": "pptx",
"version": "latest"
}
]
},
messages=[{
"role": "user",
"content": "Create a presentation about renewable energy with 5 slides"
}],
tools=[{
"type": "code_execution_20250825",
"name": "code_execution"
}]
)

print(response.content)

````

```typescript TypeScript theme={null}
import Anthropic from '@anthropic-ai/sdk';

const client = new Anthropic();

// Create a message with the PowerPoint Skill
const response = await client.beta.messages.create({
  model: 'claude-sonnet-4-5-20250929',
  max_tokens: 4096,
  betas: ['code-execution-2025-08-25', 'skills-2025-10-02'],
  container: {
    skills: [
      {
        type: 'anthropic',
        skill_id: 'pptx',
        version: 'latest'
      }
    ]
  },
  messages: [{
    role: 'user',
    content: 'Create a presentation about renewable energy with 5 slides'
  }],
  tools: [{
    type: 'code_execution_20250825',
    name: 'code_execution'
  }]
});

console.log(response.content);
````

```bash Shell theme={null}
curl https://api.anthropic.com/v1/messages \
  -H "x-api-key: $ANTHROPIC_API_KEY" \
  -H "anthropic-version: 2023-06-01" \
  -H "anthropic-beta: code-execution-2025-08-25,skills-2025-10-02" \
  -H "content-type: application/json" \
  -d '{
    "model": "claude-sonnet-4-5-20250929",
    "max_tokens": 4096,
    "container": {
      "skills": [
        {
          "type": "anthropic",
          "skill_id": "pptx",
          "version": "latest"
        }
      ]
    },
    "messages": [{
      "role": "user",
      "content": "Create a presentation about renewable energy with 5 slides"
    }],
    "tools": [{
      "type": "code_execution_20250825",
      "name": "code_execution"
    }]
  }'
```

</CodeGroup>

Let's break down what each part does:

- **`container.skills`**: Specifies which Skills Claude can use
- **`type: "anthropic"`**: Indicates this is an Anthropic-managed Skill
- **`skill_id: "pptx"`**: The PowerPoint Skill identifier
- **`version: "latest"`**: The Skill version set to the most recently published
- **`tools`**: Enables code execution (required for Skills)
- **Beta headers**: `code-execution-2025-08-25` and `skills-2025-10-02`

When you make this request, Claude automatically matches your task to the relevant Skill. Since you asked for a presentation, Claude determines the PowerPoint Skill is relevant and loads its full instructions: the second level of progressive disclosure. Then Claude executes the Skill's code to create your presentation.

## Step 3: Download the created file

The presentation was created in the code execution container and saved as a file. The response includes a file reference with a file ID. Extract the file ID and download it using the Files API:

<CodeGroup>
  ```python Python theme={null}
  # Extract file ID from response
  file_id = None
  for block in response.content:
      if block.type == 'tool_use' and block.name == 'code_execution':
          # File ID is in the tool result
          for result_block in block.content:
              if hasattr(result_block, 'file_id'):
                  file_id = result_block.file_id
                  break

if file_id: # Download the file
file_content = client.beta.files.download(
file_id=file_id,
betas=["files-api-2025-04-14"]
)

      # Save to disk
      with open("renewable_energy.pptx", "wb") as f:
          file_content.write_to_file(f.name)

      print(f"Presentation saved to renewable_energy.pptx")

````

```typescript TypeScript theme={null}
// Extract file ID from response
let fileId: string | null = null;
for (const block of response.content) {
  if (block.type === 'tool_use' && block.name === 'code_execution') {
    // File ID is in the tool result
    for (const resultBlock of block.content) {
      if ('file_id' in resultBlock) {
        fileId = resultBlock.file_id;
        break;
      }
    }
  }
}

if (fileId) {
  // Download the file
  const fileContent = await client.beta.files.download(fileId, {
    betas: ['files-api-2025-04-14']
  });

  // Save to disk
  const fs = require('fs');
  fs.writeFileSync('renewable_energy.pptx', Buffer.from(await fileContent.arrayBuffer()));

  console.log('Presentation saved to renewable_energy.pptx');
}
````

```bash Shell theme={null}
# Extract file_id from response (using jq)
FILE_ID=$(echo "$RESPONSE" | jq -r '.content[] | select(.type=="tool_use" and .name=="code_execution") | .content[] | select(.file_id) | .file_id')

# Download the file
curl "https://api.anthropic.com/v1/files/$FILE_ID/content" \
  -H "x-api-key: $ANTHROPIC_API_KEY" \
  -H "anthropic-version: 2023-06-01" \
  -H "anthropic-beta: files-api-2025-04-14" \
  --output renewable_energy.pptx

echo "Presentation saved to renewable_energy.pptx"
```

</CodeGroup>

<Note>
  For complete details on working with generated files, see the [code execution tool documentation](/en/docs/agents-and-tools/tool-use/code-execution-tool#retrieve-generated-files).
</Note>

## Try more examples

Now that you've created your first document with Skills, try these variations:

### Create a spreadsheet

<CodeGroup>
  ```python Python theme={null}
  response = client.beta.messages.create(
      model="claude-sonnet-4-5-20250929",
      max_tokens=4096,
      betas=["code-execution-2025-08-25", "skills-2025-10-02"],
      container={
          "skills": [
              {
                  "type": "anthropic",
                  "skill_id": "xlsx",
                  "version": "latest"
              }
          ]
      },
      messages=[{
          "role": "user",
          "content": "Create a quarterly sales tracking spreadsheet with sample data"
      }],
      tools=[{
          "type": "code_execution_20250825",
          "name": "code_execution"
      }]
  )
  ```

```typescript TypeScript theme={null}
const response = await client.beta.messages.create({
  model: "claude-sonnet-4-5-20250929",
  max_tokens: 4096,
  betas: ["code-execution-2025-08-25", "skills-2025-10-02"],
  container: {
    skills: [
      {
        type: "anthropic",
        skill_id: "xlsx",
        version: "latest",
      },
    ],
  },
  messages: [
    {
      role: "user",
      content: "Create a quarterly sales tracking spreadsheet with sample data",
    },
  ],
  tools: [
    {
      type: "code_execution_20250825",
      name: "code_execution",
    },
  ],
});
```

```bash Shell theme={null}
curl https://api.anthropic.com/v1/messages \
  -H "x-api-key: $ANTHROPIC_API_KEY" \
  -H "anthropic-version: 2023-06-01" \
  -H "anthropic-beta: code-execution-2025-08-25,skills-2025-10-02" \
  -H "content-type: application/json" \
  -d '{
    "model": "claude-sonnet-4-5-20250929",
    "max_tokens": 4096,
    "container": {
      "skills": [
        {
          "type": "anthropic",
          "skill_id": "xlsx",
          "version": "latest"
        }
      ]
    },
    "messages": [{
      "role": "user",
      "content": "Create a quarterly sales tracking spreadsheet with sample data"
    }],
    "tools": [{
      "type": "code_execution_20250825",
      "name": "code_execution"
    }]
  }'
```

</CodeGroup>

### Create a Word document

<CodeGroup>
  ```python Python theme={null}
  response = client.beta.messages.create(
      model="claude-sonnet-4-5-20250929",
      max_tokens=4096,
      betas=["code-execution-2025-08-25", "skills-2025-10-02"],
      container={
          "skills": [
              {
                  "type": "anthropic",
                  "skill_id": "docx",
                  "version": "latest"
              }
          ]
      },
      messages=[{
          "role": "user",
          "content": "Write a 2-page report on the benefits of renewable energy"
      }],
      tools=[{
          "type": "code_execution_20250825",
          "name": "code_execution"
      }]
  )
  ```

```typescript TypeScript theme={null}
const response = await client.beta.messages.create({
  model: "claude-sonnet-4-5-20250929",
  max_tokens: 4096,
  betas: ["code-execution-2025-08-25", "skills-2025-10-02"],
  container: {
    skills: [
      {
        type: "anthropic",
        skill_id: "docx",
        version: "latest",
      },
    ],
  },
  messages: [
    {
      role: "user",
      content: "Write a 2-page report on the benefits of renewable energy",
    },
  ],
  tools: [
    {
      type: "code_execution_20250825",
      name: "code_execution",
    },
  ],
});
```

```bash Shell theme={null}
curl https://api.anthropic.com/v1/messages \
  -H "x-api-key: $ANTHROPIC_API_KEY" \
  -H "anthropic-version: 2023-06-01" \
  -H "anthropic-beta: code-execution-2025-08-25,skills-2025-10-02" \
  -H "content-type: application/json" \
  -d '{
    "model": "claude-sonnet-4-5-20250929",
    "max_tokens": 4096,
    "container": {
      "skills": [
        {
          "type": "anthropic",
          "skill_id": "docx",
          "version": "latest"
        }
      ]
    },
    "messages": [{
      "role": "user",
      "content": "Write a 2-page report on the benefits of renewable energy"
    }],
    "tools": [{
      "type": "code_execution_20250825",
      "name": "code_execution"
    }]
  }'
```

</CodeGroup>

### Generate a PDF

<CodeGroup>
  ```python Python theme={null}
  response = client.beta.messages.create(
      model="claude-sonnet-4-5-20250929",
      max_tokens=4096,
      betas=["code-execution-2025-08-25", "skills-2025-10-02"],
      container={
          "skills": [
              {
                  "type": "anthropic",
                  "skill_id": "pdf",
                  "version": "latest"
              }
          ]
      },
      messages=[{
          "role": "user",
          "content": "Generate a PDF invoice template"
      }],
      tools=[{
          "type": "code_execution_20250825",
          "name": "code_execution"
      }]
  )
  ```

```typescript TypeScript theme={null}
const response = await client.beta.messages.create({
  model: "claude-sonnet-4-5-20250929",
  max_tokens: 4096,
  betas: ["code-execution-2025-08-25", "skills-2025-10-02"],
  container: {
    skills: [
      {
        type: "anthropic",
        skill_id: "pdf",
        version: "latest",
      },
    ],
  },
  messages: [
    {
      role: "user",
      content: "Generate a PDF invoice template",
    },
  ],
  tools: [
    {
      type: "code_execution_20250825",
      name: "code_execution",
    },
  ],
});
```

```bash Shell theme={null}
curl https://api.anthropic.com/v1/messages \
  -H "x-api-key: $ANTHROPIC_API_KEY" \
  -H "anthropic-version: 2023-06-01" \
  -H "anthropic-beta: code-execution-2025-08-25,skills-2025-10-02" \
  -H "content-type: application/json" \
  -d '{
    "model": "claude-sonnet-4-5-20250929",
    "max_tokens": 4096,
    "container": {
      "skills": [
        {
          "type": "anthropic",
          "skill_id": "pdf",
          "version": "latest"
        }
      ]
    },
    "messages": [{
      "role": "user",
      "content": "Generate a PDF invoice template"
    }],
    "tools": [{
      "type": "code_execution_20250825",
      "name": "code_execution"
    }]
  }'
```

</CodeGroup>

## Next steps

Now that you've used pre-built Agent Skills, you can:

<CardGroup cols={2}>
  <Card title="API Guide" icon="book" href="/en/api/skills-guide">
    Use Skills with the Claude API
  </Card>

  <Card title="Create Custom Skills" icon="code" href="/en/api/skills/create-skill">
    Upload your own Skills for specialized tasks
  </Card>

  <Card title="Authoring Guide" icon="pen" href="/en/docs/agents-and-tools/agent-skills/best-practices">
    Learn best practices for writing effective Skills
  </Card>

  <Card title="Use Skills in Claude Code" icon="terminal" href="/en/docs/claude-code/skills">
    Learn about Skills in Claude Code
  </Card>

  <Card title="Use Skills in the Agent SDK" icon="cube" href="/en/api/agent-sdk/skills">
    Use Skills programmatically in TypeScript and Python
  </Card>

  <Card title="Agent Skills Cookbook" icon="book-open" href="https://github.com/anthropics/anthropic-cookbook/blob/main/skills/README.md">
    Explore example Skills and implementation patterns
  </Card>
</CardGroup>

---

## 5. Memory tool

**URL:** https://docs.claude.com/en/docs/agents-and-tools/tool-use/memory-tool

# Memory tool

The memory tool enables Claude to store and retrieve information across conversations through a memory file directory. Claude can create, read, update, and delete files that persist between sessions, allowing it to build knowledge over time without keeping everything in the context window.

The memory tool operates client-side—you control where and how the data is stored through your own infrastructure.

<Note>
  The memory tool is currently in beta. To enable it, use the beta header `context-management-2025-06-27` in your API requests.

Please reach out through our [feedback form](https://forms.gle/YXC2EKGMhjN1c4L88) to share your feedback on this feature.
</Note>

## Use cases

- Maintain project context across multiple agent executions
- Learn from past interactions, decisions, and feedback
- Build knowledge bases over time
- Enable cross-conversation learning where Claude improves at recurring workflows

## How it works

When enabled, Claude automatically checks its memory directory before starting tasks. Claude can create, read, update, and delete files in the `/memories` directory to store what it learns while working, then reference those memories in future conversations to handle similar tasks more effectively or pick up where it left off.

Since this is a client-side tool, Claude makes tool calls to perform memory operations, and your application executes those operations locally. This gives you complete control over where and how the memory is stored. For security, you should restrict all memory operations to the `/memories` directory.

### Example: How memory tool calls work

When you ask Claude to help with a task, Claude automatically checks its memory directory first. Here's what a typical interaction looks like:

**1. User request:**

```
"Help me respond to this customer service ticket."
```

**2. Claude checks the memory directory:**

```
"I'll help you respond to the customer service ticket. Let me check my memory for any previous context."
```

Claude calls the memory tool:

```json theme={null}
{
  "type": "tool_use",
  "id": "toolu_01C4D5E6F7G8H9I0J1K2L3M4",
  "name": "memory",
  "input": {
    "command": "view",
    "path": "/memories"
  }
}
```

**3. Your application returns the directory contents:**

```json theme={null}
{
  "type": "tool_result",
  "tool_use_id": "toolu_01C4D5E6F7G8H9I0J1K2L3M4",
  "content": "Directory: /memories\n- customer_service_guidelines.xml\n- refund_policies.xml"
}
```

**4. Claude reads relevant files:**

```json theme={null}
{
  "type": "tool_use",
  "id": "toolu_01D5E6F7G8H9I0J1K2L3M4N5",
  "name": "memory",
  "input": {
    "command": "view",
    "path": "/memories/customer_service_guidelines.xml"
  }
}
```

**5. Your application returns the file contents:**

```json theme={null}
{
  "type": "tool_result",
  "tool_use_id": "toolu_01D5E6F7G8H9I0J1K2L3M4N5",
  "content": "<guidelines>\n<addressing_customers>\n- Always address customers by their first name\n- Use empathetic language\n..."
}
```

**6. Claude uses the memory to help:**

```
"Based on your customer service guidelines, I can help you craft a response. Please share the ticket details..."
```

## Supported models

The memory tool is available on:

- Claude Sonnet 4.5 (`claude-sonnet-4-5-20250929`)
- Claude Sonnet 4 (`claude-sonnet-4-20250514`)
- Claude Haiku 4.5 (`claude-haiku-4-5-20251001`)
- Claude Opus 4.1 (`claude-opus-4-1-20250805`)
- Claude Opus 4 (`claude-opus-4-20250514`)

## Getting started

To use the memory tool:

1. Include the beta header `context-management-2025-06-27` in your API requests
2. Add the memory tool to your request
3. Implement client-side handlers for memory operations

<Note>
  To handle memory tool operations in your application, you need to implement handlers for each memory command. Our SDKs provide memory tool helpers that handle the tool interface—you can subclass `BetaAbstractMemoryTool` (Python) or use `betaMemoryTool` (TypeScript) to implement your own memory backend (file-based, database, cloud storage, encrypted files, etc.).

For working examples, see:

- Python: [examples/memory/basic.py](https://github.com/anthropics/anthropic-sdk-python/blob/main/examples/memory/basic.py)
- TypeScript: [examples/tools-helpers-memory.ts](https://github.com/anthropics/anthropic-sdk-typescript/blob/main/examples/tools-helpers-memory.ts)
  </Note>

## Basic usage

<CodeGroup>
  ````bash cURL theme={null}
  curl https://api.anthropic.com/v1/messages \
      --header "x-api-key: $ANTHROPIC_API_KEY" \
      --header "anthropic-version: 2023-06-01" \
      --header "content-type: application/json" \
      --header "anthropic-beta: context-management-2025-06-27" \
      --data '{
          "model": "claude-sonnet-4-5",
          "max_tokens": 2048,
          "messages": [
              {
                  "role": "user",
                  "content": "I'\''m working on a Python web scraper that keeps crashing with a timeout error. Here'\''s the problematic function:\n\n```python\ndef fetch_page(url, retries=3):\n    for i in range(retries):\n        try:\n            response = requests.get(url, timeout=5)\n            return response.text\n        except requests.exceptions.Timeout:\n            if i == retries - 1:\n                raise\n            time.sleep(1)\n```\n\nPlease help me debug this."
              }
          ],
          "tools": [{
              "type": "memory_20250818",
              "name": "memory"
          }]
      }'
  ````

````python Python theme={null}
import anthropic

client = anthropic.Anthropic()

message = client.beta.messages.create(
    model="claude-sonnet-4-5",
    max_tokens=2048,
    messages=[
        {
            "role": "user",
            "content": "I'm working on a Python web scraper that keeps crashing with a timeout error. Here's the problematic function:\n\n```python\ndef fetch_page(url, retries=3):\n    for i in range(retries):\n        try:\n            response = requests.get(url, timeout=5)\n            return response.text\n        except requests.exceptions.Timeout:\n            if i == retries - 1:\n                raise\n            time.sleep(1)\n```\n\nPlease help me debug this."
        }
    ],
    tools=[{
        "type": "memory_20250818",
        "name": "memory"
    }],
    betas=["context-management-2025-06-27"]
)
````

````typescript TypeScript theme={null}
import Anthropic from "@anthropic-ai/sdk";

const anthropic = new Anthropic({
  apiKey: process.env.ANTHROPIC_API_KEY,
});

const message = await anthropic.beta.messages.create({
  model: "claude-sonnet-4-5",
  max_tokens: 2048,
  messages: [
    {
      role: "user",
      content:
        "I'm working on a Python web scraper that keeps crashing with a timeout error. Here's the problematic function:\n\n```python\ndef fetch_page(url, retries=3):\n    for i in range(retries):\n        try:\n            response = requests.get(url, timeout=5)\n            return response.text\n        except requests.exceptions.Timeout:\n            if i == retries - 1:\n                raise\n            time.sleep(1)\n```\n\nPlease help me debug this.",
    },
  ],
  tools: [
    {
      type: "memory_20250818",
      name: "memory",
    },
  ],
  betas: ["context-management-2025-06-27"],
});
````

</CodeGroup>

## Tool commands

Your client-side implementation needs to handle these memory tool commands:

### view

Shows directory contents or file contents with optional line ranges:

```json theme={null}
{
  "command": "view",
  "path": "/memories",
  "view_range": [1, 10] // Optional: view specific lines
}
```

### create

Create or overwrite a file:

```json theme={null}
{
  "command": "create",
  "path": "/memories/notes.txt",
  "file_text": "Meeting notes:\n- Discussed project timeline\n- Next steps defined\n"
}
```

### str_replace

Replace text in a file:

```json theme={null}
{
  "command": "str_replace",
  "path": "/memories/preferences.txt",
  "old_str": "Favorite color: blue",
  "new_str": "Favorite color: green"
}
```

### insert

Insert text at a specific line:

```json theme={null}
{
  "command": "insert",
  "path": "/memories/todo.txt",
  "insert_line": 2,
  "insert_text": "- Review memory tool documentation\n"
}
```

### delete

Delete a file or directory:

```json theme={null}
{
  "command": "delete",
  "path": "/memories/old_file.txt"
}
```

### rename

Rename or move a file/directory:

```json theme={null}
{
  "command": "rename",
  "old_path": "/memories/draft.txt",
  "new_path": "/memories/final.txt"
}
```

## Prompting guidance

We automatically include this instruction to the system prompt when the memory tool is included:

```
IMPORTANT: ALWAYS VIEW YOUR MEMORY DIRECTORY BEFORE DOING ANYTHING ELSE.
MEMORY PROTOCOL:
1. Use the `view` command of your `memory` tool to check for earlier progress.
2. ... (work on the task) ...
     - As you make progress, record status / progress / thoughts etc in your memory.
ASSUME INTERRUPTION: Your context window might be reset at any moment, so you risk losing any progress that is not recorded in your memory directory.
```

If you observe Claude creating cluttered memory files, you can include this instruction:

> Note: when editing your memory folder, always try to keep its content up-to-date, coherent and organized. You can rename or delete files that are no longer relevant. Do not create new files unless necessary.

You can also guide what Claude writes to memory, e.g., "Only write down information relevant to \<topic> in your memory system."

## Security considerations

Here are important security concerns when implementing your memory store:

### Sensitive information

Claude will usually refuse to write down sensitive information in memory files. However, you may want to implement stricter validation that strips out potentially sensitive information.

### File storage size

Consider tracking memory file sizes and preventing files from growing too large. Consider adding a maximum number of characters the memory read command can return, and let Claude paginate through contents.

### Memory expiration

Consider clearing out memory files periodically that haven't been accessed in an extended time.

### Path traversal protection

<Warning>
  Malicious path inputs could attempt to access files outside the `/memories` directory. Your implementation **MUST** validate all paths to prevent directory traversal attacks.
</Warning>

Consider these safeguards:

- Validate that all paths start with `/memories`
- Resolve paths to their canonical form and verify they remain within the memory directory
- Reject paths containing sequences like `../`, `..\\`, or other traversal patterns
- Watch for URL-encoded traversal sequences (`%2e%2e%2f`)
- Use your language's built-in path security utilities (e.g., Python's `pathlib.Path.resolve()` and `relative_to()`)

## Error handling

The memory tool uses the same error handling patterns as the [text editor tool](/en/docs/agents-and-tools/tool-use/text-editor-tool#handle-errors). Common errors include file not found, permission errors, and invalid paths.

## Using with Context Editing

The memory tool can be combined with [context editing](/en/docs/build-with-claude/context-editing), which automatically clears old tool results when conversation context grows beyond a configured threshold. This combination enables long-running agentic workflows that would otherwise exceed context limits.

### How they work together

When context editing is enabled and your conversation approaches the clearing threshold, Claude automatically receives a warning notification. This prompts Claude to preserve any important information from tool results into memory files before those results are cleared from the context window.

After tool results are cleared, Claude can retrieve the stored information from memory files whenever needed, effectively treating memory as an extension of its working context. This allows Claude to:

- Continue complex, multi-step workflows without losing critical information
- Reference past work and decisions even after tool results are removed
- Maintain coherent context across conversations that would exceed typical context limits
- Build up a knowledge base over time while keeping the active context window manageable

### Example workflow

Consider a code refactoring project with many file operations:

1. Claude makes numerous edits to files, generating many tool results
2. As the context grows and approaches your threshold, Claude receives a warning
3. Claude summarizes the changes made so far to a memory file (e.g., `/memories/refactoring_progress.xml`)
4. Context editing clears the older tool results automatically
5. Claude continues working, referencing the memory file when it needs to recall what changes were already completed
6. The workflow can continue indefinitely, with Claude managing both active context and persistent memory

### Configuration

To use both features together:

<CodeGroup>
  ```python Python theme={null}
  response = client.beta.messages.create(
      model="claude-sonnet-4-5",
      max_tokens=4096,
      messages=[...],
      tools=[
          {
              "type": "memory_20250818",
              "name": "memory"
          },
          # Your other tools
      ],
      betas=["context-management-2025-06-27"],
      context_management={
          "edits": [
              {
                  "type": "clear_tool_uses_20250919",
                  "trigger": {
                      "type": "input_tokens",
                      "value": 100000
                  },
                  "keep": {
                      "type": "tool_uses",
                      "value": 3
                  }
              }
          ]
      }
  )
  ```

```typescript TypeScript theme={null}
import Anthropic from '@anthropic-ai/sdk';

const anthropic = new Anthropic({
  apiKey: process.env.ANTHROPIC_API_KEY,
});

const response = await anthropic.beta.messages.create({
  model: "claude-sonnet-4-5",
  max_tokens: 4096,
  messages: [...],
  tools: [
    {
      type: "memory_20250818",
      name: "memory"
    },
    // Your other tools
  ],
  betas: ["context-management-2025-06-27"],
  context_management: {
    edits: [
      {
        type: "clear_tool_uses_20250919",
        trigger: {
          type: "input_tokens",
          value: 100000
        },
        keep: {
          type: "tool_uses",
          value: 3
        }
      }
    ]
  }
});
```

</CodeGroup>

You can also exclude memory tool calls from being cleared to ensure Claude always has access to recent memory operations:

<CodeGroup>
  ```python Python theme={null}
  context_management={
      "edits": [
          {
              "type": "clear_tool_uses_20250919",
              "exclude_tools": ["memory"]
          }
      ]
  }
  ```

```typescript TypeScript theme={null}
context_management: {
  edits: [
    {
      type: "clear_tool_uses_20250919",
      exclude_tools: ["memory"],
    },
  ];
}
```

</CodeGroup>

---

## 6. Web search tool

**URL:** https://docs.claude.com/en/docs/agents-and-tools/tool-use/web-search-tool

# Web search tool

The web search tool gives Claude direct access to real-time web content, allowing it to answer questions with up-to-date information beyond its knowledge cutoff. Claude automatically cites sources from search results as part of its answer.

<Note>
  Please reach out through our [feedback form](https://forms.gle/sWjBtsrNEY2oKGuE8) to share your experience with the web search tool.
</Note>

## Supported models

Web search is available on:

- Claude Sonnet 4.5 (`claude-sonnet-4-5-20250929`)
- Claude Sonnet 4 (`claude-sonnet-4-20250514`)
- Claude Sonnet 3.7 ([deprecated](/en/docs/about-claude/model-deprecations)) (`claude-3-7-sonnet-20250219`)
- Claude Haiku 4.5 (`claude-haiku-4-5-20251001`)
- Claude Haiku 3.5 (`claude-3-5-haiku-latest`)
- Claude Opus 4.1 (`claude-opus-4-1-20250805`)
- Claude Opus 4 (`claude-opus-4-20250514`)

## How web search works

When you add the web search tool to your API request:

1. Claude decides when to search based on the prompt.
2. The API executes the searches and provides Claude with the results. This process may repeat multiple times throughout a single request.
3. At the end of its turn, Claude provides a final response with cited sources.

## How to use web search

<Note>
  Your organization's administrator must enable web search in [Console](https://console.anthropic.com/settings/privacy).
</Note>

Provide the web search tool in your API request:

<CodeGroup>
  ```bash Shell theme={null}
  curl https://api.anthropic.com/v1/messages \
      --header "x-api-key: $ANTHROPIC_API_KEY" \
      --header "anthropic-version: 2023-06-01" \
      --header "content-type: application/json" \
      --data '{
          "model": "claude-sonnet-4-5",
          "max_tokens": 1024,
          "messages": [
              {
                  "role": "user",
                  "content": "What's the weather in NYC?"
              }
          ],
          "tools": [{
              "type": "web_search_20250305",
              "name": "web_search",
              "max_uses": 5
          }]
      }'
  ```

```python Python theme={null}
import anthropic

client = anthropic.Anthropic()

response = client.messages.create(
    model="claude-sonnet-4-5",
    max_tokens=1024,
    messages=[
        {
            "role": "user",
            "content": "What's the weather in NYC?"
        }
    ],
    tools=[{
        "type": "web_search_20250305",
        "name": "web_search",
        "max_uses": 5
    }]
)
print(response)
```

```typescript TypeScript theme={null}
import { Anthropic } from "@anthropic-ai/sdk";

const anthropic = new Anthropic();

async function main() {
  const response = await anthropic.messages.create({
    model: "claude-sonnet-4-5",
    max_tokens: 1024,
    messages: [
      {
        role: "user",
        content: "What's the weather in NYC?",
      },
    ],
    tools: [
      {
        type: "web_search_20250305",
        name: "web_search",
        max_uses: 5,
      },
    ],
  });

  console.log(response);
}

main().catch(console.error);
```

</CodeGroup>

### Tool definition

The web search tool supports the following parameters:

```json JSON theme={null}
{
  "type": "web_search_20250305",
  "name": "web_search",

  // Optional: Limit the number of searches per request
  "max_uses": 5,

  // Optional: Only include results from these domains
  "allowed_domains": ["example.com", "trusteddomain.org"],

  // Optional: Never include results from these domains
  "blocked_domains": ["untrustedsource.com"],

  // Optional: Localize search results
  "user_location": {
    "type": "approximate",
    "city": "San Francisco",
    "region": "California",
    "country": "US",
    "timezone": "America/Los_Angeles"
  }
}
```

#### Max uses

The `max_uses` parameter limits the number of searches performed. If Claude attempts more searches than allowed, the `web_search_tool_result` will be an error with the `max_uses_exceeded` error code.

#### Domain filtering

When using domain filters:

- Domains should not include the HTTP/HTTPS scheme (use `example.com` instead of `https://example.com`)
- Subdomains are automatically included (`example.com` covers `docs.example.com`)
- Subpaths are supported (`example.com/blog`)
- You can use either `allowed_domains` or `blocked_domains`, but not both in the same request.

<Note>
  Request-level domain restrictions must be compatible with organization-level domain restrictions configured in the Console. Request-level domains can only further restrict domains, not override or expand beyond the organization-level list. If your request includes domains that conflict with organization settings, the API will return a validation error.
</Note>

#### Localization

The `user_location` parameter allows you to localize search results based on a user's location.

- `type`: The type of location (must be `approximate`)
- `city`: The city name
- `region`: The region or state
- `country`: The country
- `timezone`: The [IANA timezone ID](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones).

### Response

Here's an example response structure:

```json theme={null}
{
  "role": "assistant",
  "content": [
    // 1. Claude's decision to search
    {
      "type": "text",
      "text": "I'll search for when Claude Shannon was born."
    },
    // 2. The search query used
    {
      "type": "server_tool_use",
      "id": "srvtoolu_01WYG3ziw53XMcoyKL4XcZmE",
      "name": "web_search",
      "input": {
        "query": "claude shannon birth date"
      }
    },
    // 3. Search results
    {
      "type": "web_search_tool_result",
      "tool_use_id": "srvtoolu_01WYG3ziw53XMcoyKL4XcZmE",
      "content": [
        {
          "type": "web_search_result",
          "url": "https://en.wikipedia.org/wiki/Claude_Shannon",
          "title": "Claude Shannon - Wikipedia",
          "encrypted_content": "EqgfCioIARgBIiQ3YTAwMjY1Mi1mZjM5LTQ1NGUtODgxNC1kNjNjNTk1ZWI3Y...",
          "page_age": "April 30, 2025"
        }
      ]
    },
    {
      "text": "Based on the search results, ",
      "type": "text"
    },
    // 4. Claude's response with citations
    {
      "text": "Claude Shannon was born on April 30, 1916, in Petoskey, Michigan",
      "type": "text",
      "citations": [
        {
          "type": "web_search_result_location",
          "url": "https://en.wikipedia.org/wiki/Claude_Shannon",
          "title": "Claude Shannon - Wikipedia",
          "encrypted_index": "Eo8BCioIAhgBIiQyYjQ0OWJmZi1lNm..",
          "cited_text": "Claude Elwood Shannon (April 30, 1916 – February 24, 2001) was an American mathematician, electrical engineer, computer scientist, cryptographer and i..."
        }
      ]
    }
  ],
  "id": "msg_a930390d3a",
  "usage": {
    "input_tokens": 6039,
    "output_tokens": 931,
    "server_tool_use": {
      "web_search_requests": 1
    }
  },
  "stop_reason": "end_turn"
}
```

#### Search results

Search results include:

- `url`: The URL of the source page
- `title`: The title of the source page
- `page_age`: When the site was last updated
- `encrypted_content`: Encrypted content that must be passed back in multi-turn conversations for citations

#### Citations

Citations are always enabled for web search, and each `web_search_result_location` includes:

- `url`: The URL of the cited source
- `title`: The title of the cited source
- `encrypted_index`: A reference that must be passed back for multi-turn conversations.
- `cited_text`: Up to 150 characters of the cited content

The web search citation fields `cited_text`, `title`, and `url` do not count towards input or output token usage.

<Note>
  When displaying API outputs directly to end users, citations must be included to the original source. If you are making modifications to API outputs, including by reprocessing and/or combining them with your own material before displaying them to end users, display citations as appropriate based on consultation with your legal team.
</Note>

#### Errors

When the web search tool encounters an error (such as hitting rate limits), the Claude API still returns a 200 (success) response. The error is represented within the response body using the following structure:

```json theme={null}
{
  "type": "web_search_tool_result",
  "tool_use_id": "servertoolu_a93jad",
  "content": {
    "type": "web_search_tool_result_error",
    "error_code": "max_uses_exceeded"
  }
}
```

These are the possible error codes:

- `too_many_requests`: Rate limit exceeded
- `invalid_input`: Invalid search query parameter
- `max_uses_exceeded`: Maximum web search tool uses exceeded
- `query_too_long`: Query exceeds maximum length
- `unavailable`: An internal error occurred

#### `pause_turn` stop reason

The response may include a `pause_turn` stop reason, which indicates that the API paused a long-running turn. You may provide the response back as-is in a subsequent request to let Claude continue its turn, or modify the content if you wish to interrupt the conversation.

## Prompt caching

Web search works with [prompt caching](/en/docs/build-with-claude/prompt-caching). To enable prompt caching, add at least one `cache_control` breakpoint in your request. The system will automatically cache up until the last `web_search_tool_result` block when executing the tool.

For multi-turn conversations, set a `cache_control` breakpoint on or after the last `web_search_tool_result` block to reuse cached content.

For example, to use prompt caching with web search for a multi-turn conversation:

<CodeGroup>
  ```python  theme={null}
  import anthropic

client = anthropic.Anthropic()

# First request with web search and cache breakpoint

messages = [
{
"role": "user",
"content": "What's the current weather in San Francisco today?"
}
]

response1 = client.messages.create(
model="claude-sonnet-4-5",
max_tokens=1024,
messages=messages,
tools=[{
"type": "web_search_20250305",
"name": "web_search",
"user_location": {
"type": "approximate",
"city": "San Francisco",
"region": "California",
"country": "US",
"timezone": "America/Los_Angeles"
}
}]
)

# Add Claude's response to the conversation

messages.append({
"role": "assistant",
"content": response1.content
})

# Second request with cache breakpoint after the search results

messages.append({
"role": "user",
"content": "Should I expect rain later this week?",
"cache_control": {"type": "ephemeral"} # Cache up to this point
})

response2 = client.messages.create(
model="claude-sonnet-4-5",
max_tokens=1024,
messages=messages,
tools=[{
"type": "web_search_20250305",
"name": "web_search",
"user_location": {
"type": "approximate",
"city": "San Francisco",
"region": "California",
"country": "US",
"timezone": "America/Los_Angeles"
}
}]
)

# The second response will benefit from cached search results

# while still being able to perform new searches if needed

print(f"Cache read tokens: {response2.usage.get('cache_read_input_tokens', 0)}")

````
</CodeGroup>

## Streaming

With streaming enabled, you'll receive search events as part of the stream. There will be a pause while the search executes:

```javascript  theme={null}
event: message_start
data: {"type": "message_start", "message": {"id": "msg_abc123", "type": "message"}}

event: content_block_start
data: {"type": "content_block_start", "index": 0, "content_block": {"type": "text", "text": ""}}

// Claude's decision to search

event: content_block_start
data: {"type": "content_block_start", "index": 1, "content_block": {"type": "server_tool_use", "id": "srvtoolu_xyz789", "name": "web_search"}}

// Search query streamed
event: content_block_delta
data: {"type": "content_block_delta", "index": 1, "delta": {"type": "input_json_delta", "partial_json": "{\"query\":\"latest quantum computing breakthroughs 2025\"}"}}

// Pause while search executes

// Search results streamed
event: content_block_start
data: {"type": "content_block_start", "index": 2, "content_block": {"type": "web_search_tool_result", "tool_use_id": "srvtoolu_xyz789", "content": [{"type": "web_search_result", "title": "Quantum Computing Breakthroughs in 2025", "url": "https://example.com"}]}}

// Claude's response with citations (omitted in this example)
````

## Batch requests

You can include the web search tool in the [Messages Batches API](/en/docs/build-with-claude/batch-processing). Web search tool calls through the Messages Batches API are priced the same as those in regular Messages API requests.

## Usage and pricing

Web search usage is charged in addition to token usage:

```json theme={null}
"usage": {
  "input_tokens": 105,
  "output_tokens": 6039,
  "cache_read_input_tokens": 7123,
  "cache_creation_input_tokens": 7345,
  "server_tool_use": {
    "web_search_requests": 1
  }
}
```

Web search is available on the Claude API for **\$10 per 1,000 searches**, plus standard token costs for search-generated content. Web search results retrieved throughout a conversation are counted as input tokens, in search iterations executed during a single turn and in subsequent conversation turns.

Each web search counts as one use, regardless of the number of results returned. If an error occurs during web search, the web search will not be billed.

---

## 7. Web fetch tool

**URL:** https://docs.claude.com/en/docs/agents-and-tools/tool-use/web-fetch-tool

# Web fetch tool

The web fetch tool allows Claude to retrieve full content from specified web pages and PDF documents.

<Note>
  The web fetch tool is currently in beta. To enable it, use the beta header `web-fetch-2025-09-10` in your API requests.

Please use [this form](https://forms.gle/NhWcgmkcvPCMmPE86) to provide feedback on the quality of the model responses, the API itself, or the quality of the documentation.
</Note>

<Warning>
  Enabling the web fetch tool in environments where Claude processes untrusted input alongside sensitive data poses data exfiltration risks. We recommend only using this tool in trusted environments or when handling non-sensitive data.

To minimize exfiltration risks, Claude is not allowed to dynamically construct URLs. Claude can only fetch URLs that have been explicitly provided by the user or that come from previous web search or web fetch results. However, there is still residual risk that should be carefully considered when using this tool.

If data exfiltration is a concern, consider:

- Disabling the web fetch tool entirely
- Using the `max_uses` parameter to limit the number of requests
- Using the `allowed_domains` parameter to restrict to known safe domains
  </Warning>

## Supported models

Web fetch is available on:

- Claude Sonnet 4.5 (`claude-sonnet-4-5-20250929`)
- Claude Sonnet 4 (`claude-sonnet-4-20250514`)
- Claude Sonnet 3.7 ([deprecated](/en/docs/about-claude/model-deprecations)) (`claude-3-7-sonnet-20250219`)
- Claude Haiku 4.5 (`claude-haiku-4-5-20251001`)
- Claude Haiku 3.5 (`claude-3-5-haiku-latest`)
- Claude Opus 4.1 (`claude-opus-4-1-20250805`)
- Claude Opus 4 (`claude-opus-4-20250514`)

## How web fetch works

When you add the web fetch tool to your API request:

1. Claude decides when to fetch content based on the prompt and available URLs.
2. The API retrieves the full text content from the specified URL.
3. For PDFs, automatic text extraction is performed.
4. Claude analyzes the fetched content and provides a response with optional citations.

<Note>
  The web fetch tool currently does not support web sites dynamically rendered via Javascript.
</Note>

## How to use web fetch

Provide the web fetch tool in your API request:

<CodeGroup>
  ```bash Shell theme={null}
  curl https://api.anthropic.com/v1/messages \
      --header "x-api-key: $ANTHROPIC_API_KEY" \
      --header "anthropic-version: 2023-06-01" \
      --header "anthropic-beta: web-fetch-2025-09-10" \
      --header "content-type: application/json" \
      --data '{
          "model": "claude-sonnet-4-5",
          "max_tokens": 1024,
          "messages": [
              {
                  "role": "user",
                  "content": "Please analyze the content at https://example.com/article"
              }
          ],
          "tools": [{
              "type": "web_fetch_20250910",
              "name": "web_fetch",
              "max_uses": 5
          }]
      }'
  ```

```python Python theme={null}
import anthropic

client = anthropic.Anthropic()

response = client.messages.create(
    model="claude-sonnet-4-5",
    max_tokens=1024,
    messages=[
        {
            "role": "user",
            "content": "Please analyze the content at https://example.com/article"
        }
    ],
    tools=[{
        "type": "web_fetch_20250910",
        "name": "web_fetch",
        "max_uses": 5
    }],
    extra_headers={
        "anthropic-beta": "web-fetch-2025-09-10"
    }
)
print(response)
```

```typescript TypeScript theme={null}
import { Anthropic } from "@anthropic-ai/sdk";

const anthropic = new Anthropic();

async function main() {
  const response = await anthropic.messages.create({
    model: "claude-sonnet-4-5",
    max_tokens: 1024,
    messages: [
      {
        role: "user",
        content: "Please analyze the content at https://example.com/article",
      },
    ],
    tools: [
      {
        type: "web_fetch_20250910",
        name: "web_fetch",
        max_uses: 5,
      },
    ],
    headers: {
      "anthropic-beta": "web-fetch-2025-09-10",
    },
  });

  console.log(response);
}

main().catch(console.error);
```

</CodeGroup>

### Tool definition

The web fetch tool supports the following parameters:

```json JSON theme={null}
{
  "type": "web_fetch_20250910",
  "name": "web_fetch",

  // Optional: Limit the number of fetches per request
  "max_uses": 10,

  // Optional: Only fetch from these domains
  "allowed_domains": ["example.com", "docs.example.com"],

  // Optional: Never fetch from these domains
  "blocked_domains": ["private.example.com"],

  // Optional: Enable citations for fetched content
  "citations": {
    "enabled": true
  },

  // Optional: Maximum content length in tokens
  "max_content_tokens": 100000
}
```

#### Max uses

The `max_uses` parameter limits the number of web fetches performed. If Claude attempts more fetches than allowed, the `web_fetch_tool_result` will be an error with the `max_uses_exceeded` error code. There is currently no default limit.

#### Domain filtering

When using domain filters:

- Domains should not include the HTTP/HTTPS scheme (use `example.com` instead of `https://example.com`)
- Subdomains are automatically included (`example.com` covers `docs.example.com`)
- Subpaths are supported (`example.com/blog`)
- You can use either `allowed_domains` or `blocked_domains`, but not both in the same request.

<Warning>
  Be aware that Unicode characters in domain names can create security vulnerabilities through homograph attacks, where visually similar characters from different scripts can bypass domain filters. For example, `аmazon.com` (using Cyrillic 'а') may appear identical to `amazon.com` but represents a different domain.

When configuring domain allow/block lists:

- Use ASCII-only domain names when possible
- Consider that URL parsers may handle Unicode normalization differently
- Test your domain filters with potential homograph variations
- Regularly audit your domain configurations for suspicious Unicode characters
  </Warning>

#### Content limits

The `max_content_tokens` parameter limits the amount of content that will be included in the context. If the fetched content exceeds this limit, it will be truncated. This helps control token usage when fetching large documents.

<Note>
  The `max_content_tokens` parameter limit is approximate. The actual number of input tokens used can vary by a small amount.
</Note>

#### Citations

Unlike web search where citations are always enabled, citations are optional for web fetch. Set `"citations": {"enabled": true}` to enable Claude to cite specific passages from fetched documents.

<Note>
  When displaying API outputs directly to end users, citations must be included to the original source. If you are making modifications to API outputs, including by reprocessing and/or combining them with your own material before displaying them to end users, display citations as appropriate based on consultation with your legal team.
</Note>

### Response

Here's an example response structure:

```json theme={null}
{
  "role": "assistant",
  "content": [
    // 1. Claude's decision to fetch
    {
      "type": "text",
      "text": "I'll fetch the content from the article to analyze it."
    },
    // 2. The fetch request
    {
      "type": "server_tool_use",
      "id": "srvtoolu_01234567890abcdef",
      "name": "web_fetch",
      "input": {
        "url": "https://example.com/article"
      }
    },
    // 3. Fetch results
    {
      "type": "web_fetch_tool_result",
      "tool_use_id": "srvtoolu_01234567890abcdef",
      "content": {
        "type": "web_fetch_result",
        "url": "https://example.com/article",
        "content": {
          "type": "document",
          "source": {
            "type": "text",
            "media_type": "text/plain",
            "data": "Full text content of the article..."
          },
          "title": "Article Title",
          "citations": { "enabled": true }
        },
        "retrieved_at": "2025-08-25T10:30:00Z"
      }
    },
    // 4. Claude's analysis with citations (if enabled)
    {
      "text": "Based on the article, ",
      "type": "text"
    },
    {
      "text": "the main argument presented is that artificial intelligence will transform healthcare",
      "type": "text",
      "citations": [
        {
          "type": "char_location",
          "document_index": 0,
          "document_title": "Article Title",
          "start_char_index": 1234,
          "end_char_index": 1456,
          "cited_text": "Artificial intelligence is poised to revolutionize healthcare delivery..."
        }
      ]
    }
  ],
  "id": "msg_a930390d3a",
  "usage": {
    "input_tokens": 25039,
    "output_tokens": 931,
    "server_tool_use": {
      "web_fetch_requests": 1
    }
  },
  "stop_reason": "end_turn"
}
```

#### Fetch results

Fetch results include:

- `url`: The URL that was fetched
- `content`: A document block containing the fetched content
- `retrieved_at`: Timestamp when the content was retrieved

<Note>
  The web fetch tool caches results to improve performance and reduce redundant requests. This means the content returned may not always be the latest version available at the URL. The cache behavior is managed automatically and may change over time to optimize for different content types and usage patterns.
</Note>

For PDF documents, the content will be returned as base64-encoded data:

```json theme={null}
{
  "type": "web_fetch_tool_result",
  "tool_use_id": "srvtoolu_02",
  "content": {
    "type": "web_fetch_result",
    "url": "https://example.com/paper.pdf",
    "content": {
      "type": "document",
      "source": {
        "type": "base64",
        "media_type": "application/pdf",
        "data": "JVBERi0xLjQKJcOkw7zDtsOfCjIgMCBvYmo..."
      },
      "citations": { "enabled": true }
    },
    "retrieved_at": "2025-08-25T10:30:02Z"
  }
}
```

#### Errors

When the web fetch tool encounters an error, the Claude API returns a 200 (success) response with the error represented in the response body:

```json theme={null}
{
  "type": "web_fetch_tool_result",
  "tool_use_id": "srvtoolu_a93jad",
  "content": {
    "type": "web_fetch_tool_error",
    "error_code": "url_not_accessible"
  }
}
```

These are the possible error codes:

- `invalid_input`: Invalid URL format
- `url_too_long`: URL exceeds maximum length (250 characters)
- `url_not_allowed`: URL blocked by domain filtering rules and model restrictions
- `url_not_accessible`: Failed to fetch content (HTTP error)
- `too_many_requests`: Rate limit exceeded
- `unsupported_content_type`: Content type not supported (only text and PDF)
- `max_uses_exceeded`: Maximum web fetch tool uses exceeded
- `unavailable`: An internal error occurred

## URL validation

For security reasons, the web fetch tool can only fetch URLs that have previously appeared in the conversation context. This includes:

- URLs in user messages
- URLs in client-side tool results
- URLs from previous web search or web fetch results

The tool cannot fetch arbitrary URLs that Claude generates or URLs from container-based server tools (Code Execution, Bash, etc.).

## Combined search and fetch

Web fetch works seamlessly with web search for comprehensive information gathering:

```python theme={null}
import anthropic

client = anthropic.Anthropic()

response = client.messages.create(
    model="claude-sonnet-4-5",
    max_tokens=4096,
    messages=[
        {
            "role": "user",
            "content": "Find recent articles about quantum computing and analyze the most relevant one in detail"
        }
    ],
    tools=[
        {
            "type": "web_search_20250305",
            "name": "web_search",
            "max_uses": 3
        },
        {
            "type": "web_fetch_20250910",
            "name": "web_fetch",
            "max_uses": 5,
            "citations": {"enabled": True}
        }
    ],
    extra_headers={
        "anthropic-beta": "web-fetch-2025-09-10"
    }
)
```

In this workflow, Claude will:

1. Use web search to find relevant articles
2. Select the most promising results
3. Use web fetch to retrieve full content
4. Provide detailed analysis with citations

## Prompt caching

Web fetch works with [prompt caching](/en/docs/build-with-claude/prompt-caching). To enable prompt caching, add `cache_control` breakpoints in your request. Cached fetch results can be reused across conversation turns.

```python theme={null}
import anthropic

client = anthropic.Anthropic()

# First request with web fetch
messages = [
    {
        "role": "user",
        "content": "Analyze this research paper: https://arxiv.org/abs/2024.12345"
    }
]

response1 = client.messages.create(
    model="claude-sonnet-4-5",
    max_tokens=1024,
    messages=messages,
    tools=[{
        "type": "web_fetch_20250910",
        "name": "web_fetch"
    }],
    extra_headers={
        "anthropic-beta": "web-fetch-2025-09-10"
    }
)

# Add Claude's response to conversation
messages.append({
    "role": "assistant",
    "content": response1.content
})

# Second request with cache breakpoint
messages.append({
    "role": "user",
    "content": "What methodology does the paper use?",
    "cache_control": {"type": "ephemeral"}
})

response2 = client.messages.create(
    model="claude-sonnet-4-5",
    max_tokens=1024,
    messages=messages,
    tools=[{
        "type": "web_fetch_20250910",
        "name": "web_fetch"
    }],
    extra_headers={
        "anthropic-beta": "web-fetch-2025-09-10"
    }
)

# The second response benefits from cached fetch results
print(f"Cache read tokens: {response2.usage.get('cache_read_input_tokens', 0)}")
```

## Streaming

With streaming enabled, fetch events are part of the stream with a pause during content retrieval:

```javascript theme={null}
event: message_start
data: {"type": "message_start", "message": {"id": "msg_abc123", "type": "message"}}

event: content_block_start
data: {"type": "content_block_start", "index": 0, "content_block": {"type": "text", "text": ""}}

// Claude's decision to fetch

event: content_block_start
data: {"type": "content_block_start", "index": 1, "content_block": {"type": "server_tool_use", "id": "srvtoolu_xyz789", "name": "web_fetch"}}

// Fetch URL streamed
event: content_block_delta
data: {"type": "content_block_delta", "index": 1, "delta": {"type": "input_json_delta", "partial_json": "{\"url\":\"https://example.com/article\"}"}}

// Pause while fetch executes

// Fetch results streamed
event: content_block_start
data: {"type": "content_block_start", "index": 2, "content_block": {"type": "web_fetch_tool_result", "tool_use_id": "srvtoolu_xyz789", "content": {"type": "web_fetch_result", "url": "https://example.com/article", "content": {"type": "document", "source": {"type": "text", "media_type": "text/plain", "data": "Article content..."}}}}}

// Claude's response continues...
```

## Batch requests

You can include the web fetch tool in the [Messages Batches API](/en/docs/build-with-claude/batch-processing). Web fetch tool calls through the Messages Batches API are priced the same as those in regular Messages API requests.

## Usage and pricing

Web fetch usage has **no additional charges** beyond standard token costs:

```json theme={null}
"usage": {
  "input_tokens": 25039,
  "output_tokens": 931,
  "cache_read_input_tokens": 0,
  "cache_creation_input_tokens": 0,
  "server_tool_use": {
    "web_fetch_requests": 1
  }
}
```

The web fetch tool is available on the Claude API at **no additional cost**. You only pay standard token costs for the fetched content that becomes part of your conversation context.

To protect against inadvertently fetching large content that would consume excessive tokens, use the `max_content_tokens` parameter to set appropriate limits based on your use case and budget considerations.

Example token usage for typical content:

- Average web page (10KB): \~2,500 tokens
- Large documentation page (100KB): \~25,000 tokens
- Research paper PDF (500KB): \~125,000 tokens

---

## 8. Text editor tool

**URL:** https://docs.claude.com/en/docs/agents-and-tools/tool-use/text-editor-tool

# Text editor tool

Claude can use an Anthropic-defined text editor tool to view and modify text files, helping you debug, fix, and improve your code or other text documents. This allows Claude to directly interact with your files, providing hands-on assistance rather than just suggesting changes.

## Model compatibility

| Model                                                                      | Tool Version           |
| -------------------------------------------------------------------------- | ---------------------- |
| Claude 4.x models                                                          | `text_editor_20250728` |
| Claude Sonnet 3.7 ([deprecated](/en/docs/about-claude/model-deprecations)) | `text_editor_20250124` |

<Warning>
  The `text_editor_20250728` tool for Claude 4 models does not include the `undo_edit` command. If you require this functionality, you'll need to use Claude Sonnet 3.7 ([deprecated](/en/docs/about-claude/model-deprecations)).
</Warning>

<Warning>
  Older tool versions are not guaranteed to be backwards-compatible with newer models. Always use the tool version that corresponds to your model version.
</Warning>

## When to use the text editor tool

Some examples of when to use the text editor tool are:

- **Code debugging**: Have Claude identify and fix bugs in your code, from syntax errors to logic issues.
- **Code refactoring**: Let Claude improve your code structure, readability, and performance through targeted edits.
- **Documentation generation**: Ask Claude to add docstrings, comments, or README files to your codebase.
- **Test creation**: Have Claude create unit tests for your code based on its understanding of the implementation.

## Use the text editor tool

<Tabs>
  <Tab title="Claude 4">
    Provide the text editor tool (named `str_replace_based_edit_tool`) to Claude using the Messages API.

    You can optionally specify a `max_characters` parameter to control truncation when viewing large files.

    <Note>
      `max_characters` is only compatible with `text_editor_20250728` and later versions of the text editor tool.
    </Note>

    <CodeGroup>
      ```bash Shell theme={null}
      curl https://api.anthropic.com/v1/messages \
        -H "content-type: application/json" \
        -H "x-api-key: $ANTHROPIC_API_KEY" \
        -H "anthropic-version: 2023-06-01" \
        -d '{
          "model": "claude-sonnet-4-5",
          "max_tokens": 1024,
          "tools": [
            {
              "type": "text_editor_20250728",
              "name": "str_replace_based_edit_tool",
              "max_characters": 10000
            }
          ],
          "messages": [
            {
              "role": "user",
              "content": "There'\''s a syntax error in my primes.py file. Can you help me fix it?"
            }
          ]
        }'
      ```

      ```python Python theme={null}
      import anthropic

      client = anthropic.Anthropic()

      response = client.messages.create(
          model="claude-sonnet-4-5",
          max_tokens=1024,
          tools=[
              {
                  "type": "text_editor_20250728",
                  "name": "str_replace_based_edit_tool",
                  "max_characters": 10000
              }
          ],
          messages=[
              {
                  "role": "user",
                  "content": "There's a syntax error in my primes.py file. Can you help me fix it?"
              }
          ]
      )
      ```

      ```typescript TypeScript theme={null}
      import Anthropic from '@anthropic-ai/sdk';

      const anthropic = new Anthropic();

      const response = await anthropic.messages.create({
        model: "claude-sonnet-4-5",
        max_tokens: 1024,
        tools: [
          {
            type: "text_editor_20250728",
            name: "str_replace_based_edit_tool",
            max_characters: 10000
          }
        ],
        messages: [
          {
            role: "user",
            content: "There's a syntax error in my primes.py file. Can you help me fix it?"
          }
        ]
      });
      ```

      ```java Java theme={null}
      import com.anthropic.client.AnthropicClient;
      import com.anthropic.client.okhttp.AnthropicOkHttpClient;
      import com.anthropic.models.messages.Message;
      import com.anthropic.models.messages.MessageCreateParams;
      import com.anthropic.models.messages.Model;
      import com.anthropic.models.messages.ToolStrReplaceBasedEditTool20250728;

      public class TextEditorToolExample {

          public static void main(String[] args) {
              AnthropicClient client = AnthropicOkHttpClient.fromEnv();

              ToolStrReplaceBasedEditTool20250728 editorTool = ToolStrReplaceBasedEditTool20250728.builder()
                      .build();

              MessageCreateParams params = MessageCreateParams.builder()
                      .model(Model.CLAUDE_SONNET_4_0)
                      .maxTokens(1024)
                      .addTool(editorTool)
                      .addUserMessage("There's a syntax error in my primes.py file. Can you help me fix it?")
                      .build();

              Message message = client.messages().create(params);
          }
      }
      ```
    </CodeGroup>

  </Tab>

  <Tab title="Claude Sonnet 3.7">
    Provide the text editor tool (named `str_replace_editor`) to Claude using the Messages API:

    <CodeGroup>
      ```bash Shell theme={null}
      curl https://api.anthropic.com/v1/messages \
        -H "content-type: application/json" \
        -H "x-api-key: $ANTHROPIC_API_KEY" \
        -H "anthropic-version: 2023-06-01" \
        -d '{
          "model": "claude-3-7-sonnet-20250219",
          "max_tokens": 1024,
          "tools": [
            {
              "type": "text_editor_20250124",
              "name": "str_replace_editor"
            }
          ],
          "messages": [
            {
              "role": "user",
              "content": "There'\''s a syntax error in my primes.py file. Can you help me fix it?"
            }
          ]
        }'
      ```

      ```python Python theme={null}
      import anthropic

      client = anthropic.Anthropic()

      response = client.messages.create(
          model="claude-3-7-sonnet-20250219",
          max_tokens=1024,
          tools=[
              {
                  "type": "text_editor_20250124",
                  "name": "str_replace_editor"
              }
          ],
          messages=[
              {
                  "role": "user",
                  "content": "There's a syntax error in my primes.py file. Can you help me fix it?"
              }
          ]
      )
      ```

      ```typescript TypeScript theme={null}
      import Anthropic from '@anthropic-ai/sdk';

      const anthropic = new Anthropic();

      const response = await anthropic.messages.create({
        model: "claude-3-7-sonnet-20250219",
        max_tokens: 1024,
        tools: [
          {
            type: "text_editor_20250124",
            name: "str_replace_editor"
          }
        ],
        messages: [
          {
            role: "user",
            content: "There's a syntax error in my primes.py file. Can you help me fix it?"
          }
        ]
      });
      ```

      ```java Java theme={null}
      import com.anthropic.client.AnthropicClient;
      import com.anthropic.client.okhttp.AnthropicOkHttpClient;
      import com.anthropic.models.messages.Message;
      import com.anthropic.models.messages.MessageCreateParams;
      import com.anthropic.models.messages.Model;
      import com.anthropic.models.messages.ToolTextEditor20250124;

      public class TextEditorToolExample {

          public static void main(String[] args) {
              AnthropicClient client = AnthropicOkHttpClient.fromEnv();

              ToolTextEditor20250124 editorTool = ToolTextEditor20250124.builder()
                      .build();

              MessageCreateParams params = MessageCreateParams.builder()
                      .model(Model.CLAUDE_3_7_SONNET_LATEST)
                      .maxTokens(1024)
                      .addTool(editorTool)
                      .addUserMessage("There's a syntax error in my primes.py file. Can you help me fix it?")
                      .build();

              Message message = client.messages().create(params);
          }
      }
      ```
    </CodeGroup>

  </Tab>
</Tabs>

The text editor tool can be used in the following way:

<Steps>
  <Step title="Provide Claude with the text editor tool and a user prompt">
    * Include the text editor tool in your API request
    * Provide a user prompt that may require examining or modifying files, such as "Can you fix the syntax error in my code?"
  </Step>

  <Step title="Claude uses the tool to examine files or directories">
    * Claude assesses what it needs to look at and uses the `view` command to examine file contents or list directory contents
    * The API response will contain a `tool_use` content block with the `view` command
  </Step>

  <Step title="Execute the view command and return results">
    * Extract the file or directory path from Claude's tool use request
    * Read the file's contents or list the directory contents
    * If a `max_characters` parameter was specified in the tool configuration, truncate the file contents to that length
    * Return the results to Claude by continuing the conversation with a new `user` message containing a `tool_result` content block
  </Step>

  <Step title="Claude uses the tool to modify files">
    * After examining the file or directory, Claude may use a command such as `str_replace` to make changes or `insert` to add text at a specific line number.
    * If Claude uses the `str_replace` command, Claude constructs a properly formatted tool use request with the old text and new text to replace it with
  </Step>

  <Step title="Execute the edit and return results">
    * Extract the file path, old text, and new text from Claude's tool use request
    * Perform the text replacement in the file
    * Return the results to Claude
  </Step>

  <Step title="Claude provides its analysis and explanation">
    * After examining and possibly editing the files, Claude provides a complete explanation of what it found and what changes it made
  </Step>
</Steps>

### Text editor tool commands

The text editor tool supports several commands for viewing and modifying files:

#### view

The `view` command allows Claude to examine the contents of a file or list the contents of a directory. It can read the entire file or a specific range of lines.

Parameters:

- `command`: Must be "view"
- `path`: The path to the file or directory to view
- `view_range` (optional): An array of two integers specifying the start and end line numbers to view. Line numbers are 1-indexed, and -1 for the end line means read to the end of the file. This parameter only applies when viewing files, not directories.

<Accordion title="Example view commands">
  ```json  theme={null}
  // Example for viewing a file
  {
    "type": "tool_use",
    "id": "toolu_01A09q90qw90lq917835lq9",
    "name": "str_replace_editor",
    "input": {
      "command": "view",
      "path": "primes.py"
    }
  }

// Example for viewing a directory
{
"type": "tool_use",
"id": "toolu_02B19r91rw91mr917835mr9",
"name": "str_replace_editor",
"input": {
"command": "view",
"path": "src/"
}
}

````
</Accordion>

#### str\_replace

The `str_replace` command allows Claude to replace a specific string in a file with a new string. This is used for making precise edits.

Parameters:

* `command`: Must be "str\_replace"
* `path`: The path to the file to modify
* `old_str`: The text to replace (must match exactly, including whitespace and indentation)
* `new_str`: The new text to insert in place of the old text

<Accordion title="Example str_replace command">
```json  theme={null}
{
  "type": "tool_use",
  "id": "toolu_01A09q90qw90lq917835lq9",
  "name": "str_replace_editor",
  "input": {
    "command": "str_replace",
    "path": "primes.py",
    "old_str": "for num in range(2, limit + 1)",
    "new_str": "for num in range(2, limit + 1):"
  }
}
````

</Accordion>

#### create

The `create` command allows Claude to create a new file with specified content.

Parameters:

- `command`: Must be "create"
- `path`: The path where the new file should be created
- `file_text`: The content to write to the new file

<Accordion title="Example create command">
  ```json  theme={null}
  {
    "type": "tool_use",
    "id": "toolu_01A09q90qw90lq917835lq9",
    "name": "str_replace_editor",
    "input": {
      "command": "create",
      "path": "test_primes.py",
      "file_text": "import unittest\nimport primes\n\nclass TestPrimes(unittest.TestCase):\n    def test_is_prime(self):\n        self.assertTrue(primes.is_prime(2))\n        self.assertTrue(primes.is_prime(3))\n        self.assertFalse(primes.is_prime(4))\n\nif __name__ == '__main__':\n    unittest.main()"
    }
  }
  ```
</Accordion>

#### insert

The `insert` command allows Claude to insert text at a specific location in a file.

Parameters:

- `command`: Must be "insert"
- `path`: The path to the file to modify
- `insert_line`: The line number after which to insert the text (0 for beginning of file)
- `new_str`: The text to insert

<Accordion title="Example insert command">
  ```json  theme={null}
  {
    "type": "tool_use",
    "id": "toolu_01A09q90qw90lq917835lq9",
    "name": "str_replace_editor",
    "input": {
      "command": "insert",
      "path": "primes.py",
      "insert_line": 0,
      "new_str": "\"\"\"Module for working with prime numbers.\n\nThis module provides functions to check if a number is prime\nand to generate a list of prime numbers up to a given limit.\n\"\"\"\n"
    }
  }
  ```
</Accordion>

#### undo_edit

The `undo_edit` command allows Claude to revert the last edit made to a file.

<Note>
  This command is only available in Claude Sonnet 3.7 ([deprecated](/en/docs/about-claude/model-deprecations)). It is not supported in Claude 4 models using the `text_editor_20250728`.
</Note>

Parameters:

- `command`: Must be "undo_edit"
- `path`: The path to the file whose last edit should be undone

<Accordion title="Example undo_edit command">
  ```json  theme={null}
  {
    "type": "tool_use",
    "id": "toolu_01A09q90qw90lq917835lq9",
    "name": "str_replace_editor",
    "input": {
      "command": "undo_edit",
      "path": "primes.py"
    }
  }
  ```
</Accordion>

### Example: Fixing a syntax error with the text editor tool

<Tabs>
  <Tab title="Claude 4">
    This example demonstrates how Claude 4 models use the text editor tool to fix a syntax error in a Python file.

    First, your application provides Claude with the text editor tool and a prompt to fix a syntax error:

    <CodeGroup>
      ```bash Shell theme={null}
      curl https://api.anthropic.com/v1/messages \
        -H "content-type: application/json" \
        -H "x-api-key: $ANTHROPIC_API_KEY" \
        -H "anthropic-version: 2023-06-01" \
        -d '{
          "model": "claude-sonnet-4-5",
          "max_tokens": 1024,
          "tools": [
            {
              "type": "text_editor_20250728",
              "name": "str_replace_based_edit_tool"
            }
          ],
          "messages": [
            {
              "role": "user",
              "content": "There'\''s a syntax error in my primes.py file. Can you help me fix it?"
            }
          ]
        }'
      ```

      ```python Python theme={null}
      import anthropic

      client = anthropic.Anthropic()

      response = client.messages.create(
          model="claude-sonnet-4-5",
          max_tokens=1024,
          tools=[
              {
                  "type": "text_editor_20250728",
                  "name": "str_replace_based_edit_tool"
              }
          ],
          messages=[
              {
                  "role": "user",
                  "content": "There's a syntax error in my primes.py file. Can you help me fix it?"
              }
          ]
      )
      ```

      ```typescript TypeScript theme={null}
      import Anthropic from '@anthropic-ai/sdk';

      const anthropic = new Anthropic();

      const response = await anthropic.messages.create({
        model: "claude-sonnet-4-5",
        max_tokens: 1024,
        tools: [
          {
            type: "text_editor_20250728",
            name: "str_replace_based_edit_tool"
          }
        ],
        messages: [
          {
            role: "user",
            content: "There's a syntax error in my primes.py file. Can you help me fix it?"
          }
        ]
      });
      ```

      ```java Java theme={null}
      import com.anthropic.client.AnthropicClient;
      import com.anthropic.client.okhttp.AnthropicOkHttpClient;
      import com.anthropic.models.messages.Message;
      import com.anthropic.models.messages.MessageCreateParams;
      import com.anthropic.models.messages.Model;
      import com.anthropic.models.messages.ToolStrReplaceBasedEditTool20250728;

      public class TextEditorToolExample {

          public static void main(String[] args) {
              AnthropicClient client = AnthropicOkHttpClient.fromEnv();

              ToolStrReplaceBasedEditTool20250728 editorTool = ToolStrReplaceBasedEditTool20250728.builder()
                      .build();

              MessageCreateParams params = MessageCreateParams.builder()
                      .model(Model.CLAUDE_SONNET_4_0)
                      .maxTokens(1024)
                      .addTool(editorTool)
                      .addUserMessage("There's a syntax error in my primes.py file. Can you help me fix it?")
                      .build();

              Message message = client.messages().create(params);
          }
      }
      ```
    </CodeGroup>

    Claude will use the text editor tool first to view the file:

    ```json  theme={null}
    {
      "id": "msg_01XAbCDeFgHiJkLmNoPQrStU",
      "model": "claude-sonnet-4-5",
      "stop_reason": "tool_use",
      "role": "assistant",
      "content": [
        {
          "type": "text",
          "text": "I'll help you fix the syntax error in your primes.py file. First, let me take a look at the file to identify the issue."
        },
        {
          "type": "tool_use",
          "id": "toolu_01AbCdEfGhIjKlMnOpQrStU",
          "name": "str_replace_based_edit_tool",
          "input": {
            "command": "view",
            "path": "primes.py"
          }
        }
      ]
    }
    ```

    Your application should then read the file and return its contents to Claude:

    <CodeGroup>
      ```bash Shell theme={null}
      curl https://api.anthropic.com/v1/messages \
        -H "content-type: application/json" \
        -H "x-api-key: $ANTHROPIC_API_KEY" \
        -H "anthropic-version: 2023-06-01" \
        -d '{
          "model": "claude-sonnet-4-5",
          "max_tokens": 1024,
          "tools": [
            {
              "type": "text_editor_20250728",
              "name": "str_replace_based_edit_tool"
            }
          ],
          "messages": [
            {
              "role": "user",
              "content": "There'\''s a syntax error in my primes.py file. Can you help me fix it?"
            },
            {
                  "role": "assistant",
                  "content": [
                      {
                          "type": "text",
                          "text": "I'\''ll help you fix the syntax error in your primes.py file. First, let me take a look at the file to identify the issue."
                      },
                      {
                          "type": "tool_use",
                          "id": "toolu_01AbCdEfGhIjKlMnOpQrStU",
                          "name": "str_replace_based_edit_tool",
                          "input": {
                              "command": "view",
                              "path": "primes.py"
                          }
                      }
                  ]
              },
              {
                  "role": "user",
                  "content": [
                      {
                          "type": "tool_result",
                          "tool_use_id": "toolu_01AbCdEfGhIjKlMnOpQrStU",
                          "content": "1: def is_prime(n):\n2:     \"\"\"Check if a number is prime.\"\"\"\n3:     if n <= 1:\n4:         return False\n5:     if n <= 3:\n6:         return True\n7:     if n % 2 == 0 or n % 3 == 0:\n8:         return False\n9:     i = 5\n10:     while i * i <= n:\n11:         if n % i == 0 or n % (i + 2) == 0:\n12:             return False\n13:         i += 6\n14:     return True\n15: \n16: def get_primes(limit):\n17:     \"\"\"Generate a list of prime numbers up to the given limit.\"\"\"\n18:     primes = []\n19:     for num in range(2, limit + 1)\n20:         if is_prime(num):\n21:             primes.append(num)\n22:     return primes\n23: \n24: def main():\n25:     \"\"\"Main function to demonstrate prime number generation.\"\"\"\n26:     limit = 100\n27:     prime_list = get_primes(limit)\n28:     print(f\"Prime numbers up to {limit}:\")\n29:     print(prime_list)\n30:     print(f\"Found {len(prime_list)} prime numbers.\")\n31: \n32: if __name__ == \"__main__\":\n33:     main()"
                      }
                  ]
              }
          ]
        }'
      ```

      ```python Python theme={null}
      response = client.messages.create(
          model="claude-sonnet-4-5",
          max_tokens=1024,
          tools=[
              {
                  "type": "text_editor_20250728",
                  "name": "str_replace_based_edit_tool"
              }
          ],
          messages=[
              {
                  "role": "user",
                  "content": "There's a syntax error in my primes.py file. Can you help me fix it?"
              },
              {
                  "role": "assistant",
                  "content": [
                      {
                          "type": "text",
                          "text": "I'll help you fix the syntax error in your primes.py file. First, let me take a look at the file to identify the issue."
                      },
                      {
                          "type": "tool_use",
                          "id": "toolu_01AbCdEfGhIjKlMnOpQrStU",
                          "name": "str_replace_based_edit_tool",
                          "input": {
                              "command": "view",
                              "path": "primes.py"
                          }
                      }
                  ]
              },
              {
                  "role": "user",
                  "content": [
                      {
                          "type": "tool_result",
                          "tool_use_id": "toolu_01AbCdEfGhIjKlMnOpQrStU",
                          "content": "1: def is_prime(n):\n2:     \"\"\"Check if a number is prime.\"\"\"\n3:     if n <= 1:\n4:         return False\n5:     if n <= 3:\n6:         return True\n7:     if n % 2 == 0 or n % 3 == 0:\n8:         return False\n9:     i = 5\n10:     while i * i <= n:\n11:         if n % i == 0 or n % (i + 2) == 0:\n12:             return False\n13:         i += 6\n14:     return True\n15: \n16: def get_primes(limit):\n17:     \"\"\"Generate a list of prime numbers up to the given limit.\"\"\"\n18:     primes = []\n19:     for num in range(2, limit + 1)\n20:         if is_prime(num):\n21:             primes.append(num)\n22:     return primes\n23: \n24: def main():\n25:     \"\"\"Main function to demonstrate prime number generation.\"\"\"\n26:     limit = 100\n27:     prime_list = get_primes(limit)\n28:     print(f\"Prime numbers up to {limit}:\")\n29:     print(prime_list)\n30:     print(f\"Found {len(prime_list)} prime numbers.\")\n31: \n32: if __name__ == \"__main__\":\n33:     main()"
                      }
                  ]
              }
          ]
      )
      ```

      ```typescript TypeScript theme={null}
      import Anthropic from '@anthropic-ai/sdk';

      const anthropic = new Anthropic();

      const response = await anthropic.messages.create({
        model: "claude-sonnet-4-5",
        max_tokens: 1024,
        tools: [
          {
            type: "text_editor_20250728",
            name: "str_replace_based_edit_tool"
          }
        ],
        messages: [
          {
            role: "user",
            content: "There's a syntax error in my primes.py file. Can you help me fix it?"
          },
          {
            role: "assistant",
            content: [
                {
                    type: "text",
                    text: "I'll help you fix the syntax error in your primes.py file. First, let me take a look at the file to identify the issue."
                },
                {
                    type: "tool_use",
                    id: "toolu_01AbCdEfGhIjKlMnOpQrStU",
                    name: "str_replace_based_edit_tool",
                    input: {
                        command: "view",
                        path: "primes.py"
                    }
                }
            ]
          },
          {
            role: "user",
            content: [
                {
                    type: "tool_result",
                    tool_use_id: "toolu_01AbCdEfGhIjKlMnOpQrStU",
                    content: "1: def is_prime(n):\n2:     \"\"\"Check if a number is prime.\"\"\"\n3:     if n <= 1:\n4:         return False\n5:     if n <= 3:\n6:         return True\n7:     if n % 2 == 0 or n % 3 == 0:\n8:         return False\n9:     i = 5\n10:     while i * i <= n:\n11:         if n % i == 0 or n % (i + 2) == 0:\n12:             return False\n13:         i += 6\n14:     return True\n15: \n16: def get_primes(limit):\n17:     \"\"\"Generate a list of prime numbers up to the given limit.\"\"\"\n18:     primes = []\n19:     for num in range(2, limit + 1)\n20:         if is_prime(num):\n21:             primes.append(num)\n22:     return primes\n23: \n24: def main():\n25:     \"\"\"Main function to demonstrate prime number generation.\"\"\"\n26:     limit = 100\n27:     prime_list = get_primes(limit)\n28:     print(f\"Prime numbers up to {limit}:\")\n29:     print(prime_list)\n30:     print(f\"Found {len(prime_list)} prime numbers.\")\n31: \n32: if __name__ == \"__main__\":\n33:     main()"
                }
            ]
          }
        ]
      });
      ```

      ```java Java theme={null}
      import com.anthropic.client.AnthropicClient;
      import com.anthropic.client.okhttp.AnthropicOkHttpClient;
      import com.anthropic.models.messages.Message;
      import com.anthropic.models.messages.MessageCreateParams;
      import com.anthropic.models.messages.Model;
      import com.anthropic.models.messages.ToolStrReplaceBasedEditTool20250728;

      public class TextEditorToolExample {

          public static void main(String[] args) {
              AnthropicClient client = AnthropicOkHttpClient.fromEnv();

              ToolStrReplaceBasedEditTool20250728 editorTool = ToolStrReplaceBasedEditTool20250728.builder()
                  .build();

              MessageCreateParams params = MessageCreateParams.builder()
                  .model(Model.CLAUDE_SONNET_4_0)
                  .maxTokens(1024)
                  .addTool(editorTool)
                  .addUserMessage("There's a syntax error in my primes.py file. Can you help me fix it?")
                  .build();

              Message message = client.messages().create(params);
              System.out.println(message);
          }
      }
      ```
    </CodeGroup>

    <Tip>
      **Line numbers**

      In the example above, the `view` tool result includes file contents with line numbers prepended to each line (e.g., "1: def is\_prime(n):"). Line numbers are not required, but they are essential for successfully using the `view_range` parameter to examine specific sections of files and the `insert_line` parameter to add content at precise locations.
    </Tip>

    Claude will identify the syntax error and use the `str_replace` command to fix it:

    ```json  theme={null}
    {
      "id": "msg_01VwXyZAbCdEfGhIjKlMnO",
      "model": "claude-sonnet-4-5",
      "stop_reason": "tool_use",
      "role": "assistant",
      "content": [
        {
          "type": "text",
          "text": "I found the syntax error in your primes.py file. In the `get_primes` function, there is a missing colon (:) at the end of the for loop line. Let me fix that for you."
        },
        {
          "type": "tool_use",
          "id": "toolu_01PqRsTuVwXyZAbCdEfGh",
          "name": "str_replace_based_edit_tool",
          "input": {
            "command": "str_replace",
            "path": "primes.py",
            "old_str": "    for num in range(2, limit + 1)",
            "new_str": "    for num in range(2, limit + 1):"
          }
        }
      ]
    }
    ```

    Your application should then make the edit and return the result:

    <CodeGroup>
      ```python Python theme={null}
      response = client.messages.create(
          model="claude-sonnet-4-5",
          max_tokens=1024,
          tools=[
              {
                  "type": "text_editor_20250728",
                  "name": "str_replace_based_edit_tool"
              }
          ],
          messages=[
              # Previous messages...
              {
                  "role": "assistant",
                  "content": [
                      {
                          "type": "text",
                          "text": "I found the syntax error in your primes.py file. In the `get_primes` function, there is a missing colon (:) at the end of the for loop line. Let me fix that for you."
                      },
                      {
                          "type": "tool_use",
                          "id": "toolu_01PqRsTuVwXyZAbCdEfGh",
                          "name": "str_replace_based_edit_tool",
                          "input": {
                              "command": "str_replace",
                              "path": "primes.py",
                              "old_str": "    for num in range(2, limit + 1)",
                              "new_str": "    for num in range(2, limit + 1):"
                          }
                      }
                  ]
              },
              {
                  "role": "user",
                  "content": [
                      {
                          "type": "tool_result",
                          "tool_use_id": "toolu_01PqRsTuVwXyZAbCdEfGh",
                          "content": "Successfully replaced text at exactly one location."
                      }
                  ]
              }
          ]
      )
      ```

      ```typescript TypeScript theme={null}
      const response = await anthropic.messages.create({
        model: "claude-sonnet-4-5",
        max_tokens: 1024,
        tools: [
          {
            type: "text_editor_20250728",
            name: "str_replace_based_edit_tool"
          }
        ],
        messages: [
          // Previous messages...
          {
            role: "assistant",
            content: [
              {
                type: "text",
                text: "I found the syntax error in your primes.py file. In the `get_primes` function, there is a missing colon (:) at the end of the for loop line. Let me fix that for you."
              },
              {
                type: "tool_use",
                id: "toolu_01PqRsTuVwXyZAbCdEfGh",
                name: "str_replace_based_edit_tool",
                input: {
                  command: "str_replace",
                  path: "primes.py",
                  old_str: "    for num in range(2, limit + 1)",
                  new_str: "    for num in range(2, limit + 1):"
                }
              }
            ]
          },
          {
            role: "user",
            content: [
              {
                type: "tool_result",
                tool_use_id: "toolu_01PqRsTuVwXyZAbCdEfGh",
                content: "Successfully replaced text at exactly one location."
              }
            ]
          }
        ]
      });
      ```

      ```java Java theme={null}
      import java.util.List;
      import java.util.Map;

      import com.anthropic.client.AnthropicClient;
      import com.anthropic.client.okhttp.AnthropicOkHttpClient;
      import com.anthropic.core.JsonValue;
      import com.anthropic.models.messages.ContentBlockParam;
      import com.anthropic.models.messages.Message;
      import com.anthropic.models.messages.MessageCreateParams;
      import com.anthropic.models.messages.MessageParam;
      import com.anthropic.models.messages.Model;
      import com.anthropic.models.messages.TextBlockParam;
      import com.anthropic.models.messages.ToolResultBlockParam;
      import com.anthropic.models.messages.ToolStrReplaceBasedEditTool20250728;
      import com.anthropic.models.messages.ToolUseBlockParam;

      public class TextEditorConversationExample {

          public static void main(String[] args) {
              AnthropicClient client = AnthropicOkHttpClient.fromEnv();

              MessageCreateParams params = MessageCreateParams.builder()
                      .model(Model.CLAUDE_SONNET_4_0)
                      .maxTokens(1024)
                      .addTool(ToolStrReplaceBasedEditTool20250728.builder()
                              .build())
                      // Previous messages would go here
                      .addAssistantMessageOfBlockParams(
                              List.of(
                                      ContentBlockParam.ofText(TextBlockParam.builder()
                                              .text("I found the syntax error in your primes.py file. In the `get_primes` function, there is a missing colon (:) at the end of the for loop line. Let me fix that for you.")
                                              .build()),
                                      ContentBlockParam.ofToolUse(ToolUseBlockParam.builder()
                                              .id("toolu_01PqRsTuVwXyZAbCdEfGh")
                                              .name("str_replace_based_edit_tool")
                                              .input(JsonValue.from(Map.of(
                                                      "command", "str_replace",
                                                      "path", "primes.py",
                                                      "old_str", "    for num in range(2, limit + 1)",
                                                      "new_str", "    for num in range(2, limit + 1):"
                                              )))
                                              .build()
                                      )
                              )
                      )
                      .addUserMessageOfBlockParams(List.of(
                              ContentBlockParam.ofToolResult(ToolResultBlockParam.builder()
                                      .toolUseId("toolu_01PqRsTuVwXyZAbCdEfGh")
                                      .content("Successfully replaced text at exactly one location.")
                                      .build()
                              )
                      ))
                      .build();

              Message message = client.messages().create(params);
              System.out.println(message);
          }
      }
      ```
    </CodeGroup>

    Finally, Claude will provide a complete explanation of the fix:

    ````json  theme={null}
    {
      "id": "msg_01IjKlMnOpQrStUvWxYzAb",
      "model": "claude-sonnet-4-5",
      "stop_reason": "end_turn",
      "role": "assistant",
      "content": [
        {
          "type": "text",
          "text": "I've fixed the syntax error in your primes.py file. The issue was in the `get_primes` function at line 19. There was a missing colon (:) at the end of the for loop line.\n\nHere's what I changed:\n\nFrom:\n```python\nfor num in range(2, limit + 1)\n```\n\nTo:\n```python\nfor num in range(2, limit + 1):\n```\n\nIn Python, a colon is required at the end of statements that start a new code block, such as for loops, if statements, function definitions, etc. Without the colon, Python raises a syntax error because it doesn't know where the code block begins.\n\nYour code should now run correctly. The function will properly generate a list of prime numbers up to the specified limit."
        }
      ]
    }
    ````

  </Tab>

  <Tab title="Claude Sonnet 3.7">
    This example demonstrates how Claude Sonnet 3.7 uses the text editor tool to fix a syntax error in a Python file.

    First, your application provides Claude with the text editor tool and a prompt to fix a syntax error:

    ```python  theme={null}
    import anthropic

    client = anthropic.Anthropic()

    response = client.messages.create(
        model="claude-3-7-sonnet-20250219",
        max_tokens=1024,
        tools=[
            {
                "type": "text_editor_20250124",
                "name": "str_replace_editor"
            }
        ],
        messages=[
            {
                "role": "user",
                "content": "There's a syntax error in my primes.py file. Can you help me fix it?"
            }
        ]
    )

    print(response)
    ```

    <Note>
      The Claude Sonnet 3.7 examples follow the same format as the Claude 4 examples above, using the same tool calls and responses but with the `text_editor_20250124` tool type and `str_replace_editor` name.
    </Note>

  </Tab>
</Tabs>

---

## Implement the text editor tool

The text editor tool is implemented as a schema-less tool. When using this tool, you don't need to provide an input schema as with other tools; the schema is built into Claude's model and can't be modified.

The tool type depends on the model version:

- **Claude 4**: `type: "text_editor_20250728"`
- **Claude Sonnet 3.7**: `type: "text_editor_20250124"`

<Steps>
  <Step title="Initialize your editor implementation">
    Create helper functions to handle file operations like reading, writing, and modifying files. Consider implementing backup functionality to recover from mistakes.
  </Step>

  <Step title="Handle editor tool calls">
    Create a function that processes tool calls from Claude based on the command type:

    ```python  theme={null}
    def handle_editor_tool(tool_call, model_version):
        input_params = tool_call.input
        command = input_params.get('command', '')
        file_path = input_params.get('path', '')

        if command == 'view':
            # Read and return file contents
            pass
        elif command == 'str_replace':
            # Replace text in file
            pass
        elif command == 'create':
            # Create new file
            pass
        elif command == 'insert':
            # Insert text at location
            pass
        elif command == 'undo_edit':
            # Check if it's a Claude 4 model
            if 'str_replace_based_edit_tool' in model_version:
                return {"error": "undo_edit command is not supported in Claude 4"}
            # Restore from backup for Claude 3.7
            pass
    ```

  </Step>

  <Step title="Implement security measures">
    Add validation and security checks:

    * Validate file paths to prevent directory traversal
    * Create backups before making changes
    * Handle errors gracefully
    * Implement permissions checks

  </Step>

  <Step title="Process Claude's responses">
    Extract and handle tool calls from Claude's responses:

    ```python  theme={null}
    # Process tool use in Claude's response
    for content in response.content:
        if content.type == "tool_use":
            # Execute the tool based on command
            result = handle_editor_tool(content)

            # Return result to Claude
            tool_result = {
                "type": "tool_result",
                "tool_use_id": content.id,
                "content": result
            }
    ```

  </Step>
</Steps>

<Warning>
  When implementing the text editor tool, keep in mind:

1. **Security**: The tool has access to your local filesystem, so implement proper security measures.
2. **Backup**: Always create backups before allowing edits to important files.
3. **Validation**: Validate all inputs to prevent unintended changes.
4. **Unique matching**: Make sure replacements match exactly one location to avoid unintended edits.
   </Warning>

### Handle errors

When using the text editor tool, various errors may occur. Here is guidance on how to handle them:

<AccordionGroup>
  <Accordion title="File not found">
    If Claude tries to view or modify a file that doesn't exist, return an appropriate error message in the `tool_result`:

    ```json  theme={null}
    {
      "role": "user",
      "content": [
        {
          "type": "tool_result",
          "tool_use_id": "toolu_01A09q90qw90lq917835lq9",
          "content": "Error: File not found",
          "is_error": true
        }
      ]
    }
    ```

  </Accordion>

  <Accordion title="Multiple matches for replacement">
    If Claude's `str_replace` command matches multiple locations in the file, return an appropriate error message:

    ```json  theme={null}
    {
      "role": "user",
      "content": [
        {
          "type": "tool_result",
          "tool_use_id": "toolu_01A09q90qw90lq917835lq9",
          "content": "Error: Found 3 matches for replacement text. Please provide more context to make a unique match.",
          "is_error": true
        }
      ]
    }
    ```

  </Accordion>

  <Accordion title="No matches for replacement">
    If Claude's `str_replace` command doesn't match any text in the file, return an appropriate error message:

    ```json  theme={null}
    {
      "role": "user",
      "content": [
        {
          "type": "tool_result",
          "tool_use_id": "toolu_01A09q90qw90lq917835lq9",
          "content": "Error: No match found for replacement. Please check your text and try again.",
          "is_error": true
        }
      ]
    }
    ```

  </Accordion>

  <Accordion title="Permission errors">
    If there are permission issues with creating, reading, or modifying files, return an appropriate error message:

    ```json  theme={null}
    {
      "role": "user",
      "content": [
        {
          "type": "tool_result",
          "tool_use_id": "toolu_01A09q90qw90lq917835lq9",
          "content": "Error: Permission denied. Cannot write to file.",
          "is_error": true
        }
      ]
    }
    ```

  </Accordion>
</AccordionGroup>

### Follow implementation best practices

<AccordionGroup>
  <Accordion title="Provide clear context">
    When asking Claude to fix or modify code, be specific about what files need to be examined or what issues need to be addressed. Clear context helps Claude identify the right files and make appropriate changes.

    **Less helpful prompt**: "Can you fix my code?"

    **Better prompt**: "There's a syntax error in my primes.py file that prevents it from running. Can you fix it?"

  </Accordion>

  <Accordion title="Be explicit about file paths">
    Specify file paths clearly when needed, especially if you're working with multiple files or files in different directories.

    **Less helpful prompt**: "Review my helper file"

    **Better prompt**: "Can you check my utils/helpers.py file for any performance issues?"

  </Accordion>

  <Accordion title="Create backups before editing">
    Implement a backup system in your application that creates copies of files before allowing Claude to edit them, especially for important or production code.

    ```python  theme={null}
    def backup_file(file_path):
        """Create a backup of a file before editing."""
        backup_path = f"{file_path}.backup"
        if os.path.exists(file_path):
            with open(file_path, 'r') as src, open(backup_path, 'w') as dst:
                dst.write(src.read())
    ```

  </Accordion>

  <Accordion title="Handle unique text replacement carefully">
    The `str_replace` command requires an exact match for the text to be replaced. Your application should ensure that there is exactly one match for the old text or provide appropriate error messages.

    ```python  theme={null}
    def safe_replace(file_path, old_text, new_text):
        """Replace text only if there's exactly one match."""
        with open(file_path, 'r') as f:
            content = f.read()

        count = content.count(old_text)
        if count == 0:
            return "Error: No match found"
        elif count > 1:
            return f"Error: Found {count} matches"
        else:
            new_content = content.replace(old_text, new_text)
            with open(file_path, 'w') as f:
                f.write(new_content)
            return "Successfully replaced text"
    ```

  </Accordion>

  <Accordion title="Verify changes">
    After Claude makes changes to a file, verify the changes by running tests or checking that the code still works as expected.

    ```python  theme={null}
    def verify_changes(file_path):
        """Run tests or checks after making changes."""
        try:
            # For Python files, check for syntax errors
            if file_path.endswith('.py'):
                import ast
                with open(file_path, 'r') as f:
                    ast.parse(f.read())
                return "Syntax check passed"
        except Exception as e:
            return f"Verification failed: {str(e)}"
    ```

  </Accordion>
</AccordionGroup>

---

## Pricing and token usage

The text editor tool uses the same pricing structure as other tools used with Claude. It follows the standard input and output token pricing based on the Claude model you're using.

In addition to the base tokens, the following additional input tokens are needed for the text editor tool:

| Tool                                                                                                | Additional input tokens |
| --------------------------------------------------------------------------------------------------- | ----------------------- |
| `text_editor_20250429` (Claude 4.x)                                                                 | 700 tokens              |
| `text_editor_20250124` (Claude Sonnet 3.7 ([deprecated](/en/docs/about-claude/model-deprecations))) | 700 tokens              |

For more detailed information about tool pricing, see [Tool use pricing](/en/docs/agents-and-tools/tool-use/overview#pricing).

## Integrate the text editor tool with other tools

The text editor tool can be used alongside other Claude tools. When combining tools, ensure you:

- Match the tool version with the model you're using
- Account for the additional token usage for all tools included in your request

## Change log

| Date             | Version                | Changes                                                                                                                                                                                                                                                               |
| ---------------- | ---------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| July 28, 2025    | `text_editor_20250728` | Release of an updated text editor Tool that fixes some issues and adds an optional `max_characters` parameter. It is otherwise identical to `text_editor_20250429`.                                                                                                   |
| April 29, 2025   | `text_editor_20250429` | Release of the text editor Tool for Claude 4. This version removes the `undo_edit` command but maintains all other capabilities. The tool name has been updated to reflect its str_replace-based architecture.                                                        |
| March 13, 2025   | `text_editor_20250124` | Introduction of standalone text editor Tool documentation. This version is optimized for Claude Sonnet 3.7 but has identical capabilities to the previous version.                                                                                                    |
| October 22, 2024 | `text_editor_20241022` | Initial release of the text editor Tool with Claude Sonnet 3.5 ([retired](/en/docs/about-claude/model-deprecations)). Provides capabilities for viewing, creating, and editing files through the `view`, `create`, `str_replace`, `insert`, and `undo_edit` commands. |

## Next steps

Here are some ideas for how to use the text editor tool in more convenient and powerful ways:

- **Integrate with your development workflow**: Build the text editor tool into your development tools or IDE
- **Create a code review system**: Have Claude review your code and make improvements
- **Build a debugging assistant**: Create a system where Claude can help you diagnose and fix issues in your code
- **Implement file format conversion**: Let Claude help you convert files from one format to another
- **Automate documentation**: Set up workflows for Claude to automatically document your code

As you build applications with the text editor tool, we're excited to see how you leverage Claude's capabilities to enhance your development workflow and productivity.

<CardGroup cols={3}>
  <Card title="Tool use overview" icon="screwdriver-wrench" href="/en/docs/agents-and-tools/tool-use/overview">
    Learn how to implement tool workflows for use with Claude.
  </Card>

{" "}

  <Card title="Token-efficient tool use" icon="bolt-lightning" href="/en/docs/agents-and-tools/tool-use/token-efficient-tool-use">
    Reduce latency and costs when using tools with Claude Sonnet 3.7.
  </Card>

  <Card title="Bash tool" icon="terminal" href="/en/docs/agents-and-tools/tool-use/bash-tool">
    Execute shell commands with Claude.
  </Card>
</CardGroup>

---

## 9. Computer use tool

**URL:** https://docs.claude.com/en/docs/agents-and-tools/tool-use/computer-use-tool

# Computer use tool

Claude can interact with computer environments through the computer use tool, which provides screenshot capabilities and mouse/keyboard control for autonomous desktop interaction.

<Note>
  Computer use is currently in beta and requires a [beta header](/en/api/beta-headers):

- `"computer-use-2025-01-24"` (Claude 4 models and Claude Sonnet 3.7 ([deprecated](/en/docs/about-claude/model-deprecations)))
  </Note>

## Overview

Computer use is a beta feature that enables Claude to interact with desktop environments. This tool provides:

- **Screenshot capture**: See what's currently displayed on screen
- **Mouse control**: Click, drag, and move the cursor
- **Keyboard input**: Type text and use keyboard shortcuts
- **Desktop automation**: Interact with any application or interface

While computer use can be augmented with other tools like bash and text editor for more comprehensive automation workflows, computer use specifically refers to the computer use tool's capability to see and control desktop environments.

## Model compatibility

Computer use is available for the following Claude models:

| Model                                                                      | Tool Version        | Beta Flag                 |
| -------------------------------------------------------------------------- | ------------------- | ------------------------- |
| Claude 4 models                                                            | `computer_20250124` | `computer-use-2025-01-24` |
| Claude Sonnet 3.7 ([deprecated](/en/docs/about-claude/model-deprecations)) | `computer_20250124` | `computer-use-2025-01-24` |

<Note>
  Claude 4 models use updated tool versions optimized for the new architecture. Claude Sonnet 3.7 ([deprecated](/en/docs/about-claude/model-deprecations)) introduces additional capabilities including the thinking feature for more insight into the model's reasoning process.
</Note>

<Warning>
  Older tool versions are not guaranteed to be backwards-compatible with newer models. Always use the tool version that corresponds to your model version.
</Warning>

## Security considerations

<Warning>
  Computer use is a beta feature with unique risks distinct from standard API features. These risks are heightened when interacting with the internet. To minimize risks, consider taking precautions such as:

1. Use a dedicated virtual machine or container with minimal privileges to prevent direct system attacks or accidents.
2. Avoid giving the model access to sensitive data, such as account login information, to prevent information theft.
3. Limit internet access to an allowlist of domains to reduce exposure to malicious content.
4. Ask a human to confirm decisions that may result in meaningful real-world consequences as well as any tasks requiring affirmative consent, such as accepting cookies, executing financial transactions, or agreeing to terms of service.

In some circumstances, Claude will follow commands found in content even if it conflicts with the user's instructions. For example, Claude instructions on webpages or contained in images may override instructions or cause Claude to make mistakes. We suggest taking precautions to isolate Claude from sensitive data and actions to avoid risks related to prompt injection.

We've trained the model to resist these prompt injections and have added an extra layer of defense. If you use our computer use tools, we'll automatically run classifiers on your prompts to flag potential instances of prompt injections. When these classifiers identify potential prompt injections in screenshots, they will automatically steer the model to ask for user confirmation before proceeding with the next action. We recognize that this extra protection won't be ideal for every use case (for example, use cases without a human in the loop), so if you'd like to opt out and turn it off, please [contact us](https://support.claude.com/en/).

We still suggest taking precautions to isolate Claude from sensitive data and actions to avoid risks related to prompt injection.

Finally, please inform end users of relevant risks and obtain their consent prior to enabling computer use in your own products.
</Warning>

<Card title="Computer use reference implementation" icon="computer" href="https://github.com/anthropics/anthropic-quickstarts/tree/main/computer-use-demo">
  Get started quickly with our computer use reference implementation that includes a web interface, Docker container, example tool implementations, and an agent loop.

**Note:** The implementation has been updated to include new tools for both Claude 4 models and Claude Sonnet 3.7. Be sure to pull the latest version of the repo to access these new features.
</Card>

<Tip>
  Please use [this form](https://forms.gle/BT1hpBrqDPDUrCqo7) to provide
  feedback on the quality of the model responses, the API itself, or the quality
  of the documentation - we cannot wait to hear from you!
</Tip>

## Quick start

Here's how to get started with computer use:

<CodeGroup>
  ```python Python theme={null}
  import anthropic

client = anthropic.Anthropic()

response = client.beta.messages.create(
model="claude-sonnet-4-5", # or another compatible model
max_tokens=1024,
tools=[
{
"type": "computer_20250124",
"name": "computer",
"display_width_px": 1024,
"display_height_px": 768,
"display_number": 1,
},
{
"type": "text_editor_20250124",
"name": "str_replace_editor"
},
{
"type": "bash_20250124",
"name": "bash"
}
],
messages=[{"role": "user", "content": "Save a picture of a cat to my desktop."}],
betas=["computer-use-2025-01-24"]
)
print(response)

````

```bash Shell theme={null}
curl https://api.anthropic.com/v1/messages \
  -H "content-type: application/json" \
  -H "x-api-key: $ANTHROPIC_API_KEY" \
  -H "anthropic-version: 2023-06-01" \
  -H "anthropic-beta: computer-use-2025-01-24" \
  -d '{
    "model": "claude-sonnet-4-5",
    "max_tokens": 1024,
    "tools": [
      {
        "type": "computer_20250124",
        "name": "computer",
        "display_width_px": 1024,
        "display_height_px": 768,
        "display_number": 1
      },
      {
        "type": "text_editor_20250124",
        "name": "str_replace_editor"
      },
      {
        "type": "bash_20250124",
        "name": "bash"
      }
    ],
    "messages": [
      {
        "role": "user",
        "content": "Save a picture of a cat to my desktop."
      }
    ]
  }'
````

</CodeGroup>

<Note>
  A beta header is only required for the computer use tool.

The example above shows all three tools being used together, which requires the beta header because it includes the computer use tool.
</Note>

---

## How computer use works

<Steps>
  <Step title="1. Provide Claude with the computer use tool and a user prompt" icon="toolbox">
    * Add the computer use tool (and optionally other tools) to your API request.
    * Include a user prompt that requires desktop interaction, e.g., "Save a picture of a cat to my desktop."
  </Step>

  <Step title="2. Claude decides to use the computer use tool" icon="screwdriver-wrench">
    * Claude assesses if the computer use tool can help with the user's query.
    * If yes, Claude constructs a properly formatted tool use request.
    * The API response has a `stop_reason` of `tool_use`, signaling Claude's intent.
  </Step>

  <Step title="3. Extract tool input, evaluate the tool on a computer, and return results" icon="computer">
    * On your end, extract the tool name and input from Claude's request.
    * Use the tool on a container or Virtual Machine.
    * Continue the conversation with a new `user` message containing a `tool_result` content block.
  </Step>

  <Step title="4. Claude continues calling computer use tools until it's completed the task" icon="arrows-spin">
    * Claude analyzes the tool results to determine if more tool use is needed or the task has been completed.
    * If Claude decides it needs another tool, it responds with another `tool_use` `stop_reason` and you should return to step 3.
    * Otherwise, it crafts a text response to the user.
  </Step>
</Steps>

We refer to the repetition of steps 3 and 4 without user input as the "agent loop" - i.e., Claude responding with a tool use request and your application responding to Claude with the results of evaluating that request.

### The computing environment

Computer use requires a sandboxed computing environment where Claude can safely interact with applications and the web. This environment includes:

1. **Virtual display**: A virtual X11 display server (using Xvfb) that renders the desktop interface Claude will see through screenshots and control with mouse/keyboard actions.

2. **Desktop environment**: A lightweight UI with window manager (Mutter) and panel (Tint2) running on Linux, which provides a consistent graphical interface for Claude to interact with.

3. **Applications**: Pre-installed Linux applications like Firefox, LibreOffice, text editors, and file managers that Claude can use to complete tasks.

4. **Tool implementations**: Integration code that translates Claude's abstract tool requests (like "move mouse" or "take screenshot") into actual operations in the virtual environment.

5. **Agent loop**: A program that handles communication between Claude and the environment, sending Claude's actions to the environment and returning the results (screenshots, command outputs) back to Claude.

When you use computer use, Claude doesn't directly connect to this environment. Instead, your application:

1. Receives Claude's tool use requests
2. Translates them into actions in your computing environment
3. Captures the results (screenshots, command outputs, etc.)
4. Returns these results to Claude

For security and isolation, the reference implementation runs all of this inside a Docker container with appropriate port mappings for viewing and interacting with the environment.

---

## How to implement computer use

### Start with our reference implementation

We have built a [reference implementation](https://github.com/anthropics/anthropic-quickstarts/tree/main/computer-use-demo) that includes everything you need to get started quickly with computer use:

- A [containerized environment](https://github.com/anthropics/anthropic-quickstarts/blob/main/computer-use-demo/Dockerfile) suitable for computer use with Claude
- Implementations of [the computer use tools](https://github.com/anthropics/anthropic-quickstarts/tree/main/computer-use-demo/computer_use_demo/tools)
- An [agent loop](https://github.com/anthropics/anthropic-quickstarts/blob/main/computer-use-demo/computer_use_demo/loop.py) that interacts with the Claude API and executes the computer use tools
- A web interface to interact with the container, agent loop, and tools.

### Understanding the multi-agent loop

The core of computer use is the "agent loop" - a cycle where Claude requests tool actions, your application executes them, and returns results to Claude. Here's a simplified example:

```python theme={null}
async def sampling_loop(
    *,
    model: str,
    messages: list[dict],
    api_key: str,
    max_tokens: int = 4096,
    tool_version: str,
    thinking_budget: int | None = None,
    max_iterations: int = 10,  # Add iteration limit to prevent infinite loops
):
    """
    A simple agent loop for Claude computer use interactions.

    This function handles the back-and-forth between:
    1. Sending user messages to Claude
    2. Claude requesting to use tools
    3. Your app executing those tools
    4. Sending tool results back to Claude
    """
    # Set up tools and API parameters
    client = Anthropic(api_key=api_key)
    beta_flag = "computer-use-2025-01-24" if "20250124" in tool_version else "computer-use-2024-10-22"

    # Configure tools - you should already have these initialized elsewhere
    tools = [
        {"type": f"computer_{tool_version}", "name": "computer", "display_width_px": 1024, "display_height_px": 768},
        {"type": f"text_editor_{tool_version}", "name": "str_replace_editor"},
        {"type": f"bash_{tool_version}", "name": "bash"}
    ]

    # Main agent loop (with iteration limit to prevent runaway API costs)
    iterations = 0
    while True and iterations < max_iterations:
        iterations += 1
        # Set up optional thinking parameter (for Claude Sonnet 3.7)
        thinking = None
        if thinking_budget:
            thinking = {"type": "enabled", "budget_tokens": thinking_budget}

        # Call the Claude API
        response = client.beta.messages.create(
            model=model,
            max_tokens=max_tokens,
            messages=messages,
            tools=tools,
            betas=[beta_flag],
            thinking=thinking
        )

        # Add Claude's response to the conversation history
        response_content = response.content
        messages.append({"role": "assistant", "content": response_content})

        # Check if Claude used any tools
        tool_results = []
        for block in response_content:
            if block.type == "tool_use":
                # In a real app, you would execute the tool here
                # For example: result = run_tool(block.name, block.input)
                result = {"result": "Tool executed successfully"}

                # Format the result for Claude
                tool_results.append({
                    "type": "tool_result",
                    "tool_use_id": block.id,
                    "content": result
                })

        # If no tools were used, Claude is done - return the final messages
        if not tool_results:
            return messages

        # Add tool results to messages for the next iteration with Claude
        messages.append({"role": "user", "content": tool_results})
```

The loop continues until either Claude responds without requesting any tools (task completion) or the maximum iteration limit is reached. This safeguard prevents potential infinite loops that could result in unexpected API costs.

<Warning>
  When using the computer use tool, you must include the appropriate beta flag for your model version:

  <AccordionGroup>
    <Accordion title="Claude 4 models">
      When using `computer_20250124`, include this beta flag:

      ```
      "betas": ["computer-use-2025-01-24"]
      ```
    </Accordion>

    <Accordion title="Claude Sonnet 3.7">
      When using `computer_20250124`, include this beta flag:

      ```
      "betas": ["computer-use-2025-01-24"]
      ```
    </Accordion>

  </AccordionGroup>
</Warning>

We recommend trying the reference implementation out before reading the rest of this documentation.

### Optimize model performance with prompting

Here are some tips on how to get the best quality outputs:

1. Specify simple, well-defined tasks and provide explicit instructions for each step.
2. Claude sometimes assumes outcomes of its actions without explicitly checking their results. To prevent this you can prompt Claude with `After each step, take a screenshot and carefully evaluate if you have achieved the right outcome. Explicitly show your thinking: "I have evaluated step X..." If not correct, try again. Only when you confirm a step was executed correctly should you move on to the next one.`
3. Some UI elements (like dropdowns and scrollbars) might be tricky for Claude to manipulate using mouse movements. If you experience this, try prompting the model to use keyboard shortcuts.
4. For repeatable tasks or UI interactions, include example screenshots and tool calls of successful outcomes in your prompt.
5. If you need the model to log in, provide it with the username and password in your prompt inside xml tags like `<robot_credentials>`. Using computer use within applications that require login increases the risk of bad outcomes as a result of prompt injection. Please review our [guide on mitigating prompt injections](/en/docs/test-and-evaluate/strengthen-guardrails/mitigate-jailbreaks) before providing the model with login credentials.

<Tip>
  If you repeatedly encounter a clear set of issues or know in advance the tasks
  Claude will need to complete, use the system prompt to provide Claude with
  explicit tips or instructions on how to do the tasks successfully.
</Tip>

### System prompts

When one of the Anthropic-defined tools is requested via the Claude API, a computer use-specific system prompt is generated. It's similar to the [tool use system prompt](/en/docs/agents-and-tools/tool-use/implement-tool-use#tool-use-system-prompt) but starts with:

> You have access to a set of functions you can use to answer the user's question. This includes access to a sandboxed computing environment. You do NOT currently have the ability to inspect files or interact with external resources, except by invoking the below functions.

As with regular tool use, the user-provided `system_prompt` field is still respected and used in the construction of the combined system prompt.

### Available actions

The computer use tool supports these actions:

**Basic actions (all versions)**

- **screenshot** - Capture the current display
- **left_click** - Click at coordinates `[x, y]`
- **type** - Type text string
- **key** - Press key or key combination (e.g., "ctrl+s")
- **mouse_move** - Move cursor to coordinates

**Enhanced actions (`computer_20250124`)**
Available in Claude 4 models and Claude Sonnet 3.7:

- **scroll** - Scroll in any direction with amount control
- **left_click_drag** - Click and drag between coordinates
- **right_click**, **middle_click** - Additional mouse buttons
- **double_click**, **triple_click** - Multiple clicks
- **left_mouse_down**, **left_mouse_up** - Fine-grained click control
- **hold_key** - Hold a key while performing other actions
- **wait** - Pause between actions

<Accordion title="Example actions">
  ```json  theme={null}
  // Take a screenshot
  {
    "action": "screenshot"
  }

// Click at position
{
"action": "left_click",
"coordinate": [500, 300]
}

// Type text
{
"action": "type",
"text": "Hello, world!"
}

// Scroll down (Claude 4/3.7)
{
"action": "scroll",
"coordinate": [500, 400],
"scroll_direction": "down",
"scroll_amount": 3
}

````
</Accordion>

### Tool parameters

| Parameter           | Required | Description                                               |
| ------------------- | -------- | --------------------------------------------------------- |
| `type`              | Yes      | Tool version (`computer_20250124` or `computer_20241022`) |
| `name`              | Yes      | Must be "computer"                                        |
| `display_width_px`  | Yes      | Display width in pixels                                   |
| `display_height_px` | Yes      | Display height in pixels                                  |
| `display_number`    | No       | Display number for X11 environments                       |

<Warning>
Keep display resolution at or below 1280x800 (WXGA) for best performance. Higher resolutions may cause accuracy issues due to [image resizing](/en/docs/build-with-claude/vision#evaluate-image-size).
</Warning>

<Note>
**Important**: The computer use tool must be explicitly executed by your application - Claude cannot execute it directly. You are responsible for implementing the screenshot capture, mouse movements, keyboard inputs, and other actions based on Claude's requests.
</Note>

### Enable thinking capability in Claude 4 models and Claude Sonnet 3.7

Claude Sonnet 3.7 introduced a new "thinking" capability that allows you to see the model's reasoning process as it works through complex tasks. This feature helps you understand how Claude is approaching a problem and can be particularly valuable for debugging or educational purposes.

To enable thinking, add a `thinking` parameter to your API request:

```json  theme={null}
"thinking": {
"type": "enabled",
"budget_tokens": 1024
}
````

The `budget_tokens` parameter specifies how many tokens Claude can use for thinking. This is subtracted from your overall `max_tokens` budget.

When thinking is enabled, Claude will return its reasoning process as part of the response, which can help you:

1. Understand the model's decision-making process
2. Identify potential issues or misconceptions
3. Learn from Claude's approach to problem-solving
4. Get more visibility into complex multi-step operations

Here's an example of what thinking output might look like:

```
[Thinking]
I need to save a picture of a cat to the desktop. Let me break this down into steps:

1. First, I'll take a screenshot to see what's on the desktop
2. Then I'll look for a web browser to search for cat images
3. After finding a suitable image, I'll need to save it to the desktop

Let me start by taking a screenshot to see what's available...
```

### Augmenting computer use with other tools

The computer use tool can be combined with other tools to create more powerful automation workflows. This is particularly useful when you need to:

- Execute system commands ([bash tool](/en/docs/agents-and-tools/tool-use/bash-tool))
- Edit configuration files or scripts ([text editor tool](/en/docs/agents-and-tools/tool-use/text-editor-tool))
- Integrate with custom APIs or services (custom tools)

<CodeGroup>
  ```bash Shell theme={null}
  curl https://api.anthropic.com/v1/messages \
    -H "content-type: application/json" \
    -H "x-api-key: $ANTHROPIC_API_KEY" \
    -H "anthropic-version: 2023-06-01" \
    -H "anthropic-beta: computer-use-2025-01-24" \
    -d '{
      "model": "claude-sonnet-4-5",
      "max_tokens": 2000,
      "tools": [
        {
          "type": "computer_20250124",
          "name": "computer",
          "display_width_px": 1024,
          "display_height_px": 768,
          "display_number": 1
        },
        {
          "type": "text_editor_20250124",
          "name": "str_replace_editor"
        },
        {
          "type": "bash_20250124",
          "name": "bash"
        },
        {
          "name": "get_weather",
          "description": "Get the current weather in a given location",
          "input_schema": {
            "type": "object",
            "properties": {
              "location": {
                "type": "string",
                "description": "The city and state, e.g. San Francisco, CA"
              },
              "unit": {
                "type": "string",
                "enum": ["celsius", "fahrenheit"],
                "description": "The unit of temperature, either 'celsius' or 'fahrenheit'"
              }
            },
            "required": ["location"]
          }
        }
      ],
      "messages": [
        {
          "role": "user",
          "content": "Find flights from San Francisco to a place with warmer weather."
        }
      ],
      "thinking": {
        "type": "enabled",
        "budget_tokens": 1024
      }
    }'
  ```

```Python Python theme={null}
import anthropic

client = anthropic.Anthropic()

response = client.beta.messages.create(
    model="claude-sonnet-4-5",
    max_tokens=1024,
    tools=[
        {
          "type": "computer_20250124",
          "name": "computer",
          "display_width_px": 1024,
          "display_height_px": 768,
          "display_number": 1,
        },
        {
          "type": "text_editor_20250124",
          "name": "str_replace_editor"
        },
        {
          "type": "bash_20250124",
          "name": "bash"
        },
        {
          "name": "get_weather",
          "description": "Get the current weather in a given location",
          "input_schema": {
            "type": "object",
            "properties": {
              "location": {
                "type": "string",
                "description": "The city and state, e.g. San Francisco, CA"
              },
              "unit": {
                "type": "string",
                "enum": ["celsius", "fahrenheit"],
                "description": "The unit of temperature, either 'celsius' or 'fahrenheit'"
              }
            },
            "required": ["location"]
          }
        },
    ],
    messages=[{"role": "user", "content": "Find flights from San Francisco to a place with warmer weather."}],
    betas=["computer-use-2025-01-24"],
    thinking={"type": "enabled", "budget_tokens": 1024},
)
print(response)
```

```TypeScript TypeScript theme={null}
import Anthropic from '@anthropic-ai/sdk';

const anthropic = new Anthropic();

const message = await anthropic.beta.messages.create({
  model: "claude-sonnet-4-5",
  max_tokens: 1024,
  tools: [
      {
        type: "computer_20250124",
        name: "computer",
        display_width_px: 1024,
        display_height_px: 768,
        display_number: 1,
      },
      {
        type: "text_editor_20250124",
        name: "str_replace_editor"
      },
      {
        type: "bash_20250124",
        name: "bash"
      },
      {
        name: "get_weather",
        description: "Get the current weather in a given location",
        input_schema: {
          type: "object",
          properties: {
            location: {
              type: "string",
              description: "The city and state, e.g. San Francisco, CA"
            },
            unit: {
              type: "string",
              enum: ["celsius", "fahrenheit"],
              description: "The unit of temperature, either 'celsius' or 'fahrenheit'"
            }
          },
          required: ["location"]
        }
      },
  ],
  messages: [{ role: "user", content: "Find flights from San Francisco to a place with warmer weather." }],
  betas: ["computer-use-2025-01-24"],
  thinking: { type: "enabled", budget_tokens: 1024 },
});
console.log(message);
```

```java Java theme={null}
import java.util.List;
import java.util.Map;

import com.anthropic.client.AnthropicClient;
import com.anthropic.client.okhttp.AnthropicOkHttpClient;
import com.anthropic.core.JsonValue;
import com.anthropic.models.beta.messages.BetaMessage;
import com.anthropic.models.beta.messages.MessageCreateParams;
import com.anthropic.models.beta.messages.BetaToolBash20250124;
import com.anthropic.models.beta.messages.BetaToolComputerUse20250124;
import com.anthropic.models.beta.messages.BetaToolTextEditor20250124;
import com.anthropic.models.beta.messages.BetaThinkingConfigEnabled;
import com.anthropic.models.beta.messages.BetaThinkingConfigParam;
import com.anthropic.models.beta.messages.BetaTool;

public class MultipleToolsExample {

    public static void main(String[] args) {
        AnthropicClient client = AnthropicOkHttpClient.fromEnv();

        MessageCreateParams params = MessageCreateParams.builder()
                .model("claude-sonnet-4-5")
                .maxTokens(1024)
                .addTool(BetaToolComputerUse20250124.builder()
                        .displayWidthPx(1024)
                        .displayHeightPx(768)
                        .displayNumber(1)
                        .build())
                .addTool(BetaToolTextEditor20250124.builder()
                        .build())
                .addTool(BetaToolBash20250124.builder()
                        .build())
                .addTool(BetaTool.builder()
                        .name("get_weather")
                        .description("Get the current weather in a given location")
                        .inputSchema(BetaTool.InputSchema.builder()
                                .properties(
                                        JsonValue.from(
                                                Map.of(
                                                        "location", Map.of(
                                                                "type", "string",
                                                                "description", "The city and state, e.g. San Francisco, CA"
                                                        ),
                                                        "unit", Map.of(
                                                                "type", "string",
                                                                "enum", List.of("celsius", "fahrenheit"),
                                                                "description", "The unit of temperature, either 'celsius' or 'fahrenheit'"
                                                        )
                                                )
                                        ))
                                .build()
                        )
                        .build())
                .thinking(BetaThinkingConfigParam.ofEnabled(
                        BetaThinkingConfigEnabled.builder()
                                .budgetTokens(1024)
                                .build()
                ))
                .addUserMessage("Find flights from San Francisco to a place with warmer weather.")
                .addBeta("computer-use-2025-01-24")
                .build();

        BetaMessage message = client.beta().messages().create(params);
        System.out.println(message);
    }
}
```

</CodeGroup>

### Build a custom computer use environment

The [reference implementation](https://github.com/anthropics/anthropic-quickstarts/tree/main/computer-use-demo) is meant to help you get started with computer use. It includes all of the components needed have Claude use a computer. However, you can build your own environment for computer use to suit your needs. You'll need:

- A virtualized or containerized environment suitable for computer use with Claude
- An implementation of at least one of the Anthropic-defined computer use tools
- An agent loop that interacts with the Claude API and executes the `tool_use` results using your tool implementations
- An API or UI that allows user input to start the agent loop

#### Implement the computer use tool

The computer use tool is implemented as a schema-less tool. When using this tool, you don't need to provide an input schema as with other tools; the schema is built into Claude's model and can't be modified.

<Steps>
  <Step title="Set up your computing environment">
    Create a virtual display or connect to an existing display that Claude will interact with. This typically involves setting up Xvfb (X Virtual Framebuffer) or similar technology.
  </Step>

  <Step title="Implement action handlers">
    Create functions to handle each action type that Claude might request:

    ```python  theme={null}
    def handle_computer_action(action_type, params):
        if action_type == "screenshot":
            return capture_screenshot()
        elif action_type == "left_click":
            x, y = params["coordinate"]
            return click_at(x, y)
        elif action_type == "type":
            return type_text(params["text"])
        # ... handle other actions
    ```

  </Step>

  <Step title="Process Claude's tool calls">
    Extract and execute tool calls from Claude's responses:

    ```python  theme={null}
    for content in response.content:
        if content.type == "tool_use":
            action = content.input["action"]
            result = handle_computer_action(action, content.input)

            # Return result to Claude
            tool_result = {
                "type": "tool_result",
                "tool_use_id": content.id,
                "content": result
            }
    ```

  </Step>

  <Step title="Implement the agent loop">
    Create a loop that continues until Claude completes the task:

    ```python  theme={null}
    while True:
        response = client.beta.messages.create(...)

        # Check if Claude used any tools
        tool_results = process_tool_calls(response)

        if not tool_results:
            # No more tool use, task complete
            break

        # Continue conversation with tool results
        messages.append({"role": "user", "content": tool_results})
    ```

  </Step>
</Steps>

#### Handle errors

When implementing the computer use tool, various errors may occur. Here's how to handle them:

<AccordionGroup>
  <Accordion title="Screenshot capture failure">
    If screenshot capture fails, return an appropriate error message:

    ```json  theme={null}
    {
      "role": "user",
      "content": [
        {
          "type": "tool_result",
          "tool_use_id": "toolu_01A09q90qw90lq917835lq9",
          "content": "Error: Failed to capture screenshot. Display may be locked or unavailable.",
          "is_error": true
        }
      ]
    }
    ```

  </Accordion>

  <Accordion title="Invalid coordinates">
    If Claude provides coordinates outside the display bounds:

    ```json  theme={null}
    {
      "role": "user",
      "content": [
        {
          "type": "tool_result",
          "tool_use_id": "toolu_01A09q90qw90lq917835lq9",
          "content": "Error: Coordinates (1200, 900) are outside display bounds (1024x768).",
          "is_error": true
        }
      ]
    }
    ```

  </Accordion>

  <Accordion title="Action execution failure">
    If an action fails to execute:

    ```json  theme={null}
    {
      "role": "user",
      "content": [
        {
          "type": "tool_result",
          "tool_use_id": "toolu_01A09q90qw90lq917835lq9",
          "content": "Error: Failed to perform click action. The application may be unresponsive.",
          "is_error": true
        }
      ]
    }
    ```

  </Accordion>
</AccordionGroup>

#### Follow implementation best practices

<AccordionGroup>
  <Accordion title="Use appropriate display resolution">
    Set display dimensions that match your use case while staying within recommended limits:

    * For general desktop tasks: 1024x768 or 1280x720
    * For web applications: 1280x800 or 1366x768
    * Avoid resolutions above 1920x1080 to prevent performance issues

  </Accordion>

  <Accordion title="Implement proper screenshot handling">
    When returning screenshots to Claude:

    * Encode screenshots as base64 PNG or JPEG
    * Consider compressing large screenshots to improve performance
    * Include relevant metadata like timestamp or display state

  </Accordion>

  <Accordion title="Add action delays">
    Some applications need time to respond to actions:

    ```python  theme={null}
    def click_and_wait(x, y, wait_time=0.5):
        click_at(x, y)
        time.sleep(wait_time)  # Allow UI to update
    ```

  </Accordion>

  <Accordion title="Validate actions before execution">
    Check that requested actions are safe and valid:

    ```python  theme={null}
    def validate_action(action_type, params):
        if action_type == "left_click":
            x, y = params.get("coordinate", (0, 0))
            if not (0 <= x < display_width and 0 <= y < display_height):
                return False, "Coordinates out of bounds"
        return True, None
    ```

  </Accordion>

  <Accordion title="Log actions for debugging">
    Keep a log of all actions for troubleshooting:

    ```python  theme={null}
    import logging

    def log_action(action_type, params, result):
        logging.info(f"Action: {action_type}, Params: {params}, Result: {result}")
    ```

  </Accordion>
</AccordionGroup>

---

## Understand computer use limitations

The computer use functionality is in beta. While Claude's capabilities are cutting edge, developers should be aware of its limitations:

1. **Latency**: the current computer use latency for human-AI interactions may be too slow compared to regular human-directed computer actions. We recommend focusing on use cases where speed isn't critical (e.g., background information gathering, automated software testing) in trusted environments.
2. **Computer vision accuracy and reliability**: Claude may make mistakes or hallucinate when outputting specific coordinates while generating actions. Claude Sonnet 3.7 introduces the thinking capability that can help you understand the model's reasoning and identify potential issues.
3. **Tool selection accuracy and reliability**: Claude may make mistakes or hallucinate when selecting tools while generating actions or take unexpected actions to solve problems. Additionally, reliability may be lower when interacting with niche applications or multiple applications at once. We recommend that users prompt the model carefully when requesting complex tasks.
4. **Scrolling reliability**: Claude Sonnet 3.7 introduced dedicated scroll actions with direction control that improves reliability. The model can now explicitly scroll in any direction (up/down/left/right) by a specified amount.
5. **Spreadsheet interaction**: Mouse clicks for spreadsheet interaction have improved in Claude Sonnet 3.7 with the addition of more precise mouse control actions like `left_mouse_down`, `left_mouse_up`, and new modifier key support. Cell selection can be more reliable by using these fine-grained controls and combining modifier keys with clicks.
6. **Account creation and content generation on social and communications platforms**: While Claude will visit websites, we are limiting its ability to create accounts or generate and share content or otherwise engage in human impersonation across social media websites and platforms. We may update this capability in the future.
7. **Vulnerabilities**: Vulnerabilities like jailbreaking or prompt injection may persist across frontier AI systems, including the beta computer use API. In some circumstances, Claude will follow commands found in content, sometimes even in conflict with the user's instructions. For example, Claude instructions on webpages or contained in images may override instructions or cause Claude to make mistakes. We recommend:
   a. Limiting computer use to trusted environments such as virtual machines or containers with minimal privileges
   b. Avoiding giving computer use access to sensitive accounts or data without strict oversight
   c. Informing end users of relevant risks and obtaining their consent before enabling or requesting permissions necessary for computer use features in your applications
8. **Inappropriate or illegal actions**: Per Anthropic's terms of service, you must not employ computer use to violate any laws or our Acceptable Use Policy.

Always carefully review and verify Claude's computer use actions and logs. Do not use Claude for tasks requiring perfect precision or sensitive user information without human oversight.

---

## Pricing

Computer use follows the standard [tool use pricing](/en/docs/agents-and-tools/tool-use/overview#pricing). When using the computer use tool:

**System prompt overhead**: The computer use beta adds 466-499 tokens to the system prompt

**Computer use tool token usage**:

| Model                                                                      | Input tokens per tool definition |
| -------------------------------------------------------------------------- | -------------------------------- |
| Claude 4.x models                                                          | 735 tokens                       |
| Claude Sonnet 3.7 ([deprecated](/en/docs/about-claude/model-deprecations)) | 735 tokens                       |

**Additional token consumption**:

- Screenshot images (see [Vision pricing](/en/docs/build-with-claude/vision))
- Tool execution results returned to Claude

<Note>
  If you're also using bash or text editor tools alongside computer use, those tools have their own token costs as documented in their respective pages.
</Note>

## Next steps

<CardGroup cols={2}>
  <Card title="Reference implementation" icon="github" href="https://github.com/anthropics/anthropic-quickstarts/tree/main/computer-use-demo">
    Get started quickly with our complete Docker-based implementation
  </Card>

  <Card title="Tool documentation" icon="toolbox" href="/en/docs/agents-and-tools/tool-use/overview">
    Learn more about tool use and creating custom tools
  </Card>
</CardGroup>

---

## 10. Code execution tool

**URL:** https://docs.claude.com/en/docs/agents-and-tools/tool-use/code-execution-tool

# Code execution tool

Claude can analyze data, create visualizations, perform complex calculations, run system commands, create and edit files, and process uploaded
files directly within the API conversation.
The code execution tool allows Claude to run Bash commands and manipulate files, including writing code, in a secure, sandboxed environment.

<Note>
  The code execution tool is currently in public beta.

To use this feature, add the `"code-execution-2025-08-25"` [beta header](/en/api/beta-headers) to your API requests.
</Note>

## Model compatibility

The code execution tool is available on the following models:

| Model                                                                                                     | Tool Version              |
| --------------------------------------------------------------------------------------------------------- | ------------------------- |
| Claude Opus 4.1 (`claude-opus-4-1-20250805`)                                                              | `code_execution_20250825` |
| Claude Opus 4 (`claude-opus-4-20250514`)                                                                  | `code_execution_20250825` |
| Claude Sonnet 4.5 (`claude-sonnet-4-5-20250929`)                                                          | `code_execution_20250825` |
| Claude Sonnet 4 (`claude-sonnet-4-20250514`)                                                              | `code_execution_20250825` |
| Claude Sonnet 3.7 (`claude-3-7-sonnet-20250219`) ([deprecated](/en/docs/about-claude/model-deprecations)) | `code_execution_20250825` |
| Claude Haiku 4.5 (`claude-haiku-4-5-20251001`)                                                            | `code_execution_20250825` |
| Claude Haiku 3.5 (`claude-3-5-haiku-latest`)                                                              | `code_execution_20250825` |

<Note>
  The current version `code_execution_20250825` supports Bash commands and file operations. A legacy version `code_execution_20250522` (Python only) is also available. See [Upgrade to latest tool version](#upgrade-to-latest-tool-version) for migration details.
</Note>

<Warning>
  Older tool versions are not guaranteed to be backwards-compatible with newer models. Always use the tool version that corresponds to your model version.
</Warning>

## Quick start

Here's a simple example that asks Claude to perform a calculation:

<CodeGroup>
  ```bash Shell theme={null}
  curl https://api.anthropic.com/v1/messages \
      --header "x-api-key: $ANTHROPIC_API_KEY" \
      --header "anthropic-version: 2023-06-01" \
      --header "anthropic-beta: code-execution-2025-08-25" \
      --header "content-type: application/json" \
      --data '{
          "model": "claude-sonnet-4-5",
          "max_tokens": 4096,
          "messages": [
              {
                  "role": "user",
                  "content": "Calculate the mean and standard deviation of [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]"
              }
          ],
          "tools": [{
              "type": "code_execution_20250825",
              "name": "code_execution"
          }]
      }'
  ```

```python Python theme={null}
import anthropic

client = anthropic.Anthropic()

response = client.beta.messages.create(
    model="claude-sonnet-4-5",
    betas=["code-execution-2025-08-25"],
    max_tokens=4096,
    messages=[{
        "role": "user",
        "content": "Calculate the mean and standard deviation of [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]"
    }],
    tools=[{
        "type": "code_execution_20250825",
        "name": "code_execution"
    }]
)

print(response)
```

```typescript TypeScript theme={null}
import { Anthropic } from "@anthropic-ai/sdk";

const anthropic = new Anthropic();

async function main() {
  const response = await anthropic.beta.messages.create({
    model: "claude-sonnet-4-5",
    betas: ["code-execution-2025-08-25"],
    max_tokens: 4096,
    messages: [
      {
        role: "user",
        content: "Calculate the mean and standard deviation of [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]",
      },
    ],
    tools: [
      {
        type: "code_execution_20250825",
        name: "code_execution",
      },
    ],
  });

  console.log(response);
}

main().catch(console.error);
```

</CodeGroup>

## How code execution works

When you add the code execution tool to your API request:

1. Claude evaluates whether code execution would help answer your question
2. The tool automatically provides Claude with the following capabilities:
   - **Bash commands**: Execute shell commands for system operations and package management
   - **File operations**: Create, view, and edit files directly, including writing code
3. Claude can use any combination of these capabilities in a single request
4. All operations run in a secure sandbox environment
5. Claude provides results with any generated charts, calculations, or analysis

## How to use the tool

### Execute Bash commands

Ask Claude to check system information and install packages:

<CodeGroup>
  ```bash Shell theme={null}
  curl https://api.anthropic.com/v1/messages \
      --header "x-api-key: $ANTHROPIC_API_KEY" \
      --header "anthropic-version: 2023-06-01" \
      --header "anthropic-beta: code-execution-2025-08-25" \
      --header "content-type: application/json" \
      --data '{
          "model": "claude-sonnet-4-5",
          "max_tokens": 4096,
          "messages": [{
              "role": "user",
              "content": "Check the Python version and list installed packages"
          }],
          "tools": [{
              "type": "code_execution_20250825",
              "name": "code_execution"
          }]
      }'
  ```

```python Python theme={null}
response = client.beta.messages.create(
    model="claude-sonnet-4-5",
    betas=["code-execution-2025-08-25"],
    max_tokens=4096,
    messages=[{
        "role": "user",
        "content": "Check the Python version and list installed packages"
    }],
    tools=[{
        "type": "code_execution_20250825",
        "name": "code_execution"
    }]
)
```

```typescript TypeScript theme={null}
const response = await anthropic.beta.messages.create({
  model: "claude-sonnet-4-5",
  betas: ["code-execution-2025-08-25"],
  max_tokens: 4096,
  messages: [
    {
      role: "user",
      content: "Check the Python version and list installed packages",
    },
  ],
  tools: [
    {
      type: "code_execution_20250825",
      name: "code_execution",
    },
  ],
});
```

</CodeGroup>

### Create and edit files directly

Claude can create, view, and edit files directly in the sandbox using the file manipulation capabilities:

<CodeGroup>
  ```bash Shell theme={null}
  curl https://api.anthropic.com/v1/messages \
      --header "x-api-key: $ANTHROPIC_API_KEY" \
      --header "anthropic-version: 2023-06-01" \
      --header "anthropic-beta: code-execution-2025-08-25" \
      --header "content-type: application/json" \
      --data '{
          "model": "claude-sonnet-4-5",
          "max_tokens": 4096,
          "messages": [{
              "role": "user",
              "content": "Create a config.yaml file with database settings, then update the port from 5432 to 3306"
          }],
          "tools": [{
              "type": "code_execution_20250825",
              "name": "code_execution"
          }]
      }'
  ```

```python Python theme={null}
response = client.beta.messages.create(
    model="claude-sonnet-4-5",
    betas=["code-execution-2025-08-25"],
    max_tokens=4096,
    messages=[{
        "role": "user",
        "content": "Create a config.yaml file with database settings, then update the port from 5432 to 3306"
    }],
    tools=[{
        "type": "code_execution_20250825",
        "name": "code_execution"
    }]
)
```

```typescript TypeScript theme={null}
const response = await anthropic.beta.messages.create({
  model: "claude-sonnet-4-5",
  betas: ["code-execution-2025-08-25"],
  max_tokens: 4096,
  messages: [
    {
      role: "user",
      content: "Create a config.yaml file with database settings, then update the port from 5432 to 3306",
    },
  ],
  tools: [
    {
      type: "code_execution_20250825",
      name: "code_execution",
    },
  ],
});
```

</CodeGroup>

### Upload and analyze your own files

To analyze your own data files (CSV, Excel, images, etc.), upload them via the Files API and reference them in your request:

<Note>
  Using the Files API with Code Execution requires two beta headers: `"anthropic-beta": "code-execution-2025-08-25,files-api-2025-04-14"`
</Note>

The Python environment can process various file types uploaded via the Files API, including:

- CSV
- Excel (.xlsx, .xls)
- JSON
- XML
- Images (JPEG, PNG, GIF, WebP)
- Text files (.txt, .md, .py, etc)

#### Upload and analyze files

1. **Upload your file** using the [Files API](/en/docs/build-with-claude/files)
2. **Reference the file** in your message using a `container_upload` content block
3. **Include the code execution tool** in your API request

<CodeGroup>
  ```bash Shell theme={null}
  # First, upload a file
  curl https://api.anthropic.com/v1/files \
      --header "x-api-key: $ANTHROPIC_API_KEY" \
      --header "anthropic-version: 2023-06-01" \
      --header "anthropic-beta: files-api-2025-04-14" \
      --form 'file=@"data.csv"' \

# Then use the file_id with code execution

curl https://api.anthropic.com/v1/messages \
 --header "x-api-key: $ANTHROPIC_API_KEY" \
 --header "anthropic-version: 2023-06-01" \
 --header "anthropic-beta: code-execution-2025-08-25,files-api-2025-04-14" \
 --header "content-type: application/json" \
 --data '{
"model": "claude-sonnet-4-5",
"max_tokens": 4096,
"messages": [{
"role": "user",
"content": [
{"type": "text", "text": "Analyze this CSV data"},
{"type": "container_upload", "file_id": "file_abc123"}
]
}],
"tools": [{
"type": "code_execution_20250825",
"name": "code_execution"
}]
}'

````

```python Python theme={null}
import anthropic

client = anthropic.Anthropic()

# Upload a file
file_object = client.beta.files.upload(
    file=open("data.csv", "rb"),
)

# Use the file_id with code execution
response = client.beta.messages.create(
    model="claude-sonnet-4-5",
    betas=["code-execution-2025-08-25", "files-api-2025-04-14"],
    max_tokens=4096,
    messages=[{
        "role": "user",
        "content": [
            {"type": "text", "text": "Analyze this CSV data"},
            {"type": "container_upload", "file_id": file_object.id}
        ]
    }],
    tools=[{
        "type": "code_execution_20250825",
        "name": "code_execution"
    }]
)
````

```typescript TypeScript theme={null}
import { Anthropic } from "@anthropic-ai/sdk";
import { createReadStream } from "fs";

const anthropic = new Anthropic();

async function main() {
  // Upload a file
  const fileObject = await anthropic.beta.files.create({
    file: createReadStream("data.csv"),
  });

  // Use the file_id with code execution
  const response = await anthropic.beta.messages.create({
    model: "claude-sonnet-4-5",
    betas: ["code-execution-2025-08-25", "files-api-2025-04-14"],
    max_tokens: 4096,
    messages: [
      {
        role: "user",
        content: [
          { type: "text", text: "Analyze this CSV data" },
          { type: "container_upload", file_id: fileObject.id },
        ],
      },
    ],
    tools: [
      {
        type: "code_execution_20250825",
        name: "code_execution",
      },
    ],
  });

  console.log(response);
}

main().catch(console.error);
```

</CodeGroup>

#### Retrieve generated files

When Claude creates files during code execution, you can retrieve these files using the Files API:

<CodeGroup>
  ```python Python theme={null}
  from anthropic import Anthropic

# Initialize the client

client = Anthropic()

# Request code execution that creates files

response = client.beta.messages.create(
model="claude-sonnet-4-5",
betas=["code-execution-2025-08-25", "files-api-2025-04-14"],
max_tokens=4096,
messages=[{
"role": "user",
"content": "Create a matplotlib visualization and save it as output.png"
}],
tools=[{
"type": "code_execution_20250825",
"name": "code_execution"
}]
)

# Extract file IDs from the response

def extract_file_ids(response):
file_ids = []
for item in response.content:
if item.type == 'bash_code_execution_tool_result':
content_item = item.content
if content_item.type == 'bash_code_execution_result':
for file in content_item.content:
if hasattr(file, 'file_id'):
file_ids.append(file.file_id)
return file_ids

# Download the created files

for file_id in extract_file_ids(response):
file_metadata = client.beta.files.retrieve_metadata(file_id)
file_content = client.beta.files.download(file_id)
file_content.write_to_file(file_metadata.filename)
print(f"Downloaded: {file_metadata.filename}")

````

```typescript TypeScript theme={null}
import { Anthropic } from '@anthropic-ai/sdk';
import { writeFileSync } from 'fs';

// Initialize the client
const anthropic = new Anthropic();

async function main() {
  // Request code execution that creates files
  const response = await anthropic.beta.messages.create({
    model: "claude-sonnet-4-5",
    betas: ["code-execution-2025-08-25", "files-api-2025-04-14"],
    max_tokens: 4096,
    messages: [{
      role: "user",
      content: "Create a matplotlib visualization and save it as output.png"
    }],
    tools: [{
      type: "code_execution_20250825",
      name: "code_execution"
    }]
  });

  // Extract file IDs from the response
  function extractFileIds(response: any): string[] {
    const fileIds: string[] = [];
    for (const item of response.content) {
      if (item.type === 'bash_code_execution_tool_result') {
        const contentItem = item.content;
        if (contentItem.type === 'bash_code_execution_result' && contentItem.content) {
          for (const file of contentItem.content) {
            fileIds.push(file.file_id);
          }
        }
      }
    }
    return fileIds;
  }

  // Download the created files
  const fileIds = extractFileIds(response);
  for (const fileId of fileIds) {
    const fileMetadata = await anthropic.beta.files.retrieveMetadata(fileId);
    const fileContent = await anthropic.beta.files.download(fileId);

    // Convert ReadableStream to Buffer and save
    const chunks: Uint8Array[] = [];
    for await (const chunk of fileContent) {
      chunks.push(chunk);
    }
    const buffer = Buffer.concat(chunks);
    writeFileSync(fileMetadata.filename, buffer);
    console.log(`Downloaded: ${fileMetadata.filename}`);
  }
}

main().catch(console.error);
````

</CodeGroup>

### Combine operations

A complex workflow using all capabilities:

<CodeGroup>
  ```bash Shell theme={null}
  # First, upload a file
  curl https://api.anthropic.com/v1/files \
      --header "x-api-key: $ANTHROPIC_API_KEY" \
      --header "anthropic-version: 2023-06-01" \
      --header "anthropic-beta: files-api-2025-04-14" \
      --form 'file=@"data.csv"' \
      > file_response.json

# Extract file_id (using jq)

FILE_ID=$(jq -r '.id' file_response.json)

# Then use it with code execution

curl https://api.anthropic.com/v1/messages \
 --header "x-api-key: $ANTHROPIC_API_KEY" \
      --header "anthropic-version: 2023-06-01" \
      --header "anthropic-beta: code-execution-2025-08-25,files-api-2025-04-14" \
      --header "content-type: application/json" \
      --data '{
          "model": "claude-sonnet-4-5",
          "max_tokens": 4096,
          "messages": [{
              "role": "user",
              "content": [
                  {
                      "type": "text", 
                      "text": "Analyze this CSV data: create a summary report, save visualizations, and create a README with the findings"
                  },
                  {
                      "type": "container_upload", 
                      "file_id": "'$FILE_ID'"
}
]
}],
"tools": [{
"type": "code_execution_20250825",
"name": "code_execution"
}]
}'

````

```python Python theme={null}
# Upload a file
file_object = client.beta.files.upload(
    file=open("data.csv", "rb"),
)

# Use it with code execution
response = client.beta.messages.create(
    model="claude-sonnet-4-5",
    betas=["code-execution-2025-08-25", "files-api-2025-04-14"],
    max_tokens=4096,
    messages=[{
        "role": "user",
        "content": [
            {"type": "text", "text": "Analyze this CSV data: create a summary report, save visualizations, and create a README with the findings"},
            {"type": "container_upload", "file_id": file_object.id}
        ]
    }],
    tools=[{
        "type": "code_execution_20250825",
        "name": "code_execution"
    }]
)

# Claude might:
# 1. Use bash to check file size and preview data
# 2. Use text_editor to write Python code to analyze the CSV and create visualizations
# 3. Use bash to run the Python code
# 4. Use text_editor to create a README.md with findings
# 5. Use bash to organize files into a report directory
````

```typescript TypeScript theme={null}
// Upload a file
const fileObject = await anthropic.beta.files.create({
  file: createReadStream("data.csv"),
});

// Use it with code execution
const response = await anthropic.beta.messages.create({
  model: "claude-sonnet-4-5",
  betas: ["code-execution-2025-08-25", "files-api-2025-04-14"],
  max_tokens: 4096,
  messages: [
    {
      role: "user",
      content: [
        { type: "text", text: "Analyze this CSV data: create a summary report, save visualizations, and create a README with the findings" },
        { type: "container_upload", file_id: fileObject.id },
      ],
    },
  ],
  tools: [
    {
      type: "code_execution_20250825",
      name: "code_execution",
    },
  ],
});

// Claude might:
// 1. Use bash to check file size and preview data
// 2. Use text_editor to write Python code to analyze the CSV and create visualizations
// 3. Use bash to run the Python code
// 4. Use text_editor to create a README.md with findings
// 5. Use bash to organize files into a report directory
```

</CodeGroup>

## Tool definition

The code execution tool requires no additional parameters:

```json JSON theme={null}
{
  "type": "code_execution_20250825",
  "name": "code_execution"
}
```

When this tool is provided, Claude automatically gains access to two sub-tools:

- `bash_code_execution`: Run shell commands
- `text_editor_code_execution`: View, create, and edit files, including writing code

## Response format

The code execution tool can return two types of results depending on the operation:

### Bash command response

```json theme={null}
{
  "type": "server_tool_use",
  "id": "srvtoolu_01B3C4D5E6F7G8H9I0J1K2L3",
  "name": "bash_code_execution",
  "input": {
    "command": "ls -la | head -5"
  }
},
{
  "type": "bash_code_execution_tool_result",
  "tool_use_id": "srvtoolu_01B3C4D5E6F7G8H9I0J1K2L3",
  "content": {
    "type": "bash_code_execution_result",
    "stdout": "total 24\ndrwxr-xr-x 2 user user 4096 Jan 1 12:00 .\ndrwxr-xr-x 3 user user 4096 Jan 1 11:00 ..\n-rw-r--r-- 1 user user  220 Jan 1 12:00 data.csv\n-rw-r--r-- 1 user user  180 Jan 1 12:00 config.json",
    "stderr": "",
    "return_code": 0
  }
}
```

### File operation responses

**View file:**

```json theme={null}
{
  "type": "server_tool_use",
  "id": "srvtoolu_01C4D5E6F7G8H9I0J1K2L3M4",
  "name": "text_editor_code_execution",
  "input": {
    "command": "view",
    "path": "config.json"
  }
},
{
  "type": "text_editor_code_execution_tool_result",
  "tool_use_id": "srvtoolu_01C4D5E6F7G8H9I0J1K2L3M4",
  "content": {
    "type": "text_editor_code_execution_result",
    "file_type": "text",
    "content": "{\n  \"setting\": \"value\",\n  \"debug\": true\n}",
    "numLines": 4,
    "startLine": 1,
    "totalLines": 4
  }
}
```

**Create file:**

```json theme={null}
{
  "type": "server_tool_use",
  "id": "srvtoolu_01D5E6F7G8H9I0J1K2L3M4N5",
  "name": "text_editor_code_execution",
  "input": {
    "command": "create",
    "path": "new_file.txt",
    "file_text": "Hello, World!"
  }
},
{
  "type": "text_editor_code_execution_tool_result",
  "tool_use_id": "srvtoolu_01D5E6F7G8H9I0J1K2L3M4N5",
  "content": {
    "type": "text_editor_code_execution_result",
    "is_file_update": false
  }
}
```

**Edit file (str_replace):**

```json theme={null}
{
  "type": "server_tool_use",
  "id": "srvtoolu_01E6F7G8H9I0J1K2L3M4N5O6",
  "name": "text_editor_code_execution",
  "input": {
    "command": "str_replace",
    "path": "config.json",
    "old_str": "\"debug\": true",
    "new_str": "\"debug\": false"
  }
},
{
  "type": "text_editor_code_execution_tool_result",
  "tool_use_id": "srvtoolu_01E6F7G8H9I0J1K2L3M4N5O6",
  "content": {
    "type": "text_editor_code_execution_result",
    "oldStart": 3,
    "oldLines": 1,
    "newStart": 3,
    "newLines": 1,
    "lines": ["-  \"debug\": true", "+  \"debug\": false"]
  }
}
```

### Results

All execution results include:

- `stdout`: Output from successful execution
- `stderr`: Error messages if execution fails
- `return_code`: 0 for success, non-zero for failure

Additional fields for file operations:

- **View**: `file_type`, `content`, `numLines`, `startLine`, `totalLines`
- **Create**: `is_file_update` (whether file already existed)
- **Edit**: `oldStart`, `oldLines`, `newStart`, `newLines`, `lines` (diff format)

### Errors

Each tool type can return specific errors:

**Common errors (all tools):**

```json theme={null}
{
  "type": "bash_code_execution_tool_result",
  "tool_use_id": "srvtoolu_01VfmxgZ46TiHbmXgy928hQR",
  "content": {
    "type": "bash_code_execution_tool_result_error",
    "error_code": "unavailable"
  }
}
```

**Error codes by tool type:**

| Tool        | Error Code                | Description                                       |
| ----------- | ------------------------- | ------------------------------------------------- |
| All tools   | `unavailable`             | The tool is temporarily unavailable               |
| All tools   | `execution_time_exceeded` | Execution exceeded maximum time limit             |
| All tools   | `container_expired`       | Container expired and is no longer available      |
| All tools   | `invalid_tool_input`      | Invalid parameters provided to the tool           |
| All tools   | `too_many_requests`       | Rate limit exceeded for tool usage                |
| text_editor | `file_not_found`          | File doesn't exist (for view/edit operations)     |
| text_editor | `string_not_found`        | The `old_str` not found in file (for str_replace) |

#### `pause_turn` stop reason

The response may include a `pause_turn` stop reason, which indicates that the API paused a long-running turn. You may
provide the response back as-is in a subsequent request to let Claude continue its turn, or modify the content if you
wish to interrupt the conversation.

## Containers

The code execution tool runs in a secure, containerized environment designed specifically for code execution, with a higher focus on Python.

### Runtime environment

- **Python version**: 3.11.12
- **Operating system**: Linux-based container
- **Architecture**: x86_64 (AMD64)

### Resource limits

- **Memory**: 5GiB RAM
- **Disk space**: 5GiB workspace storage
- **CPU**: 1 CPU

### Networking and security

- **Internet access**: Completely disabled for security
- **External connections**: No outbound network requests permitted
- **Sandbox isolation**: Full isolation from host system and other containers
- **File access**: Limited to workspace directory only
- **Workspace scoping**: Like [Files](/en/docs/build-with-claude/files), containers are scoped to the workspace of the API key
- **Expiration**: Containers expire 30 days after creation

### Pre-installed libraries

The sandboxed Python environment includes these commonly used libraries:

- **Data Science**: pandas, numpy, scipy, scikit-learn, statsmodels
- **Visualization**: matplotlib, seaborn
- **File Processing**: pyarrow, openpyxl, xlsxwriter, xlrd, pillow, python-pptx, python-docx, pypdf, pdfplumber, pypdfium2, pdf2image, pdfkit, tabula-py, reportlab\[pycairo], Img2pdf
- **Math & Computing**: sympy, mpmath
- **Utilities**: tqdm, python-dateutil, pytz, joblib, unzip, unrar, 7zip, bc, rg (ripgrep), fd, sqlite

## Container reuse

You can reuse an existing container across multiple API requests by providing the container ID from a previous response.
This allows you to maintain created files between requests.

### Example

<CodeGroup>
  ```python Python theme={null}
  import os
  from anthropic import Anthropic

# Initialize the client

client = Anthropic(
api_key=os.getenv("ANTHROPIC_API_KEY")
)

# First request: Create a file with a random number

response1 = client.beta.messages.create(
model="claude-sonnet-4-5",
betas=["code-execution-2025-08-25"],
max_tokens=4096,
messages=[{
"role": "user",
"content": "Write a file with a random number and save it to '/tmp/number.txt'"
}],
tools=[{
"type": "code_execution_20250825",
"name": "code_execution"
}]
)

# Extract the container ID from the first response

container_id = response1.container.id

# Second request: Reuse the container to read the file

response2 = client.beta.messages.create(
container=container_id, # Reuse the same container
model="claude-sonnet-4-5",
betas=["code-execution-2025-08-25"],
max_tokens=4096,
messages=[{
"role": "user",
"content": "Read the number from '/tmp/number.txt' and calculate its square"
}],
tools=[{
"type": "code_execution_20250825",
"name": "code_execution"
}]
)

````

```typescript TypeScript theme={null}
import { Anthropic } from '@anthropic-ai/sdk';

const anthropic = new Anthropic();

async function main() {
  // First request: Create a file with a random number
  const response1 = await anthropic.beta.messages.create({
    model: "claude-sonnet-4-5",
    betas: ["code-execution-2025-08-25"],
    max_tokens: 4096,
    messages: [{
      role: "user",
      content: "Write a file with a random number and save it to '/tmp/number.txt'"
    }],
    tools: [{
      type: "code_execution_20250825",
      name: "code_execution"
    }]
  });

  // Extract the container ID from the first response
  const containerId = response1.container.id;

  // Second request: Reuse the container to read the file
  const response2 = await anthropic.beta.messages.create({
    container: containerId,  // Reuse the same container
    model: "claude-sonnet-4-5",
    betas: ["code-execution-2025-08-25"],
    max_tokens: 4096,
    messages: [{
      role: "user",
      content: "Read the number from '/tmp/number.txt' and calculate its square"
    }],
    tools: [{
      type: "code_execution_20250825",
      name: "code_execution"
    }]
  });

  console.log(response2.content);
}

main().catch(console.error);
````

```bash Shell theme={null}
# First request: Create a file with a random number
curl https://api.anthropic.com/v1/messages \
    --header "x-api-key: $ANTHROPIC_API_KEY" \
    --header "anthropic-version: 2023-06-01" \
    --header "anthropic-beta: code-execution-2025-08-25" \
    --header "content-type: application/json" \
    --data '{
        "model": "claude-sonnet-4-5",
        "max_tokens": 4096,
        "messages": [{
            "role": "user",
            "content": "Write a file with a random number and save it to \"/tmp/number.txt\""
        }],
        "tools": [{
            "type": "code_execution_20250825",
            "name": "code_execution"
        }]
    }' > response1.json

# Extract container ID from the response (using jq)
CONTAINER_ID=$(jq -r '.container.id' response1.json)

# Second request: Reuse the container to read the file
curl https://api.anthropic.com/v1/messages \
    --header "x-api-key: $ANTHROPIC_API_KEY" \
    --header "anthropic-version: 2023-06-01" \
    --header "anthropic-beta: code-execution-2025-08-25" \
    --header "content-type: application/json" \
    --data '{
        "container": "'$CONTAINER_ID'",
        "model": "claude-sonnet-4-5",
        "max_tokens": 4096,
        "messages": [{
            "role": "user",
            "content": "Read the number from \"/tmp/number.txt\" and calculate its square"
        }],
        "tools": [{
            "type": "code_execution_20250825",
            "name": "code_execution"
        }]
    }'
```

</CodeGroup>

## Streaming

With streaming enabled, you'll receive code execution events as they occur:

```javascript theme={null}
event: content_block_start
data: {"type": "content_block_start", "index": 1, "content_block": {"type": "server_tool_use", "id": "srvtoolu_xyz789", "name": "code_execution"}}

// Code execution streamed
event: content_block_delta
data: {"type": "content_block_delta", "index": 1, "delta": {"type": "input_json_delta", "partial_json": "{\"code\":\"import pandas as pd\\ndf = pd.read_csv('data.csv')\\nprint(df.head())\"}"}}

// Pause while code executes

// Execution results streamed
event: content_block_start
data: {"type": "content_block_start", "index": 2, "content_block": {"type": "code_execution_tool_result", "tool_use_id": "srvtoolu_xyz789", "content": {"stdout": "   A  B  C\n0  1  2  3\n1  4  5  6", "stderr": ""}}}
```

## Batch requests

You can include the code execution tool in the [Messages Batches API](/en/docs/build-with-claude/batch-processing). Code execution tool calls through the Messages Batches API are priced the same as those in regular Messages API requests.

## Usage and pricing

Code execution tool usage is tracked separately from token usage. Execution time has a minimum of 5 minutes.
If files are included in the request, execution time is billed even if the tool is not used due to files being preloaded onto the container.

Each organization receives 50 free hours of usage with the code execution tool per day. Additional usage beyond the first 50 hours is billed at \$0.05 per hour, per container.

## Upgrade to latest tool version

By upgrading to `code-execution-2025-08-25`, you get access to file manipulation and Bash capabilities, including code in multiple languages. There is no price difference.

### What's changed

| Component      | Legacy                      | Current                                                           |
| -------------- | --------------------------- | ----------------------------------------------------------------- |
| Beta header    | `code-execution-2025-05-22` | `code-execution-2025-08-25`                                       |
| Tool type      | `code_execution_20250522`   | `code_execution_20250825`                                         |
| Capabilities   | Python only                 | Bash commands, file operations                                    |
| Response types | `code_execution_result`     | `bash_code_execution_result`, `text_editor_code_execution_result` |

### Backward compatibility

- All existing Python code execution continues to work exactly as before
- No changes required to existing Python-only workflows

### Upgrade steps

To upgrade, you need to make the following changes in your API requests:

1. **Update the beta header**:

   ```diff theme={null}
   - "anthropic-beta": "code-execution-2025-05-22"
   + "anthropic-beta": "code-execution-2025-08-25"
   ```

2. **Update the tool type**:

   ```diff theme={null}
   - "type": "code_execution_20250522"
   + "type": "code_execution_20250825"
   ```

3. **Review response handling** (if parsing responses programmatically):
   - The previous blocks for Python execution responses will no longer be sent
   - Instead, new response types for Bash and file operations will be sent (see Response Format section)

## Using code execution with Agent Skills

The code execution tool enables Claude to use [Agent Skills](/en/docs/agents-and-tools/agent-skills/overview). Skills are modular capabilities consisting of instructions, scripts, and resources that extend Claude's functionality.

Learn more in the [Agent Skills documentation](/en/docs/agents-and-tools/agent-skills/overview) and [Agent Skills API guide](/en/docs/agents-and-tools/agent-skills/api).

---

## 11. Bash tool

**URL:** https://docs.claude.com/en/docs/agents-and-tools/tool-use/bash-tool

# Bash tool

The bash tool enables Claude to execute shell commands in a persistent bash session, allowing system operations, script execution, and command-line automation.

## Overview

The bash tool provides Claude with:

- Persistent bash session that maintains state
- Ability to run any shell command
- Access to environment variables and working directory
- Command chaining and scripting capabilities

## Model compatibility

| Model                                                                                   | Tool Version    |
| --------------------------------------------------------------------------------------- | --------------- |
| Claude 4 models and Sonnet 3.7 ([deprecated](/en/docs/about-claude/model-deprecations)) | `bash_20250124` |

<Warning>
  Older tool versions are not guaranteed to be backwards-compatible with newer models. Always use the tool version that corresponds to your model version.
</Warning>

## Use cases

- **Development workflows**: Run build commands, tests, and development tools
- **System automation**: Execute scripts, manage files, automate tasks
- **Data processing**: Process files, run analysis scripts, manage datasets
- **Environment setup**: Install packages, configure environments

## Quick start

<CodeGroup>
  ```python Python theme={null}
  import anthropic

client = anthropic.Anthropic()

response = client.messages.create(
model="claude-sonnet-4-5",
max_tokens=1024,
tools=[
{
"type": "bash_20250124",
"name": "bash"
}
],
messages=[
{"role": "user", "content": "List all Python files in the current directory."}
]
)

````

```bash Shell theme={null}
curl https://api.anthropic.com/v1/messages \
  -H "content-type: application/json" \
  -H "x-api-key: $ANTHROPIC_API_KEY" \
  -H "anthropic-version: 2023-06-01" \
  -d '{
    "model": "claude-sonnet-4-5",
    "max_tokens": 1024,
    "tools": [
      {
        "type": "bash_20250124",
        "name": "bash"
      }
    ],
    "messages": [
      {
        "role": "user",
        "content": "List all Python files in the current directory."
      }
    ]
  }'
````

</CodeGroup>

## How it works

The bash tool maintains a persistent session:

1. Claude determines what command to run
2. You execute the command in a bash shell
3. Return the output (stdout and stderr) to Claude
4. Session state persists between commands (environment variables, working directory)

## Parameters

| Parameter | Required | Description                               |
| --------- | -------- | ----------------------------------------- |
| `command` | Yes\*    | The bash command to run                   |
| `restart` | No       | Set to `true` to restart the bash session |

\*Required unless using `restart`

<Accordion title="Example usage">
  ```json  theme={null}
  // Run a command
  {
    "command": "ls -la *.py"
  }

// Restart the session
{
"restart": true
}

````
</Accordion>

## Example: Multi-step automation

Claude can chain commands to complete complex tasks:

```python  theme={null}
# User request
"Install the requests library and create a simple Python script that fetches a joke from an API, then run it."

# Claude's tool uses:
# 1. Install package
{"command": "pip install requests"}

# 2. Create script
{"command": "cat > fetch_joke.py << 'EOF'\nimport requests\nresponse = requests.get('https://official-joke-api.appspot.com/random_joke')\njoke = response.json()\nprint(f\"Setup: {joke['setup']}\")\nprint(f\"Punchline: {joke['punchline']}\")\nEOF"}

# 3. Run script
{"command": "python fetch_joke.py"}
````

The session maintains state between commands, so files created in step 2 are available in step 3.

---

## Implement the bash tool

The bash tool is implemented as a schema-less tool. When using this tool, you don't need to provide an input schema as with other tools; the schema is built into Claude's model and can't be modified.

<Steps>
  <Step title="Set up a bash environment">
    Create a persistent bash session that Claude can interact with:

    ```python  theme={null}
    import subprocess
    import threading
    import queue

    class BashSession:
        def __init__(self):
            self.process = subprocess.Popen(
                ['/bin/bash'],
                stdin=subprocess.PIPE,
                stdout=subprocess.PIPE,
                stderr=subprocess.PIPE,
                text=True,
                bufsize=0
            )
            self.output_queue = queue.Queue()
            self.error_queue = queue.Queue()
            self._start_readers()
    ```

  </Step>

  <Step title="Handle command execution">
    Create a function to execute commands and capture output:

    ```python  theme={null}
    def execute_command(self, command):
        # Send command to bash
        self.process.stdin.write(command + '\n')
        self.process.stdin.flush()

        # Capture output with timeout
        output = self._read_output(timeout=10)
        return output
    ```

  </Step>

  <Step title="Process Claude's tool calls">
    Extract and execute commands from Claude's responses:

    ```python  theme={null}
    for content in response.content:
        if content.type == "tool_use" and content.name == "bash":
            if content.input.get("restart"):
                bash_session.restart()
                result = "Bash session restarted"
            else:
                command = content.input.get("command")
                result = bash_session.execute_command(command)

            # Return result to Claude
            tool_result = {
                "type": "tool_result",
                "tool_use_id": content.id,
                "content": result
            }
    ```

  </Step>

  <Step title="Implement safety measures">
    Add validation and restrictions:

    ```python  theme={null}
    def validate_command(command):
        # Block dangerous commands
        dangerous_patterns = ['rm -rf /', 'format', ':(){:|:&};:']
        for pattern in dangerous_patterns:
            if pattern in command:
                return False, f"Command contains dangerous pattern: {pattern}"

        # Add more validation as needed
        return True, None
    ```

  </Step>
</Steps>

### Handle errors

When implementing the bash tool, handle various error scenarios:

<AccordionGroup>
  <Accordion title="Command execution timeout">
    If a command takes too long to execute:

    ```json  theme={null}
    {
      "role": "user",
      "content": [
        {
          "type": "tool_result",
          "tool_use_id": "toolu_01A09q90qw90lq917835lq9",
          "content": "Error: Command timed out after 30 seconds",
          "is_error": true
        }
      ]
    }
    ```

  </Accordion>

  <Accordion title="Command not found">
    If a command doesn't exist:

    ```json  theme={null}
    {
      "role": "user",
      "content": [
        {
          "type": "tool_result",
          "tool_use_id": "toolu_01A09q90qw90lq917835lq9",
          "content": "bash: nonexistentcommand: command not found",
          "is_error": true
        }
      ]
    }
    ```

  </Accordion>

  <Accordion title="Permission denied">
    If there are permission issues:

    ```json  theme={null}
    {
      "role": "user",
      "content": [
        {
          "type": "tool_result",
          "tool_use_id": "toolu_01A09q90qw90lq917835lq9",
          "content": "bash: /root/sensitive-file: Permission denied",
          "is_error": true
        }
      ]
    }
    ```

  </Accordion>
</AccordionGroup>

### Follow implementation best practices

<AccordionGroup>
  <Accordion title="Use command timeouts">
    Implement timeouts to prevent hanging commands:

    ```python  theme={null}
    def execute_with_timeout(command, timeout=30):
        try:
            result = subprocess.run(
                command,
                shell=True,
                capture_output=True,
                text=True,
                timeout=timeout
            )
            return result.stdout + result.stderr
        except subprocess.TimeoutExpired:
            return f"Command timed out after {timeout} seconds"
    ```

  </Accordion>

  <Accordion title="Maintain session state">
    Keep the bash session persistent to maintain environment variables and working directory:

    ```python  theme={null}
    # Commands run in the same session maintain state
    commands = [
        "cd /tmp",
        "echo 'Hello' > test.txt",
        "cat test.txt"  # This works because we're still in /tmp
    ]
    ```

  </Accordion>

  <Accordion title="Handle large outputs">
    Truncate very large outputs to prevent token limit issues:

    ```python  theme={null}
    def truncate_output(output, max_lines=100):
        lines = output.split('\n')
        if len(lines) > max_lines:
            truncated = '\n'.join(lines[:max_lines])
            return f"{truncated}\n\n... Output truncated ({len(lines)} total lines) ..."
        return output
    ```

  </Accordion>

  <Accordion title="Log all commands">
    Keep an audit trail of executed commands:

    ```python  theme={null}
    import logging

    def log_command(command, output, user_id):
        logging.info(f"User {user_id} executed: {command}")
        logging.info(f"Output: {output[:200]}...")  # Log first 200 chars
    ```

  </Accordion>

  <Accordion title="Sanitize outputs">
    Remove sensitive information from command outputs:

    ```python  theme={null}
    def sanitize_output(output):
        # Remove potential secrets or credentials
        import re
        # Example: Remove AWS credentials
        output = re.sub(r'aws_access_key_id\s*=\s*\S+', 'aws_access_key_id=***', output)
        output = re.sub(r'aws_secret_access_key\s*=\s*\S+', 'aws_secret_access_key=***', output)
        return output
    ```

  </Accordion>
</AccordionGroup>

## Security

<Warning>
  The bash tool provides direct system access. Implement these essential safety measures:

- Running in isolated environments (Docker/VM)
- Implementing command filtering and allowlists
- Setting resource limits (CPU, memory, disk)
- Logging all executed commands
  </Warning>

### Key recommendations

- Use `ulimit` to set resource constraints
- Filter dangerous commands (`sudo`, `rm -rf`, etc.)
- Run with minimal user permissions
- Monitor and log all command execution

## Pricing

The bash tool adds **245 input tokens** to your API calls.

Additional tokens are consumed by:

- Command outputs (stdout/stderr)
- Error messages
- Large file contents

See [tool use pricing](/en/docs/agents-and-tools/tool-use/overview#pricing) for complete pricing details.

## Common patterns

### Development workflows

- Running tests: `pytest && coverage report`
- Building projects: `npm install && npm run build`
- Git operations: `git status && git add . && git commit -m "message"`

### File operations

- Processing data: `wc -l *.csv && ls -lh *.csv`
- Searching files: `find . -name "*.py" | xargs grep "pattern"`
- Creating backups: `tar -czf backup.tar.gz ./data`

### System tasks

- Checking resources: `df -h && free -m`
- Process management: `ps aux | grep python`
- Environment setup: `export PATH=$PATH:/new/path && echo $PATH`

## Limitations

- **No interactive commands**: Cannot handle `vim`, `less`, or password prompts
- **No GUI applications**: Command-line only
- **Session scope**: Persists within conversation, lost between API calls
- **Output limits**: Large outputs may be truncated
- **No streaming**: Results returned after completion

## Combining with other tools

The bash tool is most powerful when combined with the [text editor](/en/docs/agents-and-tools/tool-use/text-editor-tool) and other tools.

## Next steps

<CardGroup cols={2}>
  <Card title="Tool use overview" icon="toolbox" href="/en/docs/agents-and-tools/tool-use/overview">
    Learn about tool use with Claude
  </Card>

  <Card title="Text editor tool" icon="file-code" href="/en/docs/agents-and-tools/tool-use/text-editor-tool">
    View and edit text files with Claude
  </Card>
</CardGroup>

---

## 12. Fine-grained tool streaming

**URL:** https://docs.claude.com/en/docs/agents-and-tools/tool-use/fine-grained-tool-streaming

# Fine-grained tool streaming

Tool use now supports fine-grained [streaming](/en/docs/build-with-claude/streaming) for parameter values. This allows developers to stream tool use parameters without buffering / JSON validation, reducing the latency to begin receiving large parameters.

<Note>
  Fine-grained tool streaming is a beta feature. Please make sure to evaluate your responses before using it in production.

Please use [this form](https://forms.gle/D4Fjr7GvQRzfTZT96) to provide feedback on the quality of the model responses, the API itself, or the quality of the documentation—we cannot wait to hear from you!
</Note>

<Warning>
  When using fine-grained tool streaming, you may potentially receive invalid or partial JSON inputs. Please make sure to account for these edge cases in your code.
</Warning>

## How to use fine-grained tool streaming

To use this beta feature, simply add the beta header `fine-grained-tool-streaming-2025-05-14` to a tool use request and turn on streaming.

Here's an example of how to use fine-grained tool streaming with the API:

<CodeGroup>
  ```bash Shell theme={null}
  curl https://api.anthropic.com/v1/messages \
    -H "content-type: application/json" \
    -H "x-api-key: $ANTHROPIC_API_KEY" \
    -H "anthropic-version: 2023-06-01" \
    -H "anthropic-beta: fine-grained-tool-streaming-2025-05-14" \
    -d '{
      "model": "claude-sonnet-4-5",
      "max_tokens": 65536,
      "tools": [
        {
          "name": "make_file",
          "description": "Write text to a file",
          "input_schema": {
            "type": "object",
            "properties": {
              "filename": {
                "type": "string",
                "description": "The filename to write text to"
              },
              "lines_of_text": {
                "type": "array",
                "description": "An array of lines of text to write to the file"
              }
            },
            "required": ["filename", "lines_of_text"]
          }
        }
      ],
      "messages": [
        {
          "role": "user",
          "content": "Can you write a long poem and make a file called poem.txt?"
        }
      ],
      "stream": true
    }' | jq '.usage'
  ```

```Python Python theme={null}
import anthropic

client = anthropic.Anthropic()

response = client.beta.messages.stream(
    max_tokens=65536,
    model="claude-sonnet-4-5",
    tools=[{
      "name": "make_file",
      "description": "Write text to a file",
      "input_schema": {
        "type": "object",
        "properties": {
          "filename": {
            "type": "string",
            "description": "The filename to write text to"
          },
          "lines_of_text": {
            "type": "array",
            "description": "An array of lines of text to write to the file"
          }
        },
        "required": ["filename", "lines_of_text"]
      }
    }],
    messages=[{
      "role": "user",
      "content": "Can you write a long poem and make a file called poem.txt?"
    }],
    betas=["fine-grained-tool-streaming-2025-05-14"]
)

print(response.usage)
```

```TypeScript TypeScript theme={null}
import Anthropic from '@anthropic-ai/sdk';

const anthropic = new Anthropic();

const message = await anthropic.beta.messages.stream({
  model: "claude-sonnet-4-5",
  max_tokens: 65536,
  tools: [{
    "name": "make_file",
    "description": "Write text to a file",
    "input_schema": {
      "type": "object",
      "properties": {
        "filename": {
          "type": "string",
          "description": "The filename to write text to"
        },
        "lines_of_text": {
          "type": "array",
          "description": "An array of lines of text to write to the file"
        }
      },
      "required": ["filename", "lines_of_text"]
    }
  }],
  messages: [{
    role: "user",
    content: "Can you write a long poem and make a file called poem.txt?"
  }],
  betas: ["fine-grained-tool-streaming-2025-05-14"]
});

console.log(message.usage);
```

</CodeGroup>

In this example, fine-grained tool streaming enables Claude to stream the lines of a long poem into the tool call `make_file` without buffering to validate if the `lines_of_text` parameter is valid JSON. This means you can see the parameter stream as it arrives, without having to wait for the entire parameter to buffer and validate.

<Note>
  With fine-grained tool streaming, tool use chunks start streaming faster, and are often longer and contain fewer word breaks. This is due to differences in chunking behavior.

Example:

Without fine-grained streaming (15s delay):

```
Chunk 1: '{"'
Chunk 2: 'query": "Ty'
Chunk 3: 'peScri'
Chunk 4: 'pt 5.0 5.1 '
Chunk 5: '5.2 5'
Chunk 6: '.3'
Chunk 8: ' new f'
Chunk 9: 'eatur'
...
```

With fine-grained streaming (3s delay):

```
Chunk 1: '{"query": "TypeScript 5.0 5.1 5.2 5.3'
Chunk 2: ' new features comparison'
```

</Note>

<Warning>
  Because fine-grained streaming sends parameters without buffering or JSON validation, there is no guarantee that the resulting stream will complete in a valid JSON string.
  Particularly, if the [stop reason](/en/api/handling-stop-reasons) `max_tokens` is reached, the stream may end midway through a parameter and may be incomplete. You will generally have to write specific support to handle when `max_tokens` is reached.
</Warning>

## Handling invalid JSON in tool responses

When using fine-grained tool streaming, you may receive invalid or incomplete JSON from the model. If you need to pass this invalid JSON back to the model in an error response block, you may wrap it in a JSON object to ensure proper handling (with a reasonable key). For example:

```json theme={null}
{
  "INVALID_JSON": "<your invalid json string>"
}
```

This approach helps the model understand that the content is invalid JSON while preserving the original malformed data for debugging purposes.

<Note>
  When wrapping invalid JSON, make sure to properly escape any quotes or special characters in the invalid JSON string to maintain valid JSON structure in the wrapper object.
</Note>

---

## 13. Token-efficient tool use

**URL:** https://docs.claude.com/en/docs/agents-and-tools/tool-use/token-efficient-tool-use

# Token-efficient tool use

Starting with Claude Sonnet 3.7, Claude is capable of calling tools in a token-efficient manner. Requests save an average of 14% in output tokens, up to 70%, which also reduces latency. Exact token reduction and latency improvements depend on the overall response shape and size.

<Info>
  Token-efficient tool use is a beta feature in Claude 3.7. To use this beta feature, simply add the beta header `token-efficient-tools-2025-02-19` to a tool use request.

All [Claude 4 models](/en/docs/about-claude/models/overview) support token-efficient tool use by default. No beta header is needed, but the `token-efficient-tools-2025-02-19` header will not break an API request.
</Info>

<Warning>
  Token-efficient tool use does not currently work with [`disable_parallel_tool_use`](/en/docs/build-with-claude/tool-use#disabling-parallel-tool-use).
</Warning>

Here's an example of how to use token-efficient tools with the API in Claude Sonnet 3.7:

<CodeGroup>
  ```bash Shell theme={null}
  curl https://api.anthropic.com/v1/messages \
    -H "content-type: application/json" \
    -H "x-api-key: $ANTHROPIC_API_KEY" \
    -H "anthropic-version: 2023-06-01" \
    -H "anthropic-beta: token-efficient-tools-2025-02-19" \
    -d '{
      "model": "claude-3-7-sonnet-20250219",
      "max_tokens": 1024,
      "tools": [
        {
          "name": "get_weather",
          "description": "Get the current weather in a given location",
          "input_schema": {
            "type": "object",
            "properties": {
              "location": {
                "type": "string",
                "description": "The city and state, e.g. San Francisco, CA"
              }
            },
            "required": [
              "location"
            ]
          }
        }
      ],
      "messages": [
        {
          "role": "user",
          "content": "Tell me the weather in San Francisco."
        }
      ]
    }' | jq '.usage'
  ```

```Python Python theme={null}
import anthropic

client = anthropic.Anthropic()

response = client.beta.messages.create(
    max_tokens=1024,
    model="claude-3-7-sonnet-20250219",
    tools=[{
      "name": "get_weather",
      "description": "Get the current weather in a given location",
      "input_schema": {
        "type": "object",
        "properties": {
          "location": {
            "type": "string",
            "description": "The city and state, e.g. San Francisco, CA"
          }
        },
        "required": [
          "location"
        ]
      }
    }],
    messages=[{
      "role": "user",
      "content": "Tell me the weather in San Francisco."
    }],
    betas=["token-efficient-tools-2025-02-19"]
)

print(response.usage)
```

```TypeScript TypeScript theme={null}
import Anthropic from '@anthropic-ai/sdk';

const anthropic = new Anthropic();

const message = await anthropic.beta.messages.create({
  model: "claude-3-7-sonnet-20250219",
  max_tokens: 1024,
  tools: [{
    name: "get_weather",
    description: "Get the current weather in a given location",
    input_schema: {
      type: "object",
      properties: {
        location: {
          type: "string",
          description: "The city and state, e.g. San Francisco, CA"
        }
      },
      required: ["location"]
    }
  }],
  messages: [{
    role: "user",
    content: "Tell me the weather in San Francisco."
  }],
  betas: ["token-efficient-tools-2025-02-19"]
});

console.log(message.usage);
```

```Java Java theme={null}
import java.util.List;
import java.util.Map;

import com.anthropic.client.AnthropicClient;
import com.anthropic.client.okhttp.AnthropicOkHttpClient;
import com.anthropic.core.JsonValue;
import com.anthropic.models.beta.messages.BetaMessage;
import com.anthropic.models.beta.messages.BetaTool;
import com.anthropic.models.beta.messages.MessageCreateParams;

import static com.anthropic.models.beta.AnthropicBeta.TOKEN_EFFICIENT_TOOLS_2025_02_19;

public class TokenEfficientToolsExample {

    public static void main(String[] args) {
        AnthropicClient client = AnthropicOkHttpClient.fromEnv();

        BetaTool.InputSchema schema = BetaTool.InputSchema.builder()
                .properties(JsonValue.from(Map.of(
                        "location",
                        Map.of(
                                "type", "string",
                                "description", "The city and state, e.g. San Francisco, CA"
                        )
                )))
                .putAdditionalProperty("required", JsonValue.from(List.of("location")))
                .build();

        MessageCreateParams params = MessageCreateParams.builder()
                .model("claude-3-7-sonnet-20250219")
                .maxTokens(1024)
                .betas(List.of(TOKEN_EFFICIENT_TOOLS_2025_02_19))
                .addTool(BetaTool.builder()
                        .name("get_weather")
                        .description("Get the current weather in a given location")
                        .inputSchema(schema)
                        .build())
                .addUserMessage("Tell me the weather in San Francisco.")
                .build();

        BetaMessage message = client.beta().messages().create(params);
        System.out.println(message.usage());
    }
}
```

</CodeGroup>

The above request should, on average, use fewer input and output tokens than a normal request. To confirm this, try making the same request but remove `token-efficient-tools-2025-02-19` from the beta headers list.

<Tip>
  To keep the benefits of prompt caching, use the beta header consistently for requests you'd like to cache. If you selectively use it, prompt caching will fail.
</Tip>

---

## 14. How to implement tool use

**URL:** https://docs.claude.com/en/docs/agents-and-tools/tool-use/implement-tool-use

# How to implement tool use

## Choosing a model

We recommend using the latest Claude Sonnet (4.5) or Claude Opus (4.1) model for complex tools and ambiguous queries; they handle multiple tools better and seek clarification when needed.

Use Claude Haiku models for straightforward tools, but note they may infer missing parameters.

<Tip>
  If using Claude with tool use and extended thinking, refer to our guide [here](/en/docs/build-with-claude/extended-thinking) for more information.
</Tip>

## Specifying client tools

Client tools (both Anthropic-defined and user-defined) are specified in the `tools` top-level parameter of the API request. Each tool definition includes:

| Parameter      | Description                                                                                         |
| :------------- | :-------------------------------------------------------------------------------------------------- |
| `name`         | The name of the tool. Must match the regex `^[a-zA-Z0-9_-]{1,64}$`.                                 |
| `description`  | A detailed plaintext description of what the tool does, when it should be used, and how it behaves. |
| `input_schema` | A [JSON Schema](https://json-schema.org/) object defining the expected parameters for the tool.     |

<Accordion title="Example simple tool definition">
  ```JSON JSON theme={null}
  {
    "name": "get_weather",
    "description": "Get the current weather in a given location",
    "input_schema": {
      "type": "object",
      "properties": {
        "location": {
          "type": "string",
          "description": "The city and state, e.g. San Francisco, CA"
        },
        "unit": {
          "type": "string",
          "enum": ["celsius", "fahrenheit"],
          "description": "The unit of temperature, either 'celsius' or 'fahrenheit'"
        }
      },
      "required": ["location"]
    }
  }
  ```

This tool, named `get_weather`, expects an input object with a required `location` string and an optional `unit` string that must be either "celsius" or "fahrenheit".
</Accordion>

### Tool use system prompt

When you call the Claude API with the `tools` parameter, we construct a special system prompt from the tool definitions, tool configuration, and any user-specified system prompt. The constructed prompt is designed to instruct the model to use the specified tool(s) and provide the necessary context for the tool to operate properly:

```
In this environment you have access to a set of tools you can use to answer the user's question.
{{ FORMATTING INSTRUCTIONS }}
String and scalar parameters should be specified as is, while lists and objects should use JSON format. Note that spaces for string values are not stripped. The output is not expected to be valid XML and is parsed with regular expressions.
Here are the functions available in JSONSchema format:
{{ TOOL DEFINITIONS IN JSON SCHEMA }}
{{ USER SYSTEM PROMPT }}
{{ TOOL CONFIGURATION }}
```

### Best practices for tool definitions

To get the best performance out of Claude when using tools, follow these guidelines:

- **Provide extremely detailed descriptions.** This is by far the most important factor in tool performance. Your descriptions should explain every detail about the tool, including:
  - What the tool does
  - When it should be used (and when it shouldn't)
  - What each parameter means and how it affects the tool's behavior
  - Any important caveats or limitations, such as what information the tool does not return if the tool name is unclear. The more context you can give Claude about your tools, the better it will be at deciding when and how to use them. Aim for at least 3-4 sentences per tool description, more if the tool is complex.
- **Prioritize descriptions over examples.** While you can include examples of how to use a tool in its description or in the accompanying prompt, this is less important than having a clear and comprehensive explanation of the tool's purpose and parameters. Only add examples after you've fully fleshed out the description.

<AccordionGroup>
  <Accordion title="Example of a good tool description">
    ```JSON JSON theme={null}
    {
      "name": "get_stock_price",
      "description": "Retrieves the current stock price for a given ticker symbol. The ticker symbol must be a valid symbol for a publicly traded company on a major US stock exchange like NYSE or NASDAQ. The tool will return the latest trade price in USD. It should be used when the user asks about the current or most recent price of a specific stock. It will not provide any other information about the stock or company.",
      "input_schema": {
        "type": "object",
        "properties": {
          "ticker": {
            "type": "string",
            "description": "The stock ticker symbol, e.g. AAPL for Apple Inc."
          }
        },
        "required": ["ticker"]
      }
    }
    ```
  </Accordion>

  <Accordion title="Example poor tool description">
    ```JSON JSON theme={null}
    {
      "name": "get_stock_price",
      "description": "Gets the stock price for a ticker.",
      "input_schema": {
        "type": "object",
        "properties": {
          "ticker": {
            "type": "string"
          }
        },
        "required": ["ticker"]
      }
    }
    ```
  </Accordion>
</AccordionGroup>

The good description clearly explains what the tool does, when to use it, what data it returns, and what the `ticker` parameter means. The poor description is too brief and leaves Claude with many open questions about the tool's behavior and usage.

## Tool runner (beta)

The tool runner provides an out-of-the-box solution for executing tools with Claude. Instead of manually handling tool calls, tool results, and conversation management, the tool runner automatically:

- Executes tools when Claude calls them
- Handles the request/response cycle
- Manages conversation state
- Provides type safety and validation

We recommend that you use the tool runner for most tool use implementations.

<Note>
  The tool runner is currently in beta and only available in the [Python](https://github.com/anthropics/anthropic-sdk-python/blob/main/tools.md) and [TypeScript](https://github.com/anthropics/anthropic-sdk-typescript/blob/main/helpers.md#tool-helpers) SDKs.
</Note>

<Tabs>
  <Tab title="Python">
    ### Basic usage

    Use the `@beta_tool` decorator to define tools and `client.beta.messages.tool_runner()` to execute them.

    <Note>
      If you're using the async client, replace `@beta_tool` with `@beta_async_tool` and define the function with `async def`.
    </Note>

    ```python  theme={null}
    import anthropic
    import json
    from anthropic import beta_tool

    # Initialize client
    client = anthropic.Anthropic()

    # Define tools using the decorator
    @beta_tool
    def get_weather(location: str, unit: str = "fahrenheit") -> str:
        """Get the current weather in a given location.

        Args:
            location: The city and state, e.g. San Francisco, CA
            unit: Temperature unit, either 'celsius' or 'fahrenheit'
        """
        # In a full implementation, you'd call a weather API here
        return json.dumps({"temperature": "20°C", "condition": "Sunny"})

    @beta_tool
    def calculate_sum(a: int, b: int) -> str:
        """Add two numbers together.

        Args:
            a: First number
            b: Second number
        """
        return str(a + b)

    # Use the tool runner
    runner = client.beta.messages.tool_runner(
        model="claude-sonnet-4-5",
        max_tokens=1024,
        tools=[get_weather, calculate_sum],
        messages=[
            {"role": "user", "content": "What's the weather like in Paris? Also, what's 15 + 27?"}
        ]
    )
    for message in runner:
        print(message.content[0].text)
    ```

    The decorated function must return a content block or content block array, including text, images, or document blocks. This allows tools to return rich, multimodal responses. Returned strings will be converted to a text content block.
    If you want to return a structured JSON object to Claude, encode it to a JSON string before returning it. Numbers, booleans or other non-string primitives also must be converted to strings.

    The `@beta_tool` decorator will inspect the function arguments and the docstring to extract a json schema representation of the given function, in the example above `calculate_sum` will be turned into:

    ```json  theme={null}
    {
      "name": "calculate_sum",
      "description": "Adds two integers together.",
      "input_schema": {
        "additionalProperties": false,
        "properties": {
          "left": {
            "description": "The first integer to add.",
            "title": "Left",
            "type": "integer"
          },
          "right": {
            "description": "The second integer to add.",
            "title": "Right",
            "type": "integer"
          }
        },
        "required": ["left", "right"],
        "type": "object"
      }
    }
    ```

    ### Iterating over the tool runner

    The tool runner returned by `tool_runner()` is an iterable, which you can iterate over with a `for` loop. This is often referred to as a "tool call loop".
    Each loop iteration yields a message that was returned by Claude.

    After your code has a chance to process the current message inside the loop, the tool runner will check the message to see if Claude requested a tool use. If so, it will call the tool and send the tool result back to Claude automatically, then yield the next message from Claude to start the next iteration of your loop.

    You may end the loop at any iteration with a simple `break` statement. The tool runner will loop until Claude returns a message without a tool use.

    If you don't care about intermediate messages, instead of using a loop, you can call the `until_done()` method, which will return the final message from Claude:

    ```python  theme={null}
    runner = client.beta.messages.tool_runner(
        model="claude-sonnet-4-5",
        max_tokens=1024,
        tools=[get_weather, calculate_sum],
        messages=[
            {"role": "user", "content": "What's the weather like in Paris? Also, what's 15 + 27?"}
        ]
    )
    final_message = runner.until_done()
    print(final_message.content[0].text)
    ```

    ### Advanced usage

    Within the loop, you have the ability to fully customize the tool runner's next request to the Messages API.
    The method `runner.generate_tool_call_response()` will call the tool (if Claude triggered a tool use) and give you access to the tool result that will be sent back to the Messages API.
    The methods `runner.set_messages_params()` and `runner.append_messages()` allow you to modify the parameters for the next Messages API request.

    ```python  theme={null}
    runner = client.beta.messages.tool_runner(
        model="claude-sonnet-4-5",
        max_tokens=1024,
        tools=[get_weather],
        messages=[{"role": "user", "content": "What's the weather in San Francisco?"}]
    )
    for message in runner:
        # Get the tool response that will be sent
        tool_response = runner.generate_tool_call_response()

        # Customize the next request
        runner.set_messages_params(lambda params: {
            **params,
            "max_tokens": 2048  # Increase tokens for next request
        })

        # Or add additional messages
        runner.append_messages(
            {"role": "user", "content": "Please be concise in your response."}
        )
    ```

    ### Streaming

    When enabling streaming with `stream=True`, each value emitted by the tool runner is a `BetaMessageStream` as returned from `anthropic.messages.stream()`. The `BetaMessageStream` is itself an iterable that yields streaming events from the Messages API.

    You can use `message_stream.get_final_message()` to let the SDK do the accumulation of streaming events into the final message for you.

    ```python  theme={null}
    runner = client.beta.messages.tool_runner(
        model="claude-sonnet-4-5",
        max_tokens=1024,
        tools=[calculate_sum],
        messages=[{"role": "user", "content": "What is 15 + 27?"}],
        stream=True
    )

    # When streaming, the runner returns BetaMessageStream
    for message_stream in runner:
        for event in message_stream:
            print('event:', event)
        print('message:', message_stream.get_final_message())

    print(runner.until_done())
    ```

  </Tab>

  <Tab title="TypeScript (Zod)">
    ### Basic usage

    Use `betaZodTool()` for type-safe tool definitions with Zod validation (requires Zod 3.25.0 or higher).

    ```typescript  theme={null}
    import { Anthropic } from '@anthropic-ai/sdk';
    import { betaZodTool, betaTool } from '@anthropic-ai/sdk/helpers/beta/zod';
    import { z } from 'zod';

    const anthropic = new Anthropic();

    // Using betaZodTool (requires Zod 3.25.0+)
    const getWeatherTool = betaZodTool({
      name: 'get_weather',
      description: 'Get the current weather in a given location',
      inputSchema: z.object({
        location: z.string().describe('The city and state, e.g. San Francisco, CA'),
        unit: z.enum(['celsius', 'fahrenheit']).default('fahrenheit')
          .describe('Temperature unit')
      }),
      run: async (input) => {
        // In a full implementation, you'd call a weather API here
        return JSON.stringify({temperature: '20°C', condition: 'Sunny'});
      }
    });

    // Use the tool runner
    const runner = anthropic.beta.messages.toolRunner({
      model: 'claude-sonnet-4-5',
      max_tokens: 1024,
      tools: [getWeatherTool],
      messages: [
        {
          role: 'user',
          content: "What's the weather like in Paris?"
        }
      ]
    });

    // Process messages as they come in
    for await (const message of runner) {
      console.log(message.content[0].text);
    }
    ```

    The `run` function must return a content block or content block array, including text, images, or document blocks. This allows tools to return rich, multimodal responses. Returned strings will be converted to a text content block.
    If you want to return a structured JSON object to Claude, stringify it to a JSON string before returning it. Numbers, booleans or other non-string primitives also must be converted to strings.

    ### Iterating over the tool runner

    The tool runner returned by `toolRunner()` is an async iterable, which you can iterate over with a `for await ... of` loop. This is often referred to as a "tool call loop".
    Each loop iteration yields a messages that was returned by Claude.

    After your code had a chance to process the current message inside the loop, the tool runner will check the message to see if Claude requested a tool use. If so, it will call the tool and send the tool result back to Claude automatically, then yield the next message from Claude to start the next iteration of your loop.

    You may end the loop at any iteration with a simple `break` statement. The tool runner will loop until Claude returns a message without a tool use.

    If you don't care about intermediate messages, instead of using a loop, you may simply `await` the tool runner, which will return the final message from Claude.

    ### Advanced usage

    Within the loop, you have the ability to fully customize the tool runner's next request to the Messages API.
    The method `runner.generateToolResponse()` will call the tool (if Claude triggered a tool use) and give you access to the tool result that will be sent back to the Messages API.
    The methods `runner.setMessagesParams()` and `runner.pushMessages()` allow you to modify the parameters for the next Messages API request. The current parameters are available under `runner.params`.

    ```typescript  theme={null}
    const runner = anthropic.beta.messages.toolRunner({
      model: 'claude-sonnet-4-5',
      max_tokens: 1024,
      tools: [getWeatherTool],
      messages: [
        { role: 'user', content: "What's the weather in San Francisco?" }
      ]
    });

    for await (const message of runner) {
      // Get the tool response that will be sent
      const toolResponse = await runner.generateToolResponse();

      // Customize the next request
      runner.setMessagesParams(params => ({
        ...params,
        max_tokens: 2048  // Increase tokens for next request
      }));

      // Or add additional messages
      runner.pushMessages(
        { role: 'user', content: 'Please be concise in your response.' }
      );
    }
    ```

    ### Streaming

    When enabling streaming with `stream: true`, each value emitted by the tool runner is a `MessageStream` as returned from `anthropic.messages.stream()`. The `MessageStream` is itself an async iterable that yields streaming events from the Messages API.

    You can use `messageStream.finalMessage()` to let the SDK do the accumulation of streaming events into the final message for you.

    ```typescript  theme={null}
    const runner = anthropic.beta.messages.toolRunner({
      model: 'claude-sonnet-4-5-20250929',
      max_tokens: 1000,
      messages: [{ role: 'user', content: 'What is the weather in San Francisco?' }],
      tools: [calculatorTool],
      stream: true,
    });

    // When streaming, the runner returns BetaMessageStream
    for await (const messageStream of runner) {
      for await (const event of messageStream) {
        console.log('event:', event);
      }
      console.log('message:', await messageStream.finalMessage());
    }

    console.log(await runner);
    ```

  </Tab>

  <Tab title="TypeScript (JSON Schema)">
    ### Basic usage

    Use `betaTool()` for type-safe tool definitions based on JSON schemas. TypeScript and your editor will be aware of the type of the `input` parameter for autocompletion.

    <Note>
      The input generated by Claude will not be validated at runtime. Perform validation inside the `run` function if needed.
    </Note>

    ```typescript  theme={null}
    import { Anthropic } from '@anthropic-ai/sdk';
    import { betaZodTool, betaTool } from '@anthropic-ai/sdk/helpers/beta/json-schema';
    import { z } from 'zod';

    const anthropic = new Anthropic();

    // Using betaTool with JSON schema (no Zod required)
    const calculateSumTool = betaTool({
      name: 'calculate_sum',
      description: 'Add two numbers together',
      inputSchema: {
        type: 'object',
        properties: {
          a: { type: 'number', description: 'First number' },
          b: { type: 'number', description: 'Second number' }
        },
        required: ['a', 'b']
      },
      run: async (input) => {
        return String(input.a + input.b);
      }
    });

    // Use the tool runner
    const runner = anthropic.beta.messages.toolRunner({
      model: 'claude-sonnet-4-5',
      max_tokens: 1024,
      tools: [getWeatherTool, calculateSumTool],
      messages: [
        {
          role: 'user',
          content: "What's 15 + 27?"
        }
      ]
    });

    // Process messages as they come in
    for await (const message of runner) {
      console.log(message.content[0].text);
    }
    ```

    The `run` function must return any content block or content block array, including text, image, or document blocks. This allows tools to return rich, multimodal responses. Returned strings will be converted to a text content block.
    If you want to return a structured JSON object to Claude, encode it to a JSON string before returning it. Numbers, booleans or other non-string primitives also must be converted to strings.

    ### Iterating over the tool runner

    The tool runner returned by `toolRunner()` is an async iterable, which you can iterate over with a `for await ... of` loop. This is often referred to as a "tool call loop".
    Each loop iteration yields a messages that was returned by Claude.

    After your code had a chance to process the current message inside the loop, the tool runner will check the message to see if Claude requested a tool use. If so, it will call the tool and send the tool result back to Claude automatically, then yield the next message from Claude to start the next iteration of your loop.

    You may end the loop at any iteration with a simple `break` statement. The tool runner will loop until Claude returns a message without a tool use.

    If you don't care about intermediate messages, instead of using a loop, you may simply `await` the tool runner, which will return the final message from Claude.

    ### Advanced usage

    Within the loop, you have the ability to fully customize the tool runner's next request to the Messages API.
    The method `runner.generateToolResponse()` will call the tool (if Claude triggered a tool use) and give you access to the tool result that will be sent back to the Messages API.
    The methods `runner.setMessagesParams()` and `runner.pushMessages()` allow you to modify the parameters for the next Messages API request. The current parameters are available under `runner.params`.

    ```typescript  theme={null}
    const runner = anthropic.beta.messages.toolRunner({
      model: 'claude-sonnet-4-5',
      max_tokens: 1024,
      tools: [getWeatherTool],
      messages: [
        { role: 'user', content: "What's the weather in San Francisco?" }
      ]
    });

    for await (const message of runner) {
      // Get the tool response that will be sent
      const toolResponse = await runner.generateToolResponse();

      // Customize the next request
      runner.setMessagesParams(params => ({
        ...params,
        max_tokens: 2048  // Increase tokens for next request
      }));

      // Or add additional messages
      runner.pushMessages(
        { role: 'user', content: 'Please be concise in your response.' }
      );
    }
    ```

    ### Streaming

    When enabling streaming with `stream: true`, each value emitted by the tool runner is a `MessageStream` as returned from `anthropic.messages.stream()`. The `MessageStream` is itself an async iterable that yields streaming events from the Messages API.

    You can use `messageStream.finalMessage()` to let the SDK do the accumulation of streaming events into the final message for you.

    ```typescript  theme={null}
    const runner = anthropic.beta.messages.toolRunner({
      model: 'claude-sonnet-4-5-20250929',
      max_tokens: 1000,
      messages: [{ role: 'user', content: 'What is the weather in San Francisco?' }],
      tools: [calculatorTool],
      stream: true,
    });

    // When streaming, the runner returns BetaMessageStream
    for await (const messageStream of runner) {
      for await (const event of messageStream) {
        console.log('event:', event);
      }
      console.log('message:', await messageStream.finalMessage());
    }

    console.log(await runner);
    ```

  </Tab>
</Tabs>

<Note>
  The SDK tool runner is in beta. The rest of this document covers manual tool implementation.
</Note>

## Controlling Claude's output

### Forcing tool use

In some cases, you may want Claude to use a specific tool to answer the user's question, even if Claude thinks it can provide an answer without using a tool. You can do this by specifying the tool in the `tool_choice` field like so:

```
tool_choice = {"type": "tool", "name": "get_weather"}
```

When working with the tool_choice parameter, we have four possible options:

- `auto` allows Claude to decide whether to call any provided tools or not. This is the default value when `tools` are provided.
- `any` tells Claude that it must use one of the provided tools, but doesn't force a particular tool.
- `tool` allows us to force Claude to always use a particular tool.
- `none` prevents Claude from using any tools. This is the default value when no `tools` are provided.

<Note>
  When using [prompt caching](/en/docs/build-with-claude/prompt-caching#what-invalidates-the-cache), changes to the `tool_choice` parameter will invalidate cached message blocks. Tool definitions and system prompts remain cached, but message content must be reprocessed.
</Note>

This diagram illustrates how each option works:

<Frame>
  <img src="https://mintcdn.com/anthropic-claude-docs/LF5WV0SNF6oudpT5/images/tool_choice.png?fit=max&auto=format&n=LF5WV0SNF6oudpT5&q=85&s=fb88b9fa0da23fc231e4fece253f4406" data-og-width="1920" width="1920" data-og-height="1080" height="1080" data-path="images/tool_choice.png" data-optimize="true" data-opv="3" srcset="https://mintcdn.com/anthropic-claude-docs/LF5WV0SNF6oudpT5/images/tool_choice.png?w=280&fit=max&auto=format&n=LF5WV0SNF6oudpT5&q=85&s=11a4cfd7ab7815ea14c21e0948d060d4 280w, https://mintcdn.com/anthropic-claude-docs/LF5WV0SNF6oudpT5/images/tool_choice.png?w=560&fit=max&auto=format&n=LF5WV0SNF6oudpT5&q=85&s=c889c279adce34f1fa479bc722b3fe6f 560w, https://mintcdn.com/anthropic-claude-docs/LF5WV0SNF6oudpT5/images/tool_choice.png?w=840&fit=max&auto=format&n=LF5WV0SNF6oudpT5&q=85&s=e16651305d256ded74250f1c0dadb622 840w, https://mintcdn.com/anthropic-claude-docs/LF5WV0SNF6oudpT5/images/tool_choice.png?w=1100&fit=max&auto=format&n=LF5WV0SNF6oudpT5&q=85&s=a99b0dd3b603051efdf9536ba9307a34 1100w, https://mintcdn.com/anthropic-claude-docs/LF5WV0SNF6oudpT5/images/tool_choice.png?w=1650&fit=max&auto=format&n=LF5WV0SNF6oudpT5&q=85&s=5045888f298f7261d3ae2e1466e54027 1650w, https://mintcdn.com/anthropic-claude-docs/LF5WV0SNF6oudpT5/images/tool_choice.png?w=2500&fit=max&auto=format&n=LF5WV0SNF6oudpT5&q=85&s=8a9c615a15610a949a2dad3aaa8113b8 2500w" />
</Frame>

Note that when you have `tool_choice` as `any` or `tool`, we will prefill the assistant message to force a tool to be used. This means that the models will not emit a natural language response or explanation before `tool_use` content blocks, even if explicitly asked to do so.

<Note>
  When using [extended thinking](/en/docs/build-with-claude/extended-thinking) with tool use, `tool_choice: {"type": "any"}` and `tool_choice: {"type": "tool", "name": "..."}` are not supported and will result in an error. Only `tool_choice: {"type": "auto"}` (the default) and `tool_choice: {"type": "none"}` are compatible with extended thinking.
</Note>

Our testing has shown that this should not reduce performance. If you would like the model to provide natural language context or explanations while still requesting that the model use a specific tool, you can use `{"type": "auto"}` for `tool_choice` (the default) and add explicit instructions in a `user` message. For example: `What's the weather like in London? Use the get_weather tool in your response.`

### JSON output

Tools do not necessarily need to be client functions — you can use tools anytime you want the model to return JSON output that follows a provided schema. For example, you might use a `record_summary` tool with a particular schema. See [Tool use with Claude](/en/docs/agents-and-tools/tool-use/overview) for a full working example.

### Model responses with tools

When using tools, Claude will often comment on what it's doing or respond naturally to the user before invoking tools.

For example, given the prompt "What's the weather like in San Francisco right now, and what time is it there?", Claude might respond with:

```JSON JSON theme={null}
{
  "role": "assistant",
  "content": [
    {
      "type": "text",
      "text": "I'll help you check the current weather and time in San Francisco."
    },
    {
      "type": "tool_use",
      "id": "toolu_01A09q90qw90lq917835lq9",
      "name": "get_weather",
      "input": {"location": "San Francisco, CA"}
    }
  ]
}
```

This natural response style helps users understand what Claude is doing and creates a more conversational interaction. You can guide the style and content of these responses through your system prompts and by providing `<examples>` in your prompts.

It's important to note that Claude may use various phrasings and approaches when explaining its actions. Your code should treat these responses like any other assistant-generated text, and not rely on specific formatting conventions.

### Parallel tool use

By default, Claude may use multiple tools to answer a user query. You can disable this behavior by:

- Setting `disable_parallel_tool_use=true` when tool_choice type is `auto`, which ensures that Claude uses **at most one** tool
- Setting `disable_parallel_tool_use=true` when tool_choice type is `any` or `tool`, which ensures that Claude uses **exactly one** tool

<AccordionGroup>
  <Accordion title="Complete parallel tool use example">
    <Note>
      **Simpler with Tool runner**: The example below shows manual parallel tool handling. For most use cases, [tool runner](#tool-runner-beta) automatically handle parallel tool execution with much less code.
    </Note>

    Here's a complete example showing how to properly format parallel tool calls in the message history:

    <CodeGroup>
      ```python Python theme={null}
      import anthropic

      client = anthropic.Anthropic()

      # Define tools
      tools = [
          {
              "name": "get_weather",
              "description": "Get the current weather in a given location",
              "input_schema": {
                  "type": "object",
                  "properties": {
                      "location": {
                          "type": "string",
                          "description": "The city and state, e.g. San Francisco, CA"
                      }
                  },
                  "required": ["location"]
              }
          },
          {
              "name": "get_time",
              "description": "Get the current time in a given timezone",
              "input_schema": {
                  "type": "object",
                  "properties": {
                      "timezone": {
                          "type": "string",
                          "description": "The timezone, e.g. America/New_York"
                      }
                  },
                  "required": ["timezone"]
              }
          }
      ]

      # Initial request
      response = client.messages.create(
          model="claude-sonnet-4-5",
          max_tokens=1024,
          tools=tools,
          messages=[
              {
                  "role": "user",
                  "content": "What's the weather in SF and NYC, and what time is it there?"
              }
          ]
      )

      # Claude's response with parallel tool calls
      print("Claude wants to use tools:", response.stop_reason == "tool_use")
      print("Number of tool calls:", len([c for c in response.content if c.type == "tool_use"]))

      # Build the conversation with tool results
      messages = [
          {
              "role": "user",
              "content": "What's the weather in SF and NYC, and what time is it there?"
          },
          {
              "role": "assistant",
              "content": response.content  # Contains multiple tool_use blocks
          },
          {
              "role": "user",
              "content": [
                  {
                      "type": "tool_result",
                      "tool_use_id": "toolu_01",  # Must match the ID from tool_use
                      "content": "San Francisco: 68°F, partly cloudy"
                  },
                  {
                      "type": "tool_result",
                      "tool_use_id": "toolu_02",
                      "content": "New York: 45°F, clear skies"
                  },
                  {
                      "type": "tool_result",
                      "tool_use_id": "toolu_03",
                      "content": "San Francisco time: 2:30 PM PST"
                  },
                  {
                      "type": "tool_result",
                      "tool_use_id": "toolu_04",
                      "content": "New York time: 5:30 PM EST"
                  }
              ]
          }
      ]

      # Get final response
      final_response = client.messages.create(
          model="claude-sonnet-4-5",
          max_tokens=1024,
          tools=tools,
          messages=messages
      )

      print(final_response.content[0].text)
      ```

      ```typescript TypeScript theme={null}
      import { Anthropic } from '@anthropic-ai/sdk';

      const anthropic = new Anthropic();

      // Define tools
      const tools = [
        {
          name: "get_weather",
          description: "Get the current weather in a given location",
          input_schema: {
            type: "object",
            properties: {
              location: {
                type: "string",
                description: "The city and state, e.g. San Francisco, CA"
              }
            },
            required: ["location"]
          }
        },
        {
          name: "get_time",
          description: "Get the current time in a given timezone",
          input_schema: {
            type: "object",
            properties: {
              timezone: {
                type: "string",
                description: "The timezone, e.g. America/New_York"
              }
            },
            required: ["timezone"]
          }
        }
      ];

      // Initial request
      const response = await anthropic.messages.create({
        model: "claude-sonnet-4-5",
        max_tokens: 1024,
        tools: tools,
        messages: [
          {
            role: "user",
            content: "What's the weather in SF and NYC, and what time is it there?"
          }
        ]
      });

      // Build conversation with tool results
      const messages = [
        {
          role: "user",
          content: "What's the weather in SF and NYC, and what time is it there?"
        },
        {
          role: "assistant",
          content: response.content  // Contains multiple tool_use blocks
        },
        {
          role: "user",
          content: [
            {
              type: "tool_result",
              tool_use_id: "toolu_01",  // Must match the ID from tool_use
              content: "San Francisco: 68°F, partly cloudy"
            },
            {
              type: "tool_result",
              tool_use_id: "toolu_02",
              content: "New York: 45°F, clear skies"
            },
            {
              type: "tool_result",
              tool_use_id: "toolu_03",
              content: "San Francisco time: 2:30 PM PST"
            },
            {
              type: "tool_result",
              tool_use_id: "toolu_04",
              content: "New York time: 5:30 PM EST"
            }
          ]
        }
      ];

      // Get final response
      const finalResponse = await anthropic.messages.create({
        model: "claude-sonnet-4-5",
        max_tokens: 1024,
        tools: tools,
        messages: messages
      });

      console.log(finalResponse.content[0].text);
      ```
    </CodeGroup>

    The assistant message with parallel tool calls would look like this:

    ```json  theme={null}
    {
      "role": "assistant",
      "content": [
        {
          "type": "text",
          "text": "I'll check the weather and time for both San Francisco and New York City."
        },
        {
          "type": "tool_use",
          "id": "toolu_01",
          "name": "get_weather",
          "input": {"location": "San Francisco, CA"}
        },
        {
          "type": "tool_use",
          "id": "toolu_02",
          "name": "get_weather",
          "input": {"location": "New York, NY"}
        },
        {
          "type": "tool_use",
          "id": "toolu_03",
          "name": "get_time",
          "input": {"timezone": "America/Los_Angeles"}
        },
        {
          "type": "tool_use",
          "id": "toolu_04",
          "name": "get_time",
          "input": {"timezone": "America/New_York"}
        }
      ]
    }
    ```

  </Accordion>

  <Accordion title="Complete test script for parallel tools">
    Here's a complete, runnable script to test and verify parallel tool calls are working correctly:

    <CodeGroup>
      ```python Python theme={null}
      #!/usr/bin/env python3
      """Test script to verify parallel tool calls with the Claude API"""

      import os
      from anthropic import Anthropic

      # Initialize client
      client = Anthropic(api_key=os.environ.get("ANTHROPIC_API_KEY"))

      # Define tools
      tools = [
          {
              "name": "get_weather",
              "description": "Get the current weather in a given location",
              "input_schema": {
                  "type": "object",
                  "properties": {
                      "location": {
                          "type": "string",
                          "description": "The city and state, e.g. San Francisco, CA"
                      }
                  },
                  "required": ["location"]
              }
          },
          {
              "name": "get_time",
              "description": "Get the current time in a given timezone",
              "input_schema": {
                  "type": "object",
                  "properties": {
                      "timezone": {
                          "type": "string",
                          "description": "The timezone, e.g. America/New_York"
                      }
                  },
                  "required": ["timezone"]
              }
          }
      ]

      # Test conversation with parallel tool calls
      messages = [
          {
              "role": "user",
              "content": "What's the weather in SF and NYC, and what time is it there?"
          }
      ]

      # Make initial request
      print("Requesting parallel tool calls...")
      response = client.messages.create(
          model="claude-sonnet-4-5",
          max_tokens=1024,
          messages=messages,
          tools=tools
      )

      # Check for parallel tool calls
      tool_uses = [block for block in response.content if block.type == "tool_use"]
      print(f"\n✓ Claude made {len(tool_uses)} tool calls")

      if len(tool_uses) > 1:
          print("✓ Parallel tool calls detected!")
          for tool in tool_uses:
              print(f"  - {tool.name}: {tool.input}")
      else:
          print("✗ No parallel tool calls detected")

      # Simulate tool execution and format results correctly
      tool_results = []
      for tool_use in tool_uses:
          if tool_use.name == "get_weather":
              if "San Francisco" in str(tool_use.input):
                  result = "San Francisco: 68°F, partly cloudy"
              else:
                  result = "New York: 45°F, clear skies"
          else:  # get_time
              if "Los_Angeles" in str(tool_use.input):
                  result = "2:30 PM PST"
              else:
                  result = "5:30 PM EST"

          tool_results.append({
              "type": "tool_result",
              "tool_use_id": tool_use.id,
              "content": result
          })

      # Continue conversation with tool results
      messages.extend([
          {"role": "assistant", "content": response.content},
          {"role": "user", "content": tool_results}  # All results in one message!
      ])

      # Get final response
      print("\nGetting final response...")
      final_response = client.messages.create(
          model="claude-sonnet-4-5",
          max_tokens=1024,
          messages=messages,
          tools=tools
      )

      print(f"\nClaude's response:\n{final_response.content[0].text}")

      # Verify formatting
      print("\n--- Verification ---")
      print(f"✓ Tool results sent in single user message: {len(tool_results)} results")
      print("✓ No text before tool results in content array")
      print("✓ Conversation formatted correctly for future parallel tool use")
      ```

      ```typescript TypeScript theme={null}
      #!/usr/bin/env node
      // Test script to verify parallel tool calls with the Claude API

      import { Anthropic } from '@anthropic-ai/sdk';

      const anthropic = new Anthropic({
        apiKey: process.env.ANTHROPIC_API_KEY
      });

      // Define tools
      const tools = [
        {
          name: "get_weather",
          description: "Get the current weather in a given location",
          input_schema: {
            type: "object",
            properties: {
              location: {
                type: "string",
                description: "The city and state, e.g. San Francisco, CA"
              }
            },
            required: ["location"]
          }
        },
        {
          name: "get_time",
          description: "Get the current time in a given timezone",
          input_schema: {
            type: "object",
            properties: {
              timezone: {
                type: "string",
                description: "The timezone, e.g. America/New_York"
              }
            },
            required: ["timezone"]
          }
        }
      ];

      async function testParallelTools() {
        // Make initial request
        console.log("Requesting parallel tool calls...");
        const response = await anthropic.messages.create({
          model: "claude-sonnet-4-5",
          max_tokens: 1024,
          messages: [{
            role: "user",
            content: "What's the weather in SF and NYC, and what time is it there?"
          }],
          tools: tools
        });

        // Check for parallel tool calls
        const toolUses = response.content.filter(block => block.type === "tool_use");
        console.log(`\n✓ Claude made ${toolUses.length} tool calls`);

        if (toolUses.length > 1) {
          console.log("✓ Parallel tool calls detected!");
          toolUses.forEach(tool => {
            console.log(`  - ${tool.name}: ${JSON.stringify(tool.input)}`);
          });
        } else {
          console.log("✗ No parallel tool calls detected");
        }

        // Simulate tool execution and format results correctly
        const toolResults = toolUses.map(toolUse => {
          let result;
          if (toolUse.name === "get_weather") {
            result = toolUse.input.location.includes("San Francisco")
              ? "San Francisco: 68°F, partly cloudy"
              : "New York: 45°F, clear skies";
          } else {
            result = toolUse.input.timezone.includes("Los_Angeles")
              ? "2:30 PM PST"
              : "5:30 PM EST";
          }

          return {
            type: "tool_result",
            tool_use_id: toolUse.id,
            content: result
          };
        });

        // Get final response with correct formatting
        console.log("\nGetting final response...");
        const finalResponse = await anthropic.messages.create({
          model: "claude-sonnet-4-5",
          max_tokens: 1024,
          messages: [
            { role: "user", content: "What's the weather in SF and NYC, and what time is it there?" },
            { role: "assistant", content: response.content },
            { role: "user", content: toolResults }  // All results in one message!
          ],
          tools: tools
        });

        console.log(`\nClaude's response:\n${finalResponse.content[0].text}`);

        // Verify formatting
        console.log("\n--- Verification ---");
        console.log(`✓ Tool results sent in single user message: ${toolResults.length} results`);
        console.log("✓ No text before tool results in content array");
        console.log("✓ Conversation formatted correctly for future parallel tool use");
      }

      testParallelTools().catch(console.error);
      ```
    </CodeGroup>

    This script demonstrates:

    * How to properly format parallel tool calls and results
    * How to verify that parallel calls are being made
    * The correct message structure that encourages future parallel tool use
    * Common mistakes to avoid (like text before tool results)

    Run this script to test your implementation and ensure Claude is making parallel tool calls effectively.

  </Accordion>
</AccordionGroup>

#### Maximizing parallel tool use

While Claude 4 models have excellent parallel tool use capabilities by default, you can increase the likelihood of parallel tool execution across all models with targeted prompting:

<AccordionGroup>
  <Accordion title="System prompts for parallel tool use">
    For Claude 4 models (Opus 4.1, Opus 4, and Sonnet 4), add this to your system prompt:

    ```text  theme={null}
    For maximum efficiency, whenever you need to perform multiple independent operations, invoke all relevant tools simultaneously rather than sequentially.
    ```

    For even stronger parallel tool use (recommended if the default isn't sufficient), use:

    ```text  theme={null}
    <use_parallel_tool_calls>
    For maximum efficiency, whenever you perform multiple independent operations, invoke all relevant tools simultaneously rather than sequentially. Prioritize calling tools in parallel whenever possible. For example, when reading 3 files, run 3 tool calls in parallel to read all 3 files into context at the same time. When running multiple read-only commands like `ls` or `list_dir`, always run all of the commands in parallel. Err on the side of maximizing parallel tool calls rather than running too many tools sequentially.
    </use_parallel_tool_calls>
    ```

  </Accordion>

  <Accordion title="User message prompting">
    You can also encourage parallel tool use within specific user messages:

    ```python  theme={null}
    # Instead of:
    "What's the weather in Paris? Also check London."

    # Use:
    "Check the weather in Paris and London simultaneously."

    # Or be explicit:
    "Please use parallel tool calls to get the weather for Paris, London, and Tokyo at the same time."
    ```

  </Accordion>
</AccordionGroup>

<Warning>
  **Parallel tool use with Claude Sonnet 3.7**

Claude Sonnet 3.7 may be less likely to make make parallel tool calls in a response, even when you have not set `disable_parallel_tool_use`. To work around this, we recommend enabling [token-efficient tool use](/en/docs/agents-and-tools/tool-use/token-efficient-tool-use), which helps encourage Claude to use parallel tools. This beta feature also reduces latency and saves an average of 14% in output tokens.

If you prefer not to opt into the token-efficient tool use beta, you can also introduce a "batch tool" that can act as a meta-tool to wrap invocations to other tools simultaneously. We find that if this tool is present, the model will use it to simultaneously call multiple tools in parallel for you.

See [this example](https://github.com/anthropics/anthropic-cookbook/blob/main/tool_use/parallel_tools_claude_3_7_sonnet.ipynb) in our cookbook for how to use this workaround.
</Warning>

## Handling tool use and tool result content blocks

<Note>
  **Simpler with Tool runner**: The manual tool handling described in this section is automatically managed by [tool runner](#tool-runner-beta). Use this section when you need custom control over tool execution.
</Note>

Claude's response differs based on whether it uses a client or server tool.

### Handling results from client tools

The response will have a `stop_reason` of `tool_use` and one or more `tool_use` content blocks that include:

- `id`: A unique identifier for this particular tool use block. This will be used to match up the tool results later.
- `name`: The name of the tool being used.
- `input`: An object containing the input being passed to the tool, conforming to the tool's `input_schema`.

<Accordion title="Example API response with a `tool_use` content block">
  ```JSON JSON theme={null}
  {
    "id": "msg_01Aq9w938a90dw8q",
    "model": "claude-sonnet-4-5",
    "stop_reason": "tool_use",
    "role": "assistant",
    "content": [
      {
        "type": "text",
        "text": "I'll check the current weather in San Francisco for you."
      },
      {
        "type": "tool_use",
        "id": "toolu_01A09q90qw90lq917835lq9",
        "name": "get_weather",
        "input": {"location": "San Francisco, CA", "unit": "celsius"}
      }
    ]
  }
  ```
</Accordion>

When you receive a tool use response for a client tool, you should:

1. Extract the `name`, `id`, and `input` from the `tool_use` block.
2. Run the actual tool in your codebase corresponding to that tool name, passing in the tool `input`.
3. Continue the conversation by sending a new message with the `role` of `user`, and a `content` block containing the `tool_result` type and the following information:
   - `tool_use_id`: The `id` of the tool use request this is a result for.
   - `content`: The result of the tool, as a string (e.g. `"content": "15 degrees"`), a list of nested content blocks (e.g. `"content": [{"type": "text", "text": "15 degrees"}]`), or a list of document blocks (e.g. `"content": ["type": "document", "source": {"type": "text", "media_type": "text/plain", "data": "15 degrees"}]`). These content blocks can use the `text`, `image`, or `document` types.
   - `is_error` (optional): Set to `true` if the tool execution resulted in an error.

<Note>
  **Important formatting requirements**:

- Tool result blocks must immediately follow their corresponding tool use blocks in the message history. You cannot include any messages between the assistant's tool use message and the user's tool result message.
- In the user message containing tool results, the tool_result blocks must come FIRST in the content array. Any text must come AFTER all tool results.

For example, this will cause a 400 error:

```json theme={null}
{"role": "user", "content": [
  {"type": "text", "text": "Here are the results:"},  // ❌ Text before tool_result
  {"type": "tool_result", "tool_use_id": "toolu_01", ...}
]}
```

This is correct:

```json theme={null}
{"role": "user", "content": [
  {"type": "tool_result", "tool_use_id": "toolu_01", ...},
  {"type": "text", "text": "What should I do next?"}  // ✅ Text after tool_result
]}
```

If you receive an error like "tool_use ids were found without tool_result blocks immediately after", check that your tool results are formatted correctly.
</Note>

<AccordionGroup>
  <Accordion title="Example of successful tool result">
    ```JSON JSON theme={null}
    {
      "role": "user",
      "content": [
        {
          "type": "tool_result",
          "tool_use_id": "toolu_01A09q90qw90lq917835lq9",
          "content": "15 degrees"
        }
      ]
    }
    ```
  </Accordion>

  <Accordion title="Example of tool result with images">
    ```JSON JSON theme={null}
    {
      "role": "user",
      "content": [
        {
          "type": "tool_result",
          "tool_use_id": "toolu_01A09q90qw90lq917835lq9",
          "content": [
            {"type": "text", "text": "15 degrees"},
            {
              "type": "image",
              "source": {
                "type": "base64",
                "media_type": "image/jpeg",
                "data": "/9j/4AAQSkZJRg...",
              }
            }
          ]
        }
      ]
    }
    ```
  </Accordion>

  <Accordion title="Example of empty tool result">
    ```JSON JSON theme={null}
    {
      "role": "user",
      "content": [
        {
          "type": "tool_result",
          "tool_use_id": "toolu_01A09q90qw90lq917835lq9",
        }
      ]
    }
    ```
  </Accordion>

  <Accordion title="Example of tool result with documents">
    ```JSON JSON theme={null}
    {
      "role": "user",
      "content": [
        {
          "type": "tool_result",
          "tool_use_id": "toolu_01A09q90qw90lq917835lq9",
          "content": [
            {"type": "text", "text": "The weather is"},
            {
              "type": "document",
              "source": {
                "type": "text",
                "media_type": "text/plain",
                "data": "15 degrees"
              }
            }
          ]
        }
      ]
    }
    ```
  </Accordion>
</AccordionGroup>

After receiving the tool result, Claude will use that information to continue generating a response to the original user prompt.

### Handling results from server tools

Claude executes the tool internally and incorporates the results directly into its response without requiring additional user interaction.

<Tip>
  **Differences from other APIs**

Unlike APIs that separate tool use or use special roles like `tool` or `function`, the Claude API integrates tools directly into the `user` and `assistant` message structure.

Messages contain arrays of `text`, `image`, `tool_use`, and `tool_result` blocks. `user` messages include client content and `tool_result`, while `assistant` messages contain AI-generated content and `tool_use`.
</Tip>

### Handling the `max_tokens` stop reason

If Claude's [response is cut off due to hitting the `max_tokens` limit](/en/api/handling-stop-reasons#max-tokens), and the truncated response contains an incomplete tool use block, you'll need to retry the request with a higher `max_tokens` value to get the full tool use.

<CodeGroup>
  ```python Python theme={null}
  # Check if response was truncated during tool use
  if response.stop_reason == "max_tokens":
      # Check if the last content block is an incomplete tool_use
      last_block = response.content[-1]
      if last_block.type == "tool_use":
          # Send the request with higher max_tokens
          response = client.messages.create(
              model="claude-sonnet-4-5",
              max_tokens=4096,  # Increased limit
              messages=messages,
              tools=tools
          )
  ```

```typescript TypeScript theme={null}
// Check if response was truncated during tool use
if (response.stop_reason === "max_tokens") {
  // Check if the last content block is an incomplete tool_use
  const lastBlock = response.content[response.content.length - 1];
  if (lastBlock.type === "tool_use") {
    // Send the request with higher max_tokens
    response = await anthropic.messages.create({
      model: "claude-sonnet-4-5",
      max_tokens: 4096, // Increased limit
      messages: messages,
      tools: tools,
    });
  }
}
```

</CodeGroup>

#### Handling the `pause_turn` stop reason

When using server tools like web search, the API may return a `pause_turn` stop reason, indicating that the API has paused a long-running turn.

Here's how to handle the `pause_turn` stop reason:

<CodeGroup>
  ```python Python theme={null}
  import anthropic

client = anthropic.Anthropic()

# Initial request with web search

response = client.messages.create(
model="claude-3-7-sonnet-latest",
max_tokens=1024,
messages=[
{
"role": "user",
"content": "Search for comprehensive information about quantum computing breakthroughs in 2025"
}
],
tools=[{
"type": "web_search_20250305",
"name": "web_search",
"max_uses": 10
}]
)

# Check if the response has pause_turn stop reason

if response.stop_reason == "pause_turn": # Continue the conversation with the paused content
messages = [
{"role": "user", "content": "Search for comprehensive information about quantum computing breakthroughs in 2025"},
{"role": "assistant", "content": response.content}
]

      # Send the continuation request
      continuation = client.messages.create(
          model="claude-3-7-sonnet-latest",
          max_tokens=1024,
          messages=messages,
          tools=[{
              "type": "web_search_20250305",
              "name": "web_search",
              "max_uses": 10
          }]
      )

      print(continuation)

else:
print(response)

````

```typescript TypeScript theme={null}
import { Anthropic } from '@anthropic-ai/sdk';

const anthropic = new Anthropic();

// Initial request with web search
const response = await anthropic.messages.create({
  model: "claude-3-7-sonnet-latest",
  max_tokens: 1024,
  messages: [
    {
      role: "user",
      content: "Search for comprehensive information about quantum computing breakthroughs in 2025"
    }
  ],
  tools: [{
    type: "web_search_20250305",
    name: "web_search",
    max_uses: 10
  }]
});

// Check if the response has pause_turn stop reason
if (response.stop_reason === "pause_turn") {
  // Continue the conversation with the paused content
  const messages = [
    { role: "user", content: "Search for comprehensive information about quantum computing breakthroughs in 2025" },
    { role: "assistant", content: response.content }
  ];

  // Send the continuation request
  const continuation = await anthropic.messages.create({
    model: "claude-3-7-sonnet-latest",
    max_tokens: 1024,
    messages: messages,
    tools: [{
      type: "web_search_20250305",
      name: "web_search",
      max_uses: 10
    }]
  });

  console.log(continuation);
} else {
  console.log(response);
}
````

</CodeGroup>

When handling `pause_turn`:

- **Continue the conversation**: Pass the paused response back as-is in a subsequent request to let Claude continue its turn
- **Modify if needed**: You can optionally modify the content before continuing if you want to interrupt or redirect the conversation
- **Preserve tool state**: Include the same tools in the continuation request to maintain functionality

## Troubleshooting errors

<Note>
  **Built-in Error Handling**: [Tool runner](#tool-runner-beta) provide automatic error handling for most common scenarios. This section covers manual error handling for advanced use cases.
</Note>

There are a few different types of errors that can occur when using tools with Claude:

<AccordionGroup>
  <Accordion title="Tool execution error">
    If the tool itself throws an error during execution (e.g. a network error when fetching weather data), you can return the error message in the `content` along with `"is_error": true`:

    ```JSON JSON theme={null}
    {
      "role": "user",
      "content": [
        {
          "type": "tool_result",
          "tool_use_id": "toolu_01A09q90qw90lq917835lq9",
          "content": "ConnectionError: the weather service API is not available (HTTP 500)",
          "is_error": true
        }
      ]
    }
    ```

    Claude will then incorporate this error into its response to the user, e.g. "I'm sorry, I was unable to retrieve the current weather because the weather service API is not available. Please try again later."

  </Accordion>

  <Accordion title="Invalid tool name">
    If Claude's attempted use of a tool is invalid (e.g. missing required parameters), it usually means that the there wasn't enough information for Claude to use the tool correctly. Your best bet during development is to try the request again with more-detailed `description` values in your tool definitions.

    However, you can also continue the conversation forward with a `tool_result` that indicates the error, and Claude will try to use the tool again with the missing information filled in:

    ```JSON JSON theme={null}
    {
      "role": "user",
      "content": [
        {
          "type": "tool_result",
          "tool_use_id": "toolu_01A09q90qw90lq917835lq9",
          "content": "Error: Missing required 'location' parameter",
          "is_error": true
        }
      ]
    }
    ```

    If a tool request is invalid or missing parameters, Claude will retry 2-3 times with corrections before apologizing to the user.

  </Accordion>

  <Accordion title="<search_quality_reflection> tags">
    To prevent Claude from reflecting on search quality with \<search\_quality\_reflection> tags, add "Do not reflect on the quality of the returned search results in your response" to your prompt.
  </Accordion>

  <Accordion title="Server tool errors">
    When server tools encounter errors (e.g., network issues with Web Search), Claude will transparently handle these errors and attempt to provide an alternative response or explanation to the user. Unlike client tools, you do not need to handle `is_error` results for server tools.

    For web search specifically, possible error codes include:

    * `too_many_requests`: Rate limit exceeded
    * `invalid_input`: Invalid search query parameter
    * `max_uses_exceeded`: Maximum web search tool uses exceeded
    * `query_too_long`: Query exceeds maximum length
    * `unavailable`: An internal error occurred

  </Accordion>

  <Accordion title="Parallel tool calls not working">
    If Claude isn't making parallel tool calls when expected, check these common issues:

    **1. Incorrect tool result formatting**

    The most common issue is formatting tool results incorrectly in the conversation history. This "teaches" Claude to avoid parallel calls.

    Specifically for parallel tool use:

    * ❌ **Wrong**: Sending separate user messages for each tool result
    * ✅ **Correct**: All tool results must be in a single user message

    ```json  theme={null}
    // ❌ This reduces parallel tool use
    [
      {"role": "assistant", "content": [tool_use_1, tool_use_2]},
      {"role": "user", "content": [tool_result_1]},
      {"role": "user", "content": [tool_result_2]}  // Separate message
    ]

    // ✅ This maintains parallel tool use
    [
      {"role": "assistant", "content": [tool_use_1, tool_use_2]},
      {"role": "user", "content": [tool_result_1, tool_result_2]}  // Single message
    ]
    ```

    See the [general formatting requirements above](#handling-tool-use-and-tool-result-content-blocks) for other formatting rules.

    **2. Weak prompting**

    Default prompting may not be sufficient. Use stronger language:

    ```text  theme={null}
    <use_parallel_tool_calls>
    For maximum efficiency, whenever you perform multiple independent operations,
    invoke all relevant tools simultaneously rather than sequentially.
    Prioritize calling tools in parallel whenever possible.
    </use_parallel_tool_calls>
    ```

    **3. Measuring parallel tool usage**

    To verify parallel tool calls are working:

    ```python  theme={null}
    # Calculate average tools per tool-calling message
    tool_call_messages = [msg for msg in messages if any(
        block.type == "tool_use" for block in msg.content
    )]
    total_tool_calls = sum(
        len([b for b in msg.content if b.type == "tool_use"])
        for msg in tool_call_messages
    )
    avg_tools_per_message = total_tool_calls / len(tool_call_messages)
    print(f"Average tools per message: {avg_tools_per_message}")
    # Should be > 1.0 if parallel calls are working
    ```

    **4. Model-specific behavior**

    * Claude Opus 4.1, Opus 4, and Sonnet 4: Excel at parallel tool use with minimal prompting
    * Claude Sonnet 3.7: May need stronger prompting or [token-efficient tool use](/en/docs/agents-and-tools/tool-use/token-efficient-tool-use)
    * Claude Haiku: Less likely to use parallel tools without explicit prompting

  </Accordion>
</AccordionGroup>

---

## 15. Tool use with Claude

**URL:** https://docs.claude.com/en/docs/agents-and-tools/tool-use/overview

# Tool use with Claude

Claude is capable of interacting with tools and functions, allowing you to extend Claude's capabilities to perform a wider variety of tasks.

<Tip>
  Learn everything you need to master tool use with Claude as part of our new [courses](https://anthropic.skilljar.com/)! Please
  continue to share your ideas and suggestions using this
  [form](https://forms.gle/BFnYc6iCkWoRzFgk7).
</Tip>

Here's an example of how to provide tools to Claude using the Messages API:

<CodeGroup>
  ```bash Shell theme={null}
  curl https://api.anthropic.com/v1/messages \
    -H "content-type: application/json" \
    -H "x-api-key: $ANTHROPIC_API_KEY" \
    -H "anthropic-version: 2023-06-01" \
    -d '{
      "model": "claude-sonnet-4-5",
      "max_tokens": 1024,
      "tools": [
        {
          "name": "get_weather",
          "description": "Get the current weather in a given location",
          "input_schema": {
            "type": "object",
            "properties": {
              "location": {
                "type": "string",
                "description": "The city and state, e.g. San Francisco, CA"
              }
            },
            "required": ["location"]
          }
        }
      ],
      "messages": [
        {
          "role": "user",
          "content": "What is the weather like in San Francisco?"
        }
      ]
    }'
  ```

```python Python theme={null}
import anthropic

client = anthropic.Anthropic()

response = client.messages.create(
    model="claude-sonnet-4-5",
    max_tokens=1024,
    tools=[
        {
            "name": "get_weather",
            "description": "Get the current weather in a given location",
            "input_schema": {
                "type": "object",
                "properties": {
                    "location": {
                        "type": "string",
                        "description": "The city and state, e.g. San Francisco, CA",
                    }
                },
                "required": ["location"],
            },
        }
    ],
    messages=[{"role": "user", "content": "What's the weather like in San Francisco?"}],
)
print(response)
```

```typescript TypeScript theme={null}
import { Anthropic } from "@anthropic-ai/sdk";

const anthropic = new Anthropic({
  apiKey: process.env.ANTHROPIC_API_KEY,
});

async function main() {
  const response = await anthropic.messages.create({
    model: "claude-sonnet-4-5",
    max_tokens: 1024,
    tools: [
      {
        name: "get_weather",
        description: "Get the current weather in a given location",
        input_schema: {
          type: "object",
          properties: {
            location: {
              type: "string",
              description: "The city and state, e.g. San Francisco, CA",
            },
          },
          required: ["location"],
        },
      },
    ],
    messages: [
      {
        role: "user",
        content: "Tell me the weather in San Francisco.",
      },
    ],
  });

  console.log(response);
}

main().catch(console.error);
```

```java Java theme={null}
import java.util.List;
import java.util.Map;

import com.anthropic.client.AnthropicClient;
import com.anthropic.client.okhttp.AnthropicOkHttpClient;
import com.anthropic.core.JsonValue;
import com.anthropic.models.messages.Message;
import com.anthropic.models.messages.MessageCreateParams;
import com.anthropic.models.messages.Model;
import com.anthropic.models.messages.Tool;
import com.anthropic.models.messages.Tool.InputSchema;

public class GetWeatherExample {

    public static void main(String[] args) {
        AnthropicClient client = AnthropicOkHttpClient.fromEnv();

        InputSchema schema = InputSchema.builder()
                .properties(JsonValue.from(Map.of(
                        "location",
                        Map.of(
                                "type", "string",
                                "description", "The city and state, e.g. San Francisco, CA"))))
                .putAdditionalProperty("required", JsonValue.from(List.of("location")))
                .build();

        MessageCreateParams params = MessageCreateParams.builder()
                .model(Model.CLAUDE_OPUS_4_0)
                .maxTokens(1024)
                .addTool(Tool.builder()
                        .name("get_weather")
                        .description("Get the current weather in a given location")
                        .inputSchema(schema)
                        .build())
                .addUserMessage("What's the weather like in San Francisco?")
                .build();

        Message message = client.messages().create(params);
        System.out.println(message);
    }
}
```

</CodeGroup>

---

## How tool use works

Claude supports two types of tools:

1. **Client tools**: Tools that execute on your systems, which include:
   - User-defined custom tools that you create and implement
   - Anthropic-defined tools like [computer use](/en/docs/agents-and-tools/tool-use/computer-use-tool) and [text editor](/en/docs/agents-and-tools/tool-use/text-editor-tool) that require client implementation

2. **Server tools**: Tools that execute on Anthropic's servers, like the [web search](/en/docs/agents-and-tools/tool-use/web-search-tool) and [web fetch](/en/docs/agents-and-tools/tool-use/web-fetch-tool) tools. These tools must be specified in the API request but don't require implementation on your part.

<Note>
  Anthropic-defined tools use versioned types (e.g., `web_search_20250305`, `text_editor_20250124`) to ensure compatibility across model versions.
</Note>

### Client tools

Integrate client tools with Claude in these steps:

<Steps>
  <Step title="Provide Claude with tools and a user prompt">
    * Define client tools with names, descriptions, and input schemas in your API request.
    * Include a user prompt that might require these tools, e.g., "What's the weather in San Francisco?"
  </Step>

  <Step title="Claude decides to use a tool">
    * Claude assesses if any tools can help with the user's query.
    * If yes, Claude constructs a properly formatted tool use request.
    * For client tools, the API response has a `stop_reason` of `tool_use`, signaling Claude's intent.
  </Step>

  <Step title="Execute the tool and return results">
    * Extract the tool name and input from Claude's request
    * Execute the tool code on your system
    * Return the results in a new `user` message containing a `tool_result` content block
  </Step>

  <Step title="Claude uses tool result to formulate a response">
    * Claude analyzes the tool results to craft its final response to the original user prompt.
  </Step>
</Steps>

Note: Steps 3 and 4 are optional. For some workflows, Claude's tool use request (step 2) might be all you need, without sending results back to Claude.

### Server tools

Server tools follow a different workflow:

<Steps>
  <Step title="Provide Claude with tools and a user prompt">
    * Server tools, like [web search](/en/docs/agents-and-tools/tool-use/web-search-tool) and [web fetch](/en/docs/agents-and-tools/tool-use/web-fetch-tool), have their own parameters.
    * Include a user prompt that might require these tools, e.g., "Search for the latest news about AI" or "Analyze the content at this URL."
  </Step>

  <Step title="Claude executes the server tool">
    * Claude assesses if a server tool can help with the user's query.
    * If yes, Claude executes the tool, and the results are automatically incorporated into Claude's response.
  </Step>

  <Step title="Claude uses the server tool result to formulate a response">
    * Claude analyzes the server tool results to craft its final response to the original user prompt.
    * No additional user interaction is needed for server tool execution.
  </Step>
</Steps>

---

## Tool use examples

Here are a few code examples demonstrating various tool use patterns and techniques. For brevity's sake, the tools are simple tools, and the tool descriptions are shorter than would be ideal to ensure best performance.

<AccordionGroup>
  <Accordion title="Single tool example">
    <CodeGroup>
      ```bash Shell theme={null}
      curl https://api.anthropic.com/v1/messages \
           --header "x-api-key: $ANTHROPIC_API_KEY" \
           --header "anthropic-version: 2023-06-01" \
           --header "content-type: application/json" \
           --data \
      '{
          "model": "claude-sonnet-4-5",
          "max_tokens": 1024,
          "tools": [{
              "name": "get_weather",
              "description": "Get the current weather in a given location",
              "input_schema": {
                  "type": "object",
                  "properties": {
                      "location": {
                          "type": "string",
                          "description": "The city and state, e.g. San Francisco, CA"
                      },
                      "unit": {
                          "type": "string",
                          "enum": ["celsius", "fahrenheit"],
                          "description": "The unit of temperature, either \"celsius\" or \"fahrenheit\""
                      }
                  },
                  "required": ["location"]
              }
          }],
          "messages": [{"role": "user", "content": "What is the weather like in San Francisco?"}]
      }'
      ```

      ```Python Python theme={null}
      import anthropic
      client = anthropic.Anthropic()

      response = client.messages.create(
          model="claude-sonnet-4-5",
          max_tokens=1024,
          tools=[
              {
                  "name": "get_weather",
                  "description": "Get the current weather in a given location",
                  "input_schema": {
                      "type": "object",
                      "properties": {
                          "location": {
                              "type": "string",
                              "description": "The city and state, e.g. San Francisco, CA"
                          },
                          "unit": {
                              "type": "string",
                              "enum": ["celsius", "fahrenheit"],
                              "description": "The unit of temperature, either \"celsius\" or \"fahrenheit\""
                          }
                      },
                      "required": ["location"]
                  }
              }
          ],
          messages=[{"role": "user", "content": "What is the weather like in San Francisco?"}]
      )

      print(response)
      ```

      ```java Java theme={null}
      import java.util.List;
      import java.util.Map;

      import com.anthropic.client.AnthropicClient;
      import com.anthropic.client.okhttp.AnthropicOkHttpClient;
      import com.anthropic.core.JsonValue;
      import com.anthropic.models.messages.Message;
      import com.anthropic.models.messages.MessageCreateParams;
      import com.anthropic.models.messages.Model;
      import com.anthropic.models.messages.Tool;
      import com.anthropic.models.messages.Tool.InputSchema;

      public class WeatherToolExample {

          public static void main(String[] args) {
              AnthropicClient client = AnthropicOkHttpClient.fromEnv();

              InputSchema schema = InputSchema.builder()
                      .properties(JsonValue.from(Map.of(
                              "location", Map.of(
                                      "type", "string",
                                      "description", "The city and state, e.g. San Francisco, CA"
                              ),
                              "unit", Map.of(
                                      "type", "string",
                                      "enum", List.of("celsius", "fahrenheit"),
                                      "description", "The unit of temperature, either \"celsius\" or \"fahrenheit\""
                              )
                      )))
                      .putAdditionalProperty("required", JsonValue.from(List.of("location")))
                      .build();

              MessageCreateParams params = MessageCreateParams.builder()
                      .model(Model.CLAUDE_OPUS_4_0)
                      .maxTokens(1024)
                      .addTool(Tool.builder()
                              .name("get_weather")
                              .description("Get the current weather in a given location")
                              .inputSchema(schema)
                              .build())
                      .addUserMessage("What is the weather like in San Francisco?")
                      .build();

              Message message = client.messages().create(params);
              System.out.println(message);
          }
      }
      ```
    </CodeGroup>

    Claude will return a response similar to:

    ```JSON JSON theme={null}
    {
      "id": "msg_01Aq9w938a90dw8q",
      "model": "claude-sonnet-4-5",
      "stop_reason": "tool_use",
      "role": "assistant",
      "content": [
        {
          "type": "text",
          "text": "I'll check the current weather in San Francisco for you."
        },
        {
          "type": "tool_use",
          "id": "toolu_01A09q90qw90lq917835lq9",
          "name": "get_weather",
          "input": {"location": "San Francisco, CA", "unit": "celsius"}
        }
      ]
    }
    ```

    You would then need to execute the `get_weather` function with the provided input, and return the result in a new `user` message:

    <CodeGroup>
      ```bash Shell theme={null}
      curl https://api.anthropic.com/v1/messages \
           --header "x-api-key: $ANTHROPIC_API_KEY" \
           --header "anthropic-version: 2023-06-01" \
           --header "content-type: application/json" \
           --data \
      '{
          "model": "claude-sonnet-4-5",
          "max_tokens": 1024,
          "tools": [
              {
                  "name": "get_weather",
                  "description": "Get the current weather in a given location",
                  "input_schema": {
                      "type": "object",
                      "properties": {
                          "location": {
                              "type": "string",
                              "description": "The city and state, e.g. San Francisco, CA"
                          },
                          "unit": {
                              "type": "string",
                              "enum": ["celsius", "fahrenheit"],
                              "description": "The unit of temperature, either \"celsius\" or \"fahrenheit\""
                          }
                      },
                      "required": ["location"]
                  }
              }
          ],
          "messages": [
              {
                  "role": "user",
                  "content": "What is the weather like in San Francisco?"
              },
              {
                  "role": "assistant",
                  "content": [
                      {
                          "type": "text",
                          "text": "I'll check the current weather in San Francisco for you."
                      },
                      {
                          "type": "tool_use",
                          "id": "toolu_01A09q90qw90lq917835lq9",
                          "name": "get_weather",
                          "input": {
                              "location": "San Francisco, CA",
                              "unit": "celsius"
                          }
                      }
                  ]
              },
              {
                  "role": "user",
                  "content": [
                      {
                          "type": "tool_result",
                          "tool_use_id": "toolu_01A09q90qw90lq917835lq9",
                          "content": "15 degrees"
                      }
                  ]
              }
          ]
      }'
      ```

      ```Python Python theme={null}
      response = client.messages.create(
          model="claude-sonnet-4-5",
          max_tokens=1024,
          tools=[
              {
                  "name": "get_weather",
                  "description": "Get the current weather in a given location",
                  "input_schema": {
                      "type": "object",
                      "properties": {
                          "location": {
                              "type": "string",
                              "description": "The city and state, e.g. San Francisco, CA"
                          },
                          "unit": {
                              "type": "string",
                              "enum": ["celsius", "fahrenheit"],
                              "description": "The unit of temperature, either 'celsius' or 'fahrenheit'"
                          }
                      },
                      "required": ["location"]
                  }
              }
          ],
          messages=[
              {
                  "role": "user",
                  "content": "What's the weather like in San Francisco?"
              },
              {
                  "role": "assistant",
                  "content": [
                      {
                          "type": "text",
                          "text": "I'll check the current weather in San Francisco for you."
                      },
                      {
                          "type": "tool_use",
                          "id": "toolu_01A09q90qw90lq917835lq9",
                          "name": "get_weather",
                          "input": {"location": "San Francisco, CA", "unit": "celsius"}
                      }
                  ]
              },
              {
                  "role": "user",
                  "content": [
                      {
                          "type": "tool_result",
                          "tool_use_id": "toolu_01A09q90qw90lq917835lq9", # from the API response
                          "content": "65 degrees" # from running your tool
                      }
                  ]
              }
          ]
      )

      print(response)
      ```

      ```java Java theme={null}
       import java.util.List;
       import java.util.Map;

       import com.anthropic.client.AnthropicClient;
       import com.anthropic.client.okhttp.AnthropicOkHttpClient;
       import com.anthropic.core.JsonValue;
       import com.anthropic.models.messages.*;
       import com.anthropic.models.messages.Tool.InputSchema;

       public class ToolConversationExample {

           public static void main(String[] args) {
               AnthropicClient client = AnthropicOkHttpClient.fromEnv();

               InputSchema schema = InputSchema.builder()
                       .properties(JsonValue.from(Map.of(
                               "location", Map.of(
                                       "type", "string",
                                       "description", "The city and state, e.g. San Francisco, CA"
                               ),
                               "unit", Map.of(
                                       "type", "string",
                                       "enum", List.of("celsius", "fahrenheit"),
                                       "description", "The unit of temperature, either \"celsius\" or \"fahrenheit\""
                               )
                       )))
                       .putAdditionalProperty("required", JsonValue.from(List.of("location")))
                       .build();

               MessageCreateParams params = MessageCreateParams.builder()
                       .model(Model.CLAUDE_OPUS_4_0)
                       .maxTokens(1024)
                       .addTool(Tool.builder()
                               .name("get_weather")
                               .description("Get the current weather in a given location")
                               .inputSchema(schema)
                               .build())
                       .addUserMessage("What is the weather like in San Francisco?")
                       .addAssistantMessageOfBlockParams(
                               List.of(
                                       ContentBlockParam.ofText(
                                               TextBlockParam.builder()
                                                       .text("I'll check the current weather in San Francisco for you.")
                                                       .build()
                                       ),
                                       ContentBlockParam.ofToolUse(
                                               ToolUseBlockParam.builder()
                                                       .id("toolu_01A09q90qw90lq917835lq9")
                                                       .name("get_weather")
                                                       .input(JsonValue.from(Map.of(
                                                               "location", "San Francisco, CA",
                                                               "unit", "celsius"
                                                       )))
                                                       .build()
                                       )
                               )
                       )
                       .addUserMessageOfBlockParams(List.of(
                               ContentBlockParam.ofToolResult(
                                       ToolResultBlockParam.builder()
                                               .toolUseId("toolu_01A09q90qw90lq917835lq9")
                                               .content("15 degrees")
                                               .build()
                               )
                       ))
                       .build();

               Message message = client.messages().create(params);
               System.out.println(message);
           }
       }
      ```
    </CodeGroup>

    This will print Claude's final response, incorporating the weather data:

    ```JSON JSON theme={null}
    {
      "id": "msg_01Aq9w938a90dw8q",
      "model": "claude-sonnet-4-5",
      "stop_reason": "stop_sequence",
      "role": "assistant",
      "content": [
        {
          "type": "text",
          "text": "The current weather in San Francisco is 15 degrees Celsius (59 degrees Fahrenheit). It's a cool day in the city by the bay!"
        }
      ]
    }
    ```

  </Accordion>

  <Accordion title="Parallel tool use">
    Claude can call multiple tools in parallel within a single response, which is useful for tasks that require multiple independent operations. When using parallel tools, all `tool_use` blocks are included in a single assistant message, and all corresponding `tool_result` blocks must be provided in the subsequent user message.

    <Note>
      **Important**: Tool results must be formatted correctly to avoid API errors and ensure Claude continues using parallel tools. See our [implementation guide](/en/docs/agents-and-tools/tool-use/implement-tool-use#parallel-tool-use) for detailed formatting requirements and complete code examples.
    </Note>

    For comprehensive examples, test scripts, and best practices for implementing parallel tool calls, see the [parallel tool use section](/en/docs/agents-and-tools/tool-use/implement-tool-use#parallel-tool-use) in our implementation guide.

  </Accordion>

  <Accordion title="Multiple tool example">
    You can provide Claude with multiple tools to choose from in a single request. Here's an example with both a `get_weather` and a `get_time` tool, along with a user query that asks for both.

    <CodeGroup>
      ```bash Shell theme={null}
      curl https://api.anthropic.com/v1/messages \
           --header "x-api-key: $ANTHROPIC_API_KEY" \
           --header "anthropic-version: 2023-06-01" \
           --header "content-type: application/json" \
           --data \
      '{
          "model": "claude-sonnet-4-5",
          "max_tokens": 1024,
          "tools": [{
              "name": "get_weather",
              "description": "Get the current weather in a given location",
              "input_schema": {
                  "type": "object",
                  "properties": {
                      "location": {
                          "type": "string",
                          "description": "The city and state, e.g. San Francisco, CA"
                      },
                      "unit": {
                          "type": "string",
                          "enum": ["celsius", "fahrenheit"],
                          "description": "The unit of temperature, either 'celsius' or 'fahrenheit'"
                      }
                  },
                  "required": ["location"]
              }
          },
          {
              "name": "get_time",
              "description": "Get the current time in a given time zone",
              "input_schema": {
                  "type": "object",
                  "properties": {
                      "timezone": {
                          "type": "string",
                          "description": "The IANA time zone name, e.g. America/Los_Angeles"
                      }
                  },
                  "required": ["timezone"]
              }
          }],
          "messages": [{
              "role": "user",
              "content": "What is the weather like right now in New York? Also what time is it there?"
          }]
      }'
      ```

      ```Python Python theme={null}
      import anthropic
      client = anthropic.Anthropic()

      response = client.messages.create(
          model="claude-sonnet-4-5",
          max_tokens=1024,
          tools=[
              {
                  "name": "get_weather",
                  "description": "Get the current weather in a given location",
                  "input_schema": {
                      "type": "object",
                      "properties": {
                          "location": {
                              "type": "string",
                              "description": "The city and state, e.g. San Francisco, CA"
                          },
                          "unit": {
                              "type": "string",
                              "enum": ["celsius", "fahrenheit"],
                              "description": "The unit of temperature, either 'celsius' or 'fahrenheit'"
                          }
                      },
                      "required": ["location"]
                  }
              },
              {
                  "name": "get_time",
                  "description": "Get the current time in a given time zone",
                  "input_schema": {
                      "type": "object",
                      "properties": {
                          "timezone": {
                              "type": "string",
                              "description": "The IANA time zone name, e.g. America/Los_Angeles"
                          }
                      },
                      "required": ["timezone"]
                  }
              }
          ],
          messages=[
              {
                  "role": "user",
                  "content": "What is the weather like right now in New York? Also what time is it there?"
              }
          ]
      )
      print(response)
      ```

      ```java Java theme={null}
      import java.util.List;
      import java.util.Map;

      import com.anthropic.client.AnthropicClient;
      import com.anthropic.client.okhttp.AnthropicOkHttpClient;
      import com.anthropic.core.JsonValue;
      import com.anthropic.models.messages.Message;
      import com.anthropic.models.messages.MessageCreateParams;
      import com.anthropic.models.messages.Model;
      import com.anthropic.models.messages.Tool;
      import com.anthropic.models.messages.Tool.InputSchema;

      public class MultipleToolsExample {

          public static void main(String[] args) {
              AnthropicClient client = AnthropicOkHttpClient.fromEnv();

              // Weather tool schema
              InputSchema weatherSchema = InputSchema.builder()
                      .properties(JsonValue.from(Map.of(
                              "location", Map.of(
                                      "type", "string",
                                      "description", "The city and state, e.g. San Francisco, CA"
                              ),
                              "unit", Map.of(
                                      "type", "string",
                                      "enum", List.of("celsius", "fahrenheit"),
                                      "description", "The unit of temperature, either \"celsius\" or \"fahrenheit\""
                              )
                      )))
                      .putAdditionalProperty("required", JsonValue.from(List.of("location")))
                      .build();

              // Time tool schema
              InputSchema timeSchema = InputSchema.builder()
                      .properties(JsonValue.from(Map.of(
                              "timezone", Map.of(
                                      "type", "string",
                                      "description", "The IANA time zone name, e.g. America/Los_Angeles"
                              )
                      )))
                      .putAdditionalProperty("required", JsonValue.from(List.of("timezone")))
                      .build();

              MessageCreateParams params = MessageCreateParams.builder()
                      .model(Model.CLAUDE_OPUS_4_0)
                      .maxTokens(1024)
                      .addTool(Tool.builder()
                              .name("get_weather")
                              .description("Get the current weather in a given location")
                              .inputSchema(weatherSchema)
                              .build())
                      .addTool(Tool.builder()
                              .name("get_time")
                              .description("Get the current time in a given time zone")
                              .inputSchema(timeSchema)
                              .build())
                      .addUserMessage("What is the weather like right now in New York? Also what time is it there?")
                      .build();

              Message message = client.messages().create(params);
              System.out.println(message);
          }
      }
      ```
    </CodeGroup>

    In this case, Claude may either:

    * Use the tools sequentially (one at a time) — calling `get_weather` first, then `get_time` after receiving the weather result
    * Use parallel tool calls — outputting multiple `tool_use` blocks in a single response when the operations are independent

    When Claude makes parallel tool calls, you must return all tool results in a single `user` message, with each result in its own `tool_result` block.

  </Accordion>

  <Accordion title="Missing information">
    If the user's prompt doesn't include enough information to fill all the required parameters for a tool, Claude Opus is much more likely to recognize that a parameter is missing and ask for it. Claude Sonnet may ask, especially when prompted to think before outputting a tool request. But it may also do its best to infer a reasonable value.

    For example, using the `get_weather` tool above, if you ask Claude "What's the weather?" without specifying a location, Claude, particularly Claude Sonnet, may make a guess about tools inputs:

    ```JSON JSON theme={null}
    {
      "type": "tool_use",
      "id": "toolu_01A09q90qw90lq917835lq9",
      "name": "get_weather",
      "input": {"location": "New York, NY", "unit": "fahrenheit"}
    }
    ```

    This behavior is not guaranteed, especially for more ambiguous prompts and for less intelligent models. If Claude Opus doesn't have enough context to fill in the required parameters, it is far more likely respond with a clarifying question instead of making a tool call.

  </Accordion>

  <Accordion title="Sequential tools">
    Some tasks may require calling multiple tools in sequence, using the output of one tool as the input to another. In such a case, Claude will call one tool at a time. If prompted to call the tools all at once, Claude is likely to guess parameters for tools further downstream if they are dependent on tool results for tools further upstream.

    Here's an example of using a `get_location` tool to get the user's location, then passing that location to the `get_weather` tool:

    <CodeGroup>
      ```bash Shell theme={null}
      curl https://api.anthropic.com/v1/messages \
           --header "x-api-key: $ANTHROPIC_API_KEY" \
           --header "anthropic-version: 2023-06-01" \
           --header "content-type: application/json" \
           --data \
      '{
          "model": "claude-sonnet-4-5",
          "max_tokens": 1024,
          "tools": [
              {
                  "name": "get_location",
                  "description": "Get the current user location based on their IP address. This tool has no parameters or arguments.",
                  "input_schema": {
                      "type": "object",
                      "properties": {}
                  }
              },
              {
                  "name": "get_weather",
                  "description": "Get the current weather in a given location",
                  "input_schema": {
                      "type": "object",
                      "properties": {
                          "location": {
                              "type": "string",
                              "description": "The city and state, e.g. San Francisco, CA"
                          },
                          "unit": {
                              "type": "string",
                              "enum": ["celsius", "fahrenheit"],
                              "description": "The unit of temperature, either 'celsius' or 'fahrenheit'"
                          }
                      },
                      "required": ["location"]
                  }
              }
          ],
          "messages": [{
              "role": "user",
              "content": "What is the weather like where I am?"
          }]
      }'
      ```

      ```Python Python theme={null}
      response = client.messages.create(
          model="claude-sonnet-4-5",
          max_tokens=1024,
          tools=[
              {
                  "name": "get_location",
                  "description": "Get the current user location based on their IP address. This tool has no parameters or arguments.",
                  "input_schema": {
                      "type": "object",
                      "properties": {}
                  }
              },
              {
                  "name": "get_weather",
                  "description": "Get the current weather in a given location",
                  "input_schema": {
                      "type": "object",
                      "properties": {
                          "location": {
                              "type": "string",
                              "description": "The city and state, e.g. San Francisco, CA"
                          },
                          "unit": {
                              "type": "string",
                              "enum": ["celsius", "fahrenheit"],
                              "description": "The unit of temperature, either 'celsius' or 'fahrenheit'"
                          }
                      },
                      "required": ["location"]
                  }
              }
          ],
          messages=[{
         		  "role": "user",
          	  "content": "What's the weather like where I am?"
          }]
      )
      ```

      ```java Java theme={null}
      import java.util.List;
      import java.util.Map;

      import com.anthropic.client.AnthropicClient;
      import com.anthropic.client.okhttp.AnthropicOkHttpClient;
      import com.anthropic.core.JsonValue;
      import com.anthropic.models.messages.Message;
      import com.anthropic.models.messages.MessageCreateParams;
      import com.anthropic.models.messages.Model;
      import com.anthropic.models.messages.Tool;
      import com.anthropic.models.messages.Tool.InputSchema;

      public class EmptySchemaToolExample {

          public static void main(String[] args) {
              AnthropicClient client = AnthropicOkHttpClient.fromEnv();

              // Empty schema for location tool
              InputSchema locationSchema = InputSchema.builder()
                      .properties(JsonValue.from(Map.of()))
                      .build();

              // Weather tool schema
              InputSchema weatherSchema = InputSchema.builder()
                      .properties(JsonValue.from(Map.of(
                              "location", Map.of(
                                      "type", "string",
                                      "description", "The city and state, e.g. San Francisco, CA"
                              ),
                              "unit", Map.of(
                                      "type", "string",
                                      "enum", List.of("celsius", "fahrenheit"),
                                      "description", "The unit of temperature, either \"celsius\" or \"fahrenheit\""
                              )
                      )))
                      .putAdditionalProperty("required", JsonValue.from(List.of("location")))
                      .build();

              MessageCreateParams params = MessageCreateParams.builder()
                      .model(Model.CLAUDE_OPUS_4_0)
                      .maxTokens(1024)
                      .addTool(Tool.builder()
                              .name("get_location")
                              .description("Get the current user location based on their IP address. This tool has no parameters or arguments.")
                              .inputSchema(locationSchema)
                              .build())
                      .addTool(Tool.builder()
                              .name("get_weather")
                              .description("Get the current weather in a given location")
                              .inputSchema(weatherSchema)
                              .build())
                      .addUserMessage("What is the weather like where I am?")
                      .build();

              Message message = client.messages().create(params);
              System.out.println(message);
          }
      }
      ```
    </CodeGroup>

    In this case, Claude would first call the `get_location` tool to get the user's location. After you return the location in a `tool_result`, Claude would then call `get_weather` with that location to get the final answer.

    The full conversation might look like:

    | Role      | Content                                                                                                                                                                                                                       |
    | --------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | User      | What's the weather like where I am?                                                                                                                                                                                           |
    | Assistant | I'll find your current location first, then check the weather there. \[Tool use for get\_location]                                                                                                                            |
    | User      | \[Tool result for get\_location with matching id and result of San Francisco, CA]                                                                                                                                             |
    | Assistant | \[Tool use for get\_weather with the following input]\{ "location": "San Francisco, CA", "unit": "fahrenheit" }                                                                                                               |
    | User      | \[Tool result for get\_weather with matching id and result of "59°F (15°C), mostly cloudy"]                                                                                                                                   |
    | Assistant | Based on your current location in San Francisco, CA, the weather right now is 59°F (15°C) and mostly cloudy. It's a fairly cool and overcast day in the city. You may want to bring a light jacket if you're heading outside. |

    This example demonstrates how Claude can chain together multiple tool calls to answer a question that requires gathering data from different sources. The key steps are:

    1. Claude first realizes it needs the user's location to answer the weather question, so it calls the `get_location` tool.
    2. The user (i.e. the client code) executes the actual `get_location` function and returns the result "San Francisco, CA" in a `tool_result` block.
    3. With the location now known, Claude proceeds to call the `get_weather` tool, passing in "San Francisco, CA" as the `location` parameter (as well as a guessed `unit` parameter, as `unit` is not a required parameter).
    4. The user again executes the actual `get_weather` function with the provided arguments and returns the weather data in another `tool_result` block.
    5. Finally, Claude incorporates the weather data into a natural language response to the original question.

  </Accordion>

  <Accordion title="Chain of thought tool use">
    By default, Claude Opus is prompted to think before it answers a tool use query to best determine whether a tool is necessary, which tool to use, and the appropriate parameters. Claude Sonnet and Claude Haiku are prompted to try to use tools as much as possible and are more likely to call an unnecessary tool or infer missing parameters. To prompt Sonnet or Haiku to better assess the user query before making tool calls, the following prompt can be used:

    Chain of thought prompt

    `Answer the user's request using relevant tools (if they are available). Before calling a tool, do some analysis. First, think about which of the provided tools is the relevant tool to answer the user's request. Second, go through each of the required parameters of the relevant tool and determine if the user has directly provided or given enough information to infer a value. When deciding if the parameter can be inferred, carefully consider all the context to see if it supports a specific value. If all of the required parameters are present or can be reasonably inferred, proceed with the tool call. BUT, if one of the values for a required parameter is missing, DO NOT invoke the function (not even with fillers for the missing params) and instead, ask the user to provide the missing parameters. DO NOT ask for more information on optional parameters if it is not provided.
    `

  </Accordion>

  <Accordion title="JSON mode">
    You can use tools to get Claude produce JSON output that follows a schema, even if you don't have any intention of running that output through a tool or function.

    When using tools in this way:

    * You usually want to provide a **single** tool
    * You should set `tool_choice` (see [Forcing tool use](/en/docs/agents-and-tools/tool-use/implement-tool-use#forcing-tool-use)) to instruct the model to explicitly use that tool
    * Remember that the model will pass the `input` to the tool, so the name of the tool and description should be from the model's perspective.

    The following uses a `record_summary` tool to describe an image following a particular format.

    <CodeGroup>
      ```bash Shell theme={null}
      #!/bin/bash
      IMAGE_URL="https://upload.wikimedia.org/wikipedia/commons/a/a7/Camponotus_flavomarginatus_ant.jpg"
      IMAGE_MEDIA_TYPE="image/jpeg"
      IMAGE_BASE64=$(curl "$IMAGE_URL" | base64)

      curl https://api.anthropic.com/v1/messages \
           --header "content-type: application/json" \
           --header "x-api-key: $ANTHROPIC_API_KEY" \
           --header "anthropic-version: 2023-06-01" \
           --data \
      '{
          "model": "claude-sonnet-4-5",
          "max_tokens": 1024,
          "tools": [{
              "name": "record_summary",
              "description": "Record summary of an image using well-structured JSON.",
              "input_schema": {
                  "type": "object",
                  "properties": {
                      "key_colors": {
                          "type": "array",
                          "items": {
                              "type": "object",
                              "properties": {
                                  "r": { "type": "number", "description": "red value [0.0, 1.0]" },
                                  "g": { "type": "number", "description": "green value [0.0, 1.0]" },
                                  "b": { "type": "number", "description": "blue value [0.0, 1.0]" },
                                  "name": { "type": "string", "description": "Human-readable color name in snake_case, e.g. \"olive_green\" or \"turquoise\"" }
                              },
                              "required": [ "r", "g", "b", "name" ]
                          },
                          "description": "Key colors in the image. Limit to less than four."
                      },
                      "description": {
                          "type": "string",
                          "description": "Image description. One to two sentences max."
                      },
                      "estimated_year": {
                          "type": "integer",
                          "description": "Estimated year that the image was taken, if it is a photo. Only set this if the image appears to be non-fictional. Rough estimates are okay!"
                      }
                  },
                  "required": [ "key_colors", "description" ]
              }
          }],
          "tool_choice": {"type": "tool", "name": "record_summary"},
          "messages": [
              {"role": "user", "content": [
                  {"type": "image", "source": {
                      "type": "base64",
                      "media_type": "'$IMAGE_MEDIA_TYPE'",
                      "data": "'$IMAGE_BASE64'"
                  }},
                  {"type": "text", "text": "Describe this image."}
              ]}
          ]
      }'
      ```

      ```Python Python theme={null}
      import base64
      import anthropic
      import httpx

      image_url = "https://upload.wikimedia.org/wikipedia/commons/a/a7/Camponotus_flavomarginatus_ant.jpg"
      image_media_type = "image/jpeg"
      image_data = base64.standard_b64encode(httpx.get(image_url).content).decode("utf-8")

      message = anthropic.Anthropic().messages.create(
          model="claude-sonnet-4-5",
          max_tokens=1024,
          tools=[
              {
                  "name": "record_summary",
                  "description": "Record summary of an image using well-structured JSON.",
                  "input_schema": {
                      "type": "object",
                      "properties": {
                          "key_colors": {
                              "type": "array",
                              "items": {
                                  "type": "object",
                                  "properties": {
                                      "r": {
                                          "type": "number",
                                          "description": "red value [0.0, 1.0]",
                                      },
                                      "g": {
                                          "type": "number",
                                          "description": "green value [0.0, 1.0]",
                                      },
                                      "b": {
                                          "type": "number",
                                          "description": "blue value [0.0, 1.0]",
                                      },
                                      "name": {
                                          "type": "string",
                                          "description": "Human-readable color name in snake_case, e.g. \"olive_green\" or \"turquoise\""
                                      },
                                  },
                                  "required": ["r", "g", "b", "name"],
                              },
                              "description": "Key colors in the image. Limit to less than four.",
                          },
                          "description": {
                              "type": "string",
                              "description": "Image description. One to two sentences max.",
                          },
                          "estimated_year": {
                              "type": "integer",
                              "description": "Estimated year that the image was taken, if it is a photo. Only set this if the image appears to be non-fictional. Rough estimates are okay!",
                          },
                      },
                      "required": ["key_colors", "description"],
                  },
              }
          ],
          tool_choice={"type": "tool", "name": "record_summary"},
          messages=[
              {
                  "role": "user",
                  "content": [
                      {
                          "type": "image",
                          "source": {
                              "type": "base64",
                              "media_type": image_media_type,
                              "data": image_data,
                          },
                      },
                      {"type": "text", "text": "Describe this image."},
                  ],
              }
          ],
      )
      print(message)
      ```

      ```java Java theme={null}
      import java.io.IOException;
      import java.io.InputStream;
      import java.net.URL;
      import java.util.Base64;
      import java.util.List;
      import java.util.Map;

      import com.anthropic.client.AnthropicClient;
      import com.anthropic.client.okhttp.AnthropicOkHttpClient;
      import com.anthropic.core.JsonValue;
      import com.anthropic.models.messages.*;
      import com.anthropic.models.messages.Tool.InputSchema;

      public class ImageToolExample {

          public static void main(String[] args) throws Exception {
              AnthropicClient client = AnthropicOkHttpClient.fromEnv();

              String imageBase64 = downloadAndEncodeImage("https://upload.wikimedia.org/wikipedia/commons/a/a7/Camponotus_flavomarginatus_ant.jpg");
              // Create nested schema for colors
              Map<String, Object> colorProperties = Map.of(
                      "r", Map.of(
                              "type", "number",
                              "description", "red value [0.0, 1.0]"
                      ),
                      "g", Map.of(
                              "type", "number",
                              "description", "green value [0.0, 1.0]"
                      ),
                      "b", Map.of(
                              "type", "number",
                              "description", "blue value [0.0, 1.0]"
                      ),
                      "name", Map.of(
                              "type", "string",
                              "description", "Human-readable color name in snake_case, e.g. \"olive_green\" or \"turquoise\""
                      )
              );

              // Create the input schema
              InputSchema schema = InputSchema.builder()
                      .properties(JsonValue.from(Map.of(
                              "key_colors", Map.of(
                                      "type", "array",
                                      "items", Map.of(
                                              "type", "object",
                                              "properties", colorProperties,
                                              "required", List.of("r", "g", "b", "name")
                                      ),
                                      "description", "Key colors in the image. Limit to less than four."
                              ),
                              "description", Map.of(
                                      "type", "string",
                                      "description", "Image description. One to two sentences max."
                              ),
                              "estimated_year", Map.of(
                                      "type", "integer",
                                      "description", "Estimated year that the image was taken, if it is a photo. Only set this if the image appears to be non-fictional. Rough estimates are okay!"
                              )
                      )))
                      .putAdditionalProperty("required", JsonValue.from(List.of("key_colors", "description")))
                      .build();

              // Create the tool
              Tool tool = Tool.builder()
                      .name("record_summary")
                      .description("Record summary of an image using well-structured JSON.")
                      .inputSchema(schema)
                      .build();

              // Create the content blocks for the message
              ContentBlockParam imageContent = ContentBlockParam.ofImage(
                      ImageBlockParam.builder()
                              .source(Base64ImageSource.builder()
                                      .mediaType(Base64ImageSource.MediaType.IMAGE_JPEG)
                                      .data(imageBase64)
                                      .build())
                              .build()
              );

              ContentBlockParam textContent = ContentBlockParam.ofText(TextBlockParam.builder().text("Describe this image.").build());

              // Create the message
              MessageCreateParams params = MessageCreateParams.builder()
                      .model(Model.CLAUDE_OPUS_4_0)
                      .maxTokens(1024)
                      .addTool(tool)
                      .toolChoice(ToolChoiceTool.builder().name("record_summary").build())
                      .addUserMessageOfBlockParams(List.of(imageContent, textContent))
                      .build();

              Message message = client.messages().create(params);
              System.out.println(message);
          }

          private static String downloadAndEncodeImage(String imageUrl) throws IOException {
              try (InputStream inputStream = new URL(imageUrl).openStream()) {
                  return Base64.getEncoder().encodeToString(inputStream.readAllBytes());
              }
          }
      }
      ```
    </CodeGroup>

  </Accordion>
</AccordionGroup>

---

## Pricing

Tool use requests are priced based on:

1. The total number of input tokens sent to the model (including in the `tools` parameter)
2. The number of output tokens generated
3. For server-side tools, additional usage-based pricing (e.g., web search charges per search performed)

Client-side tools are priced the same as any other Claude API request, while server-side tools may incur additional charges based on their specific usage.

The additional tokens from tool use come from:

- The `tools` parameter in API requests (tool names, descriptions, and schemas)
- `tool_use` content blocks in API requests and responses
- `tool_result` content blocks in API requests

When you use `tools`, we also automatically include a special system prompt for the model which enables tool use. The number of tool use tokens required for each model are listed below (excluding the additional tokens listed above). Note that the table assumes at least 1 tool is provided. If no `tools` are provided, then a tool choice of `none` uses 0 additional system prompt tokens.

| Model                                                                      | Tool choice                                        | Tool use system prompt token count          |
| -------------------------------------------------------------------------- | -------------------------------------------------- | ------------------------------------------- |
| Claude Opus 4.1                                                            | `auto`, `none`<hr className="my-2" />`any`, `tool` | 346 tokens<hr className="my-2" />313 tokens |
| Claude Opus 4                                                              | `auto`, `none`<hr className="my-2" />`any`, `tool` | 346 tokens<hr className="my-2" />313 tokens |
| Claude Sonnet 4.5                                                          | `auto`, `none`<hr className="my-2" />`any`, `tool` | 346 tokens<hr className="my-2" />313 tokens |
| Claude Sonnet 4                                                            | `auto`, `none`<hr className="my-2" />`any`, `tool` | 346 tokens<hr className="my-2" />313 tokens |
| Claude Sonnet 3.7 ([deprecated](/en/docs/about-claude/model-deprecations)) | `auto`, `none`<hr className="my-2" />`any`, `tool` | 346 tokens<hr className="my-2" />313 tokens |
| Claude Haiku 4.5                                                           | `auto`, `none`<hr className="my-2" />`any`, `tool` | 346 tokens<hr className="my-2" />313 tokens |
| Claude Haiku 3.5                                                           | `auto`, `none`<hr className="my-2" />`any`, `tool` | 264 tokens<hr className="my-2" />340 tokens |
| Claude Opus 3 ([deprecated](/en/docs/about-claude/model-deprecations))     | `auto`, `none`<hr className="my-2" />`any`, `tool` | 530 tokens<hr className="my-2" />281 tokens |
| Claude Sonnet 3                                                            | `auto`, `none`<hr className="my-2" />`any`, `tool` | 159 tokens<hr className="my-2" />235 tokens |
| Claude Haiku 3                                                             | `auto`, `none`<hr className="my-2" />`any`, `tool` | 264 tokens<hr className="my-2" />340 tokens |

These token counts are added to your normal input and output tokens to calculate the total cost of a request.

Refer to our [models overview table](/en/docs/about-claude/models/overview#model-comparison-table) for current per-model prices.

When you send a tool use prompt, just like any other API request, the response will output both input and output token counts as part of the reported `usage` metrics.

---

## Next Steps

Explore our repository of ready-to-implement tool use code examples in our cookbooks:

<CardGroup cols={3}>
  <Card title="Calculator Tool" icon="calculator" href="https://github.com/anthropics/anthropic-cookbook/blob/main/tool_use/calculator_tool.ipynb">
    Learn how to integrate a simple calculator tool with Claude for precise numerical computations.
  </Card>

{" "}

  <Card title="Customer Service Agent" icon="headset" href="https://github.com/anthropics/anthropic-cookbook/blob/main/tool_use/customer_service_agent.ipynb">
    Build a responsive customer service bot that leverages client tools to
    enhance support.
  </Card>

  <Card title="JSON Extractor" icon="brackets-curly" href="https://github.com/anthropics/anthropic-cookbook/blob/main/tool_use/extracting_structured_json.ipynb">
    See how Claude and tool use can extract structured data from unstructured text.
  </Card>
</CardGroup>

---

## 16. Google Sheets add-on

**URL:** https://docs.claude.com/en/docs/agents-and-tools/claude-for-sheets

# Google Sheets add-on

> The [Claude for Sheets extension](https://workspace.google.com/marketplace/app/claude%5Ffor%5Fsheets/909417792257) integrates Claude into Google Sheets, allowing you to execute interactions with Claude directly in cells.

## Why use Claude for Sheets?

Claude for Sheets enables prompt engineering at scale by enabling you to test prompts across evaluation suites in parallel. Additionally, it excels at office tasks like survey analysis and online data processing.

Visit our [prompt engineering example sheet](https://docs.google.com/spreadsheets/d/1sUrBWO0u1-ZuQ8m5gt3-1N5PLR6r__UsRsB7WeySDQA/copy) to see this in action.

---

## Get started with Claude for Sheets

### Install Claude for Sheets

Easily enable Claude for Sheets using the following steps:

<Steps>
  <Step title="Get your Claude API key">
    If you don't yet have an API key, you can make API keys in the [Claude Console](https://console.anthropic.com/settings/keys).
  </Step>

  <Step title="Install the Claude for Sheets extension">
    Find the [Claude for Sheets extension](https://workspace.google.com/marketplace/app/claude%5Ffor%5Fsheets/909417792257) in the add-on marketplace, then click the blue `Install` btton and accept the permissions.

    <Accordion title="Permissions">
      The Claude for Sheets extension will ask for a variety of permissions needed to function properly. Please be assured that we only process the specific pieces of data that users ask Claude to run on. This data is never used to train our generative models.

      Extension permissions include:

      * **View and manage spreadsheets that this application has been installed in:** Needed to run prompts and return results
      * **Connect to an external service:** Needed in order to make calls to Claude API endpoints
      * **Allow this application to run when you are not present:** Needed to run cell recalculations without user intervention
      * **Display and run third-party web content in prompts and sidebars inside Google applications:** Needed to display the sidebar and post-install prompt
    </Accordion>

  </Step>

  <Step title="Connect your API key">
    Enter your API key at `Extensions` > `Claude for Sheets™` > `Open sidebar` > `☰` > `Settings` > `API provider`. You may need to wait or refresh for the Claude for Sheets menu to appear.
    <img src="https://mintcdn.com/anthropic-claude-docs/LF5WV0SNF6oudpT5/images/044af20-Screenshot_2024-01-04_at_11.58.21_AM.png?fit=max&auto=format&n=LF5WV0SNF6oudpT5&q=85&s=5e0b2abf471aac1f9f4c84a9bca20f2e" alt="" data-og-width="1187" width="1187" data-og-height="660" height="660" data-path="images/044af20-Screenshot_2024-01-04_at_11.58.21_AM.png" data-optimize="true" data-opv="3" srcset="https://mintcdn.com/anthropic-claude-docs/LF5WV0SNF6oudpT5/images/044af20-Screenshot_2024-01-04_at_11.58.21_AM.png?w=280&fit=max&auto=format&n=LF5WV0SNF6oudpT5&q=85&s=d2ae6b1d0a8e00d6146a527cc9b8d891 280w, https://mintcdn.com/anthropic-claude-docs/LF5WV0SNF6oudpT5/images/044af20-Screenshot_2024-01-04_at_11.58.21_AM.png?w=560&fit=max&auto=format&n=LF5WV0SNF6oudpT5&q=85&s=1acd2d438dbf0452eeb2383cc3ff33b8 560w, https://mintcdn.com/anthropic-claude-docs/LF5WV0SNF6oudpT5/images/044af20-Screenshot_2024-01-04_at_11.58.21_AM.png?w=840&fit=max&auto=format&n=LF5WV0SNF6oudpT5&q=85&s=5d394102f3e804ace9d70ac44a0243f9 840w, https://mintcdn.com/anthropic-claude-docs/LF5WV0SNF6oudpT5/images/044af20-Screenshot_2024-01-04_at_11.58.21_AM.png?w=1100&fit=max&auto=format&n=LF5WV0SNF6oudpT5&q=85&s=19a26018d349587f29e52b1fcd8fac1f 1100w, https://mintcdn.com/anthropic-claude-docs/LF5WV0SNF6oudpT5/images/044af20-Screenshot_2024-01-04_at_11.58.21_AM.png?w=1650&fit=max&auto=format&n=LF5WV0SNF6oudpT5&q=85&s=2729f60dce72ef9bb7a40e18086afb70 1650w, https://mintcdn.com/anthropic-claude-docs/LF5WV0SNF6oudpT5/images/044af20-Screenshot_2024-01-04_at_11.58.21_AM.png?w=2500&fit=max&auto=format&n=LF5WV0SNF6oudpT5&q=85&s=619b99d788343afc9e10cdf32e7dd348 2500w" />
  </Step>
</Steps>

<Warning>
  You will have to re-enter your API key every time you make a new Google Sheet
</Warning>

### Enter your first prompt

There are two main functions you can use to call Claude using Claude for Sheets. For now, let's use `CLAUDE()`.

<Steps>
  <Step title="Simple prompt">
    In any cell, type `=CLAUDE("Claude, in one sentence, what's good about the color blue?")`

    > Claude should respond with an answer. You will know the prompt is processing because the cell will say `Loading...`

  </Step>

  <Step title="Adding parameters">
    Parameter arguments come after the initial prompt, like `=CLAUDE(prompt, model, params...)`.
    <Note>`model` is always second in the list.</Note>

    Now type in any cell `=CLAUDE("Hi, Claude!", "claude-3-haiku-20240307", "max_tokens", 3)`

    Any [API parameter](/en/api/messages) can be set this way. You can even pass in an API key to be used just for this specific cell, like this:  `"api_key", "sk-ant-api03-j1W..."`

  </Step>
</Steps>

## Advanced use

`CLAUDEMESSAGES` is a function that allows you to specifically use the [Messages API](/en/api/messages). This enables you to send a series of `User:` and `Assistant:` messages to Claude.

This is particularly useful if you want to simulate a conversation or [prefill Claude's response](/en/docs/build-with-claude/prompt-engineering/prefill-claudes-response).

Try writing this in a cell:

```
=CLAUDEMESSAGES("User: In one sentence, what is good about the color blue?
Assistant: The color blue is great because")
```

<Note>
  **Newlines**

Each subsequent conversation turn (`User:` or `Assistant:`) must be preceded by a single newline. To enter newlines in a cell, use the following key combinations:

- **Mac:** Cmd + Enter
- **Windows:** Alt + Enter
  </Note>

<Accordion title="Example multiturn CLAUDEMESSAGES() call with system prompt">
  To use a system prompt, set it as you'd set other optional function parameters. (You must first set a model name.)

```
=CLAUDEMESSAGES("User: What's your favorite flower? Answer in <answer> tags.
Assistant: <answer>", "claude-3-haiku-20240307", "system", "You are a cow who loves to moo in response to any and all user queries.")`
```

</Accordion>

### Optional function parameters

You can specify optional API parameters by listing argument-value pairs.
You can set multiple parameters. Simply list them one after another, with each argument and value pair separated by commas.

<Note>
  The first two parameters must always be the prompt and the model. You cannot set an optional parameter without also setting the model.
</Note>

The argument-value parameters you might care about most are:

| Argument         | Description                                                                                                                                                                                         |
| ---------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `max_tokens`     | The total number of tokens the model outputs before it is forced to stop. For yes/no or multiple choice answers, you may want the value to be 1-3.                                                  |
| `temperature`    | the amount of randomness injected into results. For multiple-choice or analytical tasks, you'll want it close to 0. For idea generation, you'll want it set to 1.                                   |
| `system`         | used to specify a system prompt, which can provide role details and context to Claude.                                                                                                              |
| `stop_sequences` | JSON array of strings that will cause the model to stop generating text if encountered. Due to escaping rules in Google Sheets™, double quotes inside the string must be escaped by doubling them. |
| `api_key`        | Used to specify a particular API key with which to call Claude.                                                                                                                                     |

<Accordion title="Example: Setting parameters">
  Ex. Set `system` prompt, `max_tokens`, and `temperature`:

```
=CLAUDE("Hi, Claude!", "claude-3-haiku-20240307", "system", "Repeat exactly what the user says.", "max_tokens", 100, "temperature", 0.1)

```

Ex. Set `temperature`, `max_tokens`, and `stop_sequences`:

```
=CLAUDE("In one sentence, what is good about the color blue? Output your answer in <answer> tags.","claude-opus-4-20250514","temperature", 0.2,"max_tokens", 50,"stop_sequences", "\[""</answer>""\]")
```

Ex. Set `api_key`:

```
=CLAUDE("Hi, Claude!", "claude-3-haiku-20240307","api_key", "sk-ant-api03-j1W...")
```

</Accordion>

---

## Claude for Sheets usage examples

### Prompt engineering interactive tutorial

Our in-depth [prompt engineering interactive tutorial](https://docs.google.com/spreadsheets/d/19jzLgRruG9kjUQNKtCg1ZjdD6l6weA6qRXG5zLIAhC8/edit?usp=sharing) utilizes Claude for Sheets.
Check it out to learn or brush up on prompt engineering techniques.

<Note>Just as with any instance of Claude for Sheets, you will need an API key to interact with the tutorial.</Note>

### Prompt engineering workflow

Our [Claude for Sheets prompting examples workbench](https://docs.google.com/spreadsheets/d/1sUrBWO0u1-ZuQ8m5gt3-1N5PLR6r%5F%5FUsRsB7WeySDQA/copy) is a Claude-powered spreadsheet that houses example prompts and prompt engineering structures.

### Claude for Sheets workbook template

Make a copy of our [Claude for Sheets workbook template](https://docs.google.com/spreadsheets/d/1UwFS-ZQWvRqa6GkbL4sy0ITHK2AhXKe-jpMLzS0kTgk/copy) to get started with your own Claude for Sheets work!

---

## Troubleshooting

<Accordion title="NAME? Error: Unknown function: 'claude'">
  1. Ensure that you have enabled the extension for use in the current sheet
     1. Go to *Extensions* > *Add-ons* > *Manage add-ons*
     2. Click on the triple dot menu at the top right corner of the Claude for Sheets extension and make sure "Use in this document" is checked
        <img src="https://mintcdn.com/anthropic-claude-docs/LF5WV0SNF6oudpT5/images/9cce371-Screenshot_2023-10-03_at_7.17.39_PM.png?fit=max&auto=format&n=LF5WV0SNF6oudpT5&q=85&s=7ac5b747f92f68f05055ecd143bd5fa8" alt="" data-og-width="712" width="712" data-og-height="174" height="174" data-path="images/9cce371-Screenshot_2023-10-03_at_7.17.39_PM.png" data-optimize="true" data-opv="3" srcset="https://mintcdn.com/anthropic-claude-docs/LF5WV0SNF6oudpT5/images/9cce371-Screenshot_2023-10-03_at_7.17.39_PM.png?w=280&fit=max&auto=format&n=LF5WV0SNF6oudpT5&q=85&s=27a083fe65825128423ea09a03da3653 280w, https://mintcdn.com/anthropic-claude-docs/LF5WV0SNF6oudpT5/images/9cce371-Screenshot_2023-10-03_at_7.17.39_PM.png?w=560&fit=max&auto=format&n=LF5WV0SNF6oudpT5&q=85&s=9905542d704449f1727f5fe510242bb0 560w, https://mintcdn.com/anthropic-claude-docs/LF5WV0SNF6oudpT5/images/9cce371-Screenshot_2023-10-03_at_7.17.39_PM.png?w=840&fit=max&auto=format&n=LF5WV0SNF6oudpT5&q=85&s=8fed917d4e4ff142167cf8492febf442 840w, https://mintcdn.com/anthropic-claude-docs/LF5WV0SNF6oudpT5/images/9cce371-Screenshot_2023-10-03_at_7.17.39_PM.png?w=1100&fit=max&auto=format&n=LF5WV0SNF6oudpT5&q=85&s=e7d89ec0ed91b3c55a22a2e28da8ae25 1100w, https://mintcdn.com/anthropic-claude-docs/LF5WV0SNF6oudpT5/images/9cce371-Screenshot_2023-10-03_at_7.17.39_PM.png?w=1650&fit=max&auto=format&n=LF5WV0SNF6oudpT5&q=85&s=39998025b2c4afb6a49cf9efef63b266 1650w, https://mintcdn.com/anthropic-claude-docs/LF5WV0SNF6oudpT5/images/9cce371-Screenshot_2023-10-03_at_7.17.39_PM.png?w=2500&fit=max&auto=format&n=LF5WV0SNF6oudpT5&q=85&s=fe1bc4d35b3dd33c13b5c1e69e21f46a 2500w" />
  2. Refresh the page
</Accordion>

<Accordion title="#ERROR!, ⚠ DEFERRED ⚠ or ⚠ THROTTLED ⚠">
  You can manually recalculate `#ERROR!`, `⚠ DEFERRED ⚠` or `⚠ THROTTLED ⚠`cells by selecting from the recalculate options within the Claude for Sheets extension menu.

    <img src="https://mintcdn.com/anthropic-claude-docs/LF5WV0SNF6oudpT5/images/f729ba9-Screenshot_2024-02-01_at_8.30.31_PM.png?fit=max&auto=format&n=LF5WV0SNF6oudpT5&q=85&s=7bd765250352e58047c2dfb3f1a3d8e9" alt="" data-og-width="1486" width="1486" data-og-height="1062" height="1062" data-path="images/f729ba9-Screenshot_2024-02-01_at_8.30.31_PM.png" data-optimize="true" data-opv="3" srcset="https://mintcdn.com/anthropic-claude-docs/LF5WV0SNF6oudpT5/images/f729ba9-Screenshot_2024-02-01_at_8.30.31_PM.png?w=280&fit=max&auto=format&n=LF5WV0SNF6oudpT5&q=85&s=fb6b88b7a46b7322340d0839a740bc1e 280w, https://mintcdn.com/anthropic-claude-docs/LF5WV0SNF6oudpT5/images/f729ba9-Screenshot_2024-02-01_at_8.30.31_PM.png?w=560&fit=max&auto=format&n=LF5WV0SNF6oudpT5&q=85&s=fbf66142e6748a2bac8daad0007d24e6 560w, https://mintcdn.com/anthropic-claude-docs/LF5WV0SNF6oudpT5/images/f729ba9-Screenshot_2024-02-01_at_8.30.31_PM.png?w=840&fit=max&auto=format&n=LF5WV0SNF6oudpT5&q=85&s=c1e8c8648137d554ddb49b00e6007a18 840w, https://mintcdn.com/anthropic-claude-docs/LF5WV0SNF6oudpT5/images/f729ba9-Screenshot_2024-02-01_at_8.30.31_PM.png?w=1100&fit=max&auto=format&n=LF5WV0SNF6oudpT5&q=85&s=aa336dac0e2316b7699a20ec24e703e6 1100w, https://mintcdn.com/anthropic-claude-docs/LF5WV0SNF6oudpT5/images/f729ba9-Screenshot_2024-02-01_at_8.30.31_PM.png?w=1650&fit=max&auto=format&n=LF5WV0SNF6oudpT5&q=85&s=56d3ca83d0af273961f80f6122d02ccb 1650w, https://mintcdn.com/anthropic-claude-docs/LF5WV0SNF6oudpT5/images/f729ba9-Screenshot_2024-02-01_at_8.30.31_PM.png?w=2500&fit=max&auto=format&n=LF5WV0SNF6oudpT5&q=85&s=8a0522dcd0291612f474f077bcf826cb 2500w" />

</Accordion>

<Accordion title="Can't enter API key">
  1. Wait 20 seconds, then check again
  2. Refresh the page and wait 20 seconds again
  3. Uninstall and reinstall the extension
</Accordion>

---

## Further information

For more information regarding this extension, see the [Claude for Sheets Google Workspace Marketplace](https://workspace.google.com/marketplace/app/claude%5Ffor%5Fsheets/909417792257) overview page.

---

## 17. Agent Skills

**URL:** https://docs.claude.com/en/docs/agents-and-tools/agent-skills/overview

# Agent Skills

> Agent Skills are modular capabilities that extend Claude's functionality. Each Skill packages instructions, metadata, and optional resources (scripts, templates) that Claude uses automatically when relevant.

## Why use Skills

Skills are reusable, filesystem-based resources that provide Claude with domain-specific expertise: workflows, context, and best practices that transform general-purpose agents into specialists. Unlike prompts (conversation-level instructions for one-off tasks), Skills load on-demand and eliminate the need to repeatedly provide the same guidance across multiple conversations.

**Key benefits**:

- **Specialize Claude**: Tailor capabilities for domain-specific tasks
- **Reduce repetition**: Create once, use automatically
- **Compose capabilities**: Combine Skills to build complex workflows

<Note>
  For a deep dive into the architecture and real-world applications of Agent Skills, read our engineering blog: [Equipping agents for the real world with Agent Skills](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills).
</Note>

## Using Skills

Anthropic provides pre-built Agent Skills for common document tasks (PowerPoint, Excel, Word, PDF), and you can create your own custom Skills. Both work the same way. Claude automatically uses them when relevant to your request.

**Pre-built Agent Skills** are available to all users on claude.ai and via the Claude API. See the [Available Skills](#available-skills) section below for the complete list.

**Custom Skills** let you package domain expertise and organizational knowledge. They're available across Claude's products: create them in Claude Code, upload them via the API, or add them in claude.ai settings.

<Note>
  **Get started:**

- For pre-built Agent Skills: See the [quickstart tutorial](/en/docs/agents-and-tools/agent-skills/quickstart) to start using PowerPoint, Excel, Word, and PDF skills in the API
- For custom Skills: See the [Agent Skills Cookbook](https://github.com/anthropics/claude-cookbooks/tree/main/skills) to learn how to create your own Skills
  </Note>

## How Skills work

Skills leverage Claude's VM environment to provide capabilities beyond what's possible with prompts alone. Claude operates in a virtual machine with filesystem access, allowing Skills to exist as directories containing instructions, executable code, and reference materials, organized like an onboarding guide you'd create for a new team member.

This filesystem-based architecture enables **progressive disclosure**: Claude loads information in stages as needed, rather than consuming context upfront.

### Three types of Skill content, three levels of loading

Skills can contain three types of content, each loaded at different times:

### Level 1: Metadata (always loaded)

**Content type: Instructions**. The Skill's YAML frontmatter provides discovery information:

```yaml theme={null}
---
name: pdf-processing
description: Extract text and tables from PDF files, fill forms, merge documents. Use when working with PDF files or when the user mentions PDFs, forms, or document extraction.
---
```

Claude loads this metadata at startup and includes it in the system prompt. This lightweight approach means you can install many Skills without context penalty; Claude only knows each Skill exists and when to use it.

### Level 2: Instructions (loaded when triggered)

**Content type: Instructions**. The main body of SKILL.md contains procedural knowledge: workflows, best practices, and guidance:

````markdown theme={null}
# PDF Processing

## Quick start

Use pdfplumber to extract text from PDFs:

```python
import pdfplumber

with pdfplumber.open("document.pdf") as pdf:
    text = pdf.pages[0].extract_text()
```

For advanced form filling, see [FORMS.md](FORMS.md).
````

When you request something that matches a Skill's description, Claude reads SKILL.md from the filesystem via bash. Only then does this content enter the context window.

### Level 3: Resources and code (loaded as needed)

**Content types: Instructions, code, and resources**. Skills can bundle additional materials:

```
pdf-skill/
├── SKILL.md (main instructions)
├── FORMS.md (form-filling guide)
├── REFERENCE.md (detailed API reference)
└── scripts/
    └── fill_form.py (utility script)
```

**Instructions**: Additional markdown files (FORMS.md, REFERENCE.md) containing specialized guidance and workflows

**Code**: Executable scripts (fill_form.py, validate.py) that Claude runs via bash; scripts provide deterministic operations without consuming context

**Resources**: Reference materials like database schemas, API documentation, templates, or examples

Claude accesses these files only when referenced. The filesystem model means each content type has different strengths: instructions for flexible guidance, code for reliability, resources for factual lookup.

| Level                     | When Loaded             | Token Cost             | Content                                                               |
| ------------------------- | ----------------------- | ---------------------- | --------------------------------------------------------------------- |
| **Level 1: Metadata**     | Always (at startup)     | \~100 tokens per Skill | `name` and `description` from YAML frontmatter                        |
| **Level 2: Instructions** | When Skill is triggered | Under 5k tokens        | SKILL.md body with instructions and guidance                          |
| **Level 3+: Resources**   | As needed               | Effectively unlimited  | Bundled files executed via bash without loading contents into context |

Progressive disclosure ensures only relevant content occupies the context window at any given time.

### The Skills architecture

Skills run in a code execution environment where Claude has filesystem access, bash commands, and code execution capabilities. Think of it like this: Skills exist as directories on a virtual machine, and Claude interacts with them using the same bash commands you'd use to navigate files on your computer.

<img src="https://mintcdn.com/anthropic-claude-docs/4Bny2bjzuGBK7o00/images/agent-skills-architecture.png?fit=max&auto=format&n=4Bny2bjzuGBK7o00&q=85&s=44c5eab950e209f613a5a47f712550dc" alt="Agent Skills Architecture - showing how Skills integrate with the agent's configuration and virtual machine" data-og-width="2048" width="2048" data-og-height="1153" height="1153" data-path="images/agent-skills-architecture.png" data-optimize="true" data-opv="3" srcset="https://mintcdn.com/anthropic-claude-docs/4Bny2bjzuGBK7o00/images/agent-skills-architecture.png?w=280&fit=max&auto=format&n=4Bny2bjzuGBK7o00&q=85&s=fc06568b957c9c3617ea341548799568 280w, https://mintcdn.com/anthropic-claude-docs/4Bny2bjzuGBK7o00/images/agent-skills-architecture.png?w=560&fit=max&auto=format&n=4Bny2bjzuGBK7o00&q=85&s=5569fe72706deda67658467053251837 560w, https://mintcdn.com/anthropic-claude-docs/4Bny2bjzuGBK7o00/images/agent-skills-architecture.png?w=840&fit=max&auto=format&n=4Bny2bjzuGBK7o00&q=85&s=83c04e9248de7082971d623f835c2184 840w, https://mintcdn.com/anthropic-claude-docs/4Bny2bjzuGBK7o00/images/agent-skills-architecture.png?w=1100&fit=max&auto=format&n=4Bny2bjzuGBK7o00&q=85&s=d8e1900f8992d435088a565e098fd32a 1100w, https://mintcdn.com/anthropic-claude-docs/4Bny2bjzuGBK7o00/images/agent-skills-architecture.png?w=1650&fit=max&auto=format&n=4Bny2bjzuGBK7o00&q=85&s=b03b4a5df2a08f4be86889e6158975ee 1650w, https://mintcdn.com/anthropic-claude-docs/4Bny2bjzuGBK7o00/images/agent-skills-architecture.png?w=2500&fit=max&auto=format&n=4Bny2bjzuGBK7o00&q=85&s=b9cab267c168f6a480ba946b6558115c 2500w" />

**How Claude accesses Skill content:**

When a Skill is triggered, Claude uses bash to read SKILL.md from the filesystem, bringing its instructions into the context window. If those instructions reference other files (like FORMS.md or a database schema), Claude reads those files too using additional bash commands. When instructions mention executable scripts, Claude runs them via bash and receives only the output (the script code itself never enters context).

**What this architecture enables:**

**On-demand file access**: Claude reads only the files needed for each specific task. A Skill can include dozens of reference files, but if your task only needs the sales schema, Claude loads just that one file. The rest remain on the filesystem consuming zero tokens.

**Efficient script execution**: When Claude runs `validate_form.py`, the script's code never loads into the context window. Only the script's output (like "Validation passed" or specific error messages) consumes tokens. This makes scripts far more efficient than having Claude generate equivalent code on the fly.

**No practical limit on bundled content**: Because files don't consume context until accessed, Skills can include comprehensive API documentation, large datasets, extensive examples, or any reference materials you need. There's no context penalty for bundled content that isn't used.

This filesystem-based model is what makes progressive disclosure work. Claude navigates your Skill like you'd reference specific sections of an onboarding guide, accessing exactly what each task requires.

### Example: Loading a PDF processing skill

Here's how Claude loads and uses a PDF processing skill:

1. **Startup**: System prompt includes: `PDF Processing - Extract text and tables from PDF files, fill forms, merge documents`
2. **User request**: "Extract the text from this PDF and summarize it"
3. **Claude invokes**: `bash: read pdf-skill/SKILL.md` → Instructions loaded into context
4. **Claude determines**: Form filling is not needed, so FORMS.md is not read
5. **Claude executes**: Uses instructions from SKILL.md to complete the task

<img src="https://mintcdn.com/anthropic-claude-docs/4Bny2bjzuGBK7o00/images/agent-skills-context-window.png?fit=max&auto=format&n=4Bny2bjzuGBK7o00&q=85&s=0127e014bfc3dd3c86567aad8609111b" alt="Skills loading into context window - showing the progressive loading of skill metadata and content" data-og-width="2048" width="2048" data-og-height="1154" height="1154" data-path="images/agent-skills-context-window.png" data-optimize="true" data-opv="3" srcset="https://mintcdn.com/anthropic-claude-docs/4Bny2bjzuGBK7o00/images/agent-skills-context-window.png?w=280&fit=max&auto=format&n=4Bny2bjzuGBK7o00&q=85&s=a17315d47b7c5a85b389026b70676e98 280w, https://mintcdn.com/anthropic-claude-docs/4Bny2bjzuGBK7o00/images/agent-skills-context-window.png?w=560&fit=max&auto=format&n=4Bny2bjzuGBK7o00&q=85&s=267349b063954588d4fae2650cb90cd8 560w, https://mintcdn.com/anthropic-claude-docs/4Bny2bjzuGBK7o00/images/agent-skills-context-window.png?w=840&fit=max&auto=format&n=4Bny2bjzuGBK7o00&q=85&s=0864972aba7bcb10bad86caf82cb415f 840w, https://mintcdn.com/anthropic-claude-docs/4Bny2bjzuGBK7o00/images/agent-skills-context-window.png?w=1100&fit=max&auto=format&n=4Bny2bjzuGBK7o00&q=85&s=631d661cbadcbdb62fd0935b91bd09f8 1100w, https://mintcdn.com/anthropic-claude-docs/4Bny2bjzuGBK7o00/images/agent-skills-context-window.png?w=1650&fit=max&auto=format&n=4Bny2bjzuGBK7o00&q=85&s=c1f80d0e37c517eb335db83615483ae0 1650w, https://mintcdn.com/anthropic-claude-docs/4Bny2bjzuGBK7o00/images/agent-skills-context-window.png?w=2500&fit=max&auto=format&n=4Bny2bjzuGBK7o00&q=85&s=4b6d0f1baf011ff9b49de501d8d83cc7 2500w" />

The diagram shows:

1. Default state with system prompt and skill metadata pre-loaded
2. Claude triggers the skill by reading SKILL.md via bash
3. Claude optionally reads additional bundled files like FORMS.md as needed
4. Claude proceeds with the task

This dynamic loading ensures only relevant skill content occupies the context window.

## Where Skills work

Skills are available across Claude's agent products:

### Claude API

The Claude API supports both pre-built Agent Skills and custom Skills. Both work identically: specify the relevant `skill_id` in the `container` parameter along with the code execution tool.

**Prerequisites**: Using Skills via the API requires three beta headers:

- `code-execution-2025-08-25` - Skills run in the code execution container
- `skills-2025-10-02` - Enables Skills functionality
- `files-api-2025-04-14` - Required for uploading/downloading files to/from the container

Use pre-built Agent Skills by referencing their `skill_id` (e.g., `pptx`, `xlsx`), or create and upload your own via the Skills API (`/v1/skills` endpoints). Custom Skills are shared organization-wide.

To learn more, see [Use Skills with the Claude API](/en/api/skills-guide).

### Claude Code

[Claude Code](/en/docs/claude-code/overview) supports only Custom Skills.

**Custom Skills**: Create Skills as directories with SKILL.md files. Claude discovers and uses them automatically.

Custom Skills in Claude Code are filesystem-based and don't require API uploads.

To learn more, see [Use Skills in Claude Code](/en/docs/claude-code/skills).

### Claude Agent SDK

The [Claude Agent SDK](/en/api/agent-sdk/overview) supports custom Skills through filesystem-based configuration.

**Custom Skills**: Create Skills as directories with SKILL.md files in `.claude/skills/`. Enable Skills by including `"Skill"` in your `allowed_tools` configuration.

Skills in the Agent SDK are then automatically discovered when the SDK runs.

To learn more, see [Agent Skills in the SDK](/en/api/agent-sdk/skills).

### Claude.ai

[Claude.ai](https://claude.ai) supports both pre-built Agent Skills and custom Skills.

**Pre-built Agent Skills**: These Skills are already working behind the scenes when you create documents. Claude uses them without requiring any setup.

**Custom Skills**: Upload your own Skills as zip files through Settings > Features. Available on Pro, Max, Team, and Enterprise plans with code execution enabled. Custom Skills are individual to each user; they are not shared organization-wide and cannot be centrally managed by admins.

To learn more about using Skills in Claude.ai, see the following resources in the Claude Help Center:

- [What are Skills?](https://support.claude.com/en/articles/12512176-what-are-skills)
- [Using Skills in Claude](https://support.claude.com/en/articles/12512180-using-skills-in-claude)
- [How to create custom Skills](https://support.claude.com/en/articles/12512198-creating-custom-skills)
- [Teach Claude your way of working using Skills](https://support.claude.com/en/articles/12580051-teach-claude-your-way-of-working-using-skills)

## Skill structure

Every Skill requires a `SKILL.md` file with YAML frontmatter:

```yaml theme={null}
---
name: your-skill-name
description: Brief description of what this Skill does and when to use it
---

# Your Skill Name

## Instructions
[Clear, step-by-step guidance for Claude to follow]

## Examples
[Concrete examples of using this Skill]
```

**Required fields**: `name` and `description`

**Field requirements**:

`name`:

- Maximum 64 characters
- Must contain only lowercase letters, numbers, and hyphens
- Cannot contain XML tags
- Cannot contain reserved words: "anthropic", "claude"

`description`:

- Must be non-empty
- Maximum 1024 characters
- Cannot contain XML tags

The `description` should include both what the Skill does and when Claude should use it. For complete authoring guidance, see the [best practices guide](/en/docs/agents-and-tools/agent-skills/best-practices).

## Security considerations

We strongly recommend using Skills only from trusted sources: those you created yourself or obtained from Anthropic. Skills provide Claude with new capabilities through instructions and code, and while this makes them powerful, it also means a malicious Skill can direct Claude to invoke tools or execute code in ways that don't match the Skill's stated purpose.

<Warning>
  If you must use a Skill from an untrusted or unknown source, exercise extreme caution and thoroughly audit it before use. Depending on what access Claude has when executing the Skill, malicious Skills could lead to data exfiltration, unauthorized system access, or other security risks.
</Warning>

**Key security considerations**:

- **Audit thoroughly**: Review all files bundled in the Skill: SKILL.md, scripts, images, and other resources. Look for unusual patterns like unexpected network calls, file access patterns, or operations that don't match the Skill's stated purpose
- **External sources are risky**: Skills that fetch data from external URLs pose particular risk, as fetched content may contain malicious instructions. Even trustworthy Skills can be compromised if their external dependencies change over time
- **Tool misuse**: Malicious Skills can invoke tools (file operations, bash commands, code execution) in harmful ways
- **Data exposure**: Skills with access to sensitive data could be designed to leak information to external systems
- **Treat like installing software**: Only use Skills from trusted sources. Be especially careful when integrating Skills into production systems with access to sensitive data or critical operations

## Available Skills

### Pre-built Agent Skills

The following pre-built Agent Skills are available for immediate use:

- **PowerPoint (pptx)**: Create presentations, edit slides, analyze presentation content
- **Excel (xlsx)**: Create spreadsheets, analyze data, generate reports with charts
- **Word (docx)**: Create documents, edit content, format text
- **PDF (pdf)**: Generate formatted PDF documents and reports

These Skills are available on the Claude API and claude.ai. See the [quickstart tutorial](/en/docs/agents-and-tools/agent-skills/quickstart) to start using them in the API.

### Custom Skills examples

For complete examples of custom Skills, see the [Skills cookbook](https://github.com/anthropics/claude-cookbooks/tree/main/skills).

## Limitations and constraints

Understanding these limitations helps you plan your Skills deployment effectively.

### Cross-surface availability

**Custom Skills do not sync across surfaces**. Skills uploaded to one surface are not automatically available on others:

- Skills uploaded to Claude.ai must be separately uploaded to the API
- Skills uploaded via the API are not available on Claude.ai
- Claude Code Skills are filesystem-based and separate from both Claude.ai and API

You'll need to manage and upload Skills separately for each surface where you want to use them.

### Sharing scope

Skills have different sharing models depending on where you use them:

- **Claude.ai**: Individual user only; each team member must upload separately
- **Claude API**: Workspace-wide; all workspace members can access uploaded Skills
- **Claude Code**: Personal (`~/.claude/skills/`) or project-based (`.claude/skills/`)

Claude.ai does not currently support centralized admin management or org-wide distribution of custom Skills.

### Runtime environment constraints

Skills run in the code execution container with these limitations:

- **No network access**: Skills cannot make external API calls or access the internet
- **No runtime package installation**: Only pre-installed packages are available. You cannot install new packages during execution.
- **Pre-configured dependencies only**: Check the [code execution tool documentation](/en/docs/agents-and-tools/tool-use/code-execution-tool) for the list of available packages

Plan your Skills to work within these constraints.

## Next steps

<CardGroup cols={2}>
  <Card title="Get started with Agent Skills" icon="graduation-cap" href="/en/docs/agents-and-tools/agent-skills/quickstart">
    Create your first Skill
  </Card>

  <Card title="API Guide" icon="code" href="/en/api/skills-guide">
    Use Skills with the Claude API
  </Card>

  <Card title="Use Skills in Claude Code" icon="terminal" href="/en/docs/claude-code/skills">
    Create and manage custom Skills in Claude Code
  </Card>

  <Card title="Use Skills in the Agent SDK" icon="cube" href="/en/api/agent-sdk/skills">
    Use Skills programmatically in TypeScript and Python
  </Card>

  <Card title="Authoring best practices" icon="lightbulb" href="/en/docs/agents-and-tools/agent-skills/best-practices">
    Write Skills that Claude can use effectively
  </Card>
</CardGroup>

---
