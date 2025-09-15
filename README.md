# LLM Context Files

This project contains structured context files designed to help Large Language Models (LLMs) understand and work with specific programming languages and frameworks.

## About llms.txt Files

The `llms.txt` format is a standardized approach for providing LLM-friendly documentation and resources, as specified at [llmstxt.org](https://llmstxt.org/). These files offer:

- **Curated Resources**: Expert-selected articles, books, and tutorials
- **LLM-Optimized Format**: Structured markdown that's easy for AI models to parse
- **Contextual Learning**: Focused collections that teach best practices and idiomatic patterns
- **Inference-Time Help**: Resources specifically chosen to assist with coding tasks

## rust-llms.txt

The [rust-llms.txt](rust-llms.txt) file provides comprehensive context for writing idiomatic Rust code.

### Purpose

This file helps LLMs understand and generate:
- **Idiomatic Rust code** following community best practices
- **Proper error handling** patterns and techniques
- **Memory-safe** and performance-oriented solutions
- **Library design** following Rust API guidelines
- **Ownership and borrowing** patterns that avoid common pitfalls

### Content Overview

The file organizes Rust learning resources into several categories:

- Core Learning Resources
- Articles and Guides
- Interactive Learning
- Books and In-Depth Guides
- Community Resources

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

* Open Cursor `Settings/MCP` tab.
* Paste the following configuration into the MCP settings:

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
        "Rust:https://raw.githubusercontent.com/m2ux/llms/main/rust-llms.txt",
        "--transport",
        "stdio"
        "--allowed-domains",
        "*"
      ]
    }
  }
}
```

* Confirm that the server is running in your `Cursor Settings/MCP` tab.
* Best practice is to then update Cursor Global (User) rules.
* Open Cursor `Settings/Rules` and update `User Rules` with the following:

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

* Open Cascade with `CMD+L` (on Mac).
* Click `Configure MCP` to open the config file, `~/.codeium/windsurf/mcp_config.json`.
* Update with the same configuration as noted above for Cursor.
* Update `Windsurf Rules/Global rules` with the following:

```
for ANY question about Rust, use the rust-docs-mcp server to help answer -- 
+ call list_doc_sources tool to get the available llms.txt file
+ call fetch_docs tool to read it
+ reflect on the urls in llms.txt 
+ reflect on the input question 
+ call fetch_docs on any urls relevant to the question
```

##### Connect to Claude Desktop

* Open `Settings/Developer` to update `~/Library/Application\ Support/Claude/claude_desktop_config.json`.
* Add the rust-docs-mcp configuration as noted above.
* Restart Claude Desktop app.

**Note:** If you run into issues with Python version incompatibility when trying to add MCPDoc tools to Claude Desktop, you can explicitly specify the filepath to `python` executable in the `uvx` command.

Since Claude Desktop doesn't currently support global rules, append the following to your prompts:

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

In a terminal after installing Claude Code, run this command to add the MCP server to your project:

```bash
claude mcp add-json rust-docs '{"type":"stdio","command":"uvx","args":["--from", "mcpdoc", "mcpdoc", "--urls", "Rust:https://raw.githubusercontent.com/m2ux/llms/main/rust-llms.txt", "--allowed-domains", "*"]}' -s local
```

Since Claude Code doesn't currently support global rules, append the following to your prompts:

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

#### Command Line Usage

You can also run the server directly from the command line with various options:

```bash
uvx --from mcpdoc mcpdoc --urls "Rust:https://raw.githubusercontent.com/m2ux/llms/main/rust-llms.txt" --transport sse --port 8082 --host localhost --allowed-domains '*'
```

**Additional Options:**
* `--follow-redirects`: Follow HTTP redirects (defaults to False)
* `--timeout SECONDS`: HTTP request timeout in seconds (defaults to 10.0)

---

## Contributing

If you'd like to suggest improvements to the rust-llms.txt file or add additional language-specific llms.txt files to this collection, please consider the quality and relevance standards established by the original curated sources.
