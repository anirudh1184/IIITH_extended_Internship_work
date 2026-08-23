# AI/ML Engineering Internship Portfolio
 
*Generative Media Pipelines & Parameter-Efficient Language Model Adaptation*
 
---
 
## 📁 Repository Structure
 
```text
├── Comfyui/                # Local generative media workflows and assets
├── LoRA/                   # Pharma-LM Jupyter notebook and trained LoRA weights
└── README.md               # Complete technical project documentation
```
 
---
 
## 🎨 Part 1: Local Offline Generative Media Workflow (ComfyUI)
 
### Overview & Models Used
 
This phase of the project focused on setting up, optimizing, and executing advanced generative media pipelines entirely offline and locally on machine hardware. Two state-of-the-art open-source models were deployed via ComfyUI:
 
| Task | Model | Description |
|---|---|---|
| Image Generation | **Flux** (9B parameters) | High-fidelity text-to-image synthesis |
| Video Generation | **Wan 2.2** (5B parameters) | Dynamic image-to-video animation |
 
### Pipeline Architecture & Execution
 
1. **Text-to-Image (Flux 9B)** — The pipeline began by prompting the Flux 9B model to generate a rich, highly detailed landscape image featuring a city with a beautiful sky and tall buildings.
2. **Image-to-Video (Wan 2.2 5B)** — Rather than generating video from scratch, the exact output image from Flux was fed directly into Wan 2.2 using an Image-to-Video workflow to smoothly animate the urban landscape, entirely offline.
### Hardware Optimization & VRAM Engineering
 
Running a 9B image model alongside a 5B video model locally on consumer laptop hardware presented severe VRAM bottlenecks. Unoptimized execution caused memory-thrashing, system freezes, and pipeline crashes. The following engineering strategies were implemented to work around these limits:
 
- **Resolution management** — Input image resolutions were downscaled to `512x512` to reduce initial latent tensor memory footprints.
- **Temporal framing constraints** — Video generation length was strictly restricted to a safe window of `25–33 frames` to prevent exponential memory consumption during temporal attention calculations.
- **Tiled VAE decoding** — The pipeline frequently choked and stalled at the standard `VAE Decode` node because it attempted to uncompress all frames into full-resolution RGB pixels simultaneously. This was resolved by swapping to the `VAE Decode (Tiled)` node, configured with `tile_size = 256` and `temporal_size = 16`. This forced the model to process the video sequentially in small spatial-temporal chunks, flushing memory safely and preventing hardware crashes.
---
 
## 💊 Part 2: Domain Adaptation via LoRA (Pharma-LM)
 
### Project Specifications & Methodology
 
Moving from generative media to large language models, **Pharma-LM** was built to investigate parameter-efficient domain adaptation using Qwen2.5-7B.
 
| Parameter | Value |
|---|---|
| Adaptation framework | LoRA / QLoRA via Unsloth |
| Dataset | 5,000 curated records from `medalpaca/medical_meadow_medical_flashcards` |
| Hardware | Google Colab — Tesla T4 GPU (14.6 GB VRAM) |
| LoRA rank / alpha | r = 16, α = 16 (targeting attention + MLP projections) |
| Trainable parameters | ~40.3M (~0.53% of total) |
| Training outcome | 60-step SFT run completed in 7.36 minutes; loss stabilized around 0.45–0.55 |
 
### Qualitative Evaluation Highlights
 
- **Diagnostic reasoning** — Presented with an acute clinical case (tearing chest pain, elevated blood pressure), Pharma-LM correctly identified an aortic dissection and outlined appropriate surgical next steps.
- **Pharmacological mechanisms** — When probed on a statin–grapefruit interaction, the model accurately isolated furanocoumarins and explained their inhibitory effect on hepatic CYP3A4 enzymes.
---
 
## ⚠️ Scope & Responsible Use
 
Pharma-LM is an educational and research prototype demonstrating LoRA-based domain adaptation. It is **not** a certified medical device and must not be used for autonomous clinical diagnosis or patient care without expert medical oversight.
 
