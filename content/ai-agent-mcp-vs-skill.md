+++
title = "AI Agents: MCP vs SKILL — Two Sides of the Same Coin?"
date = 2026-07-15
draft = false
description = "MCP connects AI to external tools, SKILL shapes how it behaves. Here's how these two patterns complement each other in the AI agent ecosystem."

[taxonomies]
categories = ["AI"]
tags = ["ai-agents", "mcp", "skill", "llm", "architecture", "protocol"]

[extra]
+++

If you've been following the AI agent space lately, you've probably run into two terms that seem to overlap: **MCP** and **SKILL**. Both are about making AI agents more capable. Both involve giving the model something beyond its training data. But they solve fundamentally different problems — and understanding the distinction is key to building agents that actually work.

I've spent the last few months tinkering with both patterns across different platforms, and here's my take on what each one is, how they differ, and when to use which.

### What is MCP?

**MCP (Model Context Protocol)** is an open protocol developed by Anthropic that standardizes how AI applications connect to external tools and data sources. Think of it as a **USB-C for AI** — a universal connector that lets any AI model talk to any tool or data source through a well-defined interface.

Instead of hacking together custom integrations for every tool (Slack API here, database connector there, web search over there), MCP provides a single protocol. A model speaks MCP, and any server that implements the protocol can serve tools, resources, and prompts to that model.

**The key idea:** MCP is about *connectivity*. It answers the question: "How does my AI agent reach into the outside world?"

**Examples of MCP in action:**
- An MCP server that wraps the GitHub API lets your agent create PRs, review code, and manage issues.
- A filesystem MCP server lets your agent read and write files in a sandboxed directory.
- A database MCP server lets your agent run queries against PostgreSQL.

### What is SKILL?

**SKILL** (in the context of platforms like Codex) is a different beast. A SKILL is a set of local instructions — a `SKILL.md` file — that defines *how an AI agent should behave* for a specific domain or task. It's not a protocol for connecting to external tools. It's an instruction manual for the model itself.

A SKILL tells the agent:
- What context to consider before acting
- Which conventions and patterns to follow
- How to structure its output
- What safety rules or constraints apply
- Which reference files to load

**The key idea:** SKILL is about *behavior shaping*. It answers the question: "How does my AI agent know what to do and how to do it right?"

**Examples of SKILL in action:**
- A "code-reviewer" SKILL that instructs the agent to check for bugs, security issues, and style violations.
- A "zola-write" SKILL that tells the agent how to structure blog posts, what front matter to use, and what tone to adopt.
- A "latex-compile" SKILL that teaches the agent how to compile TeX projects and what fallbacks to try.

### The Core Difference

| Dimension | MCP | SKILL |
|---|---|---|
| **Purpose** | Connect to external tools/data | Shape agent behavior |
| **Nature** | Protocol / API standard | Instruction file / prompt |
| **Where it lives** | External server (separate process) | Embedded in the project or agent config |
| **What it provides** | Tools, resources, context providers | Guidelines, conventions, rules |
| **Execution** | Runtime API calls | Parsed and applied at prompt time |
| **Analogy** | USB-C for tool connectivity | Employee handbook for agent conduct |

### Why This Distinction Matters

Here's the thing: **MCP without SKILL is a tool shed without a foreman.** You have all these powerful connections to databases, APIs, and file systems, but no guidance on *which* tool to use when, *how* to chain them, or *what* to avoid.

**SKILL without MCP is a foreman without tools.** You have detailed instructions and best practices, but no way to actually execute anything beyond the model's built-in knowledge.

The magic happens when you combine them:

> A **SKILL** defines the plan and the rules. An **MCP server** provides the means to execute.

### Real-World Example

Let's say you're building an AI coding assistant.

- **MCP servers** give it access to: the codebase filesystem (read/write), the linter (run checks), the test runner (execute tests), and Git (commit/push).
- **A "web-dev" SKILL** tells the assistant: "Always run the linter after editing. Write tests first. Use React hooks not class components. Check for accessibility issues. Validate your build before committing."

Without the SKILL, the agent has tools but doesn't know your team's conventions. Without MCP, the agent knows the conventions but can't actually touch any code.

### When to Use Which

**Reach for MCP when:**
- You need your agent to interact with external services (APIs, databases, file systems).
- You want a standardized way to expose tools across different AI platforms.
- You're building a tool ecosystem that multiple agents can share.

**Reach for SKILL when:**
- You need an agent to follow project-specific conventions or guidelines.
- You're codifying domain knowledge, safety rules, or workflow steps.
- You want to ensure consistent output quality across different tasks.

### The Future: Convergence

Honestly, I think the line will blur over time. We're already seeing MCP servers that return structured prompts — which is SKILL-like behavior. And some SKILLs reference MCP servers as their execution backend. The two patterns are complementary, not competing.

My prediction? The most powerful AI agents will use **MCP for external reach** and **SKILL for internal guidance**, wired together in a layered architecture where context flows naturally between them.

### Conclusion

MCP and SKILL aren't an either/or choice. They're two different layers in the stack of a capable AI agent. MCP opens the door to the outside world. SKILL makes sure the agent walks through it the right way.

If you're building agents today, invest in both. Your agents will be smarter, safer, and far more useful.

*What's your experience with MCP and SKILL patterns? Drop a comment or reach out — I'd love to hear how you're combining them in your projects.*
