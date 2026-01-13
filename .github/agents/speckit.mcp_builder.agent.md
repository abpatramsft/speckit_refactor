---
description: Build high-quality MCP (Model Context Protocol) servers that enable LLMs to interact with external services through well-designed tools. Use this when the user asks to create an MCP server, integrate an external API via MCP, or build tools for Claude Desktop/other MCP clients (examples include building GitHub MCP server, Slack integration, database connectors, or any service integration).
---

## CRITICAL OPERATING PRINCIPLE

**YOU MUST BUILD AND DELIVER A WORKING MCP SERVER**

- **DO NOT** just provide instructions or documentation references
- **DO NOT** stop after planning without implementation
- **DO** create the complete project structure with all files
- **DO** implement all tools with proper schemas and error handling
- **DO** run `npm run build` or `python -m py_compile` to verify compilation
- **DO** create evaluations with 10 test questions
- **The user should receive a working MCP server, not just a guide**

## User Input

```text
$ARGUMENTS
```

You **MUST** consider the user input before proceeding (if not empty).

The user provides MCP server requirements: which service to integrate, what operations to support, preferred language (TypeScript/Python), and any specific features needed.

## Overview

Create MCP (Model Context Protocol) servers that enable LLMs to interact with external services through well-designed tools. The quality of an MCP server is measured by how well it enables LLMs to accomplish real-world tasks.

All reference documentation and scripts are located in `.specify/scripts/mcp/` directory.

## Four-Phase Development Process

### Phase 1: Deep Research and Planning

#### 1.1 Understand Modern MCP Design

**API Coverage vs. Workflow Tools:**
Balance comprehensive API endpoint coverage with specialized workflow tools. Workflow tools can be more convenient for specific tasks, while comprehensive coverage gives agents flexibility to compose operations. When uncertain, prioritize comprehensive API coverage.

**Tool Naming and Discoverability:**
Clear, descriptive tool names help agents find the right tools quickly. Use consistent prefixes (e.g., `github_create_issue`, `github_list_repos`) and action-oriented naming.

**Context Management:**
Agents benefit from concise tool descriptions and the ability to filter/paginate results. Design tools that return focused, relevant data.

**Actionable Error Messages:**
Error messages should guide agents toward solutions with specific suggestions and next steps.

#### 1.2 Study MCP Protocol Documentation

**Navigate the MCP specification:**

Start with the sitemap to find relevant pages: `https://modelcontextprotocol.io/sitemap.xml`

Then fetch specific pages with `.md` suffix for markdown format (e.g., `https://modelcontextprotocol.io/specification/draft.md`).

Key pages to review:
- Specification overview and architecture
- Transport mechanisms (streamable HTTP, stdio)
- Tool, resource, and prompt definitions

#### 1.3 Study Framework Documentation

**Recommended stack:**
- **Language**: TypeScript (high-quality SDK support, broad compatibility)
- **Transport**: Streamable HTTP for remote servers (stateless JSON), stdio for local servers

**Load framework documentation:**

- **MANDATORY - READ**: `.specify/scripts/mcp/reference/mcp_best_practices.md` - Core guidelines
- **For TypeScript**: Fetch `https://raw.githubusercontent.com/modelcontextprotocol/typescript-sdk/main/README.md`
- **For TypeScript**: Read `.specify/scripts/mcp/reference/node_mcp_server.md` - TypeScript patterns
- **For Python**: Fetch `https://raw.githubusercontent.com/modelcontextprotocol/python-sdk/main/README.md`
- **For Python**: Read `.specify/scripts/mcp/reference/python_mcp_server.md` - Python patterns

#### 1.4 Plan Your Implementation

**Understand the API:**
Review the service's API documentation to identify key endpoints, authentication requirements, and data models. Use web search and WebFetch as needed.

**Tool Selection:**
Prioritize comprehensive API coverage. List endpoints to implement, starting with the most common operations.

### Phase 2: Implementation

#### 2.1 Set Up Project Structure

**For TypeScript:**
- Create package.json with MCP SDK dependency
- Set up tsconfig.json with proper compiler options
- Create src/ directory for source files
- Add build scripts to package.json

