# LLM Context Files

This project contains structured context files designed to help Large Language Models (LLMs) understand and work with specific programming languages and frameworks.

## About llms.txt Files

The `llms.txt` format is a standardized approach for providing LLM-friendly documentation and resources, as specified at [llmstxt.org](https://llmstxt.org/). These files offer:

- **Curated Resources**: Expert-selected articles, books, and tutorials
- **LLM-Optimized Format**: Structured markdown that's easy for AI models to parse
- **Contextual Learning**: Focused collections that teach best practices and idiomatic patterns
- **Inference-Time Help**: Resources specifically chosen to assist with coding tasks

## rust-llms.txt

The `rust-llms.txt` file provides comprehensive context for writing idiomatic Rust code.

### Purpose

This file helps LLMs understand and generate:
- **Idiomatic Rust code** following community best practices
- **Proper error handling** patterns and techniques
- **Memory-safe** and performance-oriented solutions
- **Library design** following Rust API guidelines
- **Ownership and borrowing** patterns that avoid common pitfalls

### Content Overview

The file organizes Rust learning resources into several categories:

#### Core Learning Resources
- Official Rust documentation (The Book, Rustonomicon, etc.)
- Essential tools (clippy, blessed.rs, cheats.rs)
- Design patterns and API guidelines

#### Articles and Guides
- 37+ carefully selected articles covering idiomatic Rust patterns
- String handling, error management, and conversion techniques
- Advanced topics like state machines and parallel programming
- Historical perspective from 2015-2023 of Rust evolution

#### Interactive Learning
- 10 hands-on workshops and coding exercises
- Project-based learning (JIRA clone, text editor, etc.)
- University courses and professional training materials

#### Books and In-Depth Guides
- 9 comprehensive books for all skill levels
- From beginner-friendly guides to advanced systems programming
- Specialized topics: CLI apps, web development, embedded systems

#### Community Resources
- Forum discussions on common patterns and trade-offs
- Real-world code examples and repositories
- Conference talks from Rust experts

### Source

This file was curated from the excellent [idiomatic-rust](https://github.com/mre/idiomatic-rust) repository by Matthias Endler, which maintains a peer-reviewed collection of resources for writing concise, elegant Rust code.

### Usage

#### For LLM Applications
Include this file in your LLM context when:
- Writing Rust code that needs to follow best practices
- Reviewing existing Rust code for improvements
- Learning about specific Rust patterns or techniques
- Building Rust libraries or applications

#### For Developers
Use this as a comprehensive reference for:
- Finding authoritative resources on Rust topics
- Understanding the evolution of Rust best practices
- Discovering new techniques and patterns
- Building a systematic approach to learning Rust

### Serving via MCP (Model Context Protocol)

You can serve this `rust-llms.txt` file to IDEs and AI applications using the [mcpdoc MCP server](https://github.com/langchain-ai/mcpdoc). This provides full control over how the documentation is retrieved and allows you to audit tool calls.

#### Installation

Install mcpdoc using uvx (recommended):

```bash
uvx --from mcpdoc mcpdoc --help
```
#### IDE Integration

##### Connect to Cursor

1. Open Cursor Settings and navigate to the `MCP` tab
2. Edit the configuration file (`~/Library/Application Support/Cursor/User/globalStorage/state.vscdb` on Mac)
3. Add the rust-llms.txt server configuration:

```json
{
  "mcpServers": {
    "rust-docs-mcp": {
      "command": "uvx",
      "args": [
        "--from",
        "mcpdoc",
        "mcpdoc",
        "--urls",
        "Rust:https://github.com/m2ux/llms/blob/main/rust-llms.txt",
        "--transport",
        "stdio"
      ]
    }
  }
}
```

4. Update Cursor Global (User) Rules in `Settings/Rules`:

```
for ANY question about Rust, use the rust-docs-mcp server to help answer --
+ call list_doc_sources tool to get the available llms.txt file
+ call fetch_docs tool to read it
+ reflect on the urls in llms.txt
+ reflect on the input question
+ call fetch_docs on any urls relevant to the question
+ use this to answer the question
```

##### Connect to Windsurf

1. Open Cascade with `CMD+L` (on Mac)
2. Click `Configure MCP` to open `~/.codeium/windsurf/mcp_config.json`
3. Add the same configuration as above for Cursor
4. Update `Windsurf Rules/Global rules` with similar rules

##### Connect to Claude Desktop

1. Open `Settings/Developer` to update `~/Library/Application Support/Claude/claude_desktop_config.json`
2. Add the rust-docs-mcp configuration
3. Restart Claude Desktop app
4. Since Claude Desktop doesn't support global rules, append this to your prompts:

```
<rules>
for ANY question about Rust, use the rust-docs-mcp server to help answer --
+ call list_doc_sources tool to get the available llms.txt file
+ call fetch_docs tool to read it
+ reflect on the urls in llms.txt
+ reflect on the input question
+ call fetch_docs on any urls relevant to the question
</rules>
```

##### Connect to Claude Code

Run this command to add the MCP server:

```bash
claude mcp add-json rust-docs '{"type":"stdio","command":"uvx","args":["--from", "mcpdoc", "mcpdoc", "--urls", "Rust:https://github.com/m2ux/llms/blob/main/rust-llms.txt"]}' -s local
```

#### Command Line Usage

You can also run the server directly from the command line:

```bash
# Serve the rust-llms.txt file
uvx --from mcpdoc mcpdoc \
    --urls "Rust:https://github.com/m2ux/llms/blob/main/rust-llms.txt" \
    --transport sse \
    --port 8082 \
    --host localhost
```

---

## Contributing

If you'd like to suggest improvements to the rust-llms.txt file or add additional language-specific llms.txt files to this collection, please consider the quality and relevance standards established by the original curated sources.
