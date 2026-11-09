# Unpaired Image Deraining — Interview Preparation

> This question bank is based on the MSc dissertation, the current GitHub README, and the user's later clarification of post-graduation model refinements. It separates documented benchmark results, user-recalled follow-up results, literature-review knowledge and future research proposals.

## A. Project overview and research motivation

### Q1. What problem does your project solve?

**Suggested answer**

The project studies image deraining under unpaired training conditions. In real-world scenes, it is difficult to obtain exactly aligned rainy and clean images captured from the same viewpoint and under identical lighting. Models trained on synthetic paired rain-streak data may not generalize well to real rain streaks, raindrops, rain haze or complex backgrounds. I therefore constructed unpaired rainy and clean domains, compared representative unpaired generative models, and evaluated both restoration quality and inference efficiency.

**Likely follow-ups**

- Why is rain removal important for autonomous driving?
- What is the difference between rain streaks, raindrops and rain haze?
- Why is a clean-looking output not necessarily a better perception input?

### Q2. What does “unpaired” mean in your datasets?

**Suggested answer**

The training data contains a rainy-image domain and a clean-image domain, but the model does not rely on a one-to-one aligned rainy/clean target for each input. The source datasets may contain rainy and clean samples, but training is performed as unpaired image-to-image translation. The mixed dataset has 1,069 rainy images and 1,069 clean images; this count should not be described as 1,069 supervised aligned pairs.

### Q3. Why did you create two datasets instead of one large mixed dataset?

**Suggested answer**

The mixed dataset combines real raindrops, real rain streaks and synthetic rain streaks, aiming to cover a wider range of rain conditions. The second dataset focuses on real raindrops. Rain streaks and raindrops have different visual patterns and spatial distributions. If a model must learn all types at once, gradients from different restoration tasks can compete and the model may compromise rather than specialize. The dedicated raindrop experiment tests this hypothesis.

### Q4. What exactly was in the mixed dataset?

**Suggested answer**

It contained 402 real raindrop images, 217 real rain-streak images and 450 synthetic rain-streak images. The synthetic portion consisted of 50 Rain100, 40 Rain800, 120 Rain1400 and 240 Rain12600 images. This gives 1,069 rainy images, accompanied by 1,069 rain-free images. The second dataset contained 861 real raindrop images and 861 rain-free images. The dissertation resized images to 256 × 256.

### Q5. How did you evaluate the models?

**Suggested answer**

The dissertation used PSNR, SSIM, VIF and FID, alongside visual comparison and model inference time/FPS. PSNR measures pixel-level reconstruction error; SSIM measures structural similarity; VIF estimates visual information fidelity; FID compares feature distributions between generated and reference images. For the real-raindrop test, the dissertation reports 57 test images. It also used real rainy images and road-surveillance videos for qualitative evaluation.

**Follow-up caution**

The dissertation states a randomly selected set of 500 images from Rain1400 and Rain12600, but does not clearly resolve whether 500 is a combined total or per dataset. Verify the original evaluation script before answering that detail.

---

## B. Model architecture and differences

### Q6. Explain CycleGAN's architecture.

**Suggested answer**

CycleGAN performs unpaired translation between two domains. It has two generators, one mapping rainy images to clean images and another mapping clean images back to rainy images. It also has two discriminators, one for each domain. Adversarial losses encourage realistic target-domain outputs, while cycle-consistency loss encourages the round trip to reconstruct the original image:

\[
F(G(x))\approx x,\qquad G(F(y))\approx y
\]

For deraining, the challenge is to remove rain while preserving scene content. If cycle consistency is weighted too strongly, the generator may become reluctant to make necessary changes to the rain regions.

### Q7. How is CUT different from CycleGAN?

**Suggested answer**

CUT uses one generator and one discriminator instead of CycleGAN's two-generator/two-discriminator cycle. It introduces PatchNCE contrastive learning: features of corresponding local patches in the source and translated images are encouraged to remain similar in representation space. It therefore preserves input content without requiring a paired clean target or a reverse generator.

### Q8. What is FastCUT?

**Suggested answer**

FastCUT is a faster CUT variant. The dissertation describes its key difference as simplifying the PatchNCE training strategy to accelerate training. It retains the contrastive content-preservation idea while reducing training overhead.

### Q9. What does DCLGAN add?

**Suggested answer**

