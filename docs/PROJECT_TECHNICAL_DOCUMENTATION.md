# Unpaired Image Deraining — Technical Project Documentation

> **Project:** Benchmarking Unpaired Rain Removal Algorithms: From Generative Adversarial Network to Diffusion Models  
> **Research period:** Durham University MSc Dissertation (2023–2024), followed by post-graduation model refinement  
> **Research area:** Computer Vision / Image Restoration / Unpaired Image-to-Image Translation / Generative Models  
> **Repository:** https://github.com/engrenage0409/Unpaired-Rain-Removal-Algorithms

## 1. Project overview

### 1.1 Problem

Rain streaks, raindrops and rain haze degrade images captured by cameras. In autonomous-driving and other outdoor-vision scenarios, this degradation can affect object detection, tracking, obstacle perception and downstream decisions.

A major practical difficulty is the lack of strictly paired rainy/clean images. In real scenes, it is difficult to capture the same scene from the same camera pose and under identical lighting conditions with rain present and absent. Models trained only on synthetically paired rain-streak data may also generalize poorly to real rain streaks, raindrops, dense rain and complex backgrounds.

This project investigated **unpaired image deraining**: learning a mapping between rainy and clean image domains without relying on one-to-one aligned rainy/clean training pairs. The work combined a literature review, dataset construction, model adaptation and fine-tuning, quantitative/qualitative benchmarking, and a frame-wise video deraining validation.

### 1.2 Research contributions

The MSc dissertation established the following contributions:

1. Reviewed and categorized unpaired rain-removal methods, including model-driven methods and deep-learning approaches.
2. Constructed two unpaired training datasets: a mixed rain-streak/raindrop dataset and a real-raindrop-focused dataset.
3. Benchmarked five representative unpaired deep-learning models: CycleGAN, CUT, FastCUT, DCLGAN and CycleGAN-Turbo.
4. Evaluated restoration quality using PSNR, SSIM, VIF and FID, and compared model-level inference time / FPS.
5. Tested a video-processing workflow using frame-wise inference and identified the gap between standalone model throughput and end-to-end video performance.
6. Published a GitHub resource repository collecting paper/code references, datasets, trained-model links, visual comparisons and metric tables.

After graduation, the work continued with architecture and training refinements to CycleGAN and CycleGAN-Turbo. These later experiments must be distinguished from the original dissertation benchmark.

### 1.3 Work ownership and implementation approach

The model work was based on open-source implementations and research code. The work involved adapting and fine-tuning models, modifying network structures and training objectives, designing experiments, evaluating results and comparing trade-offs. Do not describe the work as inventing all base architectures from scratch.

The exact implementation details that are not preserved in the current dissertation/README—such as every optimizer setting, learning rate, batch size, random seed, hardware configuration and individual ablation table—should be recovered from experiment logs before being stated as facts.

---

## 2. Dataset construction and experimental protocol

### 2.1 Source datasets

The dissertation sampled images from the following sources:

- Rain100
- Rain800
- Rain1400
- Rain12600
- Real rain-streak dataset
- Real raindrop dataset

The dissertation states that images were resized to **256 × 256** before the reported experiments.

### 2.2 Unpaired training datasets

**Dataset A — Mixed rain streaks and raindrops**

Rainy-domain composition:

| Source | Rainy images |
|---|---:|
| Real raindrop dataset | 402 |
| Real rain-streak dataset | 217 |
| Rain100 | 50 |
| Rain800 | 40 |
| Rain1400 | 120 |
| Rain12600 | 240 |
| **Total rainy images** | **1,069** |

The dataset also contains **1,069 rain-free images**. The rainy and clean domains were used for unpaired training; the count should not be described as 1,069 strictly aligned supervised pairs.

**Dataset B — Real-raindrop-focused dataset**

| Domain | Images |
|---|---:|
| Real raindrop images | 861 |
| Rain-free images | 861 |

This dataset was designed to focus model learning on raindrop characteristics rather than asking one model to learn both streaks and drops from a mixed distribution.

### 2.3 Test data and visual validation

The dissertation reports:

- A randomly selected set of 500 images from the Rain1400 and Rain12600 synthetic datasets for rain-streak evaluation. The dissertation does not clearly state whether 500 means the combined total or 500 per source; do not silently assume either.
- A real-raindrop test set containing 57 images.
- Additional real rainy-day images and videos, including road-surveillance-camera footage, for visual comparison and video validation.

### 2.4 Training schedule

The dissertation README states that the five primary benchmark models—CycleGAN, CUT, FastCUT, DCLGAN and CycleGAN-Turbo—were trained using both training datasets at **100, 200 and 400 epochs**.

