# Permission Rule

## Overview

A Permission Rule defines how the system should handle a specific tool invocation — whether to automatically allow it, deny it, or prompt the user. Rules can be scoped to specific tool arguments and come from different settings sources.

## Attributes

| Attribute | Description | Type | Required | Notes |
|-----------|-------------|------|----------|-------|
| toolName | Name of the tool this rule applies to | text | Yes | Can be a glob pattern |
| behavior | What to do when this rule matches | reference to Permission Behavior dictionary | Yes | allow, deny, or ask |
| ruleContent | Optional content to match against tool arguments | text | No | e.g., specific bash command patterns |
| source | Which settings source defined this rule | reference to Settings Source dictionary | Yes | For priority resolution |

## Relationships

| Related Model | Relationship Type | Description |
|---------------|-------------------|-------------|
| Tool | many-to-many | Rules apply to one or more tools |
| Settings | many-to-one | Rules are defined within settings |
