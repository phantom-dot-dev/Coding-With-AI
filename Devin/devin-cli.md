Welcome to the master tutorial for the **Devin CLI** (`devin`). Devin operates as an autonomous digital software engineer right inside your terminal, leveraging an advanced loop of code editing, browser research, and sandbox execution.

---

## 1. Basic Usages & Most Frequently Used Commands

You can spin up Devin to tackle a project entirely autonomously from your terminal prompt, or enter an interactive session to work side-by-side.

* `devin "task"` - Launches an autonomous session to solve a specific problem.
* `devin interactive` - Opens an interactive shell session.
* `/status` - Displays active runtime loops, current spend, and agent state.
* `/stop` - Immediately halts Devin's current autonomous execution or testing loops.

```bash
# Ask Devin to autonomously find and fix a failing test suite
devin "Fix the broken edge cases in tests/auth.spec.ts"

```

---

## 2. Models Selection, Usages, & Remaining Tokens

Devin acts as an orchestrator, shifting between premium reasoning models depending on the task's complexity. You can monitor your token footprint and project costs dynamically.

* **Commands:** Use `/models` to check available routing engines, or `/model <name>` to pin a specific model.
* **Token Metrics:** Use `/status` to see input/output token counts and your remaining context budget.

```bash
# Force Devin to utilize a high-capacity model for the current terminal session
/model heavy-reasoning-v2

```

---

## 3. Effort (Reasoning Budget)

For heavy mathematical algorithms or deeply nested architectural design overhauls, you can control Devin's deep-thinking allocation.

* **Behavior:** Higher effort allows Devin's internal models to run extensive reasoning loops before proposing file modifications or executing commands.
* **Command:** `/effort <low | medium | high>`

```bash
# Dial up Devin's thinking budget for an complex database refactor
/effort high

```

---

## 4. Shell Mode & Sandboxing

Unlike typical command-line tools, Devin spins up a **secure, isolated sandbox environment** (a containerized shell) to safely install dependencies, compile code, and run test suites.

* **Direct Interception:** Prefix any command with `!` to run it inside Devin's active sandbox.
* **Autonomous Terminal Loops:** Devin can autonomously chain terminal commands to diagnose bugs.

```bash
# Force a clean build inside Devin's execution sandbox
!npm run build

```

---

## 5. Permission Modes

Devin can operate on a spectrum ranging from complete autonomy to strict human supervision. You can toggle these security profiles instantly inside the session:

| Mode | Command | Behavior |
| --- | --- | --- |
| **Autonomous** | `/mode autonomous` | Devin modifies files, executes sandbox scripts, and solves errors without waiting for approval. |
| **Interactive** | `/mode interactive` | *Default.* Devin pauses and asks for explicit confirmation (`Y/n`) before running terminal writes or changing code. |
| **Review** | `/mode review` | Read-only mode. Devin analyzes code and explains concepts but cannot mutate the environment. |

---

## 6. Settings & Permissions

Manage systemic access limits, allowed external web domains (for Devin's built-in browser tool), and credential handling.

* **Command:** Type `/settings` to access the interactive configuration matrix.
* **Command Whitelisting:** You can set rules allowing Devin to automatically run safe commands like `vitest` or `git status` while requiring a hard block confirmation for things like `rm -rf` or network requests.

---

## 7. Context, Tokens, & Clearing

As Devin reads files, runs build scripts, and tracks errors, its context window fills up. Managing this memory prevents context drift and keeps execution fast.

* `/context` - Displays a breakdown of every file, environment path, and dependency actively loaded into Devin's working memory.
* `/clear` - Wipes the conversation thread logs while keeping your targeted workspace files pinned.
* `/reset` - Erases the active session memory, dropping all file tracks and resetting token meters.

```bash
# Flush conversation logs to lower token overhead
/clear

```

---

## 8. Global Sessions

Devin keeps background tasks alive. You can kick off a massive engineering or migration job, shut down your laptop, and reconnect to the exact same execution thread later.

```bash
# Start a background task and name the global workspace
devin run "Migrate styling layer to Tailwind" --session tailwind-migration

# Re-attach and review Devin's progress from any terminal later
devin attach tailwind-migration

```

---

## 9. Plan Mode

Before writing any code for a major feature, Devin forces a strict decoupling of conceptual planning from physical file execution.

1. Trigger Plan Mode by executing `/plan <your feature goal>`.
2. Devin drops into a read-only state, scanning the project directory to build a step-by-step checklist.
3. It saves this plan file inside your workspace directory `.devin/plans/`.
4. You review the checklist steps. Once you approve, Devin executes them sequentially.

```bash
# Force Devin to draft a step-by-step execution roadmap
> /plan Integrate webhooks for Stripe checkout payment flows

```

---

## 10. Feature-Focused Workflows

To maintain control over token costs and avoid overloading Devin's context window, implement a tight, iterative development cycle:

1. **Target Ingestion:** Bring in only the specific files required for the immediate task using `/add path/to/file.ts`.
2. **Execute & Auto-Heal:** Issue your prompt. Let Devin edit the code and verify its work by running tests in the sandbox shell.
3. **Purge Overhead:** Once your git tree is clean and your feature is validated, run `/drop path/to/file.ts` or `/clear` before assigning the next sub-task.

---

## 11. Skills

**Skills** are plug-and-play engineering competencies. They are stored in markdown directories (`.devin/skills/` or globally at `~/.devin/skills/`) and lazy-load into memory only when Devin's current task matches their description.

```markdown
<!-- ~/.devin/skills/docker-audit/SKILL.md -->
---
name: docker-audit
description: Scans and hardens container configurations against security vulnerabilities.
---
# Action Protocol
1. Check all Dockerfiles for hardcoded secrets or exposed root configurations.
2. Ensure image pins use specific SHA hashes rather than mutable "latest" tags.

```

---

## 12. Model Context Protocol (MCPs)

Devin leverages the **Model Context Protocol (MCP)** to securely bridge its internal sandbox with external infrastructure such as enterprise databases, Jira, AWS instances, or GitHub repositories.

* **Setup:** Servers are configured in your global `~/.devin/mcp_config.json`.
* **Example Mapping:**

```json
{
  "mcpServers": {
    "aws-s3-manager": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-aws-s3"]
    }
  }
}

```

Devin can now interactively manage cloud storage states using tools like `mcp_aws-s3-manager_list_buckets` as part of its automated workflows.

---

## 13. Sub-Agents

When handling massive, multi-tiered project tracks, Devin converts from a single worker into a distributed software **Orchestrator**. It dispatches independent **Sub-Agents** to perform specialized tasks in parallel, isolated contexts.

* **Efficiency:** Sub-agents execute dense research, logging audits, or dependency tracing loops in their own side-contexts, passing only clean, summarized conclusions back to the primary workspace. This keeps the master context incredibly clean and cost-efficient.
* **Invocation:** Use the `@` operator to manually delegate a stream to a specialized runner.

```bash
# Dispatch a sub-agent to audit code style patterns across the codebase
> @codebase_investigator map out our routing architecture across all api routes

```

---

Now that you see how Devin handles everything from plan generation to parallel sub-agents, which workflow are you thinking about testing inside your repository first?
