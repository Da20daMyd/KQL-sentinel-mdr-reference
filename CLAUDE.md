# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Purpose

This repository collects and organizes documentation for:
- KQL (Kusto Query Language)
- Microsoft Defender Endpoint
- Microsoft Sentinel

The goal is to fetch, analyze, parse, and extract code snippets to train LLMs and AI systems to write better KQL queries and provide updated source table references. **The processed output is specifically designed for Context7 service integration.**

## Context7 Integration Requirements

Since the output targets Context7 service, prioritize:
- **Pure code snippets** over explanatory text
- **Structured, parseable formats** for easy ingestion
- **Minimal markdown decoration** around code blocks
- **Clear categorization** for Context7's indexing system

## Project Structure

Documentation should be organized as follows:
- `/kql/` - Core KQL language documentation and examples
- `/defender/` - Microsoft Defender Endpoint specific queries and tables
- `/sentinel/` - Microsoft Sentinel specific queries and tables

**in the documents on these folders include schema info (tables) and code snippets. The target is to load those MD files on CONTEXT7.**

## Development Workflow

### Fetching Documentation
Use the Firecrawl MCP tool to fetch documentation from Microsoft sources. Store raw fetched content before processing.

### Processing Documentation for Context7
1. Extract KQL code blocks with minimal surrounding context
2. Focus on executable query snippets
3. Strip unnecessary markdown formatting
4. DO NOT CHANGE, ALTER OR MODIFY THE QUERIES, JUST TAKE AS THEY ARE FROM THE DOC, DO NOT GO ON YOUR OWN INITIATIVE
4. Create JSON or structured formats suitable for Context7 ingestion
5. Maintain a catalog of snippet metadata (source, category, complexity)

### Code Snippet Extraction Standards
When extracting KQL snippets for Context7:
- **Keep snippets self-contained and runnable**
- **Remove explanatory prose** - let the code speak for itself
- **Preserve only essential comments** within the code
- **Tag with machine-readable categories**: detection, hunting, investigation, performance, analytics
- **Include minimal context** in separate metadata fields, not in the snippet itself
- **Structure output** for programmatic consumption (JSON/YAML preferred)

Example snippet format for Context7:
```json
{
  "id": "unique-snippet-id",
  "platform": "sentinel|defender|azure",
  "category": "detection",
  "tables": ["SecurityEvent", "Process"],
  "operators": ["where", "summarize", "join"],
  "snippet": "// Actual KQL code here",
  "complexity": "intermediate"
}
```

## KQL Query Standards

When working with KQL queries in this repository:
- Use consistent indentation (4 spaces)
- Place operators at the beginning of new lines
- Include comments explaining complex logic
- Document required table schemas

## Important Considerations

- KQL syntax varies slightly between Azure Data Explorer, Sentinel, and Defender
- Table names and schemas differ between platforms
- Always note which platform a query is intended for
- Preserve version information when available
- **Context7 requires clean, executable code** - avoid broken or partial snippets
