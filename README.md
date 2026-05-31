# AbletonMCP — Remote Script for Ableton Live 10

MCP (Model Context Protocol) integration for Ableton Live 10 on macOS.  
Lets Claude Code and other MCP clients control Ableton directly: create tracks, add MIDI notes, load instruments, set tempo, fire clips, and more.

## Why this fork?

The original [ableton-mcp](https://github.com/ahujasid/ableton-mcp) targets Live 11/12 (Python 3). This version is rewritten to work on **Live 10 (Python 2.7)**, solving a fundamental GIL threading issue that prevents background threads from running reliably inside Ableton's embedded Python runtime.

**Key change:** all socket I/O and Live API calls are handled in `update_display()` — a hook called by Live every ~100ms on its main thread. No background threads at all.

## Installation

### 1. Install the Remote Script

Copy the `AbletonMCP` folder to Ableton's User Remote Scripts directory:

```
~/Music/Ableton/User Library/Remote Scripts/AbletonMCP/
```

Then in Ableton → Preferences → MIDI:  
Set a **Control Surface** slot to `AbletonMCP`, Input/Output to `None`.

### 2. Install the MCP Server

Requires [uv](https://astral.sh/uv):

```bash
brew install uv
```

Add to Claude Code (`~/.claude.json`) or Claude Desktop config:

```json
{
  "mcpServers": {
    "AbletonMCP": {
      "command": "uvx",
      "args": ["ableton-mcp"]
    }
  }
}
```

### 3. Connect

In Claude Code, run `/mcp` to connect. Once connected, you can ask Claude to create tracks, add beats, load instruments, and more.

## Supported commands

| Command | Description |
|---|---|
| `get_session_info` | Tempo, time signature, track count |
| `get_track_info` | Track details, clips, devices |
| `create_midi_track` | Add a new MIDI track |
| `set_track_name` | Rename a track |
| `create_clip` | Create a MIDI clip |
| `add_notes_to_clip` | Add MIDI notes |
| `set_clip_name` | Rename a clip |
| `set_tempo` | Change BPM |
| `fire_clip` / `stop_clip` | Transport control |
| `start_playback` / `stop_playback` | Session transport |
| `load_browser_item` | Load instrument/effect by URI |
| `get_browser_tree` | Browse Ableton's library |
| `get_browser_items_at_path` | List items at browser path |

## Requirements

- Ableton Live 10 (macOS)
- MCP client (Claude Code, Claude Desktop, etc.)
- [uvx / uv](https://astral.sh/uv)

## Credits

Based on [ahujasid/ableton-mcp](https://github.com/ahujasid/ableton-mcp). Rewritten for Live 10 compatibility.
