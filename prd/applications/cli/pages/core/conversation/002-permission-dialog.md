# Permission Dialog

## Page Description

A modal overlay that appears when Claude attempts to invoke a tool that requires user approval. The dialog shows the tool name, parameters, and reason for the prompt, and offers approve/deny actions with options to create permanent rules.

**Entry points**: Triggered by permission check returning "ask-user" decision

## Access Permissions

| Role | Access |
|------|--------|
| End User | Full access (only user can approve/deny) |

## Page Structure

### Section 1: Dialog Header
- **Position**: Top of modal overlay
- **Content**:
  - Tool name and icon
  - Reason for permission prompt (e.g., "File write operation", "Shell command execution")
- **Interactions**: None (informational)

### Section 2: Tool Details
- **Position**: Center of modal
- **Content**:
  - Tool name (e.g., "Bash", "FileWrite")
  - Full command or file path being operated on
  - Syntax-highlighted preview of the command/content
  - Working directory context
- **Interactions**:
  - Scroll within the preview if content is long

### Section 3: Action Buttons
- **Position**: Bottom of modal
- **Content**:
  - **Allow** (y): Approve this single invocation
  - **Allow Always** (a): Approve and add an allow rule for this tool pattern
  - **Deny** (n): Reject this invocation
  - **Deny Always**: Reject and add a deny rule
- **Interactions**:
  - Press y → approve once
  - Press a → approve and create permanent allow rule
  - Press n → deny once
  - Press d → deny and create permanent deny rule

## Feature Descriptions

### Feature 1: Single Approval
- **Trigger**: User presses 'y'
- **Business Logic**: Return approval for this invocation only; tool execution proceeds
- **Related Models**: Permission Decision (approved)
- **Feedback**: Dialog dismisses; tool execution begins with spinner

### Feature 2: Permanent Rule Creation
- **Trigger**: User presses 'a' (allow always) or 'd' (deny always)
- **Business Logic**: Create a new permission rule in session settings matching the tool and pattern; return approval/denial
- **Related Models**: Permission Rule (new rule created), Settings (updated)
- **Feedback**: Dialog dismisses; brief confirmation that rule was added

### Feature 3: Denial
- **Trigger**: User presses 'n'
- **Business Logic**: Return denial as tool_result; Claude sees the denial and adapts
- **Related Models**: Permission Decision (denied)
- **Feedback**: Dialog dismisses; Claude receives denial and may suggest alternatives

## Page States

| State | Description |
|-------|-------------|
| Displayed | Dialog visible, awaiting user input |
| Dismissed | User made a decision, dialog closing |

## Page Navigation

| Destination | Trigger |
|-------------|---------|
| Conversation Main | After any decision (approve/deny) |
