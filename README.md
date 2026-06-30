# AgentSkills

Open skills that improve the reliability of interaction with LLM agents.
## Who this is for

People using CLI agents like Codex, Claude Code, or any LLM agent that supports the open skills format.
## Skills

- `verify-third-party-sources` - when interacting with third-party tools, the skill forces the agent to first identify their version and then use it to verify information according to a defined source priority. This helps the agent rely less on model memory and use more reliable, up-to-date data.

Install skills with your agent's installer and a GitHub URL:
- https://agentskills.io/integrate-skills
- https://developers.openai.com/codex/skills/
- https://code.claude.com/docs/en/skills
## Repository structure

- Each skill directory contains an SKILL.md file with basic instructions on how to use the skill.
- Platform-specific agent metadata is located in the agents/ directory.
## License

MIT. See `LICENSE`.