The repository links to trained model files hosted externally on Google Drive. Exact per-run hyperparameters are not fully specified in the README or dissertation text available for this documentation.

---

## 3. Model landscape: what was reviewed, what was benchmarked, and what was additionally explored

It is important to distinguish a literature-review inventory from models actually trained or evaluated by the project.

### 3.1 Primary quantitative benchmark — five models

| Model | Family | Main mechanism | Project status |
|---|---|---|---|
| CycleGAN | GAN-based unpaired translation | Two generators, two discriminators, adversarial learning and cycle consistency | Trained and benchmarked; later architecture/training refinements |
| CUT | Contrastive unpaired translation | One generator and one discriminator; PatchNCE contrastive objective | Trained and benchmarked |
| FastCUT | Contrastive unpaired translation | CUT variant with a simplified PatchNCE training strategy | Trained and benchmarked |
| DCLGAN | Dual contrastive GAN | Contrastive learning in image space and feature space | Trained and benchmarked |
| CycleGAN-Turbo | GAN + diffusion / one-step image translation | Stable-Diffusion/SD-Turbo-derived latent diffusion components adapted to unpaired image translation | Trained and benchmarked; later conditional/skip/mask/loss refinements |

### 3.2 Additional diffusion experiments

The user also reports experimenting with:

**Stable Diffusion**

- Latent Diffusion Model (LDM) family.
- General data flow: image encoder/VAE maps pixels into a latent representation; a U-Net performs latent-space denoising; the VAE decoder reconstructs an image.
- The project explored its use/adaptation for rain removal. The observed restoration quality was high, but inference was too slow for real-time/mobile validation.
- Exact project-specific structural modifications, quantitative metrics and test configuration are not currently recorded; do not invent them.

**UNIT-DDPM**

- Combines the UNIT unpaired image-translation idea with a denoising diffusion probabilistic model.
- UNIT assumes that images from two domains can be mapped into a shared latent space; DDPM introduces noise in a forward process and learns a reverse denoising process.
- The dissertation's literature discussion describes controllable noise injection at different generative steps to model and correct translation errors.
- The user reports experimenting with UNIT-DDPM and observing high restoration quality but slow inference. No mobile test or verified numerical benchmark is available in the current record.

These additional experiments should be described as additional model adaptation/evaluation, not silently added to the dissertation's five-model quantitative benchmark table.

### 3.3 Other deep-learning methods reviewed in the dissertation

The dissertation's related-work section also discusses the following methods. They are **literature-review subjects**, not evidence that every method was trained in this project.

**GAN / unpaired image-translation methods**

- DerainCycleGAN: CycleGAN-based deraining with an Unsupervised Attention Detector for Rainfall (UARD).
- UNIT: GAN + VAE with a shared latent-space assumption across two image domains.
- MUNIT: separates content and style; uses content and style encoders for the two domains.
- FUNIT: few-shot unpaired translation designed for target domains with limited samples.
- AttentionGAN: attention-guided image translation that focuses the generator on important regions.
- DiscoGAN: cross-domain image translation.
- DualGAN: dual learning for image-to-image translation.
- NLCL: unpaired deraining that combines contrastive learning and self-similarity.

**Diffusion and emerging generative methods**

- Cycle-Diffusion: uses a diffusion-model latent representation/encoding mechanism for unpaired image-to-image translation.
- DDGAN: denoising diffusion GAN.
- SYDIFF: synchronization-oriented diffusion method referenced by the dissertation.
- UNSB: Unpaired Neural Schrödinger Bridge for unpaired image translation.
- Shortest-path-regularized unpaired image translation.
- WeatherDiff: mentioned in the dissertation's discussion of adverse-weather restoration.
- The dissertation also introduces the general LDM / Stable Diffusion and SD-Turbo concepts as background for CycleGAN-Turbo.

**Other deep-learning deraining families mentioned**

- CNN-based restoration
- ResNet-based restoration
- Attention-based deraining
- DID-MDN
- UDR-S2Former

---

## 4. Architecture fundamentals of the five benchmark models

### 4.1 CycleGAN

CycleGAN performs unpaired image-to-image translation between domains X and Y.

Core components:

- Generator G: X → Y
- Generator F: Y → X
- Discriminator D_Y: distinguishes real Y-domain images from generated images
- Discriminator D_X: distinguishes real X-domain images from generated images

The two mappings are trained adversarially. Cycle consistency encourages a round trip to reconstruct the original input:

\[
F(G(x))\approx x,\qquad G(F(y))\approx y
\]

A common cycle loss is:

