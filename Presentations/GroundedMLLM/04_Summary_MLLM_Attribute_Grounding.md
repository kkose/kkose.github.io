---
title: "Grounding Multimodal LLMs with Quantitative Skin Attributes: A Retrieval Study"
math: true
---
<p class="meta">Torop, Eskandar, Kurtansky, Liu, Weber, Camps, Rotemberg, Dy, Kose · Northeastern University &amp; Memorial Sloan Kettering Cancer Center</p>

<p class="note"><a href="VLM%20Explainer.html">View the interactive presentation →</a> · <a href="{{ '/presentations/' | relative_url }}">All presentations</a></p>

**Background.** AI models for skin cancer detection reach dermatologist-level accuracy but give little insight into why they reach a decision. Some classifiers have been shown to rely on surgical markings, rulers, or hair rather than the lesion itself. Quantitative lesion attributes measured from 3D total-body photography (TBP) are both interpretable and predictive of malignancy; examples include size, border irregularity, and lesion-to-skin contrast. Multimodal large language models (MLLMs) offer a natural-language interface for clinicians but are known to struggle with quantities. We asked whether an MLLM can be grounded in these attributes and then used for image search that can be steered by attribute.

**Methods.** We fine-tuned Qwen2-VL on the SLICE-3D dataset from the ISIC 2024 challenge: 401,059 lesion tiles, each 15 mm across, cropped from 3D TBP scans of 1,042 patients at seven hospitals. Sixteen attributes were selected with dermatology experts, covering size, border shape, color variation, color asymmetry, and color and contrast inside and outside the lesion. For every image and attribute, the model was asked a question such as "What is the area in mm²?" and trained to answer with the measured value. Fine-tuning used low-rank adaptation (LoRA, rank 8) on both the vision encoder and the language decoder, for one epoch on four GPUs.

From the tuned model we derived two embeddings:

- **Image-only embedding:** the average of the image-token states in the second-to-last decoder layer. It captures general lesion appearance.
- **Attribute-conditioned embedding:** the final-layer state of the last token, after the model has read both the image and an attribute question. Asking about several attributes in one prompt yields a multi-attribute embedding, without any multi-attribute training.

Retrieval returns the most similar images by cosine similarity. Storing an embedding for every attribute combination is impractical, so a two-stage search first shortlists 200 candidates with the image-only embedding. It then re-ranks those candidates with the attribute-conditioned embedding.

Evaluation used the private test set of 511,474 images from 1,277 patients at nine sites. Two of those sites, Cairns and Monash in Australia, were never seen in training. We compared against the untuned Qwen2-VL and two dermatology foundation models, MONET and PanDerm.

**Results.**

- **Attribute prediction:** predicted values agreed closely with measured ones, with mean R² of 0.90. R² ranged from 0.79 for border jaggedness to 0.97 for lesion-to-skin contrast, and reached 0.87 and 0.90 at the two unseen sites.
- **Retrieval by attribute:** attribute-conditioned embeddings found the closest matches for all 16 attributes. The metric ranks how close each retrieved lesion is to the query compared with every lesion in the database (lower is better). For lesion area, the typical retrieved lesion ranked at the 8th percentile, versus the 33rd for PanDerm and the 36th for MONET.
- **Search variants:** the two-stage search performed almost identically to searching the full database. Even the image-only embedding outperformed all baselines, and prompting on pairs of attributes outperformed all baselines as well.
- **Diagnostic signal:** the model never saw a diagnosis during training. Even so, a logistic-regression classifier on its image-only embeddings separated malignant from benign lesions (480 malignant among 511,474 test images). It reached an AUROC of 0.928 (95% CI 0.916 to 0.939) and a partial AUC above 80% sensitivity of 0.143. PanDerm reached 0.909 and 0.131, and the confidence intervals overlap slightly.
- **Dermoscopy:** on dermoscopy images, a modality never used in training, retrieved lesions remained qualitatively similar in border irregularity and color asymmetry.

**Conclusions.** Supervising an MLLM with clinically meaningful measurements yields a representation that is interpretable, searchable by the features clinicians already use, and still diagnostically informative. Current limitations are the lack of a clinician reader study, no stratification by skin type, qualitative-only dermoscopy results, and unreported search speed. Future work will extend the framework toward conversational reasoning and validate it across populations, imaging devices, and acquisition settings.

---

![Image-only vs attribute-conditioned embeddings](fig_mllm_embeddings.png)

*Figure (paper, Fig. 1).* The image-only embedding averages the image-token states from the second-to-last decoder layer. The area-conditioned embedding is the last token's final-layer state after the model reads the image and the question "What is the area in mm²?"
