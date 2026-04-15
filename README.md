# claude_plugins

## Plugins

### graphify

Knowledge graph for codebase navigation. Parses code via tree-sitter AST locally, extracts concepts from docs via Ollama, clusters with Leiden community detection, and produces a persistent queryable graph. Claude reads the graph report before every search instead of grepping raw files. Token savings range from 10x on small projects to 2000x on large ones. Integrated into [claude_preflight](https://github.com/JKSNS/claude_preflight) for automatic sync and persistence, but works standalone.

```bash
pip install graphifyy && graphify install
```

### autoresearch

Autonomous research loop that self-improves over time. Three files define the cycle: `program.md` sets the scope and constraints (human-edited, never touched by the agent), `methodology.md` holds the current approach and evolves every cycle, and `evaluate.py` provides an objective quality metric. The loop reads the program, executes a sprint, evaluates results, updates the methodology, commits, and repeats until interrupted. Every 3 sprints it reviews trends and promotes what works.

```bash
cp plugins/autoresearch/SKILL.md ~/.claude/skills/autoresearch/SKILL.md
```

### codex

OpenAI Codex integration for architecture, code review, and rescue passes. Codex is strong at project architecture and planning. Claude Code handles the building. Use Codex to validate Claude's work, get a second opinion on complex changes, or hand off a stuck task for a fresh perspective. Both models see the same files through a shared runtime.

```json
"enabledPlugins": { "codex@openai-codex": true }
```

### ralph-loop

Run a prompt or slash command on a recurring interval. Useful for polling deployment status, running periodic checks, or executing background research loops that report back on a schedule.

```
/install-plugin ralph-loop
```

### skill-builder

Create custom `/slash` commands from natural language descriptions. Describe what you want the command to do and it generates the SKILL.md definition.

```
/install-plugin skill-builder
```

---

## Media Generation

Model recommendations, VRAM requirements, and configuration for each tool in the content creation pipeline.

### Video Generation

Wan2GP wraps multiple models in a single Gradio UI. Model selection depends on available VRAM and quality requirements. Wan 2.1 handles most use cases at 12GB. LTX-2 is the fastest option for lower VRAM systems. Hunyuan produces the highest quality output but requires 16GB+ and runs slower.

| Model | VRAM | Speed | Quality | Best for |
|---|---|---|---|---|
| Wan 2.1 | 12GB+ | Medium | High | Cinematic, detailed scenes |
| Hunyuan | 16GB+ | Slow | Highest | Realistic motion, faces |
| LTX-2 | 8GB+ | Fast | Medium | Quick iterations, prototyping |
| Flux | 16GB+ | Medium | High | Photorealistic motion |

See [media/video-gen/](media/video-gen/) for full setup, VRAM guide, and talking head configuration.

### Image and Photo Generation

Flux runs locally for full control and privacy. Runway handles inpainting, outpainting, and style transfer via API. Stability AI works for fine-tuned models and batch processing.

| Tool | Type | Best for |
|---|---|---|
| Flux | Local | Fast iteration, full control, private |
| Runway | API | Inpainting, outpainting, style transfer |
| Stability AI | API/Local | Fine-tuned models, batch processing |

See [media/image-gen/](media/image-gen/) for API setup and usage.

### Video Editing

Remotion Studio provides a browser-based visual editor with frame-by-frame preview, live prop editing, and direct rendering. The Render API enables headless programmatic rendering. FFmpeg handles fallback compositing and format conversion.

| Tool | Port | Purpose |
|---|---|---|
| Remotion Studio | :3000 | Visual editor, preview, direct render |
| Remotion Render API | :3001 | Headless render pipeline |
| FFmpeg | CLI | Fallback compositing, transcoding |

See [media/video-editing/](media/video-editing/) for component reference and workflows.

### Audio

Chatterbox provides text-to-speech with voice cloning capability. Ace Step generates background music from text prompts. MMAudio produces scene-matched sound effects from video content analysis. Edge TTS is the free default for narration with 12 neural voices.

| Tool | Purpose |
|---|---|
| Chatterbox TTS | Text-to-speech with voice cloning |
| Voxtral | Mixtral-based voice, strong on technical content |
| Edge TTS | Free neural TTS, 12 voices, no API key |
| Ace Step | Music generation from prompts |
| MMAudio | Scene-matched sound effects |

See [media/audio/](media/audio/) for voice comparison, configuration, and speech rate settings.

---

## Building Your Own

### Slash Commands

Markdown prompt templates that Claude follows as instructions. No server, no process. Place a `SKILL.md` in `~/.claude/skills/<name>/` and type `/<name>` in Claude Code. See [skills/](skills/) for the full guide with `/preflight` as a worked example.

### MCP Servers

Running processes that expose typed tool functions to Claude Code. Claude calls them like structured API endpoints instead of writing bash. See [mcps/](mcps/) for setup with FastMCP and registration in settings.json.

---

## Port Map

| Service | Port | Notes |
|---|---|---|
| Ollama | 11434 | Host side, accessed via host.docker.internal |
| Remotion Studio | 3000 | Inside container |
| Remotion Render | 3001 | Inside container |
| ComfyUI | 8188 | Local image/video generation UI |
| Wan2GP | 7860 | Gradio UI for all video models |

---

## Structure

```
claude_plugins/
|-- plugins/
|   |-- autoresearch/    # autonomous research loop
|   |-- codex/           # Codex code review and rescue
|   |-- ralph-loop/      # recurring task loops
|   +-- skill-builder/   # custom slash commands
|-- skills/
|   +-- building-skills.md   # how to build slash commands
|-- mcps/
|   +-- building-servers.md  # how to build MCP servers
+-- media/
    |-- video-gen/       # Wan 2.1, Hunyuan, LTX-2, Flux
    |-- image-gen/       # Flux, Runway, Stability
    |-- video-editing/   # Remotion, FFmpeg
    +-- audio/           # Chatterbox, Voxtral, Edge TTS, Ace Step, MMAudio
```

---

## Related

- [claude_setup](https://github.com/JKSNS/claude_setup) - containerized Claude Code environment with safety hooks and base configuration
- [claude_preflight](https://github.com/JKSNS/claude_preflight) - per-project optimization with persistent knowledge graph sync and local model routing

## License

CC BY-NC 4.0. Free to use and fork. Credit required. No commercial use.
