# Permission Decision

## Overview

Outcomes produced by the permission evaluation system when Claude attempts to invoke a tool.

## Values

| Value | Label | Description | Sort Order | Notes |
|-------|-------|-------------|------------|-------|
| approved | Approved | Tool execution is allowed to proceed | 1 | — |
| denied | Denied | Tool execution is blocked | 2 | Reason is recorded for denial tracking |
| ask-user | Ask User | User must interactively approve or deny | 3 | Displays permission dialog in terminal |
