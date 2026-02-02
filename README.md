# github-mentions

A [Clawdbot](https://github.com/trifle-labs/clawdbot) skill for monitoring and tracking GitHub mentions across your organizations.

## Features

- Uses GitHub Notifications API for reliable mention detection
- Tracks mention status: `pending` → `in_progress` → `completed`
- Configurable filtering (org-only, org-members-only)
- Org member caching with configurable refresh interval
- Prevents duplicate work by tracking what's already being addressed

## Installation

```bash
clawdhub install github-mentions
```

Or manually clone to your skills directory:
```bash
git clone https://github.com/trifle-labs/github-mentions.git ~/.openclaw/workspace/skills/github-mentions
```

## Prerequisites

- `gh` CLI installed and authenticated (`gh auth login`)
- `jq` for JSON processing

## Usage

```bash
# Check for new mentions
github-mentions check

# List all tracked mentions
github-mentions list

# List only pending mentions
github-mentions list pending

# Start working on a mention
github-mentions start owner/repo#123

# Mark a mention as done
github-mentions done owner/repo#123

# View mention details
github-mentions view owner/repo#123

# Show/set configuration
github-mentions config
github-mentions config orgMembersOnly false
```

## Configuration

Default configuration (`config.json`):
```json
{
  "orgOnly": true,
  "orgMembersOnly": true,
  "memberCacheHours": 1,
  "checkIntervalMinutes": 5
}
```

| Option | Default | Description |
|--------|---------|-------------|
| `orgOnly` | `true` | Only track mentions from repos within your orgs |
| `orgMembersOnly` | `true` | Only track mentions from org members |
| `memberCacheHours` | `1` | How often to refresh the org member list |
| `checkIntervalMinutes` | `5` | Reference for intended polling frequency |

## Workflow

1. Run `github-mentions check` periodically (e.g., every 5 minutes via heartbeat)
2. New mentions appear as `pending`
3. When you start addressing a mention: `github-mentions start owner/repo#123`
4. After responding/resolving: `github-mentions done owner/repo#123`

This prevents the bot from trying to address the same mention multiple times while it's being worked on.

## License

MIT
