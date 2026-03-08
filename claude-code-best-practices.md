# Claude Code — Best Practices

> A recommended workflow from project setup to full automation.  
> Designed as a GitHub reference guide.

---

## ① Setup & Project Onboarding

| # | What You're Doing | Command / Example | Purpose |
|---|-------------------|-------------------|---------|
| 1 | **Navigate to repo** | `cd project` | Claude works in the right context |
| 2 | **Start project analysis** | `/init` | Claude learns the project structure, dependencies, and architecture; auto-generates `CLAUDE.md` |
| 3 | **Write project rules** | Edit `CLAUDE.md` | Build/test/lint commands, architectural decisions, code style — keep it under 200 lines |

> 💡 Working without `CLAUDE.md` is the source of **60% of Claude Code issues**. A simple file resolves 90% of cases.

---

## ② Daily Coding

| # | What You're Doing | Command / Example | Purpose |
|---|-------------------|-------------------|---------|
| 4 | **Handle small tasks directly** | `Add validation to @LoginService.java` | Pin files with `@` — context is set correctly, no plan needed |
| 5 | **Plan before big tasks** | `Propose a plan to implement X` | Say "don't write code yet, just give me the plan" → review → approve → implement |
| 6 | **Verification & testing** | `Verify with: <test command>` | Use a **separate instance** for writing tests vs. reviewing code — improves accuracy |

> 💡 **Recommended flow for Step 5:**
> 1. Enter Plan Mode
> 2. Give a high-level description + point to relevant files
> 3. Let Claude research and propose an approach
> 4. Review the plan, correct misunderstandings
> 5. Approve → `implement from your plan`

---

## ③ Context Management — Do This Continuously

| # | What You're Doing | Command / Example | Purpose |
|---|-------------------|-------------------|---------|
| 7 | **Check context usage** | `/context` | See current token usage — manage proactively |
| 8 | **Compact the context** | `/compact` | Use manually at **50% max**; don't leave it to auto |
| 9 | **Reset when switching tasks** | `/clear` | Clear between Research → Plan → Implement → Validate phases |

> ⚠️ Context degradation is the most common failure mode. Intervene **before hitting 60%**.

---

## ④ Subagent Usage

A subagent is Claude spawning new Claude instances for subtasks.  
For large, parallel workloads, this creates an orchestrator → worker architecture.

| # | What You're Doing | Command / Example | Purpose |
|---|-------------------|-------------------|---------|
| 10 | **Make the main agent an orchestrator** | Add subagent instructions to `CLAUDE.md` | Main Claude coordinates, subagents execute |
| 11 | **Set up feature-specific subagents** | `.claude/skills/qa-agent.md` | Focused agents like "auth module QA" outperform generic ones |
| 12 | **Run parallel instances** | Different panels in VS Code | Advance independent tasks simultaneously — no conflicts across features |

> 💡 **Recommended subagent architecture:**
>
> ```
> Main Claude (orchestrator)
> ├── Subagent A → writes tests
> ├── Subagent B → reviews code
> └── Subagent C → updates documentation
> ```
>
> - Main agent spawns subagents via `Task(...)` and manages context itself
> - Each subagent should have **a single responsibility**
> - Don't give subagents more than 20k tokens of MCP — leaves no room for real work
> - Build **feature-specific** agents, not generic ones (e.g. `payment-service-agent`, not `backend-engineer`)

---

## ⑤ Automation

| # | What You're Doing | Command / Example | Purpose |
|---|-------------------|-------------------|---------|
| 13 | **Write custom slash commands** | `/security-review` | Add Markdown to `.claude/commands/` → reusable prompt template |
| 14 | **Set up hooks** | `/hooks` | `CLAUDE.md` is advisory; hooks are mandatory — lint/test runs automatically after every file edit |

> 💡 Hook example prompt: `"Write a hook that runs eslint after every file edit"`  
> ⚠️ Too many complex slash commands is an anti-pattern — keep it simple.

---

## Golden Rules

```
1. Always create CLAUDE.md for every project — keep it under 200 lines
2. Big task = plan first, code second
3. Compact or clear context before hitting 60%
4. Separate the instance that writes tests from the one that reviews code
5. Small tasks → plain Claude Code
   Large / parallel tasks → subagent architecture
6. Hooks are stronger than CLAUDE.md — put critical rules in hooks
```

---

## References

- [Anthropic — Claude Code Best Practices](https://code.claude.com/docs/en/best-practices)
- [Claude Code Docs](https://docs.claude.com/en/docs/claude-code/overview)
