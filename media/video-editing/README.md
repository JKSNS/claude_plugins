# Video Editing

## Tools

| Tool | Port | Purpose |
|---|---|---|
| Remotion Studio | :3000 | Browser-based visual editor, frame-by-frame preview |
| Remotion Render API | :3001 | Headless render pipeline, programmatic access |
| FFmpeg | CLI | Fallback compositing, transcoding, format conversion |

## Remotion Studio

Browser-based editor. Preview compositions, scrub frame-by-frame,
edit props in real-time, render from the UI.

```bash
# Launch Studio
solveshow studio

# With props from a previous run
solveshow studio --props output/props.json

# Specific port
solveshow studio --port 3001
```

### What you can do in Studio

- Scrub timeline frame-by-frame
- Edit props (durations, text, theme, aspect ratio)
- Switch compositions
- Render directly from browser
- Hot reload (edit React components, Studio reloads)

## Remotion CLI

```bash
cd rendering/remotion

# Render with custom props
npx remotion render SolveShowVideo output.mp4 --props=props.json

# Render specific frames
npx remotion render SolveShowVideo output.mp4 --props=props.json --frames=0-30

# Bundle for deployment
npx remotion bundle
```

## Render Server

Headless rendering via HTTP API.

```bash
python main.py ai-render-server    # starts on :3001

curl http://localhost:3001/health
curl http://localhost:3001/compositions
```

## Preview workflow

Run pipeline through TTS but skip the slow render. Get props you can
edit visually before committing.

```bash
# Generate props + audio, then open Studio
solveshow preview --url "..." --studio

# Generate props only
solveshow preview --url "..."
solveshow studio --props output/props.json
```

## Visual components

| Type | Component | Use case |
|---|---|---|
| `title_card` | HookCard | Opening slide |
| `code_reveal` | CodeReveal + EditorChrome | Animated code typing |
| `code_walkthrough` | CodeWalkthrough | Split code + explanation |
| `code_magic_move` | MagicMoveReveal | Animated code morph |
| `complexity_chart` | ComplexityChart | Animated Big-O curves |
| `diagram` | DiagramBuilder | SVG flow diagrams |
| `terminal` | TerminalReplay | macOS terminal with ANSI colors |
| `memory_layout` | MemoryLayout | Stack/heap pointer visualization |
| `outro_card` | OutroCard | Closing slide with CTA |

## Aspect ratios

| Ratio | Resolution | Platform |
|---|---|---|
| `9:16` | 1080x1920 | YouTube Shorts, TikTok (default) |
| `1:1` | 1080x1080 | Instagram Reels |
| `16:9` | 1920x1080 | YouTube standard |

## Troubleshooting

```bash
# Shared memory error (Docker)
docker run --shm-size=2g ...

# Render hangs on Linux
npx remotion render --gl=swiftshader --enable-multi-process-on-linux

# TypeScript errors (WSL2)
cd rendering/remotion && NODE_OPTIONS=--jitless npx tsc --noEmit

# Port 3000 in use (Studio auto-detects free port)
solveshow studio
```
