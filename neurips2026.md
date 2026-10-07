---
title: Three papers at NeurIPS 2026
description: Three papers from our group at NeurIPS 2026 — CD-RCM, Semantically Coherent Calibration, and grounding MLLMs with quantitative skin attributes.
permalink: /neurips2026/
---
This year our group has three papers at NeurIPS 2026 — one in the main conference and two at workshops — and together they tell a fairly coherent story about what it takes to make AI useful in dermatology. **CD-RCM** (main conference) tackles the imaging side: reflectance confocal microscopy gives clinicians cellular-resolution views of living skin, but only as sparse, anisotropic depth stacks, and we show how a feedforward transformer can fill in the missing depths in under a second. **Semantically Coherent Calibration** (Med-Reasoner workshop) tackles trust: a vision-language model can look well-calibrated on average while quietly over- or under-stating malignancy risk for specific patient subgroups, and we show how to discover those subgroups in a form clinicians can actually read. **Grounding MLLMs with Quantitative Skin Attributes** (VLM4RWD workshop) tackles interpretability: we fine-tune a multimodal LLM to predict measurable lesion attributes and find that the resulting embeddings are both clinically steerable and, without any diagnostic supervision, more diagnostic than existing dermatology foundation models. Seeing more, knowing when to trust the model, and explaining its reasoning in clinical terms — that is the thread running through all three.

