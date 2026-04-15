# Video Generation

AI video generation from text prompts. Model selection depends on VRAM,
speed requirements, and output quality.

## Models

| Model | VRAM | Speed | Quality | Best for |
|---|---|---|---|---|
| Wan 2.1 | 12GB | Medium | High | General-purpose, cinematic shots |
| Wan 2.1 (int8/GGUF) | 8GB | Medium | Good | Lower VRAM systems |
| Hunyuan | 16GB+ | Slow | Highest | Realistic human motion, faces |
| LTX-2 | 8GB+ | Fast | Medium | Quick iterations, talking heads (with Id Lora) |
| LTX-2 Distilled | 6GB | Fastest | Lower | Prototyping, minimum VRAM |
| Flux (video) | 16GB+ | Medium | High | Photorealistic motion, stills + short clips |

## VRAM Guide

| VRAM | Max Resolution | Recommended |
|---|---|---|
| 6 GB | 480p | LTX-2 Distilled |
| 8 GB | 720p | Wan 2.1 (GGUF/int8) |
| 12 GB | 720p | Wan 2.1 |
| 16 GB | 1080p | Wan 2.1 or Hunyuan |
| 24 GB+ | 1080p+ | Any model, full quality |

## Wan2GP Setup

Wan2GP wraps all models in a single Gradio UI at port 7860.

```bash
git clone --depth 1 https://github.com/deepbeepmeep/Wan2GP.git ~/Wan2GP
cd ~/Wan2GP
conda create -n wan2gp python=3.11.14
conda activate wan2gp
pip install torch==2.10.0 torchvision torchaudio --index-url https://download.pytorch.org/whl/cu130
pip install -r requirements.txt
python wgp.py
```

Serves at `http://localhost:7860`.

## Configuration

```yaml
wan2gp:
  default_model: wan_t2v    # wan_t2v | ltx2 | hunyuan | flux
  video:
    width: 1280
    height: 720
    num_frames: 81          # ~3.2s at 25fps
    steps: 20               # more = better quality, slower
    guidance_scale: 5.0
    seed: 42
    generation_timeout: 300
```

## Reduce VRAM usage

- Lower resolution: `width: 640, height: 480`
- Use quantized model: `default_model: wan2.1-int8`
- Reduce frames: `num_frames: 41` (~1.6s clips)
- Use LTX-2 Distilled (fastest, lowest VRAM)

## Talking head setup

Requires LTX-2 with Id Lora and a presenter face image.

```yaml
talking_head_enabled: true
talking_head_frames: 72        # ~3s at 24fps
talking_head_steps: 10         # LTX-2 needs fewer steps
presenter_image: "/path/to/face.jpg"
```

Syncs to TTS audio and overlays as picture-in-picture (bottom-right,
25% of frame width).

## Troubleshooting

```bash
# Check Wan2GP is running
curl http://127.0.0.1:7860/info

# Check CUDA
python3 -c "import torch; print(torch.cuda.is_available())"

# Out of VRAM: lower resolution or switch to LTX-2 Distilled
```
