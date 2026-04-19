# Local 3D-Generator (Trellis 2) Implementation

The objective is to establish an offline, local deployment of Microsoft's Trellis 2 (a 4-billion parameter image-to-3D generation model) running securely within the ComfyUI ecosystem. Given hardware constraints (6GB VRAM GPUs), the baseline model is impossible to load normally. This plan establishes the architecture needed to circumvent these limitations using aggressive GGUF quantization and framework-level memory management.

## User Review Required
> [!IMPORTANT]
> The reliance on system page files for VRAM offloading can result in slow initial model load times. Users must confirm they have adequate fast SSD storage capacity to support a 32GB page file.

## Proposed Changes

### Core Infrastructure
- **Framework**: ComfyUI (orchestrated via ComfyUI-Easy-Install)
- **Model Standard**: GGUF (Specifically Q4 quantization level to cap VRAM at ~6.1GB)
- **Hardware Profile**: 6GB NVIDIA GPU, Windows OS.

### Node & Dependency Path
The visual node-graph backend will be modified to support the quantized inputs:
- Integration of `ComfyUI-Trellis2` custom nodes.
- Deprecation of standard safetensors loaders in favor of the `ComfyUI-GGUF` custom node to interface directly with the Q4 Trellis variants.
- Strict enforcement of Torch 2.8.0+cu128 to ensure maximum compatibility with the attention mechanisms used in Trellis.

## Open Questions

> [!WARNING]
> While Q4 quantization mathematically fits into 6GB, spikes during the actual meshing process might trigger OOM (Out Of Memory). Have you ensured that the output mesh/texture resolution is capped at 512?

## Verification Plan

### Automated Tests
- Validate PyTorch environment recognizing the 6GB GPU and custom Torch hooks.

### Manual Verification
- Deploy the ComfyUI startup script with low-VRAM arguments (`--lowvram --force-fp16 --disable-pinned-memory`).
- Load a baseline image into the `ComfyUI-Trellis2` nodes.
- Monitor active VRAM consumption during workflow execution to ensure it remains below the 6.0GB threshold.
- Verify final generated 3D output mesh opens successfully in standard 3D viewers.
