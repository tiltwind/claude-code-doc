# Settings Source

## Overview

Defines the sources from which settings can be loaded, in priority order (later sources override earlier ones).

## Values

| Value | Label | Description | Sort Order | Notes |
|-------|-------|-------------|------------|-------|
| user | User Settings | Global per-user settings (~/.claude/settings.json) | 1 | Lowest priority |
| project | Project Settings | Shared project settings (.claude/settings.json) | 2 | Committed to version control |
| local | Local Settings | Gitignored local settings (.claude/local-settings.json) | 3 | Personal, per-project overrides |
| flag | Flag Settings | Settings passed via --settings CLI flag | 4 | Session-specific overrides |
| policy | Policy Settings | Enterprise managed settings (MDM or remote API) | 5 | Highest priority; cannot be overridden |