DCLGAN uses dual contrastive learning at both image space and feature space. It tries to improve the correspondence between input and translated images at more than one representation level. In the dissertation benchmark it did not outperform CycleGAN-Turbo and showed weaker restoration on several test conditions.

### Q10. What is CycleGAN-Turbo?

**Suggested answer**

CycleGAN-Turbo combines unpaired image-to-image translation with Stable Diffusion / SD-Turbo-derived latent diffusion components. In a latent-diffusion pipeline, a VAE encoder compresses the image into latent space, a U-Net performs denoising/translation in latent space, and a VAE decoder reconstructs the image. SD-Turbo uses Adversarial Diffusion Distillation to reduce the denoising steps. CycleGAN-Turbo adapts the diffusion components for one-step unpaired image translation and adversarial training, reducing the number of trainable weights compared with a full diffusion pipeline.

### Q11. How does Stable Diffusion work at a high level?

**Suggested answer**

Stable Diffusion is a latent diffusion model. Instead of running the diffusion process directly on full-resolution pixels, a VAE maps images to a lower-dimensional latent representation. A U-Net predicts noise or denoising updates in latent space, and a VAE decoder maps the final latent back to image space. Its quality can be strong, but iterative denoising or the latent-generation pipeline can be expensive for real-time image restoration.

**Project-specific boundary**

The user reports testing/adapting Stable Diffusion for rain removal and observing high restoration quality but slow inference. Exact project-specific architecture modifications and quantitative scores are not currently recorded.

### Q12. What is UNIT-DDPM?

**Suggested answer**

UNIT assumes that two image domains share a latent space and maps images from each domain into that shared representation. DDPM introduces a forward noise process and learns a reverse denoising process. UNIT-DDPM combines the unpaired translation idea with diffusion denoising. The dissertation describes controllable noise injection at different generative steps to model and correct translation errors.

**Project-specific boundary**

The user reports experimenting with UNIT-DDPM and observing high restoration quality but slow inference. Exact metrics and detailed custom modifications are not currently recorded.

### Q13. Which models were actually benchmarked, and which were only reviewed or additionally explored?

**Suggested answer**

The dissertation's primary quantitative benchmark contains five models: CycleGAN, CUT, FastCUT, DCLGAN and CycleGAN-Turbo. The user additionally reports experiments with Stable Diffusion and UNIT-DDPM, but those are not represented in the dissertation's five-model quantitative tables. The related-work section reviews many more methods; literature review should not be presented as model implementation.

---

## C. CycleGAN modifications

### Q14. What limitations did you identify in the original CycleGAN?

**Suggested answer**

The original CycleGAN had difficulty removing heavy rain, rain haze and dense streaks. It could remove rain while also smoothing background details, shifting colors or introducing local artifacts. Complex scenes containing people, vehicles, vegetation and detailed street backgrounds were particularly challenging because rain patterns and background edges can look similar.

### Q15. Why did you add CBAM and SE, and where?

**Suggested answer**

I modified the Generator ResBlocks and inserted CBAM/SE in intermediate feature stages. SE recalibrates channel importance, while CBAM combines channel and spatial attention. I placed them in the middle feature-processing stages to emphasize useful rain-related and scene features without indiscriminately increasing computation across every layer.

**Likely follow-up**

- What is the difference between SE and CBAM?
- Why not add attention to every layer?
- How did you compare the attention variants?

The exact block indices, channel dimensions and ablation values should be recovered from code/logs before giving numeric details.

### Q16. Why introduce a wavelet branch?

**Suggested answer**

Rain streaks contain fine, high-frequency patterns, while broader background structures and illumination are more strongly represented in lower-frequency components. A wavelet branch gives the network an explicit high-/low-frequency decomposition path. The branch is fused with spatial features so the model can distinguish rain patterns from scene content more effectively.

**Important nuance**

High-frequency information also contains genuine edges, text and fine textures. Wavelet decomposition alone cannot identify rain; it must be combined with spatial/contextual features and learned fusion.

### Q17. Why predict a rain mask as well as the derained image?

**Suggested answer**

The Generator has two outputs: the restored image and a rain-region mask. The mask makes the rain-localization objective explicit. The training objective uses mask constraints together with cycle-consistency so the model is encouraged to modify rain-affected regions while preserving unaffected background structure.

**Likely follow-ups**

