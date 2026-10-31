---
name: linear-time-logger
description: Time logs on Linear issues, stored as formatted comments. Use when logging time or hours worked on a Linear issue (e.g. "log 2h on MET-12"), correcting a time log, or reporting/summing logged hours per person, issue, project or period.
---

# Linear time log

Linear has no native time tracking, so each time entry is one comment on the issue, in a fixed line format. Tools below are from the Linear MCP server (`whoami`, `save_comment`, `list_comments`, `list_issues`, `list_users`, `delete_comment`); the prefix depends on how Linear is connected.

## Format

```
⏱ <email> | <YYYY-MM-DD> | <hours>h | <description>
```

Example: `⏱ gabor.percze@metistech.hu | 2026-10-31 | 1.5h | Debugged OAuth redirect issue`

- **Marker**: the line starts with `⏱` followed by one space. It is what identifies a comment as a time log.
- **email**: the Linear email of the person who did the work.
- **date**: the day the work was done, absolute ISO date.
- **hours**: decimal hours, up to 2 decimals, then `h`: `0.25h`, `1.5h`, `3h`. Convert other units exactly (90 min → `1.5h`, 20 min → `0.33h`).
- **description**: one line, last field. Split a line on the first three ` | `; everything after the third belongs to the description, so it may contain `|` or `-`.

The comment body is exactly this one line, nothing else.

## Logging time

1. **Resolve the fields.**
   - Issue: use the identifier the user gave (e.g. `MET-12`). If they described it instead, search with `list_issues` (`query`) and confirm the match before posting.
   - Email: the authenticated user from `whoami`. If the user logs for a colleague, look up that colleague's email with `list_users`.
   - Date: today unless stated; resolve relative dates ("yesterday", "Friday") to the absolute date.
   - Hours and description: from the request. Ask when either is missing; write the description as a short past-tense summary of the work.
2. **Post** one comment per entry with `save_comment` (`issueId`, `body`). Several entries in one request → several comments.
3. **Report back** each posted line with its issue identifier.

Done when every requested entry exists as its own correctly formatted comment.

## Correcting a log

Find the comment with `list_comments` on the issue, then rewrite it with `save_comment` (`id`, new `body`) in the same format. Use `delete_comment` only when the user asks to remove the entry. Edit only `⏱` comments whose email matches the authenticated user, unless the user explicitly says to change a colleague's entry.

## Reporting

1. **Collect issues** in scope with `list_issues`, using the filters the user gave (team, project, assignee, cycle). Set `includeArchived: true` for past periods. Leave out `updatedAt` filtering: a new comment may not bump the issue's `updatedAt`, so it can drop issues that have logs. Page through every `cursor` until done.
2. **Collect logs**: `list_comments` for each issue (page through all), keep comments whose body starts with `⏱`, parse the four fields.
3. **Filter** by the work date field (not the comment's `createdAt`) and by person if asked.
4. **Summarise** as a markdown table grouped the way the user asked (default: per person, then per issue), with a total row. Include issue identifiers.
5. **Flag malformed lines** (a `⏱` comment that does not parse: bad date, missing `h`, wrong field count) in a separate list with issue and author, rather than dropping them silently.

Done when every issue in scope has been read and every `⏱` comment is either in the totals or in the malformed list.
