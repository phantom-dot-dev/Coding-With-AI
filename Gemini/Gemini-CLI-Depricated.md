### Overviews:
- Models Selection, Usages and Remaining tokens
- Effort
- Using Gemini In VS Code
- Shell Mode
- Modes
- Settings & Permissions
- VS Code Extension
- Context, Tokens & Clearing
- Global Sessions
- Gemini.md File & /init
- Plan Mode
- Adding Feature Focused Workflow and keeping context and memory usage in control
- Skills
- MCPs
- Sub-Agents


Welcome to the ultimate tutorial for the **Gemini CLI**. This guide covers everything from basic terminal interaction to advanced multi-agent orchestration workflows.

---

## 1. Models Selection, Usages, & Remaining Tokens

Gemini CLI routes your prompts to different models based on the complexity of your task. You can explicitly choose a model, check your current token usage, and see how much of your context window is remaining.

* **View active model and tokens:** Type `/status` or look at the ambient status bar in your terminal.
* **Switching models:** Use `/model <model-name>` (e.g., `/model gemini-2.5-pro` or `gemini-2.5-flash`).

```bash
# Example interactive session commands
/status
/model gemini-2.5-pro

```

---

## 2. Effort (Reasoning Control)

For models supporting dynamic reasoning (like the Gemini 2.5 Pro series), you can control the "thinking" budget or **Effort** level allocated to complex problem-solving.

* **Commands:** Use `/effort` to view or change settings.
* **Levels:** `low`, `medium`, `high`. Higher effort uses more reasoning tokens before responding, perfect for complex debugging or architecture design.

```bash
# Set thinking budget to maximum for a tough bug
/effort high

```

---

## 3. Using Gemini in VS Code & the Extension

You can interact with Gemini CLI right inside VS Code using two primary methods:

### The Integrated Terminal

Launch `gemini` inside your VS Code terminal. It automatically reads environment variables to add all open VS Code workspace folders directly into your session context.

### The VS Code Extension

Install the official **Gemini Developer Suite** extension from the marketplace. (aka Gemini code assistance, deprecated, replaced by Antigravity IDE)

* **Inline Code Completion:** Press `Tab` to accept suggestions.
* **Sidebar Chat:** Hit `Ctrl+Shift+G` (or `Cmd+Shift+G` on Mac) to toggle a dedicated chat view that shares context with your open files.

---

## 4. Shell Mode

If you want to run terminal commands without constantly exiting your interactive Gemini session, **Shell Mode** is your best friend.

* **Trigger:** Prefix any command with `!` to run it directly in your local shell.
* **Use Case:** Running tests or building your project while getting advice.

```bash
# Ask Gemini to fix a test, then run it without leaving the CLI
!npm run test

```

---

## 5. Modes & Approval Settings

Gemini CLI operates in different safety and behavioral profiles called **Modes**:

| Mode | Behavior |
| --- | --- |
| **Default** | Prompts you for confirmation before executing file edits or terminal commands. |
| **Auto-Edit (YOLO)** | Executes file mutations automatically without asking for permission (use with caution!). |
| **Plan Mode** | A read-only mode that forces the AI to draft a plan before touching anything (see Section 8). |

You can cycle through these execution modes interactively by pressing `Shift+Tab`.

---

## 6. Settings & Permissions

Manage global preferences, default models, and tool execution boundaries using the interactive configuration engine.

* **Command:** Type `/settings` inside the CLI.
* **UI:** An interactive menu will open allowing you to toggle permissions, configure allowed domains for browser tools, and set your default approval mode.

---

## 7. Context, Tokens, & Clearing

As you read files and chat, your context window fills up. If the session gets sluggish or you change tasks, you need to manage your memory.

* **Show context status:** `/context` shows exactly what files are currently injected into the model's memory.
* **Clear chat history:** `/clear` wipes the conversation logs but keeps pinned files.
* **Reset everything:** `/reset` clears history, unpins files, and resets the token counter.

```bash
# Wipe out the session history to start fresh
/clear

```

---

## 8. Global Sessions

By default, your terminal session is tied to the current directory. However, you can create and reconnect to **Global Sessions** across multiple terminal windows.

```bash
# Start a named global session from any directory
gemini --session frontend-refactor

# Reconnect to that exact same session later from another window
gemini --attach frontend-refactor

```

