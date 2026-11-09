# Resume Project Entries — Unpaired Image Deraining

This file contains resume-ready Chinese and English versions. Use the CV version for Computer Vision roles and the general algorithm version for broader algorithm roles. Do not use this project as a primary Agent project. Its relevance to VLM roles is secondary rather than central.

## 1. CV Algorithm Engineer — Chinese

### 无配对图像除雨与生成模型架构优化
**Durham University MSc Dissertation + Post-graduation Research**

- **无配对数据集构建与多模型 Benchmark：** 基于 Rain100、Rain800、Rain1400、Rain12600 及真实雨丝、雨滴数据集，构建混合雨型（1,069 张雨图及 1,069 张无雨图）和纯真实雨滴（861 张雨图及 861 张无雨图）两套无配对数据集；基于开源实现适配、微调并对比 CycleGAN、CUT、FastCUT、DCLGAN、CycleGAN-Turbo、Stable Diffusion、UNIT-DDPM 等生成模型，采用 PSNR、SSIM、VIF、FID 和 FPS 评估恢复质量与计算效率。
- **CycleGAN 架构与训练优化：** 改进 Generator ResBlock，引入 CBAM/SE 注意力、小波高低频分支及辅助 Rain Mask Head；优化上采样、多尺度特征融合、Skip Connection、PatchGAN 局部/全局判别策略，并联合调整对抗、循环一致性、感知、结构及掩码损失，开展归一化、分阶段训练、WGAN-GP、雨型增强和领域微调实验。毕业后持续优化实验中，PSNR 约提升 6 dB、SSIM 约提升 0.20、FID 约降低 10。
- **CycleGAN-Turbo 条件扩散优化：** 增加雨强度条件分支，优化 Skip/Zero-Conv 特征通路，增加轻量 Rain Mask Head 并调整复合损失；后续实验中 FID 约降低 10，PSNR/SSIM 基本稳定。
- **视频除雨验证：** 采用逐帧调用方式处理连续视频，选择 CycleGAN 开展视频测试；模型级推理约 100 FPS，但端到端处理仍存在延迟和输出清晰度下降，分析单帧推理与真实视频处理之间的性能差距。

## 2. CV Algorithm Engineer — English

### Unpaired Image Deraining & Generative Model Architecture Optimization
**Durham University MSc Dissertation + Post-graduation Research**

- **Unpaired Dataset Construction & Benchmarking:** Constructed mixed-rain (1,069 rainy and 1,069 clean images) and real-raindrop-focused (861 rainy and 861 clean images) unpaired datasets from Rain100, Rain800, Rain1400, Rain12600 and real-world sources. Adapted and fine-tuned open-source CycleGAN, CUT, FastCUT, DCLGAN, CycleGAN-Turbo, Stable Diffusion and UNIT-DDPM models, evaluating restoration quality and efficiency using PSNR, SSIM, VIF, FID and FPS.
- **CycleGAN Architecture & Training Optimization:** Enhanced Generator ResBlocks with CBAM/SE attention, wavelet-based high-/low-frequency features and an auxiliary Rain Mask Head. Optimized upsampling, multi-scale feature fusion, skip connections and local/global PatchGAN discrimination; jointly refined adversarial, cycle-consistency, perceptual, structural and mask objectives, alongside normalization, staged training, WGAN-GP, rain augmentation and domain fine-tuning. Follow-up post-graduation experiments achieved approximately +6 dB PSNR, +0.20 SSIM and -10 FID.
- **CycleGAN-Turbo Conditional Diffusion Optimization:** Added rain-intensity conditioning, optimized Skip/Zero-Conv pathways, introduced a lightweight Rain Mask Head and modified the composite loss. Follow-up experiments achieved approximately -10 FID with broadly stable PSNR/SSIM.
- **Video Deraining Validation:** Applied frame-wise inference to continuous video sequences and selected CycleGAN for video testing. Model-level inference reached approximately 100 FPS, while end-to-end processing still showed latency and output-clarity degradation.

