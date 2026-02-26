# Buffer CLI

A (non-official) command-line interface for [Buffer](https://buffer.com) via [Buffer API]([developers.buffer.com](developers.buffer.com/)) — manage your social media accounts, channels, and posts from the terminal.

 **Claude Code users:** this repo includes a [skill](/.claude/skills/skill.md) — type `/buffer` for an interactive posting UI using this CLI.

![morphing ui-large](https://github.com/user-attachments/assets/c20ebfd6-79f9-4663-b087-79041fec9b08)



## Quick Start

```bash
# Install (macOS Apple Silicon — see below for other platforms)
URL=https://github.com/erickhun/buffer-cli/releases/latest/download
curl -sL $URL/buffer-darwin-arm64 -o /usr/local/bin/buffer
chmod +x /usr/local/bin/buffer

# Authenticate
export BUFFER_AUTH_TOKEN="your-token"

# Go
buffer get-account
```

Example output:

```json
{
  "name": "Jane",
  "email": "jane@example.com",
  "timezone": "America/New_York",
  "organizations": [
    {
      "id": "org_abc123",
      "name": "My Company",
      "limits": { "channels": 20, "scheduledPosts": 5000 }
    }
  ]
}
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

# Publish a post now
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

### Output Formats

```bash
buffer get-account -o json     # JSON (default)
buffer get-account -o text     # Plain text
buffer get-account -o markdown # Markdown
buffer get-account -o raw      # Raw MCP response
```

## Installation

### macOS

```bash
# Apple Silicon
curl -sL https://github.com/erickhun/buffer-cli/releases/latest/download/buffer-darwin-arm64 -o /usr/local/bin/buffer && chmod +x /usr/local/bin/buffer

# Intel
curl -sL https://github.com/erickhun/buffer-cli/releases/latest/download/buffer-darwin-amd64 -o /usr/local/bin/buffer && chmod +x /usr/local/bin/buffer
```

### Linux

```bash
# x86_64
curl -sL https://github.com/erickhun/buffer-cli/releases/latest/download/buffer-linux-amd64 -o /usr/local/bin/buffer && chmod +x /usr/local/bin/buffer

# ARM64
curl -sL https://github.com/erickhun/buffer-cli/releases/latest/download/buffer-linux-arm64 -o /usr/local/bin/buffer && chmod +x /usr/local/bin/buffer
```

### Windows

Download `buffer-windows-amd64.exe` from the [latest release](https://github.com/erickhun/buffer-cli/releases/latest).

## Authentication

Get a Buffer access token from [developers.buffer.com](https://developers.buffer.com/), then:

```bash
export BUFFER_AUTH_TOKEN="your-token"
```

Or pass it per command with `--auth-token "your-token"`.

## How This CLI Is Built

This binary is generated from Buffer's public [MCP server](https://mcp.buffer.com/mcp) by [Diego Castillo](https://x.com/diegocasmo) using [clihub](https://github.com/erickhun/clihub), an open-source tool that turns any MCP server into a compiled CLI.

No hand-written application code — clihub connects to the MCP server, discovers available tools, and generates a self-contained Go binary with one subcommand per tool.

**Build it yourself:**

```bash
# Clone clihub
git clone https://github.com/erickhun/clihub.git && cd clihub

# Generate the buffer CLI
go run . generate \
  --url https://mcp.buffer.com/mcp \
  --name buffer \
  --exclude-tools introspect_schema,execute_query,execute_mutation
```

This produces the same binary — you can audit the [clihub source](https://github.com/erickhun/clihub) and verify the build yourself.

## License

MIT
