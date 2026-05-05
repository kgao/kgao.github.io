---
layout: blog
category: blogs
title: 'Agentic Coding Is a Spectrum: A Principle for AI vs Human Leverage'
date: '2026-05-05'
author: Kgao
tags: 'AI,Agentic Coding,GitHub Copilot,Developer Productivity,Engineering'
public: true
---

> Inspired by Lars Faye's article *"Agentic Coding is a Trap"* and many recent conversations with engineers about how they actually use Copilot CLI, Copilot in VS Code, and the GitHub coding agent in practice.

## ***The Problem***

Agentic coding tools have exploded in the last year. We now have at least three serious surfaces to choose from:

- **Copilot CLI** — terminal-native agent with `/plan`, `/fleet`, autopilot, model switching, and GitHub-native MCP integration.
- **GitHub Copilot in VS Code** — editor-native agent with Plan agent, local/background/cloud handoff, and strong file-level visibility.
- **GitHub Copilot coding agent** — assign an issue inside GitHub, the agent works asynchronously in an isolated environment and opens a PR for review.

Each surface can be configured to run the same underlying model. So the real question is no longer *"which AI is smarter?"* — it's:

> **Where do we want the human reasoning loop to live?**

The answer should not be one-size-fits-all. Yet today, most teams adopt a single workflow as default and stretch it across every kind of task. **We don't have a shared principle for how much AI leverage is appropriate for a given task.**

## ***The Insight***

Agentic coding use cases vary widely. The right level of AI leverage depends on three task properties:

1. **Ambiguity** — how unclear are the requirements or solution path?
2. **Creativity** — how novel or design-heavy is the work?
3. **Criticality** — what is the blast radius if it goes wrong?

The more of these dimensions a task scores high on, the more a human needs to stay close to the code. The lower the scores, the more we can safely delegate.

This gives us a spectrum, not a switch.

![Agentic Coding Dashboard](../../../../../img/blog/agentic_coding_dashboard.png)

## ***A Heuristic Model***

To make the spectrum concrete, here is a simple heuristic: rate a task on ambiguity, creativity, and criticality from 1 to 5, then read off a recommended AI automation percentage.

![AI vs Human Leverage Heatmap](../../../../../img/blog/ai_human_coding_leverage_heatmap.png)

The pattern is intuitive:

- **Top-right corners** (high ambiguity, high criticality, high creativity) → AI ≈ 20–35%. Human-led, AI-assisted.
- **Middle band** (medium everything) → AI ≈ 45–60%. Human-guided AI collaboration.
- **Bottom-left corners** (low ambiguity, low criticality, routine work) → AI ≈ 70–85%. Delegate, then review carefully.

This isn't empirical data — it's a decision aid. But it gives teams a shared vocabulary for talking about *how much* to lean on AI on any given task.

## ***Three Bands, Three Workflows***

Mapped to the agentic coding surfaces we have today:

![Rule of Thumb by Task Class](../../../../../img/blog/tool_option_preview.png)

### Band A — Routine, bounded work (~70–85% AI)
Test additions, doc updates, narrow bug fixes, repetitive migrations.
- **Best surface:** GitHub Copilot coding agent (assign-to-issue → PR), or Copilot CLI in autopilot / `/fleet` mode.
- **Human role:** crisp acceptance criteria, careful PR review, merge.

### Band B — Normal product work (~45–70% AI)
Feature work in known architecture, refactors with business logic, multi-module bug fixes.
- **Best surface:** Copilot in VS Code (Plan + Agent), or Copilot CLI in interactive `/plan` mode.
- **Human role:** co-design the plan, stay in the loop, inspect diffs continuously.
- **Why this is the best default:** strongest balance of speed, visibility, and understanding.

### Band C — Ambiguous, critical, creative work (~15–45% AI)
New architecture, security/auth, performance-sensitive code, exploratory engineering.
- **Best surface:** human-led local coding; AI as advisor, reviewer, and snippet generator.
- **Human role:** drive design and implementation directly. The act of writing the code *is* part of thinking through the problem.

## ***The Underlying Principle***

> **Use AI more as tasks become routine, bounded, and low-risk. Use humans more as tasks become ambiguous, creative, and critical.**

The mistake is not "using AI too much." The mistake is using *the same level* of AI delegation for every task — usually because that's what the team's tooling default happens to be.

The trap Lars Faye describes is real: when PR review becomes your first real point of contact with the implementation, you accumulate comprehension debt and the very skills required to supervise the agent well start to atrophy.

The way out isn't to abandon agentic coding. It's to **match the level of automation to the shape of the task** — and to make that match an explicit, shared engineering decision rather than an accidental side effect of which tool is open.

## ***A Simple Team Policy***

If you want one sentence to put in your team handbook:

> *Default to Copilot in VS Code (Plan + Agent) for normal product work. Use Copilot CLI for terminal-native, parallel, or autopilot-friendly tasks. Reserve issue-to-PR cloud agent delegation for clearly scoped backlog work. For ambiguous, critical, or creative work, stay human-led and use AI as an advisor.*

That's the principle. The percentages are just a way of making it visible.
