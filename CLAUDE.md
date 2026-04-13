# Roamer — CustomInk Usability Testing

Automated usability testing of CustomInk.com via a Claude Code skill that browses as a think-aloud study participant, navigating via accessibility elements for speed.

## Project Structure

```
.claude/skills/customink-a11y-navigator/
  SKILL.md        # Skill instructions (behavioral rules, navigation, protocol)
  REFERENCE.md    # Templates, WCAG criteria, JS snippets, cost tables
.env              # Test account credentials (not committed — copy from .env.example)
.env.example      # Credential template with placeholders
lessons.md        # Learnings from test runs
reports/          # Test run output (not committed — contains screenshots & reports)
  <test-name>/    # Grouped by task (e.g., save-favorites, checkout-flow)
    YYYY-MM-DD-HHMMSS/
      screenshots/
      narration.md
      report.md
```

## Quick Reference

- **Skill:** `/customink-a11y-navigator` — all operational rules live in SKILL.md
- **Credentials:** `.env` — placeholder values must be replaced before login flows work
- **Lessons:** Track learnings from test runs in `lessons.md` at project root

## Error Handling

If Chrome MCP tools fail to connect, report the error and stop — do not retry indefinitely.
