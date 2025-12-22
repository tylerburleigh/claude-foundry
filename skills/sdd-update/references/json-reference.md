# JSON Structure Reference

Reference for the JSON structures used in specs.

## Task Status Values

- `pending` - Not yet started
- `in_progress` - Currently being worked on
- `completed` - Successfully finished
- `blocked` - Cannot proceed due to dependencies or issues

## Journal Entry Structure

Journal entries are stored in a top-level `journal` array:

```json
{
  "journal": [
    {
      "timestamp": "2025-10-18T14:30:00Z",
      "entry_type": "decision",
      "title": "Brief title",
      "task_id": "task-1-2",
      "author": "claude-sonnet-4.5",
      "content": "Detailed explanation",
      "metadata": {}
    }
  ]
}
```

## Verification Result Structure

Stored in verification task metadata:

```json
{
  "verify-1-1": {
    "metadata": {
      "verification_result": {
        "date": "2025-10-18T16:45:00Z",
        "status": "PASSED",
        "output": "Command output",
        "notes": "Additional context"
      }
    }
  }
}
```

## Folder Structure

```
specs/
├── pending/      # Backlog - planned but not activated
├── active/       # Currently being implemented
├── completed/    # Finished and verified
└── archived/     # Old or superseded
```
