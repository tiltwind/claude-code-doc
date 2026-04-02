# Agent Type

## Overview

Defines the built-in agent types available in Claude Code's multi-agent system. Each type has a distinct role, tool set, and permission model, following the principle of role separation — the same model performs better when responsibilities are clearly partitioned.

## Values

| Value | Label | Description | Sort Order | Notes |
|-------|-------|-------------|------------|-------|
| general-purpose | General Purpose | Full-capability agent for complex, multi-step tasks | 1 | Default agent type; has access to all tools |
| explore | Explore | Read-only code exploration agent | 2 | Strictly read-only: Glob, Grep, FileRead, Bash (only ls/git status/git log). Cannot create, modify, or delete files. External users default to Haiku model for speed/cost; internal users inherit main model. |
| plan | Plan | Planning agent that designs implementation strategies without executing | 3 | Can read and search but cannot write or execute. Returns step-by-step plans. |
| verification | Verification | Adversarial testing agent that tries to break implementations | 4 | Has its own 130-line prompt — the most carefully designed in the codebase. Core directive: "try to break it". Cannot edit/write files. Forces actual test execution per change type (frontend → browser, CLI → stdout/stderr, DB migration → up/down data). Outputs VERDICT: PASS, FAIL, or PARTIAL. |
| claude-code-guide | Claude Code Guide | Usage guidance agent for Claude Code features | 5 | Answers questions about CLI features, hooks, MCP servers, settings, SDK |
| statusline-setup | Statusline Setup | Status bar configuration agent | 6 | Specialized for configuring the user's status line display |
