# Permission Behavior

## Overview

Defines the behaviors that can be applied to individual permission rules, determining how the system handles specific tool invocations.

## Values

| Value | Label | Description | Sort Order | Notes |
|-------|-------|-------------|------------|-------|
| allow | Allow | Automatically approve the tool invocation without prompting | 1 | Can be scoped to specific tool arguments |
| deny | Deny | Automatically reject the tool invocation without prompting | 2 | Silently blocks the operation |
| ask | Ask | Force a user prompt regardless of the current permission mode | 3 | Overrides auto-approval modes |
