### Overviews:
- Installation
- Basic use
- Most frequently used command
- Models Selection, Usages and Remaining tokens
- Effort
- Shell Mode
- Modes
- Settings & Permissions
- Context, Tokens & Clearing
- Global Sessions
- antigravity.md & AGENTS.md Files & /init
- Plan Mode
- Adding Feature Focused Workflow and keeping context and memory usage in control
- Skills
- MCPs
- Sub-Agents



Welcome to the definitive guide for **Antigravity CLI** (`agy`). Replacing the deprecated Gemini CLI, Antigravity CLI is Google's high-performance, terminal-first interface built in Go. It interfaces directly with the **Antigravity 2.0** agent harness, pairing Gemini models (like the Gemini 3.5 series) with advanced agentic loops (multi-step reasoning, tool execution, and concurrent background sub-agents).

---

## 1. Antigravity cli Installation & Authentication:

mac - 

linux - 

termux -



### First Launch & Authentication

Launch the interface by typing `agy` in your terminal. Follow the interactive prompts to authenticate via Google OAuth using your Google Account or a Google Cloud Platform (GCP) project.

```bash
# Verify installation
agy --version

# Initial launch & login initialization
agy

```

---

## 2. Basic Use & Most Frequently Used Commands

Antigravity CLI boots into an interactive Terminal User Interface (TUI). You can issue standard natural language requests or control the agent environment using `/` slash commands.

* `/help` - Opens the documentation dashboard split into commands, shortcuts, and keybindings.
* `/status` - Displays details on the active workspace, active model, and session metrics.
* `/config` or `/settings` - Accesses the interactive system configurations.
* `Ctrl + K` - Automatically approves a staged tool execution or code modification.

```bash
# Ask a direct engineering question right from your prompt
> Explain how our local Express server handles error bubbles from asynchronous middleware.

```


### Basic level 2 usages:
- Ask about a certain file `/add file-or-directory-name` and then ask about it through `/mode plan`

- check the plan result by ctrl+alt+click
- accept or cancel `/cancel` the result
- include some more notes (search Coding with ai browser conversation)

---

## 3. Models Selection, Usages, & Remaining Tokens

By default, the CLI links to highly efficient developer models like **Gemini 3.5 Flash** to maintain ultra-low latency.

* **List Available Models:** Execute `agy models` from your native shell or `/models` inside the interactive session.
* **Switching Models:** Use the `--model` flag at startup or `/model <name>` dynamically inline.
* **Inspecting Tokens:** Use `/status` to view exact real-time input/output tokens and prompt caching efficiency.

```bash
# Launch the CLI hardwired to the high-reasoning Pro engine
agy --model gemini-3.5-pro

# Dynamic switch inside an open session
/model gemini-3.5-flash

```

---

## 4. Effort (Reasoning Control)

When dealing with complex code generation, tricky architectural adjustments, or deep logical debugging, you can explicitly dial up the agent's internal reasoning or thinking budget.

* **Command:** `/effort <low | medium | high>`
* **Impact:** Setting effort to `high` grants the Gemini model extra token allocations to draft hidden chains of thought, evaluating solutions before modifying your files.

```bash
# Scale up reasoning power to tackle a multi-file dependency bug
/effort high

```

---

## 5. Shell Mode

Antigravity agents do not work in isolation—they can query your operating system, check build conditions, and test your runtime logs directly using **Shell Mode**.

* **User Manual Interception:** Simply prefix an inline statement with `!` to pass a command down to your underlying system shell without closing the agent wrapper.
* **Autonomous Loops:** You can explicitly authorize the agent to execute test commands on its own. It will analyze your build output, capture failures, and iterate on repairs until the code compiles perfectly.

```bash
# Quick local check inside the session
!git status

# Instruct the agent to run a shell command and auto-heal any errors
> Run "npm run test" and fix any failing edge-cases found in users.test.ts

```

---

## 6. Permission Modes

To protect your file systems from unsafe or unexpected automation, Antigravity CLI implements strict tool boundaries. You can toggle these execution styles instantly using `Shift + Tab` or via `/config`:

| Mode | Behavior |
| --- | --- |
| **`request-review`** *(Default)* | Absolute safety. The agent pauses and displays an approval banner before executing any command or modifying files. |
| **`proceed-in-sandbox`** | Runs terminal executions inside an isolated virtual container to preview side-effects safely. |
| **`always-proceed`** *(YOLO)* | Gives the agent total autonomy. Edits are committed instantly—ideal for rapid feature building on clean git trees. |
| **`strict`** | Disables terminal command execution entirely, converting the agent into a read-only advisor. |

---

## 7. Context, Tokens, & Clearing

As you explore a project, your session context fills up with code snippets, dependency patterns, and CLI histories. To keep your token spend down and prevent semantic noise ("context drift"), you must handle your memory footprint gracefully.

* `/context` - Prints a granular list of files, paths, and metadata actively loaded into the agent's immediate focus window.
* `/clear` - Discards long-form chat interaction history while keeping your pinned workspace files intact.
* `/reset` - Completely wipes the current session stack, flushing the conversation, dropping pinned paths, and returning token calculations back to zero.