## CD-RCM: Generalizable Continuous-Depth Novel View Synthesis for Reflectance Confocal Microscopy
Tooba Imtiaz, Milind Rajadhyaksha, **Kivanc Kose**, Jennifer Dy  
<span class="tag">Main conference</span>NeurIPS 2026 poster · [OpenReview](https://openreview.net/forum?id=0WaapzByGt) · [Presentation]({{ '/Presentations/CDRCM/CD-RCM%20Explainer.html' | relative_url }})
{: .meta}

Reflectance confocal microscopy (RCM) provides noninvasive "optical biopsies" of skin by capturing en-face images at successive depths. The catch is that lateral resolution (~0.5 µm) is about six times finer than axial resolution (~3 µm), so the resulting z-stacks are strongly anisotropic — fine for viewing slice by slice, but poor for the histopathology-style cross-sections clinicians are trained on. Spline interpolation between slices produces blocky, anatomically implausible transitions.

CD-RCM is the first novel-view synthesis method built specifically for RCM. The key observation is that RCM's acquisition geometry is nothing like a camera orbiting an object: there is no parallax, only pure axial translation through tissue. We model the microscope as a virtual pinhole camera translating along *z*, which lets us use Plücker ray embeddings and a decoder-only transformer (building on LVSM) to synthesize slices at *arbitrary* query depths from just three input slices — in a single feedforward pass, with no per-stack optimization. To preserve cellular texture, we also introduce a skin-specific perceptual loss built on a DINOv3 backbone adapted to RCM data via LoRA.

<figure>
  <img src="{{ '/images/neurips2026/cdrcm-overview.png' | relative_url }}" alt="Overview of the CD-RCM architecture" loading="lazy">
  <figcaption>Overview of CD-RCM. Sparse input slices and their Plücker ray embeddings are tokenized and processed by a decoder-only transformer; target ray tokens condition the synthesis of unseen depths. Training combines photometric, LPIPS, and skin-specific perceptual losses.</figcaption>
</figure>

<figure>
  <img src="{{ '/images/neurips2026/cdrcm-crosssections.png' | relative_url }}" alt="Sagittal, coronal and oblique cross-sections of input and CD-RCM-densified stacks" loading="lazy">
  <figcaption>Cross-sectional and arbitrary-plane views of densified stacks. CD-RCM removes the staircase artifacts visible in sparsely sampled input stacks, revealing continuous tissue morphology.</figcaption>
</figure>

- Trained and evaluated on 216 in vivo RCM stacks from a clinical VivaScope 1500
- Outperforms B-spline, cubic spline, and Gaussian interpolation on PSNR, SSIM, LPIPS, and our skin-specific perceptual metric
- 10× densification of a full stack in 0.8 seconds on a single GPU
- Enables isotropic 3D visualization with sagittal, coronal, and oblique cuts through living skin

## Learning Semantically Coherent Calibration Groups
Brandon Dominique, Prudence Lam, Max Torop, Nicholas Kurtansky, Jochen Weber, **Kivanc Kose**, Veronica Rotemberg, Jennifer Dy  
<span class="tag">Workshop</span>NeurIPS 2026 Med-Reasoner poster · [OpenReview](https://openreview.net/forum?id=Ie4y5TGU5B)
{: .meta}

A model's confidence shapes whether a clinician trusts or overrides it. Standard calibration methods fix confidence in aggregate, and multicalibration extends this to predefined groups — but in practice the groups that matter are often unknown. Existing group-discovery methods either optimize purely for calibration (and collapse into one or two uninterpretable groups) or purely for semantic similarity (and produce coherent-looking groups that still mix well- and poorly-calibrated samples).

Semantically Coherent Calibration (SCC) jointly optimizes both. A learned soft grouping function partitions a VLM's multimodal embedding space so that samples in each group share both a calibration need and a semantic neighborhood, and each group gets its own Platt or temperature scaling parameters. The result is a set of groups a practitioner can inspect: on ISIC 2024, for example, the group requiring the largest downward correction skews older and 75% male — a concrete, testable hypothesis about where the model's training data may be thin.

<figure>
  <img src="{{ '/images/neurips2026/scc-concept.png' | relative_url }}" alt="Illustration of group discovery along calibration-need and semantic axes" loading="lazy">
  <figcaption>Group discovery along two independent axes: calibration need (color) and semantic category (shape). (a) Grouping solely on calibration need mixes distinct semantic categories together. (b) Grouping solely on semantic similarity produces semantically coherent groups that still mix well- and poorly-calibrated samples, leaving calibration need unaddressed within each group. (c) SCC jointly optimizes both objectives, discovering groups that are homogeneous in calibration need and semantic category, with interpretable descriptions (e.g., “Mostly male, older, lower extremity”).</figcaption>
</figure>

<figure>
  <img src="{{ '/images/neurips2026/scc-groups.png' | relative_url }}" alt="Representative images from the four groups SCC discovers on ISIC 2024" loading="lazy">
  <figcaption>Representative images from the four groups SCC discovers on ISIC 2024, annotated by risk level and shared characteristics (a), compared with the groups found by the GC+TS baseline (b).</figcaption>
</figure>

- Evaluated on four VLMs (Qwen2-VL 2B/7B, LLaVA-1.5 7B/13B) across four dermatology datasets (ISIC 2020, ISIC 2024, MIDAS, MILK)
- Statistically significant ECE improvements in 11 of 16 settings, with up to 79% reduction over competitive baselines
- Up to 26× higher within-group cosine similarity than prior group-discovery methods
- Unlike temperature-scaling-only approaches, the Platt-scaling variants can also improve accuracy and AUC on weakly performing base models

## Grounding Multimodal Large Language Models with Quantitative Skin Attributes: A Retrieval Study
Max Torop, Masih Eskandar, Nicholas Kurtansky, Jinyang Liu, Jochen Weber, Octavia Camps, Veronica Rotemberg, Jennifer Dy, **Kivanc Kose**  
<span class="tag">Workshop</span>NeurIPS 2026 VLM4RWD poster · [OpenReview](https://openreview.net/forum?id=UI85FtXYdf) · [Presentation]({{ '/Presentations/GroundedMLLM/VLM%20Explainer.html' | relative_url }})
{: .meta}

Skin cancer classifiers can match dermatologists on accuracy while learning to rely on rulers, hair, and surgical markings rather than the lesion. Multimodal LLMs promise interpretable, conversational reasoning, but they give little control over which visual information their representations actually encode — and they are notoriously bad at quantification.

We fine-tune Qwen2-VL on the SLICE-3D dataset of 3D total-body-photography tiles to predict 16 quantitative lesion attributes known to be predictive of malignancy: area, diameter, border irregularity, color asymmetry, lesion–skin contrast, and more. Because the decoder merges image and text, the same image embedding can be steered at query time toward any attribute — or any *combination* of attributes — simply by asking. This turns a standard embedding into a clinically transparent vector for composed retrieval ("find lesions like this one, but matched on area and border irregularity"), with a hierarchical retrieval scheme that avoids storing an embedding per attribute combination.

<figure>
  <img src="{{ '/images/neurips2026/grounding-embeddings.png' | relative_url }}" alt="Image-only and attribute-conditioned embedding functions in the MLLM decoder" loading="lazy">
  <figcaption>Image-only vs. attribute-conditioned embeddings. The image embedding averages penultimate-layer image tokens; the attribute-conditioned embedding is the final token after appending a question such as "What is the area in mm²?"</figcaption>
</figure>

<figure>
  <img src="{{ '/images/neurips2026/grounding-retrieval.png' | relative_url }}" alt="Top-5 retrieval results for image-only and area-conditioned embeddings" loading="lazy">
  <figcaption>Top-5 retrieval for three query lesions. Image-only retrieval returns visually similar lesions; area-conditioned retrieval returns lesions that are visually similar <em>and</em> matched on area.</figcaption>
</figure>

- Mean R² of 0.90 across 16 attributes on a 511k-image private test set, including two held-out hospitals
- Attribute-conditioned retrieval substantially outperforms MONET, PanDerm, and the untuned base model
- Multi-attribute composition works despite training only on single-attribute questions
- With no diagnostic supervision at all, a linear probe on our embeddings reaches 0.928 AUROC for malignancy, ahead of PanDerm (0.909) and MONET (0.890)
- Retrieval transfers qualitatively to dermoscopy images never seen in training


[← Back to News]({{ '/news/' | relative_url }})