\[
L_{\mathrm{cyc}}=
\mathbb E_{x}\|F(G(x))-x\|_1+
\mathbb E_{y}\|G(F(y))-y\|_1
\]

For rain removal, the central challenge is that the model must remove rain without changing unrelated scene content. A high cycle-consistency weight can also discourage the generator from making necessary rain-removal changes, so the loss weights must be balanced.

### 4.2 CUT

CUT simplifies the CycleGAN-style translation setup to one generator and one discriminator. It uses:

- Adversarial loss for target-domain realism.
- PatchNCE loss to preserve input content by contrasting corresponding source/translated patches in feature space.

The dissertation describes PatchNCE as comparing local patch representations from the source image and translated image. This provides content-preservation pressure without requiring paired target images.

### 4.3 FastCUT

FastCUT is a faster CUT variant. The dissertation describes its main distinction as simplifying the PatchNCE loss/training strategy to accelerate training.

### 4.4 DCLGAN

DCLGAN uses dual contrastive learning, comparing information in both image space and feature space. The intention is to improve content correspondence and generated-image quality in unpaired translation.

### 4.5 CycleGAN-Turbo

The dissertation describes CycleGAN-Turbo as combining CycleGAN-style unpaired image translation with Stable Diffusion / SD-Turbo components.

General latent-diffusion data flow:

1. VAE encoder compresses an input image into latent features.
2. U-Net performs denoising / latent transformation.
3. VAE decoder reconstructs the output image.

CycleGAN-Turbo adapts the diffusion foundation for one-step image translation and adversarial learning. The dissertation describes combining three latent-diffusion modules into an end-to-end network with fewer trainable weights. SD-Turbo uses Adversarial Diffusion Distillation (ADD) to reduce the number of denoising steps and accelerate generation.

---

## 5. Post-graduation CycleGAN architecture and training refinements

This section records the user's later work, which was performed **after the MSc dissertation**. These modifications should not be presented as if they were all included in the dissertation's original benchmark.

### 5.1 Problem analysis

The original CycleGAN baseline showed limitations under:

- Heavy rain, rain haze and dense rain streaks.
- Removal of rain while also erasing fine scene details.
- Color shifts and local artifacts.
- Complex backgrounds such as streets, people, vehicles and vegetation, where edges could become blurred.

### 5.2 Generator modifications

**A. Intermediate residual-block attention**

- Modified the Generator ResBlocks.
- Inserted CBAM and SE attention in intermediate feature stages rather than indiscriminately adding attention to every layer.
- SE emphasizes channel-wise feature recalibration.
- CBAM combines channel and spatial attention.
- Goal: help the network focus on rain-related features while retaining useful background representations.

**B. Wavelet high-/low-frequency branch**

- Added a wavelet-based branch to process high- and low-frequency information.
- High-frequency features can contain rain streak edges and fine textures; low-frequency features capture broader structures and illumination/background components.
- The branch is intended to help distinguish rain patterns from scene content.
- Important caveat: background edges also contain high-frequency information, so frequency separation alone cannot identify rain. It must be learned/fused with spatial and semantic context.

**C. Auxiliary rain-mask prediction head**

- Generator predicts both the derained image and a rain-streak mask.
- The mask provides an explicit estimate of where rain is present.
- The mask constraint is incorporated into the cycle-consistency training objective, according to the user's description.
- Intended behavior: allow stronger changes in rain-affected regions while preserving non-rain background structure.
- Exact mask-loss equation, mask-label source and mask thresholding procedure are not documented in the available record and should be recovered from code/logs before claiming a specific formulation.

**D. Upsampling modification**

- Replaced transposed convolution (ConvTranspose) with Upsample + ordinary Convolution.
- Motivation: transposed convolution can produce checkerboard artifacts due to uneven overlap; resize-convolution can reduce this risk.

**E. Multi-scale spatial feature handling**

The explored modifications include:

- Skip connections.
- Multi-scale feature concatenation.
- Pyramid pooling / pyramid feature extraction.

Purpose: preserve fine rain details while retaining broader scene layout and context.

**F. Normalization experiments**

The user reports experimenting with normalization alternatives:

- InstanceNorm (IN): local appearance/style and texture statistics.
- LayerNorm (LN).
- GroupNorm (GN).
- AdaIN / conditional normalization as rain-condition-aware alternatives.
- BatchNorm was considered less stable for small-batch/small-dataset settings.

Do not claim a particular normalization variant won unless the experiment log confirms the final selection.

### 5.3 Discriminator modifications

The original PatchGAN discriminator emphasizes local patches. The user reports testing:

- Local PatchGAN to focus on rain streaks and local texture realism.
- A global discriminator to constrain full-image structure and color consistency.
- Multi-scale PatchGAN discriminators to cover fine streaks and broader rain/haze patterns.
- Balancing discriminator strength so that an overly strong discriminator does not make the generator overly conservative or destabilize learning.

### 5.4 Loss-function design

The user reports combining or testing:

- Adversarial loss.
- Cycle-consistency loss.
- VGG perceptual loss.
- Structural / SSIM-oriented loss.
- Rain-mask loss / mask constraints.

Conceptual roles:

| Objective | Intended constraint |
|---|---|
| Adversarial | Output should resemble the clean-image domain |
| Cycle consistency | Preserve the ability to reconstruct the source domain after round-trip translation |
| Perceptual (VGG) | Preserve higher-level visual features and reduce detail degradation |
| SSIM / structural | Preserve local structure and contrast relationships |
| Rain-mask constraint | Focus modifications on rain-affected regions and protect clean regions |

Cycle loss should not be increased blindly: an excessive cycle penalty can discourage necessary changes to rain regions.

### 5.5 Training and data strategy

The user reports experimenting with:

- Rain-type augmentation for fine streaks, heavy streaks, raindrops and rain haze.
- Synthetic rain-layer generation to expand rain appearance variation.
- Generator warm-up before enabling full adversarial training.
- WGAN-GP as an alternative adversarial formulation to improve training stability.
- Pretraining on public deraining data followed by domain-specific fine-tuning on the user's target rain images.
- Freezing lower downsampling layers to retain general edge/texture features while updating intermediate residual blocks and higher-level layers for target-domain adaptation.

Exact optimizer, learning-rate schedule, batch size, gradient-penalty coefficient, loss weights and ablation values are not currently documented.

### 5.6 Post-graduation CycleGAN results

The user recalls the following approximate improvements relative to the original CycleGAN:

| Metric | Recalled change |
|---|---:|
| PSNR | approximately +6 dB |
| SSIM | approximately +0.20 |
| FID | approximately -10 |

These are **post-graduation follow-up results recalled by the user**, not values from the dissertation's original benchmark tables. The exact test set, baseline checkpoint, image preprocessing and metric implementation must be verified from logs before public claims or final resume submission.

---

## 6. Post-graduation CycleGAN-Turbo architecture refinements

The Turbo changes are separate from the CycleGAN modifications because the underlying architecture is different.

### 6.1 Rain-intensity conditioning branch

- Added a condition branch to encode rain intensity / rain category.
- Intended categories include light rain, heavy rain and rain haze.
- Injected the condition into U-Net layers so the model can adapt restoration behavior to different rain conditions.
- Motivation: avoid under-removing heavy rain and over-modifying light-rain images.

### 6.2 Skip / Zero-Conv pathway optimization

- Optimized the skip / zero-convolution pathways.
- These pathways help retain high-frequency information that may otherwise be smoothed by VAE compression.
- Goal: preserve thin structures, edges and fine image details through the latent-diffusion pipeline.

### 6.3 Lightweight rain-mask head

- Added a small convolutional head after the main U-Net output.
- The head predicts a rain-region mask.
- The mask is used to discourage unnecessary changes to clean background regions.
- This is an auxiliary head; it does not replace the main U-Net.

### 6.4 Composite loss modification

The user reports modifying a composite objective incorporating:

- Cycle-consistency loss.
- Perceptual loss.
- Gradient loss.
- Structural loss.
- Rain-streak attention / mask-related loss.
- Lightweight adversarial loss.

The precise weights and mathematical implementation are not available in the current project record.

### 6.5 Post-graduation CycleGAN-Turbo results

The user recalls:

- FID decreased by approximately 10.
- PSNR and SSIM changed little.

Interpretation should be cautious: a lower FID suggests improved similarity of generated-image feature distributions, but does not by itself prove better pixel-level reconstruction or temporal consistency.

**LoRA is not included as a completed modification.** It was discussed as a possible/preferred fine-tuning route but was not confirmed as implemented.

---

## 7. Additional diffusion model experiments

### 7.1 Stable Diffusion

The project explored the use of Stable Diffusion for rain removal through an open-source implementation/adaptation and fine-tuning.

General LDM architecture:

\[
x \xrightarrow{\mathrm{VAE\ Encoder}} z
\xrightarrow{\mathrm{U\text{-}Net}} \tilde z
\xrightarrow{\mathrm{VAE\ Decoder}} \hat x
\]

The user observed high restoration quality but slow inference, making it unsuitable for the intended real-time/mobile test. No verified numerical PSNR/SSIM/FID result is available in the current record.

### 7.2 UNIT-DDPM

UNIT-DDPM combines:

- UNIT's shared-latent-space assumption for unpaired image translation.
- DDPM's forward noise process and learned reverse denoising process.

The dissertation's related-work explanation describes injecting controllable noise at different generative steps to model and correct translation errors. The user reports adapting/testing this model and observing high restoration quality but slow inference. No mobile test or verified numerical result is recorded.

### 7.3 Why diffusion quality and speed can conflict

Diffusion models can provide strong generative priors and detailed outputs, but iterative denoising or a larger latent-generation pipeline can impose significant inference cost. CycleGAN-Turbo uses a one-step/accelerated diffusion-based translation approach, but the dissertation still measured a much lower frame rate than CycleGAN/CUT/FastCUT.

---

## 8. Dissertation benchmark results

### 8.1 Table I — Rain1400 and Rain2600 labels as printed in the dissertation

The dissertation table header says **Rain2600**, while other sections of the dissertation discuss **Rain12600**. This is an unresolved naming inconsistency in the source and should be checked against the original evaluation files. The table values below are transcribed as printed.

| Model | Rain1400 PSNR ↑ | SSIM ↑ | VIF ↑ | FID ↓ |
|---|---:|---:|---:|---:|
| CycleGAN | 25.512 | 0.836 | 0.801 | 93.216 |
| CUT | 23.175 | 0.825 | 0.784 | 109.544 |
| FastCUT | 23.162 | 0.764 | 0.828 | 91.423 |
| DCLGAN | 22.824 | 0.731 | 0.833 | 102.650 |
| CycleGAN-Turbo | **26.886** | **0.886** | 0.841 | **59.255** |

| Model | Second dataset PSNR ↑ | SSIM ↑ | VIF ↑ | FID ↓ |
|---|---:|---:|---:|---:|
| CycleGAN | 22.583 | 0.824 | 0.819 | 135.351 |
| CUT | 19.530 | 0.864 | 0.795 | 74.882 |
| FastCUT | 22.962 | 0.808 | 0.817 | 62.657 |
| DCLGAN | 21.124 | 0.737 | 0.833 | 127.650 |
| CycleGAN-Turbo | **27.168** | **0.901** | 0.818 | **61.788** |

### 8.2 Table II — Model-level inference time and frame rate

| Model | Inference time (s) | FPS |
|---|---:|---:|
| CycleGAN | 0.01 | 100 |
| CUT | 0.01 | 100 |
| FastCUT | 0.01 | 100 |
| DCLGAN | 0.03 | 33.33 |
| CycleGAN-Turbo | 0.66 | 1.52 |

The dissertation's discussion rounds CycleGAN-Turbo to 1.51 FPS in one place; the table reports 1.52 FPS. Use approximately 1.5 FPS when discussing the result.

**Important:** these are model-level inference measurements, not proof of end-to-end video throughput. The dissertation's video demonstration showed processing delay and degraded output clarity, and the dissertation explicitly notes that this was not true real-time performance.

### 8.3 Table III — Mixed-rain training versus raindrop-specific training

**Mixed-rain training dataset**

| Model | PSNR ↑ | SSIM ↑ | VIF ↑ | FID ↓ |
|---|---:|---:|---:|---:|
| CycleGAN | 22.804 | 0.795 | 0.829 | 124.748 |
| CUT | 20.539 | 0.726 | 0.854 | 122.445 |
| FastCUT | 20.867 | 0.734 | 0.806 | 129.581 |
| DCLGAN | 20.460 | 0.740 | 0.881 | 131.616 |
| CycleGAN-Turbo | 21.987 | 0.815 | 0.780 | 41.920 |

**Raindrop-specific training dataset**

| Model | PSNR ↑ | SSIM ↑ | VIF ↑ | FID ↓ |
|---|---:|---:|---:|---:|
| CycleGAN | 23.818 | 0.809 | 0.855 | 79.512 |
| CUT | 22.535 | 0.747 | 0.862 | 101.411 |
| FastCUT | 23.110 | 0.769 | 0.860 | 77.795 |
| DCLGAN | 22.069 | 0.720 | 0.920 | 132.977 |
| CycleGAN-Turbo | **24.983** | **0.823** | 0.804 | **38.481** |

### 8.4 Main experimental observations

1. CycleGAN-Turbo achieved the strongest overall Rain1400 PSNR/SSIM and lowest FID among the five benchmark models, but its inference speed was much lower.
2. CycleGAN, CUT and FastCUT reached approximately 100 FPS at the model-inference level in the reported setup.
3. DCLGAN had lower throughput and weaker results on several restoration metrics in the benchmark.
4. The dedicated raindrop dataset improved most models' raindrop-removal results compared with mixed-rain training. DCLGAN is an exception on some metrics, so the conclusion should not be stated as universal.
5. Mixed training can create competing feature-learning demands across rain streaks and raindrops. A model trained to handle multiple rain types may not specialize as effectively in one type.
6. Dense rain, dense raindrops and complex scenes such as portraits remain difficult. Background edges and high-frequency textures can be mistaken for rain, causing blur or artifacts.

