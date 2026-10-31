# Metistech Claude Code plugins

Internal plugin for the Metistech team. Skills show up as `metistech:<skill>`.

## Skills

- **metistech:linear-time-logger** — log time on Linear issues as formatted comments, fix entries, and report hours per person, issue, project or period.

## Setup

1. Connect Linear with your own Linear account. Changes are attributed to whoever is signed in, and the email in each time log comes from that account, so don't share a connection.
   - **claude.ai connector:** claude.ai → Settings → Connectors → Linear. This only reaches Claude Code if you are signed in to Claude Code with the same claude.ai account.
   - **Direct connection** (otherwise): `claude mcp add --transport http linear https://mcp.linear.app/mcp`, then `/mcp` to sign in.
2. Install the plugin:
   ```
   claude plugin marketplace add gabcc/metistech-claude-plugins
   claude plugin install metistech@metistech-plugins
   ```
3. Start a new Claude Code session and try: "log 1h on MET-<n> for testing the time logger".

## Using the time logger

Linear has no native time tracking, so each entry is one comment on the issue, in this format:

```
⏱ <email> | <YYYY-MM-DD> | <hours>h | <description>
```

Example: `⏱ gabor.percze@metistech.hu | 2026-10-31 | 1.5h | Debugged OAuth redirect issue`

The date is the day the work was done, and hours are decimal. Ask in plain language:

- **Log:** "log 2h on MET-12 for the API review", "log 30 min yesterday on MET-7 fixing the export", "log 1h on MET-9 for <colleague>"
- **Correct:** "change my 2h on MET-12 to 2.5h", "delete my last log on MET-7"
- **Report:** "how many hours on MET-12?", "sum my hours for October", "hours per person on project X this week"

Edit or delete a log by asking Claude, or by editing the comment in Linear. Keep the format intact, or the entry shows up as malformed in reports.