## 3. General Algorithm Engineer — Chinese

### 无配对图像恢复与生成模型算法研究
**Durham University MSc Dissertation + Post-graduation Research**

- **无配对学习与生成模型 Benchmark：** 构建混合雨型与真实雨滴两套无配对数据集，对 GAN、Contrastive Learning 和 Diffusion 生成范式开展模型适配、微调与比较，覆盖 CycleGAN、CUT、FastCUT、DCLGAN、CycleGAN-Turbo、Stable Diffusion 和 UNIT-DDPM；建立基于 PSNR、SSIM、VIF、FID 和 FPS 的综合评估体系。
- **CycleGAN 算法优化：** 联合优化生成器、判别器与训练目标，引入注意力、频域分解、雨区辅助预测和多尺度特征融合，结合对抗、循环一致性、感知、结构和掩码损失，并研究归一化、训练稳定性、数据增强及领域微调。后续实验中 PSNR 约提升 6 dB、SSIM 约提升 0.20、FID 约降低 10。
- **CycleGAN-Turbo 条件生成优化：** 增加雨强度条件编码，优化 Skip/Zero-Conv 特征传递和辅助雨痕预测分支，调整复合损失；后续实验中 FID 约降低 10，PSNR/SSIM 基本稳定。
- **视频处理与性能分析：** 将图像恢复模型接入连续视频逐帧处理流程，对比模型级吞吐量、恢复质量与端到端延迟，识别模型推理速度与实际视频应用性能之间的差异。

## 4. General Algorithm Engineer — English

### Unpaired Image Restoration & Generative Model Research
**Durham University MSc Dissertation + Post-graduation Research**

- **Unpaired Learning & Generative Model Benchmarking:** Constructed mixed-rain and real-raindrop unpaired datasets and adapted, fine-tuned and compared GAN, contrastive-learning and diffusion models, including CycleGAN, CUT, FastCUT, DCLGAN, CycleGAN-Turbo, Stable Diffusion and UNIT-DDPM. Established an evaluation framework using PSNR, SSIM, VIF, FID and FPS.
- **CycleGAN Algorithm Optimization:** Jointly optimized generators, discriminators and training objectives using attention, frequency decomposition, auxiliary rain-region prediction and multi-scale feature fusion. Investigated adversarial, cycle-consistency, perceptual, structural and mask losses, normalization, training stability, data augmentation and domain adaptation. Follow-up experiments achieved approximately +6 dB PSNR, +0.20 SSIM and -10 FID.
- **CycleGAN-Turbo Conditional Generation Optimization:** Introduced rain-intensity conditioning, optimized Skip/Zero-Conv feature pathways, added an auxiliary rain-mask branch and modified the composite loss. Follow-up experiments achieved approximately -10 FID with broadly stable PSNR/SSIM.
- **Video Processing & Performance Analysis:** Applied image restoration models to continuous video through frame-wise inference, comparing model throughput, restoration quality and end-to-end latency to identify practical video-processing bottlenecks.

## 5. Resume usage notes

- CV version: recommended as the primary academic project for computer-vision roles.
- General algorithm version: emphasize experimental design, generative-model adaptation, architecture optimization and metric-driven evaluation.
- VLM version: do not list as a standalone VLM project; generative vision experience can be mentioned selectively if space allows.
- Agent version: omit.
- The dissertation's five-model quantitative benchmark and post-graduation refinements are separate phases. Keep this distinction in interviews.
- The post-graduation metric changes (+6 dB PSNR, +0.20 SSIM, approximately -10 FID for CycleGAN; approximately -10 FID for Turbo) are recalled values pending verification from experiment logs.
- Frame-wise video inference is not temporal video modeling. Do not claim ConvLSTM/ConvGRU, temporal alignment or temporal consistency training as completed work.