---

## 9. Video deraining: completed work versus future research

### 9.1 What was completed in the dissertation

- Processed video as a sequence of individual frames.
- Called the image deraining model independently for each frame.
- Used CycleGAN for the video validation because of its favorable model-level speed and restoration-quality trade-off.
- Observed end-to-end delay and degraded output clarity.
- Did not use ConvLSTM, ConvGRU, optical-flow feature alignment, recurrent hidden states or temporal consistency loss in the completed video pipeline.

Correct description: **frame-wise video deraining validation**, not a temporally recurrent video deraining network.

### 9.2 Future research direction — not yet implemented

The user selected a future research target of real-time video deraining while exploring both CycleGAN and CycleGAN-Turbo.

Potential research plan:

**Track A — Temporal CycleGAN**

- Retain the improved CycleGAN spatial backbone.
- Extract lower-resolution feature maps.
- Add lightweight temporal alignment.
- Compare ConvGRU and ConvLSTM for causal temporal fusion.
- Reuse the rain-mask head to guide temporal refinement.
- Add motion-aware temporal consistency loss with occlusion/reliability masking.
- Measure end-to-end latency and sustained FPS.

**Track B — Temporal CycleGAN-Turbo**

- Retain the rain-intensity condition branch and optimized skip/zero-conv pathways.
- Explore latent-space temporal feature fusion.
- Reuse the lightweight rain-mask head.
- Investigate mixed precision, inference optimization and possibly student-model distillation because the original Turbo benchmark was approximately 1.5 FPS.
- Do not assume temporal fusion alone will make Turbo real-time.

**Potential shared real-time architecture**

\[
I_t \rightarrow \mathrm{Spatial\ Encoder}
\rightarrow \mathrm{Temporal\ Alignment}
\rightarrow \mathrm{ConvGRU/ConvLSTM}
\rightarrow \mathrm{Decoder}
\rightarrow \hat I_t
\]

A causal architecture uses the current frame and prior hidden state, without waiting for future frames. This is more suitable for live camera streams than a symmetric multi-frame window that requires future-frame buffering.

Potential temporal objective:

\[
L_{\mathrm{total}} =
L_{\mathrm{spatial}}+
\lambda_t L_{\mathrm{temporal}}+
\lambda_m L_{\mathrm{mask}}
\]

Temporal consistency must be motion-aware. Occlusion, newly revealed regions and moving objects require reliability/visibility masks to avoid ghosting and over-smoothing.

### 9.3 Future evaluation metrics

- PSNR / SSIM: frame-level reconstruction quality.
- VIF / FID: visual information and distribution-level quality.
- Temporal warping error: consistency after motion alignment.
- Flicker / temporal stability: frame-to-frame instability.
- Sustained FPS, P50/P95 latency: real-time performance.
- Peak VRAM and parameter count: deployment cost.
- Downstream detection/tracking performance: whether deraining improves perception, not only visual appearance.

---

## 10. Literature-review taxonomy from the dissertation

This inventory is included to support technical interviews. It is a summary of methods discussed in the dissertation, not a claim that every method was implemented.

### 10.1 Model-driven single-image methods

- Guided filtering / bootstrap filtering.
- Multi-guided filtering.
- Guided L0 smoothing.
- Morphological Component Analysis (MCA), dictionary learning and sparse coding.
- HOG feature descriptors with K-means dictionary construction.
- Depth-guided rain-streak removal using sparse coding.
- Rain-layer priors / layer decomposition.

### 10.2 Model-driven video methods

- Spatio-temporal physical-motion / fuzzy models.
- Temporal and chromatic property clustering using K-means.
- Kalman-filter-based rain removal.
- Gaussian Mixture Models (GMM).
- Probabilistic rain-column models using temporal rain characteristics.
- Spatio-temporal frequency models with fuzzy Gaussian assumptions.
- Low-rank matrix decomposition.
- Sparse-matrix rain modeling.
- Joint low-rank and sparse decomposition.
- Multi-scale convolutional sparse coding (MS-CSC).

### 10.3 Deep-learning methods

