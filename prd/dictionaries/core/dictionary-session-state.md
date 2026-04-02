# Session State

## Overview

States of a conversation session throughout its lifecycle.

## Values

| Value | Label | Description | Sort Order | Notes |
|-------|-------|-------------|------------|-------|
| initializing | Initializing | Session is starting up, loading context and settings | 1 | — |
| active | Active | Session is ready for user input and conversation | 2 | Normal operating state |
| processing | Processing | Claude is generating a response or executing tools | 3 | User can cancel with Ctrl-C |
| compacting | Compacting | Message history is being compressed to fit context window | 4 | Automatic when context limit approaches |
| backgrounded | Backgrounded | Session is running in the background | 5 | Can be resumed |
| ended | Ended | Session has been terminated | 6 | Terminal state |