**For Python:**
- Create virtual environment setup
- Add dependencies (fastmcp, httpx, pydantic)
- Create module structure
- Add requirements.txt

See language-specific guides in `.specify/scripts/mcp/reference/` for detailed structure.

#### 2.2 Implement Core Infrastructure

Create shared utilities:
- API client with authentication (headers, tokens, etc.)
- Error handling helpers (proper error classes, actionable messages)
- Response formatting (JSON/Markdown conversion)
- Pagination support (cursors, page numbers)

#### 2.3 Implement Tools

For each tool:

**Input Schema:**
- Use Zod (TypeScript) or Pydantic (Python)
- Include constraints and clear descriptions
- Add examples in field descriptions

**Output Schema:**
- Define `outputSchema` where possible for structured data
- Use `structuredContent` in tool responses (TypeScript SDK feature)
- Helps clients understand and process tool outputs

**Tool Description:**
- Concise summary of functionality
- Parameter descriptions
- Return type schema

**Implementation:**
- Async/await for I/O operations
- Proper error handling with actionable messages
- Support pagination where applicable
- Return both text content and structured data when using modern SDKs

**Annotations:**
- `readOnlyHint`: true/false
- `destructiveHint`: true/false
- `idempotentHint`: true/false
- `openWorldHint`: true/false

### Phase 3: Review and Test

#### 3.1 Code Quality Review

Review for:
- No duplicated code (DRY principle)
- Consistent error handling across all tools
- Full type coverage (TypeScript) or type hints (Python)
- Clear, action-oriented tool descriptions

#### 3.2 Build and Test

**For TypeScript:**
- **EXECUTE IMMEDIATELY**: `npm install` to install dependencies
- **EXECUTE IMMEDIATELY**: `npm run build` to verify compilation
- **DO NOT** ask user to run - execute yourself
- Fix any compilation errors
- **EXECUTE IMMEDIATELY**: `npx @modelcontextprotocol/inspector` for interactive testing
- Test each tool with various inputs

**For Python:**
- **EXECUTE IMMEDIATELY**: `python -m py_compile <your_server>.py` to verify syntax
- **DO NOT** ask user to run - execute yourself
- Fix any syntax errors
- Install and test with MCP Inspector if available
- Test each tool with various inputs

#### 3.3 Verify Against Quality Checklist

**Universal Requirements:**
- [ ] All tools have clear, action-oriented names
- [ ] Tool descriptions are concise and informative
- [ ] Input schemas use proper validation
- [ ] Error messages are actionable and specific
- [ ] Pagination is supported where needed
- [ ] Response formats are consistent (JSON or Markdown)
- [ ] Authentication is properly implemented
- [ ] All tools have appropriate annotations (readOnly, destructive, etc.)

**TypeScript-Specific:**
- [ ] All types are properly defined
- [ ] No TypeScript errors on build
- [ ] Output schemas defined where applicable
- [ ] Structured content used in responses

**Python-Specific:**
- [ ] Type hints are complete
- [ ] Pydantic models for all schemas
- [ ] No syntax errors on compile
- [ ] Proper async/await usage

### Phase 4: Create Evaluations

#### 4.1 Read Evaluation Guide

**MANDATORY - READ ENTIRE FILE**: Read `.specify/scripts/mcp/reference/evaluation.md` completely from start to finish. **NEVER set any range limits when reading this file.**

#### 4.2 Create 10 Evaluation Questions

Follow the process outlined in the evaluation guide:

1. **Tool Inspection**: List available tools and understand their capabilities
2. **Content Exploration**: Use READ-ONLY operations to explore available data
3. **Question Generation**: Create 10 complex, realistic questions
4. **Answer Verification**: Solve each question yourself to verify answers

#### 4.3 Evaluation Requirements

Ensure each question is:
- **Independent**: Not dependent on other questions
- **Read-only**: Only non-destructive operations required
- **Complex**: Requiring multiple tool calls and deep exploration
- **Realistic**: Based on real use cases humans would care about
- **Verifiable**: Single, clear answer that can be verified by string comparison
- **Stable**: Answer won't change over time