---

## 9. Gemini.md File & `/init`

You can bootstrap project-specific instructions and rules using a workspace configuration file.

* **Command:** Run `/init` in your project root.
* **Result:** Creates a `gemini.md` file.
* **Usage:** Anything written in this file (coding standards, architectural rules, tech stack details) is automatically loaded as system instructions whenever you launch `gemini` in this directory.

```markdown
# Project Rules
- Always write strict TypeScript.
- Use Tailwind CSS for styling.
- All backend components must log errors to standard output.

```

---


### Gemini CLI resolves GEMINI.md files hierarchically. It checks three locations:
- Global Level, at ~/.gemini/GEMINI.md in your user home directory. Use this for global preferences, system personas, and general coding standards that apply to all your projects.Project Root: 
- Project Root, at ./GEMINI.md in your root repository. Use this for workspace-wide context, rules, and tech stacks.Subdirectories: 
- Subdirectories, Gemini CLI will automatically pick up GEMINI.md files placed deep within subfolder to provide localized context for specific microservices or modules.

## 10. Plan Mode

**Plan Mode** separates *thinking* from *doing*. It restricts Gemini to read-only tools (`read_file`, `grep_search`), allowing it to analyze your system safely without accidentally mutating your codebase.

1. Type `/plan <your goal>` or switch to it via `Shift+Tab`.
2. Gemini uses the `ask_user` tool to clarify ambiguities.
3. It writes an execution blueprint as a Markdown file in your `.gemini/plans/` directory.
4. Once you approve the markdown plan, Gemini switches to edit mode and executes it.

```bash
/plan Migrate user database schema to add a billing_status string field

```

---

## 11. Feature-Focused Workflows (Keeping Context in Control)

To prevent your primary context window from overflowing ("context rot"), use a strict step-by-step workflow:

1. **Isolate your files:** Explicitly add only the files you need using `/add path/to/file`.
2. **Execute the task:** Let Gemini make changes.
3. **Drop context early:** Once a sub-feature is complete and committed to git, run `/clear` or remove files with `/drop path/to/file` before moving to the next feature.

---

## 12. Agent Skills

**Skills** are self-contained modular capabilities packaged into discrete directories. They allow you to bundle custom tools, reference manuals, or script templates without permanently bloating your system prompt.

* **Discovery Tiers:** Workspace (`.gemini/skills/`), User Global (`~/.gemini/skills/`), or Extension-provided.
* **Structure:** A skill folder contains a `SKILL.md` file with frontmatter defining its triggers.

```markdown
---
name: api-tester
description: Assists with generating and testing cURL requests for the local backend.
---
# Core Mandates
- Always check if the local server is running on port 8080 before testing.
- Format all JSON payloads cleanly.

```

* **Management:** Use `/skills` to list active skills. They auto-activate when your prompt matches the skill's description.

---

## 13. Model Context Protocol (MCPs)

The **Model Context Protocol (MCP)** connects Gemini CLI to your entire external engineering stack (GitHub, Postgres, Slack, etc.).

* **How it works:** You expose local or remote MCP servers in your configuration. Gemini CLI accesses them dynamically as standard tools.
* **Example configuration (`~/.gemini/config.json`):**

```json
{
  "mcpServers": {
    "postgres-db": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-postgres", "postgresql://localhost/dev_db"]
    }
  }
}
}

```

Inside the session, Gemini can now run queries directly using `mcp_postgres-db_execute_query`.

---

## 14. Sub-Agents

When dealing with massive scale, Gemini CLI shifts from a chatbot to an **Orchestrator**. It dispatches specialized **Sub-Agents** to perform heavy lifting (like code auditing or bug hunting) in isolated side-contexts.

* **Token efficiency:** Sub-agents run their own dense loops of file reads and tool executions, returning only a clean, finalized summary to your primary agent. This keeps your main chat lightning fast.
* **Built-in Specialists:** `codebase_investigator`, `cli_help`, and `browser_agent`.
* **Explicit invocation:** Use the `@` syntax to force delegation.

```bash
# Explicitly command a sub-agent to explore system dependencies
@codebase_investigator trace how user authentication tokens flow from the middleware to the database

```

---
