# comet-mcp

[![npm version](https://img.shields.io/npm/v/comet-mcp.svg)](https://www.npmjs.com/package/comet-mcp)

**Give Claude Code a browser that thinks.**

An MCP server that connects Claude Code to [Perplexity Comet](https://www.perplexity.ai/comet) - enabling agentic web browsing, deep research, and real-time task monitoring.

![Demo](demo.gif)

## Why?

Existing web tools for Claude Code fall short:
- **WebSearch/WebFetch** only return static text - no interaction, no login, no dynamic content
- **Browser automation MCPs** (like browser-use) are agentic but use a generic LLM to control a browser - less polished, more fragile

**Comet is Perplexity's native agentic browser** - their AI is purpose-built for web research, deeply integrated with search, and battle-tested. Give it a goal, it figures out how to get there.

**comet-mcp** bridges Claude Code and Comet: Claude's coding intelligence + Perplexity's web intelligence.

## Quick Start

### 1. Configure Claude Code

Add to `~/.claude.json` or `.mcp.json`:

```json
{
  "mcpServers": {
    "comet-bridge": {
      "command": "npx",
      "args": ["-y", "comet-mcp"]
    }
  }
}
```

### 2. Start Comet Browser

Download [Perplexity Comet](https://www.perplexity.ai/comet) and launch with remote debugging:

```bash
# macOS
/Applications/Comet.app/Contents/MacOS/Comet --remote-debugging-port=9222
```

### 3. Use in Claude Code

```
You: "Use Comet to research the top AI frameworks in 2025"
Claude: [connects to Comet, delegates research, monitors progress, returns results]
```

## Local Development Setup

If you want to clone this MCP server and make your own modifications, follow these steps:

### 1. Clone and Set Up the Repository

```bash
# Clone your fork
git clone https://github.com/YOUR-USERNAME/comet-mcp.git
cd comet-mcp

# Install dependencies
npm install

# Build the project
npm run build
```

### 2. Configure Claude Code to Use Your Local Version

Edit Claude Code's MCP configuration file at `~/.claude.json`:

```json
{
  "mcpServers": {
    "comet-mcp": {
      "type": "stdio",
      "command": "node",
      "args": ["/absolute/path/to/comet-mcp/dist/index.js"],
      "env": {}
    }
  }
}
```

Replace `/absolute/path/to/comet-mcp` with the actual path where you cloned the repo.

### 3. Restart Claude Code

After updating the config, restart Claude Code to load your local MCP server.

### 4. Development Workflow

When making changes:

```bash
# Make your code changes in src/

# Rebuild
npm run build

# Restart Claude Code to pick up changes
```

### 5. Testing Your Changes

You can test the MCP server directly without Claude Code:

```bash
# Run the server
node dist/index.js

# Or use the MCP inspector for debugging
npx @modelcontextprotocol/inspector node dist/index.js
```

The inspector gives you a UI to test MCP tools interactively.

### Development Tips

- Keep the terminal open where you run builds to see any TypeScript errors
- Check Claude Code logs if the server doesn't connect (usually in `~/.claude/debug/`)
- You can have multiple MCP servers configured - just give them different names in the config
- The built files are in `dist/` directory (not `build/`)

## Tools

| Tool | Description |
|------|-------------|
| `comet_connect` | Connect to Comet (auto-starts if needed) |
| `comet_ask` | Send a task and wait for response |
| `comet_poll` | Check task progress |
| `comet_stop` | Stop current task |
| `comet_screenshot` | Capture current page |
| `comet_mode` | Switch modes: search, research, labs, learn |

## Architecture

```
Claude Code <-> MCP <-> comet-mcp <-> CDP <-> Comet Browser <-> Perplexity AI
```

## Requirements

- Node.js 18+
- [Perplexity Comet Browser](https://www.perplexity.ai/comet)
- Claude Code (or any MCP client)

## Troubleshooting

**"Cannot connect to Comet"**
- Make sure Comet is running with `--remote-debugging-port=9222`
- Check if port 9222 is available

**"Tools not showing in Claude"**
- Restart Claude Code after config changes

## License

MIT

---

[Report Issues](https://github.com/hanzili/comet-mcp/issues) · [Contribute](https://github.com/hanzili/comet-mcp)
