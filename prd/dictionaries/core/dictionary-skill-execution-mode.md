# Skill Execution Mode

## Overview

Defines how skills are executed when invoked by the user or Claude.

## Values

| Value | Label | Description | Sort Order | Notes |
|-------|-------|-------------|------------|-------|
| inline | Inline | Skill prompt is expanded into the current conversation context | 1 | Shares the main conversation's token budget |
| forked | Forked | Skill runs as a sub-agent with a separate token budget | 2 | Isolated from main conversation |
| local | Local | Skill executes a local command without AI involvement | 3 | Deterministic execution |
| local-jsx | Local JSX | Skill renders a React UI component in the terminal | 4 | For interactive commands |
