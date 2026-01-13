# MCP Server Builder for SpecKit

This directory contains reference documentation and helper scripts for building high-quality MCP (Model Context Protocol) servers within the SpecKit framework.

## Overview

The MCP Builder agent (`speckit.mcp_builder`) helps you create MCP servers that enable LLMs to interact with external services through well-designed tools. Whether you're integrating GitHub, Slack, databases, or any other API, this agent guides you through a proven four-phase development process.

## Directory Structure

```
.specify/scripts/mcp/
├── README.md                           # This file
├── SKILL.md                            # Complete development guide
├── reference/
│   ├── evaluation.md                   # Evaluation creation guide
│   ├── mcp_best_practices.md          # Universal MCP guidelines
│   ├── node_mcp_server.md             # TypeScript patterns and examples
│   └── python_mcp_server.md           # Python patterns and examples
└── scripts/
    ├── evaluation.py                   # Run evaluations against MCP servers
    ├── connections.py                  # Helper for managing MCP connections
    ├── example_evaluation.xml          # Example evaluation format
    └── requirements.txt                # Python dependencies for scripts
```

## Quick Start

### Create an MCP Server

Use the agent to build a complete MCP server:

```bash
@speckit.mcp_builder Create an MCP server for the GitHub API with support for:
- Listing repositories
- Creating issues
- Managing pull requests
- Searching code
```

The agent will:
1. Research the GitHub API and MCP best practices
2. Create complete project structure (TypeScript or Python)
3. Implement all tools with proper schemas
4. Build and test the server
5. Create 10 evaluation questions
6. Deliver a working MCP server

### Example Output

You'll receive:
- Complete source code for the MCP server
- Package configuration (package.json or requirements.txt)
- README with installation and usage instructions
- evaluation.xml with 10 test questions
- Configuration examples

## Four-Phase Development Process

### Phase 1: Research and Planning
- Study MCP protocol documentation
- Understand the target API
- Plan tool coverage and naming
- Design error handling strategy

### Phase 2: Implementation
- Set up project structure
- Implement core infrastructure (auth, error handling)
- Create individual tools with schemas
- Add proper annotations (readOnly, destructive, etc.)

### Phase 3: Review and Test
- Code quality review (DRY, types, errors)
- Build verification (npm run build or python compile)
- Interactive testing with MCP Inspector
- Quality checklist validation

### Phase 4: Evaluations
- Create 10 complex, realistic questions
- Verify answers through exploration
- Generate evaluation.xml
- Optional: Run automated evaluation

## Language Support

### TypeScript (Recommended)
- High-quality SDK support
- Excellent type safety
- Broad compatibility with MCP clients
- Strong AI model support for code generation

**Example Structure:**
```
my-mcp-server/
├── package.json
├── tsconfig.json
├── src/
│   ├── index.ts           # Main server
│   ├── types.ts           # Type definitions
│   ├── client.ts          # API client
│   └── tools/
│       ├── list.ts        # List operations
│       ├── create.ts      # Create operations
│       └── search.ts      # Search operations
└── evaluation.xml
```

### Python
- Fast development with FastMCP
- Clean async/await syntax
- Excellent for data processing

**Example Structure:**
```
my_mcp_server/
├── requirements.txt
├── server.py              # Main server
├── models.py              # Pydantic models
├── client.py              # API client
├── tools/
│   ├── __init__.py
│   ├── list_tools.py
│   ├── create_tools.py
│   └── search_tools.py
└── evaluation.xml
```

## Key Design Principles

### 1. Comprehensive API Coverage
Prioritize exposing the full API surface area. Let agents compose operations flexibly rather than creating rigid workflows.

### 2. Clear Tool Naming
- Use consistent prefixes: `github_create_issue`, `github_list_repos`
- Action-oriented: create, list, get, update, delete
- Descriptive and discoverable

