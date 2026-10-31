# Metistech Claude Code plugins

Internal plugin for the Metistech team. Skills show up as `metistech:<skill>`.

## Skills

- **metistech:linear-time-logger** — log time on Linear issues as formatted comments, fix entries, and report hours per person, issue, project or period.
  Format: `⏱ <email> | <YYYY-MM-DD> | <hours>h | <description>`

## Setup

1. Connect Linear: claude.ai → Settings → Connectors → Linear, or in Claude Code:
   `claude mcp add --transport http linear https://mcp.linear.app/mcp`, then `/mcp` to sign in.
2. Install the plugin (you need read access to this private repo, and `git clone` from GitHub must already work for you):
   ```
   claude plugin marketplace add gabcc/metistech-claude-plugins
   claude plugin install metistech@metistech-plugins
   ```
3. Start a new Claude Code session and try: "log 1h on MET-1 for testing the time logger".

## Updating

Bump `version` in `.claude-plugin/plugin.json`, then on each machine:
`claude plugin marketplace update metistech-plugins && claude plugin update metistech@metistech-plugins`
