---
title: Research
permalink: /research/
---
I develop machine learning and computer vision methods that make noninvasive imaging practical in the clinic. The common thread is translation: building tools that standardize image acquisition, support real-time decisions and hold up in prospective clinical evaluation, not only on retrospective benchmarks. My work spans the Optical Imaging Lab and the Dermatology Imaging Informatics Lab in the MSK Dermatology Service, in close collaboration with clinicians and with engineering groups at Northeastern University.

## Reflectance confocal microscopy and videomosaicking

Reflectance confocal microscopy (RCM) images skin at cellular resolution without a biopsy, but its adoption has been limited by variable image acquisition, a small field of view and the need for expert readers. I have developed algorithms that identify skin layers, segment cellular patterns associated with malignancy and classify lesions with supervised and weakly supervised learning. Two of my segmentation methods are integrated into a research RCM device at MSK for prospective testing, and our mosaicking and digital staining work is part of the current device software.

To overcome the small field of view, I led the development of videomosaicking for handheld RCM: reconstructing large, high-resolution mosaics from videos acquired along free-form paths while correcting for motion blur, contact-angle distortion and depth changes. We have used it for lesion margin mapping, intraoperative evaluation of surgical wounds and detection of sparse disease features, and I am now extending it to oral imaging.

For ex vivo confocal microscopy during Mohs surgery, we developed 3D mosaicking with intensity projection that recovers the curved epidermal margin missed by single-plane mosaics — the region where superficial basal cell carcinoma most often hides. We also trained a deep learning model that detects basal cell carcinoma directly from in vivo RCM stacks.

## AI for dermoscopy, smartphone and 3D total-body photography

With clinical colleagues at MSK and around the world, I build and evaluate models for melanoma and skin cancer detection from dermoscopy, smartphone photos and 3D total-body photography (3D TBP), including automated triage of cancer-suspicious lesions from 3D TBP, longitudinal lesion monitoring, and pediatric lesion diagnosis.

## Open datasets and benchmarks (ISIC)

I am co-PI of the NIH-funded [International Skin Imaging Collaboration (ISIC)](https://www.isic-archive.com) Archive, the largest public repository of dermatologic images. I work on data ingestion, quality control and curation pipelines, on extending the archive to multimodal data (dermoscopy, clinical and total-body photography, and more), and on coordinating ISIC's international machine-learning challenges. I led the technical work behind the SLICE-3D dataset of 400,000 lesion crops from 3D TBP, used for the ISIC 2024 challenge.

## Trustworthy and interpretable machine learning

Clinical AI has to be reliable, not only accurate. With Jennifer Dy's group at Northeastern, I work on out-of-distribution detection, calibration and fairness benchmarking, feature-interaction explanations for neural networks (SmoothHess, NeurIPS 2023), and grounding multimodal large language models in quantitative, clinically meaningful skin attributes.

## Oral and head-and-neck cancer imaging

I apply these methods beyond the skin: AI-assisted RCM for intraoperative margin assessment in oral squamous cell carcinoma, in vivo detection of oral lesions in outpatient clinics in India, and smartphone-based oral lesion screening for low-resource settings.

## Research funding

### Current

<ul class="dated">
<li><span class="when">2022–2027</span><span><strong>MISIC: A Multimodal Open-Source International Skin Imaging Collaboration Informatics Platform for Automated Skin Cancer Detection</strong><br><span class="meta">NIH U24 CA264369 · co-PI with V. Rotemberg</span></span></li>
<li><span class="when">2023–2028</span><span><strong>ISICREPO: ISIC Skin Imaging Repository Enhancements for Promoting Interoperability and Utilization</strong><br><span class="meta">NIH U24 CA285296 · co-PI with A. Halpern</span></span></li>
<li><span class="when">2024–2026</span><span><strong>Generalizable and reliable AI for early detection of melanoma</strong><br><span class="meta">CDMRP ME230206 · co-PI with A. Halpern</span></span></li>
<li><span class="when">2024–2027</span><span><strong>Enabling AI for Early Detection of Melanoma in Smartphone Images</strong><br><span class="meta">Melanoma Research Alliance · co-PI with A. Halpern</span></span></li>
<li><span class="when">2024–2028</span><span><strong>Non-invasive, quantitative microscopic biomarkers for chemotherapy-induced peripheral neuropathy</strong><br><span class="meta">NCI · Site PI (PI: D. Kang)</span></span></li>
<li><span class="when">2024–2028</span><span><strong>Probe-based light sheet microscopy (pLSM) for screening of anal cancer</strong><br><span class="meta">NCI · Site PI (PI: D. Kang)</span></span></li>
<li><span class="when">2023–2026</span><span><strong>FINDMEL: Developing an application for Following Images of Nevi to Detect Melanoma</strong><br><span class="meta">CDMRP Melanoma Academy Scholar Award · Investigator (PI: V. Rotemberg)</span></span></li>
</ul>

### Completed

<ul class="dated">
<li><span class="when">2023–2025</span><span><strong>Mobile phone-based deep learning algorithm for oral lesion screening in low-resource settings</strong><br><span class="meta">NCI R21 CA274717 · Investigator (PI: M. Rajadhyaksha)</span></span></li>
<li><span class="when">2020–2024</span><span><strong>Simultaneous coaxial widefield imaging and reflectance confocal microscopy for improved diagnosis of skin cancers in vivo</strong><br><span class="meta">NIBIB R01 EB028752 (PIs: D. Dickensheets, M. Rajadhyaksha, O. Camps, B. Fox)</span></span></li>
<li><span class="when">2019–2024</span><span><strong>Confocal videomosaicking microscopy to guide surgery of superficially spreading skin cancers</strong><br><span class="meta">NCI R01 CA240771 and supplement · Project Investigator (PIs: M. Rajadhyaksha, O. Camps)</span></span></li>
<li><span class="when">2015–2019</span><span><strong>Automated Image Guidance of Diagnosing Skin Cancer with Confocal Microscopy</strong><br><span class="meta">NCI R01 CA199673 · Project Investigator (PI: M. Rajadhyaksha)</span></span></li>
</ul>
