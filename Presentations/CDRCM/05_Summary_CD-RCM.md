---
title: "CD-RCM: Generalizable Continuous-Depth Novel View Synthesis for Reflectance Confocal Microscopy"
math: true
---
<p class="meta">Imtiaz, Rajadhyaksha, Kose*, Dy* · Northeastern University &amp; Memorial Sloan Kettering Cancer Center (*equal contribution)</p>

<p class="note"><a href="CD-RCM%20Explainer.html">View the interactive presentation →</a> · <a href="{{ '/presentations/' | relative_url }}">All presentations</a></p>

## Clinical motivation

Reflectance confocal microscopy (RCM) provides non-invasive, cellular-resolution "optical biopsies" in vivo. It is used for tumor diagnosis, inflammatory disease, and surgical-margin assessment.

Stacks are acquired as en face sections at discrete depths. Lateral resolution (~0.5 µm) is about **6× finer** than axial optical sectioning (~3 µm). The resulting volumes are anisotropic and hard to read across depth, especially near the dermal–epidermal junction (DEJ). Clinicians must mentally reconstruct vertical structure.

Vertical sections built from raw stacks show staircase artifacts. Spline interpolation has no anatomical priors, and denser sampling costs acquisition time.

**Goal:** synthesize anatomically plausible slices at arbitrary depths to make the volume isotropic, with no per-patient optimization.

## Why a new formulation

Existing approaches don't fit RCM:

- **Surface novel view synthesis (NVS)** (NeRF, 3D Gaussian Splatting, generalizable pixelSplat/GS-LRM/LVSM) reconstructs opaque surfaces from geometric parallax.
- **Sparse-view CT** (radiative Gaussian splatting) models X-ray attenuation and needs per-case optimization.

RCM images *internal* tissue by axial sectioning, with almost no viewpoint change, and shallower sections occlude deeper ones. Each section also integrates signal from neighboring depths (finite axial point-spread function), which the model can exploit.

## Method

**Virtual camera.**

- Slice *i* is modeled as a pinhole camera under pure z-translation: $$c_i=[0,0,z_i]^T$$, $$z_i=i\Delta_z$$, with identity rotation. Camera-to-world is $$T_{c2w}=[I_3,\,-c_i;\,0^T,1]$$.
- **Stack canonicalization:** $$c_i\leftarrow(c_i-\bar c)/c_\infty$$.
- **Intrinsics:** $$f_x=f_y=\alpha H$$, $$u_0=v_0=H/2$$.
- Pixel-wise **Plücker ray** embeddings $$P_i\in\mathbb R^{H\times W\times 6}$$ encode depth.

**Architecture (LVSM-inspired, see figure).**

1. Input slices and their rays are split into 8×8 patches, concatenated channel-wise, and projected to d = 768 tokens.
2. Target depths enter as **ray-only** tokens.
3. The combined sequence passes through **24 bidirectional self-attention + MLP blocks** (QK-norm, no causal mask).
4. Only the updated target tokens are decoded, via Linear → sigmoid → unpatchify, into $$\hat I_t$$.

The model has 170.8M parameters and is trained from scratch.

**Skin-specific perceptual (SPF) extractor.**

- **Backbone:** DINOv3 ViT adapted to RCM mosaic crops with **LoRA (rank 16)** on all attention projections.
- **Training scheme:** DINO multi-crop (2 global 224², 8 local 96² crops), with an EMA teacher (m = 0.9995).
- **Distillation:** cross-entropy on the CLS token and on mean-pooled patch tokens.
- **Compute:** 4 epochs, 3 days on 1× A6000. The extractor is then frozen.

**Objective.**

$$\mathcal L=\mathcal L_{MSE}+\lambda\,\mathcal L_{LPIPS}+\gamma\,\mathcal L_{SPF},\qquad \mathcal L_{SPF}=\tfrac1T\|\phi_s(\hat I_t)-\phi_s(I_t)\|_1$$

Here λ = 0.5 and γ = 0.05. $$\mathcal L_{SPF}$$ uses final-layer features.

**Training and inference.**

- Training samples windows of M = 9 consecutive slices. The first, middle, and last are inputs (~6 µm apart), and 4 random intermediate slices are targets.
- Training is coarse-to-fine: 20k steps at 256² followed by 10k steps at 512².

| Setting | Value |
|---|---|
| Optimizer | AdamW, β = (0.9, 0.95), weight decay 0.05 |
| Learning rate | peak 4e-4, 2k warmup, cosine schedule |
| Stability | gradient clipping 1.0; skip steps with gradient norm > 5 |
| Efficiency | FlashAttention-2, bf16 |
| Hardware | 4× A6000, ~4 days |

At inference, any three evenly spaced slices yield slices at continuous, non-integer depths within the stack.

## Data

- **Stacks:** 216 in vivo stacks from a VivaScope 1500, imaging healthy arm and trunk skin of volunteers aged 20–50.
- **Slices:** 50–65 per stack, 1000² pixels, 1.5 µm steps.
- **Preprocessing:** SIFT affine registration (Fiji), then a 960² center crop resized to 512². The last 5 low-signal slices are dropped.
- **Split:** 156 / 60 at the stack level.

## Results

| Method | PSNR ↑ | SSIM ↑ | LPIPS ↓ | L_SPF ↓ |
|---|---|---|---|---|
| B-spline | 22.02 | 0.471 | 0.375 | 0.161 |
| Cubic spline | 22.09 | 0.469 | 0.378 | 0.163 |
| Gaussian (B-spline + σ=1 blur) | 22.87 | 0.522 | 0.477 | 0.164 |
| **CD-RCM** | **23.36** | **0.581** | **0.314** | **0.145** |

Gaussian smoothing raises PSNR but destroys high-frequency cellular detail. CD-RCM avoids this accuracy–sharpness trade-off.

**Ablation (256²).** SPF with final-layer features gives 23.35 / 0.597 / 0.288 / 0.172. That beats LPIPS + VGG perceptual loss (22.65 / 0.567 / 0.329 / 0.216) and multi-layer SPF (22.39 / 0.522 / 0.338 / 0.246).

**Speed.** Densifying a full stack 10× takes **0.8 s** on one A6000. The densified volumes give smooth sagittal, coronal, and arbitrary oblique cuts without staircase artifacts.

**Baselines note.** Fixed-depth frame predictors can't query arbitrary depths. A diffusion model was tried but lost high-frequency structure.

## Clinical takeaways and limitations

CD-RCM makes **histology-like virtual vertical and oblique sectioning of living skin** practical. In the future, sparser acquisition could mean faster exams and fewer motion artifacts. It does **not** increase optical resolution, and synthesized slices must be labeled as predictions to avoid misreading hallucinated structure.

Limitations:

- Healthy skin only, from a single device.
- Small dataset, with no subject-level split metadata.
- Residual misalignment in some stacks.
- No uncertainty estimates.
- No downstream clinical evaluation yet.

Next steps: DEJ and strata tasks, mosaics, lesions, and other optical-sectioning modalities such as line-field confocal OCT.

---

![CD-RCM architecture](fig_cdrcm_architecture.png)

*Figure (from the paper, Fig. 2).* Sparse input slices and their Plücker rays, together with ray-only target tokens, pass through a 24-block decoder-only transformer. Only the target tokens are decoded, and training uses the MSE + LPIPS + L_SPF loss.
