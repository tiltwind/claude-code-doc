# Settings View

## Page Description

Displays and allows management of Claude Code settings across all sources (user, project, local, policy). Accessible via `/config` command or by asking Claude to update settings.

**Entry points**: `/config` command; Claude using ConfigTool

## Access Permissions

| Role | Access |
|------|--------|
| End User | View all settings; modify user, project, and local settings |
| Enterprise Administrator | Define policy settings (indirectly) |

## Page Structure

### Section 1: Settings Overview
- **Position**: Full page view
- **Content**:
  - Current permission mode and rules
  - Configured MCP servers and their status
  - Active hooks
  - Model preference
  - Theme and keybindings
  - Environment variables
- **Interactions**:
  - Settings are typically modified by asking Claude or editing settings.json directly
  - ConfigTool provides programmatic modification

## Feature Descriptions

### Feature 1: View Settings
- **Trigger**: User invokes /config
- **Business Logic**: Load and merge settings from all sources; display resolved values with source annotations
- **Related Models**: Settings
- **Feedback**: Formatted display of current settings

### Feature 2: Update Settings
- **Trigger**: User asks Claude to change a setting, or Claude uses ConfigTool
- **Business Logic**: Write to appropriate settings file (user, project, or local); validate schema; reload
- **Related Models**: Settings (updated)
- **Feedback**: Confirmation message with the change made

## Page Navigation

| Destination | Trigger |
|-------------|---------|
| Conversation Main | After viewing/modifying settings |
