# 🔧 MCP Server Configuration Guide

> **Note:** MagicAi (Fitness Life Mantra) is built on the Neocortex framework. This guide uses "Neocortex" to refer to the underlying platform architecture.

> This guide explains how to add MCP servers to MagicAi/Neocortex by defining their configuration in JSON format. Each MCP server entry is stored in the database and supports different transport types: `stdio`, `SSE`, and `StreamableHTTP`.

## 🛡️ MCP Philosophy: Local-First & Private

MagicAi (powered by Neocortex) is designed for **local-first, private use**. We prioritize:

- **Local MCP servers** running in your network (via `stdio`)
- **Environment-based secrets** (never hardcoded tokens)
- **Explicit external server warnings** (⚠️ for remote/hosted MCPs)
- **No external registries** - you control what connects to your AI assistant

### Installing MCP Servers

To add an MCP server:

1. **Run the MCP server locally** (via `npx`, `npm`, `pnpm`, `uv`, `docker`, etc.)
2. **Add its configuration** through the UI (Settings → MCP Servers)
3. **Use environment variables** for secrets (never paste tokens directly)
4. **Enable/test** the server - no restart needed

---

You can add new MCP servers effortlessly through the UI — no need to restart the app. Each tool is available instantly and can be tested independently outside of chat. This is perfect for quick debugging and reliable development workflows.

![add-mcp-server](https://github.com/user-attachments/assets/f66ae118-883e-4638-b4fc-9f9849566da2)

<br/>

<br/>

## 🖥️ Stdio Type

Used for locally executed tools that run via a command-line interface.

**Example:**

```json
{
  "command": "npx",
  "args": ["@playwright/mcp@latest"]
}
```

- `command`: Required. The CLI command to launch the server.
- `args`: Optional. A list of arguments to pass to the command.

## 🌐 SSE / StreamableHTTP Type

Used for remote servers that communicate via HTTP (SSE or streaming).

**Example:**

```json
{
  "url": "https://api.example.com",
  "headers": {
    "Authorization": "Bearer sk-..."
  }
}
```

- `url`: Required. The endpoint to connect to.
- `headers`: Optional. HTTP headers such as authorization tokens.

You don't need to specify the transport type manually — it is inferred based on the structure:

- If `command` is present → it's a `stdio` config
- If `url` is present → it's a `SSE` or `StreamableHTTP` config

## 💾 File-based Configuration (for local dev)

By default, MCP server configs are stored in the database.
However, for local development, you can also use a file-based approach by enabling the following setting:

```env
# Whether to use file-based MCP config (default: false)
FILE_BASED_MCP_CONFIG=true
```

Then, create a `.mcp-config.json` file in the project root and define your servers there. Example:

```jsonc
// .mcp-config.json
{
  "playwright": {
    "command": "npx",
    "args": ["@playwright/mcp@latest"]
  }
}
```

Simply paste your configuration in the MCP Configuration form (or .mcp-config.json) to register a new tool.
