# Configuration

## Default Values

| Parameter | Value | Description |
|-----------|-------|-------------|
| `min_models` | 1 | Minimum models for valid review |
| `recommended_models` | 2 | Recommended for quality feedback |
| `timeout_per_model` | 120s | Per-model timeout |
| `max_review_time` | 300s | Total review timeout |
| `summary_max_lines` | 50 | Max lines in conversation summary |

## Output Isolation Settings

| Setting | Value | Description |
|---------|-------|-------------|
| `write_to_files` | true | Always write to files |
| `suppress_verbose_output` | true | Don't echo full review |
| `return_format` | summary | Return summary to conversation |
| `ensure_directory` | true | Create `.reviews/` if missing |
| `overwrite_existing` | true | Replace previous reviews |