#### 4.4 Create Evaluation XML File

Create an XML file named `evaluation.xml` with this structure:

```xml
<evaluation>
  <qa_pair>
    <question>First complex question requiring multiple tool calls...</question>
    <answer>Expected answer (exact string match)</answer>
  </qa_pair>
  <qa_pair>
    <question>Second complex question...</question>
    <answer>Expected answer</answer>
  </qa_pair>
  <!-- 8 more qa_pairs... total of 10 -->
</evaluation>
```

**DO NOT** ask user to create this - create it yourself based on your exploration of the MCP server's capabilities.

#### 4.5 Optional: Run Evaluation

If you want to test the evaluation:
- **EXECUTE**: `python .specify/scripts/mcp/scripts/evaluation.py --server <path-to-server> --eval evaluation.xml`
- Review results and adjust questions if needed

## Deliverables Checklist

At the end, ensure you've delivered:

1. **Complete Project Structure**
   - [ ] All source files created
   - [ ] Package.json/requirements.txt with dependencies
   - [ ] README.md with usage instructions
   - [ ] Configuration files (tsconfig.json, etc.)

2. **Working MCP Server**
   - [ ] All planned tools implemented
   - [ ] Successfully builds without errors
   - [ ] Authentication properly configured
   - [ ] Error handling comprehensive

3. **Evaluation Suite**
   - [ ] evaluation.xml with 10 complex questions
   - [ ] All answers verified as correct
   - [ ] Questions are realistic and independent

4. **Documentation**
   - [ ] README explains how to install and run
   - [ ] Tool descriptions are clear
   - [ ] Configuration instructions provided
   - [ ] Example usage included

## Final Delivery

Report to the user:
- Location of the MCP server files
- How to install dependencies
- How to run the server
- How to test with MCP Inspector
- How to run evaluations
- **The user should have a complete, working MCP server ready to use**

## Key Design Principles

### Tool Naming
- Use consistent prefixes for related tools
- Action-oriented names (create, list, get, update, delete)
- Clear and descriptive (avoid abbreviations)

### Error Handling
- Catch and wrap API errors with context
- Provide actionable error messages
- Include specific suggestions for resolution
- Log errors appropriately

### Response Formatting
- **JSON** for structured data that agents process
- **Markdown** for human-readable summaries
- Include both when using modern SDKs (structuredContent)
- Support pagination for large datasets

### Security
- Never log sensitive data (tokens, passwords)
- Validate all inputs rigorously
- Use environment variables for credentials
- Follow principle of least privilege

### Performance
- Use async/await for all I/O operations
- Implement connection pooling where applicable
- Cache responses when appropriate
- Support batch operations where possible

## Reference Documentation

All documentation is in `.specify/scripts/mcp/`:

- [SKILL.md](.specify/scripts/mcp/SKILL.md) - Complete development guide
- [reference/mcp_best_practices.md](.specify/scripts/mcp/reference/mcp_best_practices.md) - Universal MCP guidelines
- [reference/node_mcp_server.md](.specify/scripts/mcp/reference/node_mcp_server.md) - TypeScript patterns and examples
- [reference/python_mcp_server.md](.specify/scripts/mcp/reference/python_mcp_server.md) - Python patterns and examples
- [reference/evaluation.md](.specify/scripts/mcp/reference/evaluation.md) - Evaluation creation guide

## Helper Scripts

Available in `.specify/scripts/mcp/scripts/`:

- **evaluation.py** - Run evaluations against your MCP server
- **connections.py** - Helper for managing MCP connections
- **example_evaluation.xml** - Example evaluation format
- **requirements.txt** - Python dependencies for scripts

## Dependencies

**For TypeScript Projects:**
```json
{
  "@modelcontextprotocol/sdk": "^1.0.0",
  "zod": "^3.22.0"
}
```

**For Python Projects:**
```txt
fastmcp
httpx
pydantic
```

**For Testing:**
```bash
npm install -g @modelcontextprotocol/inspector
```

Remember: The goal is to create an MCP server that enables LLMs to effectively accomplish real-world tasks with the external service. Focus on comprehensive API coverage, clear tool design, and proper error handling.
