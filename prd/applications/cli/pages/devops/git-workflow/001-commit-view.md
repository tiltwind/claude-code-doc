# Commit View

## Page Description

Displays the git commit workflow when the user invokes `/commit`. Shows change analysis, generated commit message, and commit execution progress. This is rendered as an inline conversation flow rather than a separate page.

**Entry points**: `/commit` slash command

## Access Permissions

| Role | Access |
|------|--------|
| End User | Invoke and review commit |
| Claude Assistant | Analyze changes, generate message, execute commit |

## Page Structure

### Section 1: Change Analysis
- **Position**: Inline in conversation
- **Content**:
  - Git status summary (files modified, added, deleted)
  - Diff preview (staged and unstaged changes)
  - Recent commit history for style reference
- **Interactions**:
  - Scroll through diff preview

### Section 2: Commit Message
- **Position**: Inline in conversation
- **Content**:
  - Generated commit message with title and body
  - Co-Authored-By attribution line
- **Interactions**:
  - User can provide feedback to adjust the message before Claude creates the commit

### Section 3: Execution Status
- **Position**: Inline in conversation
- **Content**:
  - File staging progress
  - Commit execution result
  - Post-commit git status verification
- **Interactions**:
  - None (informational output)

## Feature Descriptions

### Feature 1: AI-Assisted Commit
- **Trigger**: User invokes /commit
- **Business Logic**: Follow Commit Flow procedure — analyze changes, generate message, stage files, create commit
- **Related Models**: Session, Message
- **Feedback**: Step-by-step progress in conversation; final commit hash shown

## Page Navigation

| Destination | Trigger |
|-------------|---------|
| Conversation Main | After commit completes or is cancelled |
| Permission Dialog | If bash/git commands require approval |
