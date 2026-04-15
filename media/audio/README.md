# Audio Tools

## Text-to-Speech

### Edge TTS

Free neural TTS from Microsoft. No API key required. 12 voices available with natural prosody and expression. Default for narration pipelines because it costs nothing and sounds good enough for production.

```bash
pip install edge-tts
```

Recommended voices:

| Voice | Character |
|---|---|
| `en-US-AvaNeural` | Expressive, caring (default) |
| `en-US-JennyNeural` | Friendly (default rotation) |
| `en-US-AriaNeural` | Positive, confident |
| `en-US-AndrewNeural` | Warm, confident |
| `en-US-BrianNeural` | Approachable, casual |

Compare all 12 voices side by side:

```bash
solveshow voice-samples --text "Your sample text here."
```

### Chatterbox TTS

Voice cloning capable TTS. Feed it a reference audio clip and it reproduces the voice characteristics. Runs locally through the Wan2GP audio pipeline.

```yaml
audio:
  tts_model: "chatterbox"
```

### Voxtral (Mixtral Voice)

Mixtral-based voice integration. Higher quality synthesis with better handling of technical content, code narration, and multilingual text.

## Music Generation

### Ace Step

Background music generated from text prompts. Describe the mood, genre, and instrumentation and it produces a track. Ace Step 1.5 Turbo XL provides higher quality output at the cost of longer generation time.

```yaml
audio:
  music_model: "ace_step"
  bg_music_volume: 0.15
```

## Sound Effects

### MMAudio

Scene-matched sound effects generated from video content analysis. Analyzes the visual content and produces appropriate ambient audio and effects.

## Configuration

```yaml
tts:
  provider: edge-tts
  voice: ""              # empty = alternating Ava + Jenny
  speed: 1.1             # 1.0 = normal, 1.1 = slightly faster

audio:
  tts_model: "chatterbox"
  music_model: "ace_step"
  bg_music_volume: 0.15
```

Speech rate baseline is 155 words per minute, multiplied by the speed setting.