- How was the mask supervised?
- Was the mask generated from synthetic rain layers, labels or a learned target?
- What exact loss was used?
- How did you prevent the mask from suppressing real background edges?

The exact mask target construction and formula are not available in the current record and must be checked rather than guessed.

### Q18. Why replace ConvTranspose with Upsample + Conv?

**Suggested answer**

Transposed convolution can create checkerboard artifacts because of uneven overlap during upsampling. Resizing first and then applying a standard convolution can reduce this artifact pattern and produce more uniform spatial reconstruction.

### Q19. Why use multi-scale features and skip connections?

**Suggested answer**

Rain removal needs both local detail and broader scene context. Fine rain streaks require high-resolution features, while rain haze and overall color/structure require larger receptive fields. Skip connections and multi-scale feature fusion preserve fine spatial information while allowing deeper layers to model broader context. Pyramid pooling provides additional multi-scale context.

### Q20. Why modify the discriminator?

**Suggested answer**

A PatchGAN discriminator focuses on local patches and texture realism, which is useful for rain streaks but may not sufficiently constrain global color and scene consistency. I therefore explored local PatchGAN together with a global discriminator, as well as multi-scale PatchGAN configurations. The discriminator must be balanced: if it becomes too strong, the generator can become conservative or training can become unstable.

### Q21. Explain the CycleGAN loss design.

**Suggested answer**

The objective combines adversarial loss and cycle-consistency loss with additional deraining-specific constraints:

- VGG perceptual loss to preserve higher-level visual features.
- SSIM/structural loss to protect structure and local contrast.
- Rain-mask constraints to focus changes on rain-affected regions.

The key is balancing the objectives. Excessive cycle-consistency weight can prevent the model from removing rain strongly enough, while excessive adversarial pressure can introduce artifacts or unstable training.

### Q22. What training strategies did you investigate?

**Suggested answer**

I investigated rain-type augmentation for fine streaks, heavy streaks, raindrops and haze; synthetic rain-layer generation; generator warm-up before full adversarial training; WGAN-GP as an alternative adversarial formulation; and pretraining on public deraining datasets followed by target-domain fine-tuning. In the fine-tuning strategy, lower downsampling layers can be frozen to retain general edge/texture features, while intermediate residual blocks and higher-level layers are adapted to the target rain distribution.

**Likely follow-ups**

- What was the gradient-penalty coefficient?
- How long was generator warm-up?
- Which layers were frozen?
- Which normalization variant worked best?

These numerical/configuration details require experiment logs.

### Q23. What normalization methods did you compare?

**Suggested answer**

The experiments considered InstanceNorm, LayerNorm and GroupNorm, as well as adaptive/conditional normalization approaches such as AdaIN. InstanceNorm can normalize instance-level appearance statistics; LayerNorm and GroupNorm offer alternative normalization behavior that may be useful in small-batch settings; conditional normalization can incorporate rain-condition information. The exact winning configuration should be confirmed from experiment records.

---

## D. CycleGAN-Turbo modifications

### Q24. What did you modify in CycleGAN-Turbo?

**Suggested answer**

I added a rain-intensity condition branch to encode conditions such as light rain, heavy rain and rain haze, and injected the condition into U-Net layers. I optimized the Skip/Zero-Conv feature pathways to retain high-frequency details that can be weakened by VAE compression. I also added a lightweight convolutional Rain Mask Head after the main U-Net output and modified the composite loss to include cycle-consistency, perceptual, gradient, structural, rain-attention and lightweight adversarial constraints.

### Q25. Why is the skip / zero-convolution pathway important?

**Suggested answer**

The VAE compresses the input into latent space, which can lose fine details. Skip pathways help preserve information that should bypass or complement the compressed latent representation. Zero-convolution layers provide trainable pathways whose initial behavior can be controlled during adaptation. Optimizing these pathways can help retain high-frequency details and input structure.

### Q26. Did you use LoRA?

**Suggested answer**

LoRA was discussed as a possible parameter-efficient fine-tuning route, but it is not confirmed as an implemented modification in the current project record. I would not claim it as completed work unless the training code or logs verify it.

### Q27. Why did Turbo's PSNR/SSIM barely change while FID improved?

**Suggested answer**

PSNR and SSIM measure pixel-level and structural similarity, whereas FID measures the distance between feature distributions of generated and reference images. A lower FID can indicate that generated outputs are more similar to the reference distribution in feature space without producing a large change in pixel-level reconstruction metrics. It does not automatically prove better temporal consistency or better downstream perception.