### 3. Actionable Error Messages
```typescript
// Bad
throw new Error("Request failed");

// Good
throw new Error(
  "Failed to create issue: Repository 'user/repo' not found. " +
  "Check that the repository exists and you have access."
);
```

### 4. Proper Response Formatting
- **JSON** for structured data agents process
- **Markdown** for human-readable summaries
- Use `structuredContent` when available

### 5. Security Best Practices
- Never log sensitive data
- Validate all inputs
- Use environment variables for credentials
- Follow principle of least privilege

## Testing Your MCP Server

### With MCP Inspector (Recommended)

```bash
# For TypeScript servers
npm run build
npx @modelcontextprotocol/inspector

# For Python servers
python -m py_compile server.py
# Use MCP Inspector or test directly
```

### With Evaluations

```bash
# Run the evaluation suite
python .specify/scripts/mcp/scripts/evaluation.py \
  --server path/to/your/server \
  --eval evaluation.xml
```

The evaluation script will:
- Connect to your MCP server
- Ask each question from evaluation.xml
- Compare answers to expected results
- Report pass/fail for each question

## Example MCP Servers

### GitHub Integration
```
Tools:
- github_list_repos: List user/org repositories
- github_create_issue: Create new issue
- github_search_code: Search code across repos
- github_list_prs: List pull requests
- github_create_pr: Create pull request
```

### Slack Integration
```
Tools:
- slack_list_channels: List available channels
- slack_send_message: Post message to channel
- slack_search_messages: Search message history
- slack_list_users: List workspace members
```

### Database Connector
```
Tools:
- db_list_tables: List database tables
- db_query: Execute SELECT query
- db_get_schema: Get table schema
- db_insert: Insert records
- db_update: Update records
```

## Reference Documentation

### Core Guides
- [SKILL.md](./SKILL.md) - Complete MCP server development guide
- [mcp_best_practices.md](./reference/mcp_best_practices.md) - Universal guidelines

### Language-Specific
- [node_mcp_server.md](./reference/node_mcp_server.md) - TypeScript patterns
- [python_mcp_server.md](./reference/python_mcp_server.md) - Python patterns

### Advanced Topics
- [evaluation.md](./reference/evaluation.md) - Creating effective evaluations

## Helper Scripts

### evaluation.py
Run automated evaluations against your MCP server:

```python
python .specify/scripts/mcp/scripts/evaluation.py \
  --server ./my-server \
  --eval evaluation.xml \
  --output results.json
```

### connections.py
Helper utilities for managing MCP server connections in your tests.

## Quality Checklist

Before delivering an MCP server, verify:

- [ ] All planned tools implemented
- [ ] Successfully builds without errors
- [ ] All tools have clear descriptions
- [ ] Input schemas use proper validation
- [ ] Output schemas defined where applicable
- [ ] Error messages are actionable
- [ ] Authentication properly configured
- [ ] Pagination supported where needed
- [ ] Tool annotations complete (readOnly, destructive, etc.)
- [ ] README with installation instructions
- [ ] 10 evaluation questions created
- [ ] All evaluation answers verified

## Dependencies

### For Development
- **TypeScript**: @modelcontextprotocol/sdk, zod
- **Python**: fastmcp, httpx, pydantic

### For Testing
```bash
npm install -g @modelcontextprotocol/inspector
```

### For Helper Scripts
```bash
pip install -r .specify/scripts/mcp/scripts/requirements.txt
```

## Getting Help

When using `@speckit.mcp_builder`:
- Be specific about which API/service to integrate
- Mention preferred language (TypeScript or Python)
- List key operations you want to support
- Specify any special requirements (auth, pagination, etc.)

The agent will handle the complete development process and deliver a working MCP server ready to use with Claude Desktop or other MCP clients.

## Source

These resources are adapted from [Anthropic's MCP Builder Skill](https://github.com/anthropics/skills/tree/main/skills/mcp-builder) to work within the SpecKit framework for GitHub Copilot-based development.
