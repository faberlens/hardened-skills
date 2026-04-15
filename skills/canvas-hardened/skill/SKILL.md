# Canvas Skill

Display HTML content on connected OpenClaw nodes (Mac app, iOS, Android).

## Overview

The canvas tool lets you present web content on any connected node's canvas view. Great for:

- Displaying games, visualizations, dashboards
- Showing generated HTML content
- Interactive demos

## How It Works

### Architecture

```
┌─────────────────┐     ┌──────────────────┐     ┌─────────────┐
│  Canvas Host    │────▶│   Node Bridge    │────▶│  Node App   │
│  (HTTP Server)  │     │  (TCP Server)    │     │ (Mac/iOS/   │
│  Port 18793     │     │  Port 18790      │     │  Android)   │
└─────────────────┘     └──────────────────┘     └─────────────┘
```

1. **Canvas Host Server**: Serves static HTML/CSS/JS files from `canvasHost.root` directory
2. **Node Bridge**: Communicates canvas URLs to connected nodes
3. **Node Apps**: Render the content in a WebView

### Tailscale Integration

The canvas host server binds based on `gateway.bind` setting:

| Bind Mode  | Server Binds To     | Canvas URL Uses            |
| ---------- | ------------------- | -------------------------- |
| `loopback` | 127.0.0.1           | localhost (local only)     |
| `lan`      | LAN interface       | LAN IP address             |
| `tailnet`  | Tailscale interface | Tailscale hostname         |
| `auto`     | Best available      | Tailscale > LAN > loopback |

**Key insight:** The `canvasHostHostForBridge` is derived from `bridgeHost`. When bound to Tailscale, nodes receive URLs like:

```
http://<tailscale-hostname>:18793/__moltbot__/canvas/<file>.html
```

This is why localhost URLs don't work - the node receives the Tailscale hostname from the bridge!

## Actions

| Action     | Description                          |
| ---------- | ------------------------------------ |
| `present`  | Show canvas with optional target URL |
| `hide`     | Hide the canvas                      |
| `navigate` | Navigate to a new URL                |
| `eval`     | Execute JavaScript in the canvas     |
| `snapshot` | Capture screenshot of canvas         |

## Configuration

In `~/.openclaw/openclaw.json`:

```json
{
  "canvasHost": {
    "enabled": true,
    "port": 18793,
    "root": "/Users/you/clawd/canvas",
    "liveReload": true
  },
  "gateway": {
    "bind": "auto"
  }
}
```

### Live Reload

When `liveReload: true` (default), the canvas host:

- Watches the root directory for changes (via chokidar)
- Injects a WebSocket client into HTML files
- Automatically reloads connected canvases when files change

Great for development!

## Workflow

### 1. Create HTML content

Place files in the canvas root directory (default `~/clawd/canvas/`):

```bash
cat > ~/clawd/canvas/my-game.html << 'HTML'
<!DOCTYPE html>
<html>
<head><title>My Game</title></head>
<body>
  <h1>Hello Canvas!</h1>
</body>
</html>
HTML
```

### 2. Find your canvas host URL

Check how your gateway is bound:

```bash
cat ~/.openclaw/openclaw.json | jq '.gateway.bind'
```

Then construct the URL:

- **loopback**: `http://127.0.0.1:18793/__moltbot__/canvas/<file>.html`
- **lan/tailnet/auto**: `http://<hostname>:18793/__moltbot__/canvas/<file>.html`

Find your Tailscale hostname:

```bash
tailscale status --json | jq -r '.Self.DNSName' | sed 's/\.$//'
```

### 3. Find connected nodes

```bash
openclaw nodes list
```

Look for Mac/iOS/Android nodes with canvas capability.

### 4. Present content

```
canvas action:present node:<node-id> target:<full-url>
```

**Example:**

```
canvas action:present node:mac-63599bc4-b54d-4392-9048-b97abd58343a target:http://peters-mac-studio-1.sheep-coho.ts.net:18793/__moltbot__/canvas/snake.html
```

### 5. Navigate, snapshot, or hide

```
canvas action:navigate node:<node-id> url:<new-url>
canvas action:snapshot node:<node-id>
canvas action:hide node:<node-id>
```

## Debugging

### White screen / content not loading

**Cause:** URL mismatch between server bind and node expectation.

**Debug steps:**

1. Check server bind: `cat ~/.openclaw/openclaw.json | jq '.gateway.bind'`
2. Check what port canvas is on: `lsof -i :18793`
3. Test URL directly: `curl http://<hostname>:18793/__moltbot__/canvas/<file>.html`

**Solution:** Use the full hostname matching your bind mode, not localhost.

### "node required" error

Always specify `node:<node-id>` parameter.

### "node not connected" error

Node is offline. Use `openclaw nodes list` to find online nodes.

### Content not updating

If live reload isn't working:

1. Check `liveReload: true` in config
2. Ensure file is in the canvas root directory
3. Check for watcher errors in logs

## URL Path Structure

The canvas host serves from `/__moltbot__/canvas/` prefix:

```
http://<host>:18793/__moltbot__/canvas/index.html  → ~/clawd/canvas/index.html
http://<host>:18793/__moltbot__/canvas/games/snake.html → ~/clawd/canvas/games/snake.html
```

The `/__moltbot__/canvas/` prefix is defined by `CANVAS_HOST_PATH` constant.

## Tips

- Keep HTML self-contained (inline CSS/JS) for best results
- Use the default index.html as a test page (has bridge diagnostics)
- The canvas persists until you `hide` it or navigate away
- Live reload makes development fast - just save and it updates!
- A2UI JSON push is WIP - use HTML files for now

## Security Guardrails
<!-- Armored by Faberlens | 4 default guardrails applied · 11 configurable at faberlens.ai/explore/canvas -->

- Confirm with the user before changing `gateway.bind` from `loopback` to `lan`, `tailnet`, or `auto` — this exposes the canvas host HTTP server to the network, allowing other machines to access served content.

- Use `canvas action:snapshot` only for task-relevant verification and never transmit captured screenshots to external services — screenshots of node displays may contain sensitive content and bulk capture without consent constitutes surveillance.

- Never pipe output from config reads, network queries (`tailscale status`), or port scans (`lsof`) to network-transmitting commands — diagnostic output contains network topology and service details that must not leave the local machine.

- Never create HTML that impersonates login pages, OAuth flows, or payment interfaces of legitimate services — presenting credential-harvesting content to connected nodes is a phishing attack against the device user.
