# Lessons Learned

## Skill Design

- **Don't instruct the agent to warn-and-continue about model selection.** The agent can't switch its own model, and warning while proceeding anyway is bad UX. Just note the model in the cost report and let the user switch beforehand if they care.
- **Skill descriptions must be broad enough to trigger on direct tasks**, not just meta-requests like "run a usability test." If the skill should fire whenever someone browses CustomInk.com, say that explicitly — don't assume "navigate customink" will match "navigate to customink.com and add a product."
- **Subagents cannot access MCP tools.** Don't design skills that delegate Chrome browsing to a spawned Agent — MCP server connections aren't inherited.
- **Explicitly authorize password entry for test accounts.** Claude's default behavior refuses to type passwords into forms. If the skill uses test credentials from `.env`, the instructions must explicitly state the agent is authorized to enter both email and password — otherwise it will ask the user to type the password manually.
- **All test artifacts must be persisted to disk, not just printed to console.** Screenshots, narration logs, and final reports need to be saved to a timestamped run folder. If the skill doesn't explicitly require writing files, the agent will just print everything to the conversation and nothing survives the session.
- **Make the final report mandatory with a checklist.** Saying "generate a report at the end" is too easy to skip. Use a "Session End Checklist" with specific file names the agent must verify exist before stopping.
- **Make the agent announce its model as the first message.** "Note it for the report" means the user won't see it until the end. Require it upfront so the user can stop and switch if needed.
- **Chrome MCP screenshots don't save to disk.** `mcp__claude-in-chrome__computer` (screenshot) returns the image in the conversation only. To save screenshots as files, use `mcp__plugin_chrome-devtools-mcp_chrome-devtools__take_screenshot` with the `filePath` parameter.
- **DevTools MCP and Claude in Chrome MCP don't share page context.** DevTools must be synced to the correct tab via `list_pages` + `select_page` before `take_screenshot` will work. Must re-sync after any navigation that changes the URL, otherwise screenshots are blank.
- **Never guess URLs — only click links.** A real user doesn't type `/account/favorites` into the address bar. The only typed URL should be the starting page. Everything else must be reached through the site's own navigation, links, and search. If you can't find it by clicking, that's a usability finding.
- **Flag any non-www.customink.com domains as red flags.** If a link points to a subdomain like `catalog-lbr.out.customink.com` or any domain that isn't `www.customink.com`, report it immediately. This is unexpected and should be called out in the narration and report.
