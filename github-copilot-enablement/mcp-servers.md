# MCP Servers

## Overview
Model Context Protocol (MCP) servers extend Copilot with custom tools and data sources.

## What is MCP?

MCP is a protocol that allows AI assistants to:
- Access external data sources
- Use specialized tools
- Integrate with APIs and services
- Extend functionality beyond built-in capabilities

## Built-in MCP Servers

### GitHub MCP Server
Enabled by default in Copilot CLI.

**Available Tools:**
- Repository operations (list, search, read files)
- Issue management (list, read, search)
- Pull request operations (list, read, diff, review)
- Commit history (list, get details)
- Branch management (list)
- Code search across repositories
- GitHub Actions (workflows, runs, logs)

**Usage Examples:**
```
"List open issues in this repository"
"Show me the diff for PR #123"
"Find all TypeScript files using async/await"
"What were the last 10 commits?"
"Show failed GitHub Actions runs"
```

### Default Tool Subset
For CLI use, only a subset of GitHub tools are enabled by default to reduce complexity:
- Basic repository read operations
- Issue/PR reading
- Code search
- Commit history

### Enabling All GitHub Tools
```bash
# Enable all GitHub MCP tools
copilot --enable-all-github-mcp-tools

# Enable specific toolsets
copilot --add-github-mcp-toolset actions
copilot --add-github-mcp-toolset pull-requests

# Enable specific tools
copilot --add-github-mcp-tool get_workflow_run
copilot --add-github-mcp-tool search_code
```

## Custom MCP Servers

### Configuration File
Location: `~/.copilot/mcp-config.json`

### Basic Structure
```json
{
  "mcpServers": {
    "server-name": {
      "command": "node",
      "args": ["/path/to/server.js"],
      "env": {
        "API_KEY": "your-key-here"
      }
    }
  }
}
```

### Example: Database MCP Server
```json
{
  "mcpServers": {
    "database": {
      "command": "node",
      "args": ["./mcp-servers/database-server.js"],
      "env": {
        "DB_HOST": "localhost",
        "DB_PORT": "5432",
        "DB_NAME": "myapp"
      }
    }
  }
}
```

### Example: API Documentation Server
```json
{
  "mcpServers": {
    "api-docs": {
      "command": "python",
      "args": ["./mcp-servers/api-docs-server.py"],
      "env": {
        "DOCS_PATH": "/path/to/openapi.yaml"
      }
    }
  }
}
```

## Session-Specific MCP Configuration

### Additional MCP Config (Temporary)
```bash
# Add MCP server for this session only
copilot --additional-mcp-config '{"mcpServers":{"temp":{"command":"node","args":["server.js"]}}}'

# Load from file
copilot --additional-mcp-config @/path/to/config.json
```

### Use Cases
- Testing new MCP servers
- Project-specific integrations
- Temporary API access
- Development and debugging

## Creating an MCP Server

### MCP Server Structure
```javascript
// simple-mcp-server.js
const { MCPServer } = require('@modelcontextprotocol/sdk');

const server = new MCPServer({
  name: 'simple-server',
  version: '1.0.0'
});

// Define a tool
server.addTool({
  name: 'get_data',
  description: 'Retrieve data from custom source',
  inputSchema: {
    type: 'object',
    properties: {
      id: { type: 'string', description: 'Data ID' }
    },
    required: ['id']
  },
  handler: async (input) => {
    // Your logic here
    return { data: `Data for ${input.id}` };
  }
});

server.start();
```

### Tool Definition
```javascript
server.addTool({
  name: 'tool-name',
  description: 'Clear description for AI',
  inputSchema: {
    // JSON Schema for inputs
    type: 'object',
    properties: {
      param1: { type: 'string' },
      param2: { type: 'number' }
    },
    required: ['param1']
  },
  handler: async (input) => {
    // Implementation
    return result;
  }
});
```

## Common MCP Server Use Cases

### 1. Internal API Access
```javascript
// company-api-server.js
server.addTool({
  name: 'query_internal_api',
  description: 'Query company internal API',
  handler: async (input) => {
    const response = await fetch(`${INTERNAL_API}/${input.endpoint}`);
    return response.json();
  }
});
```

### 2. Database Queries
```javascript
// db-server.js
server.addTool({
  name: 'query_database',
  description: 'Execute safe read-only database queries',
  handler: async (input) => {
    // Validate query is safe (read-only)
    if (!input.query.toLowerCase().startsWith('select')) {
      throw new Error('Only SELECT queries allowed');
    }
    return await db.query(input.query);
  }
});
```

### 3. Documentation Access
```javascript
// docs-server.js
server.addTool({
  name: 'search_docs',
  description: 'Search internal documentation',
  handler: async (input) => {
    return await searchDocs(input.query);
  }
});
```

