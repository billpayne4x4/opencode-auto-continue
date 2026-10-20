# opencode-auto-continue

OpenCode 2 plugin that sends `continue` when a session goes idle, so work resumes without a manual prompt.

## How It Works

1. Listens for `session.idle`
2. Waits `delayMs` so a queued user prompt can start first
3. Sends `continue` to the session
4. Repeats when the session goes idle again

The plugin stays out of the way when:

- the user interrupted the session — it resumes only after the user starts a new turn
- a permission request is waiting for an answer
- the session is a child session created by a subagent
- the consecutive limit or throttle is reached

## Installation

Add the plugin to the `plugins` array in `opencode.jsonc`:

```jsonc
{
  "plugins": [
    "opencode-auto-continue@https://github.com/developing-today/opencode-auto-continue/archive/refs/tags/latest.tar.gz",
    // ... other plugins
  ]
}
```

Restart OpenCode. The plugin is installed and loaded automatically.

## Commands

`/auto-continue` and `/ac` are equivalent.

| Command | Description |
|---------|-------------|
| `/ac` | Show current settings and state |
| `/ac on` | Enable auto-continue for the current session |
| `/ac off` | Disable auto-continue for the current session |
| `/ac reset` | Clear the consecutive counter and allow continuing again |
| `/ac status` | Same as `/ac` |

Session settings last until the session ends. Use the config file to change the defaults for every session.

## Configuration

Create `opencode-auto-continue.jsonc` in your project's `.opencode/` directory. Every field is optional.

```jsonc
{
  // Send continue this long after the session goes idle.
  "delayMs": 1000,

  // Minimum time between two auto-continues in the same session.
  "throttleMs": 5000,

  // Stop after this many continues without a user turn. 0 means no limit.
  "maxConsecutive": 0,

  // Set false to disable the plugin without removing it.
  "enabled": true
}
```

| Setting | Type | Default | Description |
|---------|------|---------|-------------|
| `delayMs` | number | `1000` | Delay after `session.idle` before sending `continue` |
| `throttleMs` | number | `5000` | Minimum time between auto-continues in one session |
| `maxConsecutive` | number | `0` | Consecutive continue limit, `0` for unlimited |
| `enabled` | boolean | `true` | Default state for new sessions |

## Stopping It

Press Esc to interrupt the current turn. The plugin will not restart the session until you send a message yourself. Run `/ac off` to disable it for the rest of the session.

## Building from Source

```bash
git clone https://github.com/developing-today/opencode-auto-continue.git
cd opencode-auto-continue
npm install
npm run build
```

## License

MIT
