# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A multi-agent content creation system managed by a lead agent (CEO agent) that orchestrates a team of specialized sub-agents. The agents, their roles, and workflows will be defined as the project evolves.

## Project Structure

```
.claude/
├── agents/    # Custom agent definitions for this project
├── skills/    # Reusable skills available to agents
└── commands/  # Custom slash commands for this project
```

All agent, skill, and command configurations live under `.claude/` and will be populated incrementally.
