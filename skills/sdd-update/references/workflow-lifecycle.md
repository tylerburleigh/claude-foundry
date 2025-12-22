# Spec Lifecycle Workflows

Moving specs between folders and PR handoff.

## Moving Specs Between Folders

### Activate from Backlog

Move a spec from pending/ to active/ when ready to start work:

```bash
mcp__plugin_foundry_foundry-mcp__lifecycle action="activate" spec_id={spec-id}
```

This updates metadata status to "active" and makes the spec visible to sdd-next.

### Move to Completed

Use `lifecycle action="complete"` to properly complete and move a spec:

```bash
mcp__plugin_foundry_foundry-mcp__lifecycle action="complete" spec_id={spec-id}
```

### Archive Superseded Specs

Move specs that are no longer relevant:

```bash
mcp__plugin_foundry_foundry-mcp__lifecycle action="move" spec_id={spec-id} to_folder="archived"
```

---

## Git Push & PR Handoff

When a spec is completed, the workflow can automatically push commits to the remote repository. Pull request creation is handed off to the dedicated PR workflow.

### Push-Only Scope (PRs Deferred)

- `Skill(foundry:sdd-update)` handles local commits and optional pushes during completion workflows, but it does **not** create pull requests.
- Pull requests are beyond the scope of your responsibilities.
- If push automation is disabled or fails, finish the spec update, run `git push -u origin <branch>` manually, and then involve the PR skill/agent.
