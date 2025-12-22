# Manual Fix Patterns

## Fixing Circular Dependencies

1. Identify the cycle from validation output
2. Understand the intent of each dependency
3. Remove the dependency that represents "nice to have" vs "must have"
4. Re-validate to confirm cycle is broken

**Example:**
```
Cycle: task-3-2 -> task-3-5 -> task-3-2

Analysis:
- task-3-2 "Create API client" depends on task-3-5 "Define interfaces"
- task-3-5 "Define interfaces" depends on task-3-2 "Create API client"

Resolution: Interfaces should be defined first
Remove: task-3-5's dependency on task-3-2
```

## Fixing Orphaned Dependencies

1. Check if the referenced task ID is a typo
2. If typo: correct the task ID
3. If intentional removal: delete the dependency reference