```bash
# Clear accumulated conversational logs
/clear

```

---

## 8. Global Sessions

Antigravity tracks environment configurations in a central space rather than dropping messy local configuration assets into your clean software repositories. You can spin up isolated, named global workspaces and multiplex them across your machine.

```bash
# Initialize an isolated backend development tracking channel
agy --session api-overhaul

# Connect a parallel terminal split into that exact same session 
agy --attach api-overhaul

```

---

## 9. Configuration Blueprints (`antigravity.md`, `AGENTS.md`, & `/init`)

You can lock in consistent rules, coding styles, and architecture mandates by checking local policy definitions directly into your project workspace.

### Workspace Bootstrapping

Running `/init` creates structural blueprints that act as persistent system instructions whenever an `agy` context initializes in that path.

* `antigravity.md` - Holds structural, language, style, and testing requirements for the core agent loop.
* `AGENTS.md` - Explicitly dictates roles, tool constraints, and team layout assignments for any spawned sub-agents.

```markdown
<!-- Sample snippet of a project's antigravity.md -->
# Engineering Mandates
- Framework: Next.js App Router with strict TypeScript
- Testing: All components must maintain 90%+ unit test coverage via Vitest
- Styling: Only functional Tailwind utility classes are permitted

```

---

## 10. Plan Mode

When preparing to initiate a massive project modification, you don't want an AI jumping in blindly. **Plan Mode** splits reasoning away from active execution.

1. Toggle Plan Mode by typing `/mode plan` or passing your request directly to the plan module.
2. The agent restricts its tools strictly to read-only capabilities (`read_file`, `grep_search`).
3. It drafts a comprehensive architectural change blueprint inside `.antigravitycli/plans/`.
4. You inspect the markdown proposal. Once verified, you switch back to execution mode to let the agent apply the changes step-by-step.

```bash
# Request a comprehensive plan to rewrite your app's payment gateway
> /mode plan Replace Stripe checkout hooks with a dynamic Paddle subscription service

```

---

## 11. Feature-Focused Workflows

Keep your agent accurate and your memory usage lean by breaking down large tasks into tight, episodic cycles:

```
[Isolate Files] ---> [Run Prompt / Fix] ---> [Test via Shell] ---> [Reset Context]
  (/add file)          (agy "...")          (!npm run test)         (/clear)

```

1. **Target Ingestion:** Explicitly load only the files needed for the sub-task using `/add path/to/target.ts`.
2. **Execute:** Run the change prompt, letting the agent edit code and confirm correctness via internal shell verification commands.
3. **Flush Context:** Once your git status is green and committed, execute `/clear` or `/drop path/to/target.ts` to clear out background noise before starting on the next component.

---

## 12. Skills

**Skills** are modular capabilities wrapped in clean markdown folders that supercharge your agent with specialized features. Using *progressive disclosure*, Antigravity CLI does not pack massive skill logs into the primary system prompt; it reads basic descriptions initially and loads the full instructions only when needed.

* **Locations:** Workspace (`.antigravity/skills/`) or Global User Space (`~/.gemini/antigravity-cli/skills/`).
* **Format:** A skill contains a folder with a `SKILL.md` layout establishing its activation parameters:

```markdown
<!-- ~/.gemini/antigravity-cli/skills/db-migrate/SKILL.md -->
---
name: db-migrate
description: Handles generation and diagnostic checks for Prisma migration scripts.
---
# Operational Runbook
1. Always audit generated SQL definitions against the target schema snapshot.
2. Run database connection connectivity checks before executing live mutations.

```

---

## 13. Model Context Protocol (MCPs)

The **Model Context Protocol (MCP)** acts as a universal link, mapping Antigravity CLI directly to enterprise logging platforms, remote databases, Slack networks, or external software tools.

Unlike historical configurations, Antigravity CLI splits its MCP definitions out into a dedicated external orchestration matrix.

* **Path:** Configured in `~/.gemini/antigravity-cli/mcp_config.json`
* **Example Setup:**

```json
{
  "mcpServers": {
    "production-db": {
      "command": "docker",
      "args": ["run", "--rm", "-i", "armlimited/arm-mcp:latest"],
      "env": {}
    }
  }
}

```

Once mapped, your active agent can seamlessly execute actions like `mcp_production-db_analyze_query` inline as if it were a native command line feature.

---

## 14. Sub-Agents

For massive, multi-threaded refactoring pipelines, a single agent loop can easily bog down. Antigravity CLI solves this by functioning as an **Orchestrator Hub**, spawning isolated, concurrent background **Sub-Agents**.

* **Asynchronous Processing:** Sub-agents branch off into isolated context windows to do intense research or multi-file builds without locking up your primary terminal interface.
* **Built-in Specialists:** Invoke tools like `@codebase_investigator` or `@browser_agent` directly.
* **Monitoring:** Monitor active background task loops cleanly via the `/agents` command panel.

```bash
# Spin up an isolated sub-agent thread to map database flows in the background
> @codebase_investigator trace all foreign key constraints linked to user session deletions

```

---
