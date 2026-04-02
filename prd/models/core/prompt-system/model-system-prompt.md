# System Prompt

## Overview

The System Prompt is the assembled instruction set sent to Claude on each API call. It is constructed from static (cacheable) sections and dynamic (per-turn) sections, separated by a SYSTEM_PROMPT_DYNAMIC_BOUNDARY marker. This architecture enables API-level prompt caching — if the static prefix is byte-identical across requests, the API can skip re-processing it, significantly reducing latency and cost.

## Attributes

| Attribute | Description | Type | Required | Notes |
|-----------|-------------|------|----------|-------|
| staticSections | Ordered list of cacheable prompt sections | list of PromptSection | Yes | Placed before the dynamic boundary |
| dynamicSections | Ordered list of per-turn prompt sections | list of PromptSection | Yes | Placed after the dynamic boundary |
| dynamicBoundaryMarker | Marker string separating static from dynamic content | text | Yes | SYSTEM_PROMPT_DYNAMIC_BOUNDARY |
| totalTokenEstimate | Estimated token count of the assembled prompt | number | Yes | Used for context budget calculations |

### Static Sections (Cacheable)

| Section | Source Function | Description |
|---------|---------------|-------------|
| Identity | getSimpleIntroSection | Claude's identity and role positioning |
| System Rules | getSimpleSystemSection | Core system operating rules |
| Task Behavior | getSimpleDoingTasksSection | What to do and not do when performing tasks (the most valuable behavior constraints) |
| Risk Actions | getActionsSection | Rules for risky/destructive actions requiring care |
| Tool Syntax | getUsingYourToolsSection | How to use the available tools |
| Tone & Style | getSimpleToneAndStyleSection | Communication tone and formatting preferences |
| Output Efficiency | getOutputEfficiencySection | Rules for concise, direct output |

### Dynamic Sections (Per-Turn)

| Section | Description | Caching |
|---------|-------------|---------|
| Session Guidance | Currently enabled tools, behavior guidance based on active capabilities | Recomputed per turn |
| Memory (CLAUDE.md) | Content from CLAUDE.md files in project hierarchy | Cached until /clear or /compact |
| Environment Info | OS, shell, cwd, model name, date | Cached per session |
| Language & Output Preferences | User language and output style preferences | Cached per session |
| MCP Server Instructions | Instructions injected by connected MCP servers | DANGEROUS (recomputed each turn, since servers may connect/disconnect) |
| Function Result Clearing | Notes about cleared/truncated previous tool results | Recomputed per turn |
| Token Budget | Budget hints when token budget is configured | Recomputed per turn |

## Relationships

| Related Model | Relationship Type | Description |
|---------------|-------------------|-------------|
| Session | many-to-one | Each API call within a session assembles a system prompt |
| Tool | one-to-many | Each tool contributes its own prompt section via its prompt() method |
| MCP Server | one-to-many | Connected MCP servers inject instruction sections |
| Settings | reference | CLAUDE.md and settings influence dynamic sections |
