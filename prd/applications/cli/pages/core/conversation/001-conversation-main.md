# Conversation Main

## Page Description

The primary interface of Claude Code — a full-screen terminal conversation view where the user interacts with Claude. The screen is divided into a scrollable message history area and a fixed input area at the bottom. This is the default and always-visible page.

**Entry points**: Application startup; resume from session; return from modal overlays

## Access Permissions

| Role | Access |
|------|--------|
| End User | Full access |

## Page Structure

### Section 1: Status Line
- **Position**: Top row, full width
- **Content**:
  - Model name and variant (e.g., "Claude Opus 4.6 (1M)")
  - Session cost estimate (e.g., "$0.42")
  - Token usage indicator
  - Remote session URL (if connected to claude.ai)
  - Brief mode indicator (if active)
- **Interactions**:
  - Click on model name → open model selection
  - Click on cost → show detailed cost breakdown

### Section 2: Message History
- **Position**: Main area between status line and input, scrollable
- **Content**:
  - User messages: displayed with ">" prefix, user text
  - Assistant messages: streamed markdown with syntax highlighting
  - Tool invocations: collapsible tool name + parameters + result summary
  - System messages: context notes, compaction summaries
  - Progress indicators: spinners for active tool execution
  - Diff views: inline file change diffs with syntax highlighting
- **Interactions**:
  - Scroll up/down to browse history (mouse wheel, Page Up/Down, vim j/k)
  - Click on file paths to navigate (in IDE integration)
  - Expand/collapse tool invocation details
  - Select text to copy (auto-copy on select if configured)

### Section 3: Input Area
- **Position**: Fixed at bottom, multi-line capable
- **Content**:
  - Text input cursor with prompt indicator
  - Current input text (supports multi-line with Shift+Enter)
  - File/command autocomplete suggestions (when typing)
  - Voice recording indicator (when push-to-talk active)
  - Paste preview (when pasting images or large text)
- **Interactions**:
  - Type text → builds prompt
  - Enter → submit prompt
  - Shift+Enter → new line in input
  - Up arrow → previous prompt history
  - Tab → accept autocomplete suggestion
  - `/` → triggers slash command completion
  - `@` → triggers file/mention completion
  - Escape → cancel current streaming response
  - Ctrl+C → interrupt; double Ctrl+C → exit
  - Ctrl+D → exit

### Section 4: Toolbar
- **Position**: Bottom row below input, full width
- **Content**:
  - Available keybinding hints (context-dependent)
  - Permission mode indicator (Default / AcceptEdits / Auto / Plan)
  - Brief mode toggle status
  - Active agent/teammate indicators
- **Interactions**:
  - Click on permission mode → cycle through modes
  - Click on brief toggle → toggle brief mode

## Feature Descriptions

### Feature 1: Prompt Submission
- **Trigger**: User presses Enter with non-empty input
- **Business Logic**: Execute user-prompt-submit hooks → route slash commands → assemble context → call Claude API (reference: Conversation Flow procedure)
- **Related Models**: Session (state → processing), Message (new user + assistant messages)
- **Feedback**: Input clears; spinner appears; streaming response begins rendering

### Feature 2: Streaming Response
- **Trigger**: Claude API begins streaming
- **Business Logic**: Incrementally render markdown text, code blocks with syntax highlighting, and tool use indicators
- **Related Models**: Message (assistant), Tool Result
- **Feedback**: Text appears incrementally; code blocks render with language labels; tool spinner shows active tool name

### Feature 3: Conversation History Navigation
- **Trigger**: User scrolls or uses keyboard navigation
- **Business Logic**: Virtual scroll through message history; load from disk if session was resumed
- **Related Models**: Conversation History
- **Feedback**: Smooth scroll through messages; "scrolled to top" indicator

### Feature 4: Session Resume
- **Trigger**: User runs `claude --resume` or `/resume`
- **Business Logic**: Load conversation history from disk; display session summary; continue from last message
- **Related Models**: Session, Conversation History
- **Feedback**: History loads; "Resumed session" indicator; ready for new input

### Feature 5: Context Compaction Notification
- **Trigger**: System triggers message compaction
- **Business Logic**: Display notification that context is being compressed (reference: Message Compaction procedure)
- **Related Models**: Session (state → compacting)
- **Feedback**: "Compacting conversation..." spinner; resumes to active state

## Data Display Rules

- **Default Sort**: Messages displayed in chronological order (oldest first)
- **Pagination**: Infinite scroll with virtual rendering for performance
- **Empty State**: Welcome message with getting-started hints and example prompts
- **Error State**: Error banner at top of message area with retry suggestion

## Page States

| State | Description |
|-------|-------------|
| Welcome | No messages yet; shows getting-started tips |
| Active | Messages present; input ready for new prompt |
| Processing | Claude is responding; spinner active; input shows "Escape to cancel" |
| Scrolled | User has scrolled up in history; "jump to bottom" hint shown |
| Compacting | Context is being compressed; spinner shows compaction status |

## Page Navigation

| Destination | Trigger |
|-------------|---------|
| Permission Dialog | Claude requests a tool that requires approval |
| Task List | User invokes task-related commands |
| Settings View | User invokes /config or settings commands |
| Commit View | User invokes /commit |
