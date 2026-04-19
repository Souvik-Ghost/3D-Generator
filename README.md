# 3D-Generator (Trellis 2 Local Deployment)

This project contains the configuration and setup instructions for running Microsoft's **Trellis 2** (4B parameter image-to-3D generation model) locally. It is specifically optimized to run on **6GB VRAM GPUs** using GGUF quantization within the ComfyUI ecosystem.

## Dependencies and Requirements

- **GPU:** NVIDIA GPU with at least 6GB VRAM (Q4 Quantization recommended)
- **OS:** Windows (assumes page file is increased to 32GB+ on SSD)
- **Framework:** ComfyUI

## Setup Instructions

Please follow this exact installation order to successfully set up the environment using ComfyUI-Easy-Install:

1. **Install Base Framework:** Downlod and extract `ComfyUI-Easy-Install`, then run the `.bat` file to start the installation.
2. **Enable Long Paths:** Go to Add-ons → **Long-Paths-Enabler**
3. **Install PyTorch:** Go to Add-ons → Torch-Pack → select **Torch 2.8.0+cu128**
4. **Install Trellis 2 Nodes:** Go to Add-ons → **Trellis2**
5. **Install GGUF Loader:** Open ComfyUI Manager → search for and install **ComfyUI-GGUF** (by city96)
6. **Quantize Model:** Go to Add-ons → Easy-model2GGUF → **Convert to Q4** quantization.

## Running the Project

To avoid Out-Of-Memory (OOM) errors and optimize for low VRAM, launch ComfyUI with the following command-line flags:

```bash
python main.py --lowvram --force-fp16 --disable-pinned-memory
```

### OOM Recovery Checklist

If you encounter memory issues during generation, verify the following:
1. Close all unnecessary applications (browsers, Discord, game launchers).
2. Ensure you are using the **Q4** quantization model.
3. Keep output resolution to **512x512** (do not use 1024x1024 on 6GB VRAM).
4. Reduce the token count in the generation prompt.
5. Set texture resolution to **512**.
6. Ensure the `--disable-pinned-memory` flag is present in your startup script.
7. Increase your Windows page file to at least 32GB on an SSD.

## Quantization VRAM Reference

| Level | VRAM | Quality | Recommended For |
|-------|------|---------|-----------------|
| Q4 | ~6.1GB | Minimal loss | **6GB cards ⭐** |
| Q5 | ~6.5GB | Very minor | 8GB cards |
| Q6 | ~7GB | Negligible | 8GB cards |
| Q8 | ~8GB | Near-lossless | 10GB+ cards |
| Full | ~10-11GB| Baseline | 12GB+ cards |

## Key References
- Easy-Install: [github.com/Tavris1/ComfyUI-Easy-Install](https://github.com/Tavris1/ComfyUI-Easy-Install)
- Trellis Nodes: [github.com/visualbruno/ComfyUI-Trellis2](https://github.com/visualbruno/ComfyUI-Trellis2)
- Alt Wrapper: [github.com/PozzettiAndrea/ComfyUI-TRELLIS2](https://github.com/PozzettiAndrea/ComfyUI-TRELLIS2)
- GGUF Loader: [github.com/city96/ComfyUI-GGUF](https://github.com/city96/ComfyUI-GGUF)
- Official Model: [github.com/microsoft/TRELLIS.2](https://github.com/microsoft/TRELLIS.2)
