# Permission Mode

## Overview

Defines the operating modes that control how tool execution permissions are evaluated. Each mode represents a different balance between safety and convenience.

## Values

| Value | Label | Description | Sort Order | Notes |
|-------|-------|-------------|------------|-------|
| default | Default | Standard mode; prompts user for permission on sensitive operations | 1 | Recommended for normal use |
| plan | Plan | Planning mode; Claude can only read and search, not modify files or run commands | 2 | Used during structured planning phases |
| accept-edits | Accept Edits | Auto-approves file read/write/edit operations without prompting | 3 | Shell commands still require approval |
| auto | Auto | Classifier-based auto-approval with dangerous pattern safety checks | 4 | Uses ML classifier to evaluate safety |
| bypass | Bypass | Bypasses all permission checks | 5 | Dangerous; for trusted environments only |
