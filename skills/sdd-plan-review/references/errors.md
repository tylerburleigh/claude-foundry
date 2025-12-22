# Error Handling

| Error | Behavior | Impact |
|-------|----------|--------|
| Timeout (>120s) | Retry with backoff | Longer wait |
| Rate limit (429) | Sequential fallback | Slower execution |
| Auth failure | Skip tool, continue | Reduced confidence |
| Parse failure | Use other responses | No impact if >=2 succeed |
| All tools fail | Return error with troubleshooting | Review fails |

## Graceful Degradation

| Models Succeeded | Confidence Level | Behavior |
|------------------|------------------|----------|
| 3/3 | High | Full review |
| 2/3 | Medium | Review continues with note |
| 1/3 | Low | Warning issued |
| 0/3 | Failed | Review fails with troubleshooting |

## Common Troubleshooting

**No tools available:**
1. Check `mcp__plugin_foundry_foundry-mcp__review action="list-tools"` output
2. Verify provider configuration
3. Ensure API keys are set

**Timeout issues:**
1. Check network connectivity
2. Reduce review scope (use `quick` type)
3. Retry with increased timeout

**Parse failures:**
1. Check provider response format
2. Verify spec JSON is valid
3. Review error logs for details
