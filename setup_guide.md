# Trellis 2 Low-VRAM Setup Guide

This document outlines the exact "Setup Plan" to move from a fresh Windows installation to a working 3D generator environment.

## Phase 1: OS & Hardware Optimization
1.  **Page File Expansion**:
    *   Navigate to: System Properties → Advanced → Performance → Settings → Advanced → Virtual Memory → Change.
    *   Uncheck "Automatically manage paging file size".
    *   Select your fastest SSD.
    *   Set Initial/Maximum size to **32768** MB (32GB).
2.  **GPU Driver Check**:
    *   Update to the latest NVIDIA Studio Drivers (Game Ready drivers are also acceptable).
3.  **Hardware Acceleration**:
    *   Ensure "Hardware-accelerated GPU scheduling" is **ON** in Windows Graphics Settings.

## Phase 2: Environment Provisioning (The "Easy" Way)
We utilize `ComfyUI-Easy-Install` to skip manual Python/CUDA conflicts.

1.  **Clone/Download**: `https://github.com/Tavris1/ComfyUI-Easy-Install`
2.  **Initialization**: Run the `.bat` file. This installs a portable Python environment.
3.  **Dependency Alignment**: 
    *   Inside the ComfyUI UI, go to **Add-ons**.
    *   Install **Torch-Pack** and select **Torch 2.8.0+cu128**. *Warning: Earlier versions may cause OOM errors during the tensor tiling phase.*

## Phase 3: Model Logistics
1.  **Download GGUF Weights**:
    *   Download `trellis_v2_q4.gguf` from HuggingFace (city96/TRELLIS-GGUF).
    *   Place in: `ComfyUI/models/unet/` or `ComfyUI/models/checkpoints/` depending on your specific loader node.
2.  **Custom Nodes**:
    *   Install **ComfyUI-Manager**.
    *   Use Manager to install **ComfyUI-GGUF** and **ComfyUI-Trellis2**.

## Phase 4: Execution Strategy (Setup Plan)
The following steps ensure the first run doesn't crash:
- **Rule 1**: No background apps (Chrome/Discord).
- **Rule 2**: Use the starter script `python main.py --lowvram --force-fp16 --disable-pinned-memory`.
- **Rule 3**: Set the "SISO" (Single Input, Single Output) resolution to 512.

## Troubleshooting Logic
| Symptom | Action |
| :--- | :--- |
| **CUDA Out of Memory** | Add `--disable-pinned-memory`. Check if model is truly Q4. |
| **Slow Meshing** | Expected on 6GB. CPU will take over for marching cubes. |
| **Black Output** | Verify Torch version. Use cu128 specifically. |
| **Dependencies failing** | Re-run `Long-Paths-Enabler` from Add-ons. |