### 4. Custom Code Analysis
```javascript
// analyzer-server.js
server.addTool({
  name: 'analyze_dependencies',
  description: 'Analyze project dependencies',
  handler: async (input) => {
    const packageJson = JSON.parse(fs.readFileSync('package.json'));
    return analyzeDependencies(packageJson);
  }
});
```

## Managing MCP Servers

### Disabling MCP Servers
```bash
# Disable all built-in MCPs
copilot --disable-builtin-mcps

# Disable specific MCP
copilot --disable-mcp-server github-mcp-server
copilot --disable-mcp-server database
```

### Listing Active Servers
```bash
# Check logs
cat ~/.copilot/logs/latest.log | grep "MCP"

# Look for loaded servers
cat ~/.copilot/logs/latest.log | grep "Loading MCP server"
```

## Security Considerations

### Best Practices
1. **Validate Inputs**: Always sanitize and validate inputs
2. **Read-Only by Default**: Limit write operations
3. **Authentication**: Use API keys, tokens
4. **Rate Limiting**: Prevent abuse
5. **Error Handling**: Don't expose sensitive errors
6. **Audit Logging**: Log all operations

### Example: Secure Server
```javascript
server.addTool({
  name: 'query_api',
  handler: async (input) => {
    // Validate input
    if (!input.id || typeof input.id !== 'string') {
      throw new Error('Invalid input');
    }
    
    // Sanitize
    const safeId = sanitize(input.id);
    
    // Rate limit
    await checkRateLimit(safeId);
    
    // Authenticate
    const headers = {
      'Authorization': `Bearer ${API_TOKEN}`
    };
    
    // Execute
    try {
      const response = await fetch(`${API_URL}/${safeId}`, { headers });
      return response.json();
    } catch (error) {
      // Don't expose internal errors
      throw new Error('Query failed');
    }
  }
});
```

## Tool Permissions

### Allowing MCP Tools
```bash
# Allow specific MCP tool
copilot --allow-tool 'database(query_database)'

# Allow all tools from MCP server
copilot --allow-tool 'database'

# Deny specific tool but allow others
copilot --allow-tool 'database' --deny-tool 'database(write_data)'
```

### Permission Syntax
```
'mcp-server-name(tool-name)'
'mcp-server-name'  # all tools from server
```

## Debugging MCP Servers

### Enable Debug Logging
```bash
copilot --log-level debug
```

### Check Logs
```bash
# View latest log
tail -f ~/.copilot/logs/latest.log

# Search for MCP errors
grep -i "mcp\|error" ~/.copilot/logs/latest.log
```

### Common Issues

**Server Not Loading**
```bash
# Check config syntax
cat ~/.copilot/mcp-config.json | jq .

# Verify command exists
which node
which python
```

**Tool Not Available**
```bash
# Verify tool is registered
# Check server logs
# Ensure tool description is clear
```

**Authentication Errors**
```bash
# Check environment variables
echo $API_KEY

# Verify credentials in config
```

## Example MCP Servers

### Jira Integration
```json
{
  "mcpServers": {
    "jira": {
      "command": "node",
      "args": ["./mcp-servers/jira-server.js"],
      "env": {
        "JIRA_URL": "https://company.atlassian.net",
        "JIRA_TOKEN": "your-token"
      }
    }
  }
}
```

Usage:
```
"List all open tickets assigned to me in Jira"
"Create a bug ticket for this issue"
```

### Confluence Docs
```json
{
  "mcpServers": {
    "confluence": {
      "command": "python",
      "args": ["./mcp-servers/confluence-server.py"],
      "env": {
        "CONFLUENCE_URL": "https://company.atlassian.net/wiki",
        "CONFLUENCE_TOKEN": "your-token"
      }
    }
  }
}
```

Usage:
```
"Search Confluence for API documentation"
"What does our security policy say about authentication?"
```

## Advanced Configuration

### Multiple Environments
```json
{
  "mcpServers": {
    "database-dev": {
      "command": "node",
      "args": ["./db-server.js"],
      "env": { "DB_ENV": "development" }
    },
    "database-prod": {
      "command": "node",
      "args": ["./db-server.js"],
      "env": { "DB_ENV": "production" }
    }
  }
}
```

### Conditional Loading
Use session-specific config for environment-based loading:
```bash
# Development
copilot --additional-mcp-config @./mcp-dev.json

# Production
copilot --additional-mcp-config @./mcp-prod.json
```

## Resources

### MCP SDK
- **Node.js**: `@modelcontextprotocol/sdk`
- **Python**: `mcp` package
- **Documentation**: https://modelcontextprotocol.io

### Example Servers
- GitHub: Built-in reference implementation
- Community servers: https://github.com/topics/mcp-server
