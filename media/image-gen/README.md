# Image and Photo Generation

## Providers

Flux runs locally for full control and privacy. Runway handles inpainting, outpainting, and style transfer through their API. Stability AI works well for fine-tuned models and batch processing. Wan2GP's Z-Image turbo mode generates stylized thumbnails through the same Gradio UI used for video generation.

| Tool | Type | Best for |
|---|---|---|
| Flux | Local | Fast iteration, full control, private |
| Runway | API | Inpainting, outpainting, style transfer |
| Stability AI | API/Local | Fine-tuned models, batch processing |
| Wan2GP (image mode) | Local | AI thumbnails from prompts |

## API Keys

Set in `.env`:

```
FLUX_API_KEY=...
RUNWAY_API_KEY=...
STABILITY_API_KEY=...
```

## Usage

```bash
# Flux API
python main.py generate-assets --provider flux --images 8

# Runway (video loops from images)
python main.py generate-assets --provider runway --videos 3

# Stability AI
python main.py generate-assets --provider stability

# All providers, all genres
python main.py generate-assets --all --provider flux
```

## Thumbnail Rendering (PIL)

Standalone thumbnail generation without AI models. Uses Pillow to composite title, difficulty badge, code snippet, and platform logo.

```python
from solveshow.rendering.thumbnail import ThumbnailRenderer
from solveshow.pipeline.config import Config

renderer = ThumbnailRenderer(Config())
img = renderer.render(
    title="Two Sum",
    difficulty="easy",
    code_snippet="def twoSum(nums, target):\n    seen = {}",
    platform="leetcode",
)
img.save("thumbnail.png")
```
