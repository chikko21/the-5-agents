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

## Obsidian Vault

The project root is the Obsidian vault. Notes and session logs live under `vault/`:

```
vault/
├── Meeting Notes/     # Code sessions, architecture, decisions
├── Content Briefs/    # Editorial briefs, campaign specs
├── Publishing Log/    # Publish runs, outcomes
└── Brand Guidelines/  # Voice, tone, visuals
```

Each folder has an `_index.md` listing all topics inside it.

## Mandatory Session Protocol

**At the start of every session or task**, invoke the `obsidian-vault-workflow` skill:
1. Identify the task topic
2. Open `vault/<folder>/_index.md` to locate the topic file
3. Read the topic file fully (Overview + Session Log) if it exists
4. Read 2–3 most recent `vault/Meeting Notes/` entries

**At the end of every task**, append a dated session entry to the relevant topic file (or create a new one) and update `_index.md`.
