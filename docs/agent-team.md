# Agent team

We will use a small custom agent team in GitHub Copilot CLI inside a Codespace to orchestrate the work for Mona's Project Pulse dashboard.

- Orchestrator — Model: Claude Opus 4.7 (copilot). Responsibility: coordinates the full workflow, delegates tasks to the specialist agents, keeps phases organized, and validates that the final dashboard hangs together. Definition: `.github/agents/orchestrator.agent.md`.
- Planner — Model: Claude Opus 4.7 (copilot). Responsibility: researches the repository, identifies requirements and edge cases, and produces the implementation plan for Project Pulse before coding begins. Definition: `.github/agents/planner.agent.md`.
- Coder — Model: GPT-5.5 (copilot). Responsibility: writes the actual dashboard code, keeps the implementation aligned with repo patterns, and creates support files such as `.vscode/launch.json` so the app can run as a browser preview. Definition: `.github/agents/coder.agent.md`.
- Designer — Model: Gemini 3.1 Pro (copilot). Responsibility: shapes the dashboard UX, visual hierarchy, accessibility, layout, and interaction design so the first view clearly reads as a polished Project Pulse frontend. Definition: `.github/agents/designer.agent.md`.

How the team will work together:

1. The Orchestrator starts the process and asks the Planner to research the repo and build a plan.
2. The Planner identifies implementation steps, file ownership, dependencies, and validation expectations.
3. The Designer focuses on the dashboard experience and design choices for Project Pulse.
4. The Coder implements the UI, styling, and runnable preview configuration.
5. The Orchestrator reviews the integrated result and reports the final outcome to the user.

This team is managed from GitHub Copilot CLI in a Codespace, with the Orchestrator acting as the coordinator across the specialized agent definitions under `.github/agents/`.