- CNN / ResNet / attention-based deraining.
- DID-MDN.
- UDR-S2Former.
- CycleGAN and DerainCycleGAN.
- CUT and FastCUT.
- DCLGAN.
- UNIT, MUNIT and FUNIT.
- AttentionGAN.
- DiscoGAN and DualGAN.
- NLCL.
- CycleGAN-Turbo.
- Cycle-Diffusion.
- UNIT-DDPM.
- DDGAN.
- SYDIFF.
- UNSB.
- Shortest-path-regularized unpaired image translation.
- WeatherDiff (mentioned in the adverse-weather discussion).

---

## 11. Resume-ready project descriptions

### 11.1 CV Engineer — Chinese

**无配对图像除雨与生成模型架构优化**  
Durham University MSc Dissertation + Post-graduation Research

- **无配对数据集构建与多模型 Benchmark：** 基于 Rain100、Rain800、Rain1400、Rain12600 及真实雨丝、雨滴数据集，构建混合雨型（1,069 张雨图及 1,069 张无雨图）和纯真实雨滴（861 张雨图及 861 张无雨图）两套无配对数据集；基于开源实现适配、微调并对比 CycleGAN、CUT、FastCUT、DCLGAN、CycleGAN-Turbo、Stable Diffusion、UNIT-DDPM 等生成模型，采用 PSNR、SSIM、VIF、FID 和 FPS 评估恢复质量与计算效率。
- **CycleGAN 架构与训练优化：** 改进 Generator ResBlock，引入 CBAM/SE 注意力、小波高低频分支及辅助 Rain Mask Head；优化上采样、多尺度特征融合、Skip Connection、PatchGAN 局部/全局判别策略，并联合调整对抗、循环一致性、感知、结构及掩码损失，开展归一化、分阶段训练、WGAN-GP、雨型增强和领域微调实验。毕业后持续优化实验中，PSNR 约提升 6 dB、SSIM 约提升 0.20、FID 约降低 10。
- **CycleGAN-Turbo 条件扩散优化：** 增加雨强度条件分支，优化 Skip/Zero-Conv 特征通路，增加轻量 Rain Mask Head 并调整复合损失；后续实验中 FID 约降低 10，PSNR/SSIM 基本稳定。
- **视频除雨验证：** 采用逐帧调用方式处理连续视频，选择 CycleGAN 开展视频测试；模型级推理约 100 FPS，但端到端处理仍存在延迟和输出清晰度下降，分析单帧推理与真实视频处理之间的性能差距。

### 11.2 CV Engineer — English

**Unpaired Image Deraining & Generative Model Architecture Optimization**  
Durham University MSc Dissertation + Post-graduation Research

- **Unpaired Dataset Construction & Benchmarking:** Constructed mixed-rain (1,069 rainy and 1,069 clean images) and real-raindrop-focused (861 rainy and 861 clean images) unpaired datasets from Rain100, Rain800, Rain1400, Rain12600 and real-world sources. Adapted and fine-tuned open-source CycleGAN, CUT, FastCUT, DCLGAN, CycleGAN-Turbo, Stable Diffusion and UNIT-DDPM models, evaluating restoration quality and efficiency using PSNR, SSIM, VIF, FID and FPS.
- **CycleGAN Architecture & Training Optimization:** Enhanced Generator ResBlocks with CBAM/SE attention, wavelet-based high-/low-frequency features and an auxiliary Rain Mask Head. Optimized upsampling, multi-scale feature fusion, skip connections and local/global PatchGAN discrimination; jointly refined adversarial, cycle-consistency, perceptual, structural and mask objectives, alongside normalization, staged training, WGAN-GP, rain augmentation and domain fine-tuning. Follow-up post-graduation experiments achieved approximately +6 dB PSNR, +0.20 SSIM and -10 FID.
- **CycleGAN-Turbo Conditional Diffusion Optimization:** Added rain-intensity conditioning, optimized Skip/Zero-Conv pathways, introduced a lightweight Rain Mask Head and modified the composite loss. Follow-up experiments achieved approximately -10 FID with broadly stable PSNR/SSIM.
- **Video Deraining Validation:** Applied frame-wise inference to continuous video sequences and selected CycleGAN for video testing. Model-level inference reached approximately 100 FPS, while end-to-end processing still showed latency and output-clarity degradation.

### 11.3 General Algorithm Engineer — Chinese

**无配对图像恢复与生成模型算法研究**  
Durham University MSc Dissertation + Post-graduation Research

