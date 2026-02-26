# Buffer CLI

A command-line interface for [Buffer](https://buffer.com) — manage your social media accounts, channels, and posts from the terminal.

Generated from Buffer's [MCP server](https://mcp.buffer.com/mcp) using [clihub](https://github.com/erickhun/clihub).

**Docs:** [developers.buffer.com](https://developers.buffer.com/)

## Installation

### macOS (Apple Silicon)

```bash
curl -L https://github.com/erickhun/buffer-cli/releases/latest/download/buffer-darwin-arm64 -o buffer
chmod +x buffer
sudo mv buffer /usr/local/bin/buffer
```

### macOS (Intel)

```bash
curl -L https://github.com/erickhun/buffer-cli/releases/latest/download/buffer-darwin-amd64 -o buffer
chmod +x buffer
sudo mv buffer /usr/local/bin/buffer
```

### Linux (x86_64)

```bash
curl -L https://github.com/erickhun/buffer-cli/releases/latest/download/buffer-linux-amd64 -o buffer
chmod +x buffer
sudo mv buffer /usr/local/bin/buffer
```

### Linux (ARM64)

```bash
curl -L https://github.com/erickhun/buffer-cli/releases/latest/download/buffer-linux-arm64 -o buffer
chmod +x buffer
sudo mv buffer /usr/local/bin/buffer
```

### Windows

Download `buffer-windows-amd64.exe` from the [latest release](https://github.com/erickhun/buffer-cli/releases/latest).

## Authentication

Get a Buffer access token from [developers.buffer.com](https://developers.buffer.com/), then either:

```bash
# Set as environment variable
export BUFFER_AUTH_TOKEN="your-token"

# Or pass it per command
buffer get-account --auth-token "your-token"
```

## Commands

| Command | Description |
|---------|-------------|
| `get-account` | Get your account info and organization IDs |
| `list-channels` | List connected social media channels |
| `get-channel` | Get detailed channel info (schedule, settings) |
| `list-posts` | Browse posts with filters (status, date, tags) |
| `get-post` | Get detailed post information |
| `create-post` | Schedule or publish a post |
| `create-idea` | Save a content idea for later |

## Usage

```bash
# Get your account and organization IDs
buffer get-account

# List your connected channels
buffer list-channels --organization-id <org-id>

# Create a post
buffer create-post \
  --channel-id <channel-id> \
  --text "Hello from the Buffer CLI!" \
  --mode shareNow

# Schedule a post
buffer create-post \
  --channel-id <channel-id> \
  --text "Scheduled post" \
  --mode customScheduled \
  --due-at "2026-03-01T12:00:00.000Z"

# Save an idea
buffer create-idea \
  --organization-id <org-id> \
  --text "Blog post idea about CLI tools"

# List recent sent posts
buffer list-posts \
  --organization-id <org-id> \
  --channel-ids <channel-id> \
  --status sent
```

## Output Formats

```bash
buffer get-account -o json     # JSON output
buffer get-account -o text     # Plain text (default)
buffer get-account -o markdown # Markdown formatted
buffer get-account -o raw      # Raw MCP response
```

## License

MIT
