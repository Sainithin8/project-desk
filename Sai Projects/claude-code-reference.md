# Claude Code — Daily Reference Guide

> Each section has a **Challenge** to try it hands-on.

---

## 1. Keyboard Shortcuts

### Input & Navigation
| Shortcut | Action |
|---|---|
| `↑` / `↓` | Browse previous commands |
| `Ctrl+R` | Search through command history |
| `Ctrl+C` | Cancel current input |
| `Ctrl+C` (during response) | Interrupt Claude mid-reply |
| `Esc` | Clear current input line |
| `Tab` | Autocomplete slash commands |

### Multiline Input
| Shortcut | Action |
|---|---|
| `Shift+Enter` | Add a new line without submitting |

**Challenge:** Type a long message, use `Shift+Enter` to write it across multiple lines, then send.

---

## 2. Slash Commands (Built-in)

| Command | What it does |
|---|---|
| `/help` | Show all available commands and skills |
| `/clear` | Clear the conversation context window |
| `/compact` | Summarize conversation to free up context |
| `/model` | Switch Claude model (Sonnet, Opus, Haiku) |
| `/fast` | Toggle fast output mode |
| `/status` | Show current session info |
| `/memory` | View and manage Claude's memory |
| `/config` | View or change settings |
| `/permissions` | View current permission settings |
| `/cost` | Show token usage and cost for the session |

**Challenge:** Run `/cost` to see how many tokens you've used so far.

---

## 3. MCP Tools (Connected Services)

Claude Code can connect to external services via MCP (Model Context Protocol):

| Service | What Claude can do |
|---|---|
| Gmail | Read, search, send emails |
| Google Calendar | View/create events |
| Google Drive | Read and search files |
| IDE (VS Code) | Run code, get diagnostics |
| Custom MCP servers | Anything you configure |

### Connecting
- Configured via `/update-config` or `settings.json`
- Once connected, just describe the task naturally

**Challenge:** Ask Claude "What MCP tools are available?" to see what's connected.

---

## 4. Slash Commands (Skills)

| Command | What it does |
|---|---|
| `/commit` | Generate a smart git commit message and commit |
| `/review` | Review a pull request |
| `/init` | Initialize a `CLAUDE.md` file for your project |
| `/simplify` | Review changed code for quality and clean it up |
| `/security-review` | Security audit of current branch changes |
| `/insights` | Analyze your Claude Code usage patterns |
| `/schedule` | Create scheduled/recurring Claude tasks |
| `/loop` | Run a command repeatedly on an interval |
| `/statusline` | Configure the Claude Code status line UI |
| `/update-config` | Update `settings.json` (hooks, permissions, env vars) |

**Challenge:** Run `/init` in a project folder to auto-generate a `CLAUDE.md`.

---

## 5. Prompt Tricks

| Trick | Example | What it does |
|---|---|---|
| `! command` | `! git status` | Run a shell command and inject output into context |
| `@filename` | `@src/app.py` | Reference a file directly in your prompt |

### Examples
```
! npm run test          → runs tests and Claude sees the output
@package.json           → Claude reads the file inline
```

> **Note:** There is no special `#` or `?` prefix syntax in Claude Code. To save a preference, just tell Claude directly: _"Remember that I prefer async/await over .then() chains"_ — it will store that in memory. To ask a question, just ask normally.

**Challenge:** Try `! git log --oneline -10` and ask Claude to summarize recent changes.

---

## 6. Memory System

Claude has a persistent memory across conversations stored in your project's `.claude/` directory:
- **Windows:** `C:\Users\<you>\.claude\projects\<project>\memory\`
- **Mac/Linux:** `~/.claude/projects/<project>/memory/`

### Memory Types
| Type | What to store |
|---|---|
| `user` | Your role, preferences, skill level |
| `feedback` | How you want Claude to behave |
| `project` | Goals, deadlines, context behind work |
| `reference` | Where things live (Linear, Grafana, Slack) |

### How to trigger memory saves
- Just tell Claude something worth remembering: _"Remember that I prefer TypeScript over JavaScript"_
- Claude will save it automatically if it's relevant
- Use `/memory` to view or edit saved memories

**Challenge:** Tell Claude your preferred coding language or style and ask it to remember.

---

## 7. What Claude Can Do (Code Tasks)

| Task | Example prompt |
|---|---|
| Explain code | "Explain how this function works" |
| Fix a bug | "This function throws a null error, fix it" |
| Refactor | "Refactor this to be more readable" |
| Add a feature | "Add pagination to this API endpoint" |
| Write tests | "Write unit tests for this module" |
| Review code | "Review this for security issues" |
| Generate types | "Generate TypeScript types for this JSON" |
| Document | "Add JSDoc comments to these functions" |

**Challenge:** Paste a function and ask Claude to explain it, then ask it to add error handling.

---

## 8. Git & GitHub Integration

| Task | Example prompt |
|---|---|
| Smart commit | `/commit` |
| PR review | `/review` or "Review PR #42" |
| Create PR | "Create a PR for these changes" |
| Check status | `! git status` |
| View diff | `! git diff` |
| Branch work | "Create a new branch called feature/login" |
| Merge conflicts | "Help me resolve this merge conflict" |

Claude can use `gh` CLI for GitHub operations (issues, PRs, releases).

**Challenge:** Make a small change to a file and run `/commit` — let Claude write the message.

---

## 9. Plan Mode

Plan mode makes Claude **think before acting** — great for complex tasks.

### How it works
1. Claude explores the codebase
2. Writes a detailed plan
3. Asks for your approval before touching any files

### How to trigger
- Prefix your prompt: `"Plan: add OAuth login to my app"`
- Or enable it in settings

### Why use it
- For multi-file changes
- When you want to review the approach first
- For risky refactors

**Challenge:** Ask Claude to plan (not implement) adding a new feature to one of your projects.

---

## 10. Automation & Scheduling

### Hooks — Automated triggers
Set commands to run automatically on Claude events:
- Before/after tool calls
- When Claude stops
- On file saves

Configure via `/update-config` — e.g.:  
_"Run prettier after every file edit"_

### Scheduled Tasks
Use `/schedule` to create cron-based Claude agents:  
_"Every Monday, summarize my open GitHub PRs"_

### Loop Mode
Use `/loop` to repeat a task on an interval:  
_"/loop 10m check if the build is passing"_

**Challenge:** Ask Claude to set up a hook that runs `git status` after each session.

---

## 11. Permissions & Safety


Claude is careful about risky actions and will ask before:
- Deleting files or branches
- Force pushing
- Sending messages or posting to external services
- Modifying CI/CD pipelines

### Controlling permissions
| Command | Action |
|---|---|
| `/permissions` | View current permission settings |
| `/update-config` | Add or change allowed actions |

### Example
_"Allow Claude to run npm commands without asking"_

**Challenge:** Run `/permissions` to see what Claude is and isn't allowed to do by default.

---

## 12. Tips & Power Combos


### Combos that work great together
- `! git diff` → "What changed and why does it matter?"
- `@errorlog.txt` → "What's causing this error?"
- `/compact` → then continue a long session without losing context
- `/security-review` → before any PR merge
- `/insights` → weekly to see your usage patterns

### Golden rules
- Be specific: "Fix the login bug in `auth.js:42`" beats "fix bugs"
- Reference files with `@` for precision
- Use `/clear` when switching to a new task
- Save important preferences with a `#` note

---

*Work through each point at your own pace to build strong Claude Code fluency.*
