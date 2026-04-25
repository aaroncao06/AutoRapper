# MCP Server Suggestions for ML Research

Useful MCP (Model Context Protocol) servers that can enhance the research workflow. Configure these in `.mcp.json` at the project root or `~/.claude/.mcp.json` for all projects.

## Semantic Scholar

Provides programmatic access to academic paper search, citation graphs, and paper metadata. Useful for `/lit-review` and `/plan-task` Phase 3 (Literature & Baselines).

**API (no auth required for basic queries):**
```
https://api.semanticscholar.org/graph/v1/paper/search?query={topic}&limit=20&fields=title,authors,year,abstract,citationCount,venue
```

**MCP config (when a package becomes available):**
```json
{
  "mcpServers": {
    "semantic-scholar": {
      "type": "stdio",
      "command": "npx",
      "args": ["semantic-scholar-mcp"]
    }
  }
}
```

**Current workaround:** Use WebFetch to query the API directly. The `/lit-review` command does this automatically.

## GitHub

If your research code is hosted on GitHub, the GitHub MCP server enables direct PR creation, issue management, and code search.

```json
{
  "mcpServers": {
    "github": {
      "type": "stdio",
      "command": "npx",
      "args": ["@anthropic/github-mcp"],
      "env": {
        "GITHUB_TOKEN": "$GITHUB_TOKEN"
      }
    }
  }
}
```

## Filesystem (Enhanced)

For projects with large file trees where the built-in file tools are slow.

```json
{
  "mcpServers": {
    "filesystem": {
      "type": "stdio",
      "command": "npx",
      "args": ["@anthropic/filesystem-mcp", "--root", "."]
    }
  }
}
```

## How to Configure

1. Create `.mcp.json` in your project root (project-scoped) or `~/.claude/.mcp.json` (global)
2. Add the server configs above
3. Claude Code will automatically discover and connect to them
4. Verify with `/mcp` command in a session

## Notes

- MCP servers are still evolving. Check the [MCP registry](https://github.com/modelcontextprotocol/servers) for the latest available servers.
- For Semantic Scholar, the free API has rate limits (100 requests/5 minutes). Sufficient for literature reviews but not bulk scraping.
- API keys should be stored in `.env` and referenced via `$ENV_VAR` syntax in `.mcp.json`.
