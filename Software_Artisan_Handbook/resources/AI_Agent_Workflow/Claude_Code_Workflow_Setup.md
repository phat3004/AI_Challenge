# Claude Code Workflow Setup

This page describes Papaya's preferred Claude Code workflow setup. The goal is to make agent work safer, easier to review, and easier to turn into repeatable automation.

## Default Workflow

Use Claude Code as the primary coding tool, with Superpowers-style flow for most non-trivial tasks:

1. **Brainstorm** -- clarify the task, constraints, risks, and success criteria.
2. **Plan** -- let Claude inspect the codebase and propose a solution.
3. **Review** -- human reviews the plan before implementation.
4. **Implement** -- Claude edits code in a dedicated branch/worktree.
5. **Verify** -- Claude runs tests, type checks, linters, screenshots, or other relevant checks.
6. **Review agents** -- reviewer or quality-control agents inspect the diff.
7. **PR** -- Claude prepares the PR, including agent workflow notes and verification evidence.

Simple, low-risk fixes can skip the full brainstorm/plan flow, but they still need a clear prompt, narrow scope, and verification.

## Branch, Worktree, and PR Rules

Prefer this structure:

- **1 task = 1 branch = 1 worktree = 1 PR**
- Do not edit directly on `main`, `dev`, or the primary local worktree.
- Do not run multiple parallel worktrees inside the same Claude Code session.
- Remove the task worktree after the PR is merged.
- Keep each PR focused on one coherent change.

This keeps context, git state, review scope, and cleanup simple.

## Superpowers Plugin

Use the Superpowers plugin for most tasks, especially work that involves design, implementation, debugging, or review.

Recommended default:
- Start with "brainstorm and plan" for non-trivial work.
- Use the generated plan as the contract for implementation.
- Run reviewer and quality-control agents before PR review.
- Use the workflow's checkpoints instead of letting a session drift.

Skip the full workflow only for small obvious fixes, such as typo fixes, one-line config changes, or targeted documentation edits.

## Harnesses

Claude Code works best when surrounded by stable harnesses:

- **Skills** -- team conventions, domain knowledge, and repeatable review rules.
- **Hooks** -- safety checks, formatting, output filtering, and workflow automation.
- **Subagents** -- investigation, code review, QA, debugging, and documentation workers.
- **Statusline** -- visible session state.
- **Rules/settings** -- permission boundaries, auto mode configuration, and default models.

The more stable a rule is, the more it should move out of prompts and into a harness.

## Statusline

Use a custom Claude Code statusline so the engineer can see session state at a glance.

Useful fields:
- Current model
- Current worktree directory
- Current branch
- Context usage
- Current plan or usage limit
- Cost or duration, if useful

The statusline helps prevent common mistakes, such as editing the wrong worktree or continuing a session with an unhealthy context window.

## Context and Compacting

Use context deliberately:

- Start a fresh session for a new task.
- Use `/clear` between unrelated tasks.
- Use `/compact` before the session becomes noisy.
- Prefer compacting around 300k-500k tokens when the session is still coherent, instead of waiting for emergency compaction near the limit.
- Add compact instructions so Claude preserves decisions, changed files, test results, risks, and remaining work.

A 1M-token window is useful for hard tasks, but it is not a substitute for context hygiene. Context rot is real: stale decisions, old errors, and irrelevant exploration can reduce output quality.

## Auto Mode

Auto mode is a good default when the prompt is clear and the environment is configured.

Use auto mode for routine development actions:
- Reading and editing files in the task worktree
- Running project tests and linters
- Committing and preparing PRs
- Using trusted internal CLIs and APIs

Keep explicit approval for risky actions:
- Production data changes
- Destructive database operations
- Force pushes
- Secret or environment file access
- External data exfiltration risks
- Customer account changes

Configure trusted infrastructure and deny rules in Claude Code settings or managed settings. Do not rely only on memory or prompt wording for safety.

## CLI, API, SDK, and MCP

Prefer the most stable and automatable tool surface:

1. **CLI** -- best when a reliable command exists (`gh`, project CLI, cloud CLI, internal tools).
2. **API/SDK** -- best for repeatable automation and scripts.
3. **MCP** -- best when Claude needs a richer interactive tool interface.

CLI/API/SDK paths are often easier for Claude to automate, easier to test, and cheaper in context than large tool surfaces. MCP is still valuable when it provides capabilities that are otherwise hard to script.

## Model Defaults

Use the latest team-approved Opus-class model for complex main sessions. As of the current Anthropic model guidance, Claude Opus 4.7 is positioned for complex reasoning and agentic coding with a 1M-token context window.

Use smaller or faster models for subagents when appropriate:
- **Sonnet** -- balanced code review, QA, and implementation support.
- **Haiku** -- fast exploration, lightweight checks, summarization, and narrow investigations.

Avoid hardcoding model names into long-lived process docs unless the team commits to keeping them updated. Prefer phrases such as "latest team-approved Opus-class model" where possible.

## Linear and PR Automation

Use automation to keep task tracking current:

- Move Linear/Jira status when implementation starts, PR opens, review starts, and PR merges.
- Upload or link plans/specs when useful.
- Add comments with verification results and known risks.
- Include agent workflow notes in PR descriptions.
- Remove the worktree after merge.

If Claude repeatedly performs the same tracking task manually, turn it into automation.

## PR Quality Gate

Before marking a PR ready for human review, ask Claude Code to run reviewer or quality-control agents.

The agents should check:
- Scope creep
- Convention skill compliance
- Missing tests or weak verification
- Business logic assumptions
- Security-sensitive changes
- Data migration and permission risks
- Unnecessary abstractions
- Leftover TODOs, debug logs, mock data, or placeholders

The human reviewer should receive a small, clear review surface: what changed, why, what was verified, and what remains risky.