---

## E. Results and evaluation

### Q28. What were the main Rain1400 results?

**Suggested answer**

In the dissertation's Table I, CycleGAN-Turbo achieved 26.886 PSNR, 0.886 SSIM, 0.841 VIF and 59.255 FID on Rain1400. CycleGAN achieved 25.512 PSNR, 0.836 SSIM, 0.801 VIF and 93.216 FID. Turbo had the strongest overall quality metrics in this table, but its inference speed was much lower.

### Q29. What did the inference-speed comparison show?

**Suggested answer**

The dissertation reports approximately 0.01 seconds / 100 FPS for CycleGAN, CUT and FastCUT; 0.03 seconds / 33.33 FPS for DCLGAN; and 0.66 seconds / 1.52 FPS for CycleGAN-Turbo. These are model-level inference measurements. They should not be described as sustained end-to-end video FPS.

### Q30. Why did you choose CycleGAN for video validation?

**Suggested answer**

CycleGAN offered a favorable quality-speed trade-off among the tested models. Turbo had better restoration quality in the benchmark but was too slow for the intended real-time workflow. CycleGAN's model-level inference was approximately 100 FPS, making it the more practical candidate for frame-wise video validation.

### Q31. Did the video pipeline actually achieve real-time performance?

**Suggested answer**

No. The model was called independently for each frame, rather than using temporal feature fusion. Although standalone model inference was approximately 100 FPS, the end-to-end video process showed delay and degraded output clarity. The dissertation explicitly identified that this did not constitute true real-time performance.

### Q32. What was the most important finding from the mixed-rain versus raindrop-specific experiments?

**Suggested answer**

The raindrop-specific dataset improved most models' raindrop-removal metrics compared with mixed-rain training. This supports the idea that different rain types create competing feature-learning demands. However, it is not universal: DCLGAN did not improve on every metric, so the conclusion must be model- and metric-specific.

### Q33. What are the main limitations of the original research?

**Suggested answer**

The benchmark models struggled with dense rain and complex backgrounds, especially where rain and background edges overlap. Mixed rain types could conflict during training. The fastest image models did not automatically produce a truly real-time video pipeline, and Turbo's quality advantage came with a large inference cost. The project also did not implement temporal recurrence or motion-aware alignment.

---

## F. Video deraining and future research

### Q34. Why is frame-wise inference insufficient for video deraining?

**Suggested answer**

Each frame is processed independently, so the model does not reuse information from previous frames. It cannot exploit temporal redundancy, stabilize rain removal across frames or reduce flicker through motion-aware information sharing. The same scene content may be restored slightly differently in adjacent frames.

### Q35. How would you extend CycleGAN to video deraining?

**Suggested answer**

I would retain the improved CycleGAN as the spatial backbone, extract lower-resolution feature maps, and add a causal temporal module such as ConvGRU or ConvLSTM. If motion is significant, I would add lightweight feature alignment before temporal fusion. The decoder would reconstruct the current frame, and a motion-aware temporal consistency loss would reduce flicker while masking occlusions and unreliable correspondences.

### Q36. Why not simply concatenate several RGB frames?

**Suggested answer**

Concatenating RGB frames is a simple baseline, but moving objects and camera motion mean that the same scene point may appear at different spatial locations. Direct concatenation can mix misaligned features and produce ghosting. Feature alignment or deformable sampling can help align corresponding content before fusion.

### Q37. ConvGRU or ConvLSTM—which would you choose?

**Suggested answer**

I would benchmark both. ConvLSTM maintains separate cell and hidden states and offers richer memory control, while ConvGRU uses a simpler gated structure and can be lighter. For a real-time causal pipeline, I would begin with ConvGRU at a low-resolution encoder feature stage, then compare quality, memory and latency against ConvLSTM.

### Q38. How would you extend CycleGAN-Turbo to video?

**Suggested answer**

I would explore latent-space temporal fusion while retaining the rain-condition branch, optimized skip/zero-conv pathways and mask head. However, Turbo's original benchmark speed was approximately 1.5 FPS, so temporal fusion alone cannot solve its latency problem. I would first profile and optimize the one-step inference pipeline, then consider mixed precision, model compilation and student-model distillation.

### Q39. What temporal loss would you use?

**Suggested answer**

