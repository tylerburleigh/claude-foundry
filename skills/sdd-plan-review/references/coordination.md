# Model Coordination

## Parallel Execution

- All available models consulted simultaneously
- ThreadPoolExecutor manages concurrent requests
- Each model gets same prompt with spec content
- Independent failures don't block others

## Execution Timeline

```
T+0s:   Dispatch reviews to all available models
T+60s:  Fast models typically complete
T+120s: Timeout for slow models
T+130s: Synthesize available responses
T+150s: Generate report and summary
```

## Resource Management

- Timeout per model: 120 seconds
- Max review time: 300 seconds (5 minutes)
- Min models required: 1
- Recommended models: 2
