# Unpaired-Rain-Removal-Algorithms

Rain removal algorithms that do not rely on paired datasets are classified into two main categories: 

Model-driven method that are independent of datasets and deep learning method that use unpaired datasets. The specific classification is shown below.

![image](https://github.com/engrenage0409/Unpaired-Rain-Removal-Algorithms/blob/main/Picture/1.png)

Synthetic rain streaks removal

![image](https://github.com/engrenage0409/Unpaired-Rain-Removal-Algorithms/blob/main/Picture/2.png)

Raindrop removal

![image](https://github.com/engrenage0409/Unpaired-Rain-Removal-Algorithms/blob/main/Picture/4.png)

Real rain streaks remval

![image](https://github.com/engrenage0409/Unpaired-Rain-Removal-Algorithms/blob/main/Picture/5.png)

Video rain removal

![image](https://github.com/engrenage0409/Unpaired-Rain-Removal-Algorithms/blob/main/Picture/6.png)

Real-time rain removal

![image](https://github.com/engrenage0409/Unpaired-Rain-Removal-Algorithms/blob/main/Picture/7.png)

# Pre-training model

Two Training dataset: 

1.Mix rain streaks and raindrop dataset: 

Includes 402 real raindrop images, 217 real rain streak images and 450 synthetic rain streak images with different rain sizes, totalling 1069 images and 1069 no rain images.

2.Raindrops dataset:

Includes 861 real raindrop images, and 861 no-rain images.

These two datasets are used to train the following models: CycleGAN, CUT, FastCUT, DCLGAN, and CycleGAN-turbo. Model links are provided below.

[Model](https://drive.google.com/drive/folders/1-jQz5AnNsVhR3aSf74chavBh_D7ZKwN4?usp=drive_link)


# Model-driven

# Deep learning
## GAN variants
Cyclegan [[PDF](https://junyanz.github.io/CycleGAN/)] [[Pytorch](https://github.com/junyanz/pytorch-CycleGAN-and-pix2pix?tab=readme-ov-file)] [[Tensorflow
](https://github.com/xiaowei-hu/CycleGAN-tensorflow)]

DerainCylclegan[[PDF](https://arxiv.org/pdf/1912.07015)][[Code](https://github.com/OaDsis/DerainCycleGAN.git )] 

CUT[[PDF](https://arxiv.org/pdf/2007.15651)] [[Code](https://github.com/taesungp/contrastive-unpaired-translation.git )] 

DCLGAN[[PDF](https://arxiv.org/pdf/2104.07689)]  [[Code](https://github.com/JunlinHan/DCLGAN.git)] 

UNIT[[PDF](https://arxiv.org/pdf/1703.00848)]  [[Code](https://github.com/NVlabs/imaginaire.git)] 

MUNIT[[PDF](https://arxiv.org/pdf/1804.04732)]  [[Code](https://github.com/NVlabs/imaginaire.git)] 

FUNIT[[PDF](https://nvlabs.github.io/FUNIT/)]  [[Code](https://github.com/NVlabs/imaginaire.git)] 

Attentiongan[[PDF](https://arxiv.org/pdf/1911.11897)]  [[Code](https://github.com/Ha0Tang/AttentionGAN.git)] 

DiscoGAN[[PDF](https://arxiv.org/pdf/1703.05192)]  [[Code](https://github.com/SKTBrain/DiscoGAN.git)] 

DualGAN[[PDF](https://arxiv.org/pdf/1704.02510)]  [[Code](https://github.com/togheppi/DualGAN.git)] 

Unsupervised Deraining Where Contrastive Learning Meets Self-similarity [[PDF](https://arxiv.org/pdf/2203.11509)]  [[Code](https://github.com/yunguo224/NLCL.git)] 

## Diffusion-Based Methods
CycleGAN-Turbo[[PDF](https://arxiv.org/pdf/2403.12036)]  [[Code](https://github.com/GaParmar/img2img-turbo.git)] 

Dual Diffusion Implicit Bridges [[PDF](https://openreview.net/forum?id=5HLoTvVGDe)]  [[Code](https://github.com/suxuann/ddib.git)] 

Cycle-diffusion[[PDF](https://arxiv.org/pdf/2210.05559)]  [[Code](https://github.com/ChenWu98/cycle-diffusion.git)] 

UNIT-DDPM[[PDF](https://arxiv.org/pdf/2104.05358)]

DDGAN[[PDF](https://arxiv.org/pdf/2112.07804)]  [[Code](https://github.com/NVlabs/denoising-diffusion-gan.git)]

## Other emerging models
UNSB [[PDF](https://arxiv.org/pdf/2305.15086)]  [[Code](https://github.com/cyclomon/UNSB.git)] 

SYDIFF[[PDF](https://arxiv.org/pdf/2306.05178)]  [[Code](https://github.com/KAIST-Visual-AI-Group/SyncDiffusion)] 