A motion-aware temporal consistency loss can compare the current restored frame with the previous restored frame warped into the current coordinate system. A visibility/reliability mask should exclude occluded, newly revealed or poorly aligned regions. Otherwise, the loss may create ghosting or over-smooth moving objects.

### Q40. How would you prove the video model is truly real-time?

**Suggested answer**

I would measure the complete pipeline, not just neural-network inference: video decoding, preprocessing, host/device transfer, alignment, temporal fusion, model inference, postprocessing and encoding/display. I would report sustained FPS, P50/P95 latency, memory usage and resolution. I would also evaluate temporal flicker and downstream detection/tracking performance.

---

## G. Research rigor and ownership

### Q41. Were the base architectures implemented from scratch?

**Suggested answer**

No. The work used open-source implementations as the starting point. My contribution was adapting and fine-tuning them for deraining, modifying architecture components and training objectives, constructing datasets, designing experiments, evaluating results and comparing their practical trade-offs.

### Q42. Which results belong to the MSc dissertation and which are post-graduation?

**Suggested answer**

The dissertation includes the two unpaired datasets, five-model benchmark, 100/200/400-epoch experiments, quantitative tables and frame-wise video validation. The later CBAM/SE, wavelet, mask, multi-scale, discriminator, normalization and training refinements to CycleGAN, as well as rain conditioning, skip/zero-conv, mask-head and composite-loss refinements to Turbo, were performed after graduation. Their approximate metric changes are recalled values and should be supported with the original logs before formal publication.

### Q43. Why should an interviewer trust the post-graduation metric improvements?

**Suggested answer**

I would present the test-set definition, baseline checkpoint, modified checkpoint, preprocessing, metric implementation and evaluation script. If any of those records are missing, I would clearly label the values as approximate experimental recollections rather than present them as a fully reproducible result. The correct next step is to recover the logs and rerun both models under the same evaluation protocol.

### Q44. What is currently available in the GitHub repository?

**Suggested answer**

The current repository contains the README, visual comparison images, benchmark tables, model/dataset references and external links to trained models. The current GitHub tree does not contain the model training or inference source code. I would not describe it as a fully reproducible implementation repository unless the code is added.

---

## H. Questions to prepare with original code/logs

These are high-probability technical follow-ups for which the current document does not contain enough implementation evidence. Recover the actual values instead of guessing.

1. Which exact Generator ResBlocks contain CBAM and SE? Are both used together or compared as alternatives?
2. Which wavelet family was used (e.g. Haar or another basis)? How many decomposition levels? How are high/low-frequency features fused?
3. How is the Rain Mask Head supervised? What are the mask targets and loss equations?
4. How exactly is the mask incorporated into cycle consistency?
5. What are the dimensions and fusion operations for skip connections, multi-scale concatenation and pyramid pooling?
6. Is the discriminator implemented as two independent discriminators, a multi-scale PatchGAN, or both in separate experiments?
7. Which normalization configuration produced the best result?
8. What were the exact weights for adversarial, cycle, perceptual, SSIM/structural and mask losses?
9. What WGAN-GP configuration and gradient-penalty coefficient were used?
10. How long was generator warm-up, and what triggered the adversarial-training stage?
11. Which layers were frozen during domain fine-tuning?
12. What exact rain-intensity encoding was used in CycleGAN-Turbo, and at which U-Net blocks was it injected?
13. Which Skip/Zero-Conv layers were changed, and how were their weights initialized?
14. What was the exact Turbo composite-loss equation and its weights?
15. What hardware, input size, batch size and warm-up procedure produced the 100 FPS and 1.52 FPS measurements?
16. What are the exact test-set/checkpoint/preprocessing details behind the post-graduation +6 dB PSNR, +0.20 SSIM and -10 FID recollections?

## 15. Short interview closing statement

> The key contribution was not simply applying existing generative models to rain removal. I built two unpaired training datasets, established a comparative benchmark across GAN, contrastive-learning and diffusion-based methods, and then continued improving two distinct architectures after graduation. For CycleGAN, I focused on spatial/frequency feature modeling, rain-mask-guided restoration and training objectives. For CycleGAN-Turbo, I focused on rain-conditioned latent generation, skip-path detail preservation and auxiliary mask prediction. Finally, frame-wise video validation exposed the gap between image-model inference speed and true real-time video processing, which motivates the next stage of temporal modeling.