- **无配对学习与生成模型 Benchmark：** 构建混合雨型与真实雨滴两套无配对数据集，对 GAN、Contrastive Learning 和 Diffusion 生成范式开展模型适配、微调与比较，覆盖 CycleGAN、CUT、FastCUT、DCLGAN、CycleGAN-Turbo、Stable Diffusion 和 UNIT-DDPM；建立基于 PSNR、SSIM、VIF、FID 和 FPS 的综合评估体系。
- **CycleGAN 算法优化：** 联合优化生成器、判别器与训练目标，引入注意力、频域分解、雨区辅助预测和多尺度特征融合，结合对抗、循环一致性、感知、结构和掩码损失，并研究归一化、训练稳定性、数据增强及领域微调。后续实验中 PSNR 约提升 6 dB、SSIM 约提升 0.20、FID 约降低 10。
- **CycleGAN-Turbo 条件生成优化：** 增加雨强度条件编码，优化 Skip/Zero-Conv 特征传递和辅助雨痕预测分支，调整复合损失；后续实验中 FID 约降低 10，PSNR/SSIM 基本稳定。
- **视频处理与性能分析：** 将图像恢复模型接入连续视频逐帧处理流程，对比模型级吞吐量、恢复质量与端到端延迟，识别模型推理速度与实际视频应用性能之间的差异。

### 11.4 General Algorithm Engineer — English

**Unpaired Image Restoration & Generative Model Research**  
Durham University MSc Dissertation + Post-graduation Research

- **Unpaired Learning & Generative Model Benchmarking:** Constructed mixed-rain and real-raindrop unpaired datasets and adapted, fine-tuned and compared GAN, contrastive-learning and diffusion models, including CycleGAN, CUT, FastCUT, DCLGAN, CycleGAN-Turbo, Stable Diffusion and UNIT-DDPM. Established an evaluation framework using PSNR, SSIM, VIF, FID and FPS.
- **CycleGAN Algorithm Optimization:** Jointly optimized generators, discriminators and training objectives using attention, frequency decomposition, auxiliary rain-region prediction and multi-scale feature fusion. Investigated adversarial, cycle-consistency, perceptual, structural and mask losses, normalization, training stability, data augmentation and domain adaptation. Follow-up experiments achieved approximately +6 dB PSNR, +0.20 SSIM and -10 FID.
- **CycleGAN-Turbo Conditional Generation Optimization:** Introduced rain-intensity conditioning, optimized Skip/Zero-Conv feature pathways, added an auxiliary rain-mask branch and modified the composite loss. Follow-up experiments achieved approximately -10 FID with broadly stable PSNR/SSIM.
- **Video Processing & Performance Analysis:** Applied image restoration models to continuous video through frame-wise inference, comparing model throughput, restoration quality and end-to-end latency to identify practical video-processing bottlenecks.

---

## 12. Open items to verify before final resume submission

| Item | Current status |
|---|---|
| Exact post-graduation CycleGAN test set and checkpoint | To verify |
| Exact post-graduation Turbo test set and checkpoint | To verify |
| Metric implementation and evaluation preprocessing | To verify |
| Exact optimizer / LR / batch size / loss weights | Not present in current record |
| Exact CBAM/SE insertion block indices and channel dimensions | Not present in current record |
| Exact wavelet family / decomposition levels / fusion operator | Not present in current record |
| Exact rain-mask supervision and cycle-loss formulation | Not present in current record |
| Stable Diffusion / UNIT-DDPM detailed modifications and metrics | To recover from experiment records |
| Hardware and end-to-end video latency | To verify |
| Rain2600 vs Rain12600 Table I heading | Source inconsistency; check original test files |

## 13. Repository contents and reproducibility note

The repository currently contains:

- README.md with the research overview, dataset links, model references, trained-model links and benchmark summaries.
- Picture/ with architecture/classification illustrations, qualitative comparisons, video screenshots and metric tables.
- External Google Drive links to trained models and datasets.

The current GitHub tree does **not** contain the model training or inference source code files (for example, train.py or inference.py). Do not describe the repository as a fully reproducible code implementation repository unless those code files are added later.

---

## 14. Key interview positioning

A concise way to describe the project:

> My MSc research focused on unpaired image deraining, where I constructed mixed-rain and raindrop-specific datasets and benchmarked GAN, contrastive-learning and diffusion-based translation models. I then continued the work after graduation by improving CycleGAN's spatial/frequency feature modeling, rain-mask-guided restoration and training objectives, and adapting CycleGAN-Turbo with rain conditioning and optimized latent feature pathways. I also validated frame-wise deraining on video and identified the gap between standalone inference throughput and end-to-end real-time processing.

The most important distinction to communicate is:

- MSc dissertation: dataset construction, literature review, five-model benchmark, metric/speed comparison and frame-wise video validation.
- Post-graduation: additional CycleGAN and CycleGAN-Turbo architecture/training refinements and approximate recalled metric changes.
- Future research: causal temporal video deraining using alignment and ConvGRU/ConvLSTM; not yet implemented.
