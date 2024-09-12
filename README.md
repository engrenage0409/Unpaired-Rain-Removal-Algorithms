# Benchmarking Unpaired Rain Removal Algorithms: From Generative Adversarial Network to Diffusion Models

I systematically review current unpaired rain removal algorithms and randomly selected images from synthetic datasets such as Rain100, Rain800, Rain1400, and Rain12600, as well as real rain streak and raindrop datasets, to form two unpaired datasets: a dataset containing only raindrops and a mixed dataset containing rain streaks and raindrops to evaluate some of these algorithms based on visual quality and quantitative metrics. 

**I summarized links to the relevant papers and code for current unpaired rain removal algorithms, along with the datasets I used and the models I trained after different training epochs. Detailed links, visual comparisons, and quantitative metrics can be found in the README.md file.**

---

Rain removal algorithms that do not rely on paired datasets are classified into two main categories: 

Model-driven method that are independent of datasets and deep learning method that use unpaired datasets. 

The specific classification is shown below.

![image](https://github.com/engrenage0409/Unpaired-Rain-Removal-Algorithms/blob/main/Picture/1.png)

Synthetic rain streaks removal:
<p align="center">
    <img src="https://github.com/engrenage0409/Unpaired-Rain-Removal-Algorithms/blob/main/Picture/2.png" alt="Synthetic rain streaks removal
" width="1000">
</p>

Raindrop removal:
<p align="center">
    <img src="https://github.com/engrenage0409/Unpaired-Rain-Removal-Algorithms/blob/main/Picture/4.png" alt="Synthetic rain streaks removal
" width="1000">
</p>

Real rain streaks remval:
<p align="center">
    <img src="https://github.com/engrenage0409/Unpaired-Rain-Removal-Algorithms/blob/main/Picture/5.png" alt="Synthetic rain streaks removal
" width="1000">
</p>

Video rain removal:
<p align="center">
    <img src="https://github.com/engrenage0409/Unpaired-Rain-Removal-Algorithms/blob/main/Picture/6.png" alt="Synthetic rain streaks removal
" width="600">
</p>

Real-time rain removal:
<p align="center">
    <img src="https://github.com/engrenage0409/Unpaired-Rain-Removal-Algorithms/blob/main/Picture/7.png" alt="Synthetic rain streaks removal
" width="600">
</p>

Quantitative metrics:

<p align="center">
    <img src="https://github.com/engrenage0409/Unpaired-Rain-Removal-Algorithms/blob/main/Picture/table1.png" alt="Synthetic rain streaks removal
" width="600">
</p>

<p align="center">
    <img src="https://github.com/engrenage0409/Unpaired-Rain-Removal-Algorithms/blob/main/Picture/table2.png" alt="Synthetic rain streaks removal
" width="600">
</p>

<p align="center">
    <img src="https://github.com/engrenage0409/Unpaired-Rain-Removal-Algorithms/blob/main/Picture/table3.png" alt="Synthetic rain streaks removal
" width="600">
</p>

# Datasets

We form two Training dataset: 

1.Mix rain streaks and raindrop dataset: 

Includes 402 real raindrop images from a real raindrop dataset, 217 real rain streak images from a real rain streak dataset and 450 synthetic rain streak images with different rain sizes. Specifically, it contains 50 images from Rain100, 40 images from Rain800, 120 images from Rain1400, and 240 images from Rain12600. These images represent varying rain conditions including light rain, medium rain, and heavy rain for different scenes, with approximately two images of each rain condition type per scene. There are a total of 1069 images and 1069 corresponding rain-free images.

2.Raindrops dataset:

Includes 861 real raindrop images, and 861 no-rain images from real raindrop dataset .

Here is the specific download link:

Mix rain streaks and raindrop dataset[[Google Drive]](https://drive.google.com/file/d/1PpskLaEGP-2kL4q8dd7nbjLZ03v16sIc/view?usp=drive_link)

Raindrops dataset[[Paper]](https://openaccess.thecvf.com/content_cvpr_2018/papers/Qian_Attentive_Generative_Adversarial_CVPR_2018_paper.pdf)[[Google Drive]](https://drive.google.com/open?id=1e7R76s6vwUJxILOcAsthgDLPSnOrQ49K)

Rain100[[Paper]](https://arxiv.org/pdf/1609.07769)[[Baidu Netdisk]](https://pan.baidu.com/s/1J0q6Mrno9aMCsaWZUtmbkg#list/path=%2F)

Rain800[[Paper]](https://arxiv.org/pdf/1701.05957)[[Google Drive]](https://drive.google.com/file/d/1cMXWICiblTsRl1zjN8FizF5hXOpVOJz4/view)

Rain1400, Rain12600[[Paper]](https://openaccess.thecvf.com/content_cvpr_2017/papers/Fu_Removing_Rain_From_CVPR_2017_paper.pdf)[[Baidu Netdisk]](https://pan.baidu.com/s/1J0q6Mrno9aMCsaWZUtmbkg#list/path=%2F)

Real rain streak[[Paper]](https://openaccess.thecvf.com/content_CVPR_2019/html/Li_Single_Image_Deraining_A_Comprehensive_Benchmark_Analysis_CVPR_2019_paper.html)[[Access Code: npsy]](https://pan.baidu.com/share/init?surl=XctM1xT9KKq3JU_OXPJiLg)


# Trained Models
Two datasets are used to train the following models: CycleGAN, CUT, FastCUT, DCLGAN, and CycleGAN-turbo, with each model being trained for 100 epochs, 200 epochs, and 400 epochs respectively. Trained models links are provided below.

[[Models]](https://drive.google.com/drive/folders/1-jQz5AnNsVhR3aSf74chavBh_D7ZKwN4?usp=drive_link)


# Model-driven
Specific details can be found in the model-driven section of the following link. [[Model-driven]](https://github.com/hongwang01/Video-and-Single-Image-Deraining.git)

# Deep Learning
## GAN Variants
* Cyclegan [[PDF](https://junyanz.github.io/CycleGAN/)] [[Pytorch](https://github.com/junyanz/pytorch-CycleGAN-and-pix2pix?tab=readme-ov-file)] [[Tensorflow
](https://github.com/xiaowei-hu/CycleGAN-tensorflow)]

* DerainCylclegan[[PDF](https://arxiv.org/pdf/1912.07015)][[Code](https://github.com/OaDsis/DerainCycleGAN.git )] 

* CUT[[PDF](https://arxiv.org/pdf/2007.15651)] [[Code](https://github.com/taesungp/contrastive-unpaired-translation.git )] 

* DCLGAN[[PDF](https://arxiv.org/pdf/2104.07689)]  [[Code](https://github.com/JunlinHan/DCLGAN.git)] 

* UNIT[[PDF](https://arxiv.org/pdf/1703.00848)]  [[Code](https://github.com/NVlabs/imaginaire.git)] 

* MUNIT[[PDF](https://arxiv.org/pdf/1804.04732)]  [[Code](https://github.com/NVlabs/imaginaire.git)] 

* FUNIT[[PDF](https://nvlabs.github.io/FUNIT/)]  [[Code](https://github.com/NVlabs/imaginaire.git)] 

* Attentiongan[[PDF](https://arxiv.org/pdf/1911.11897)]  [[Code](https://github.com/Ha0Tang/AttentionGAN.git)] 

* DiscoGAN[[PDF](https://arxiv.org/pdf/1703.05192)]  [[Code](https://github.com/SKTBrain/DiscoGAN.git)] 

* DualGAN[[PDF](https://arxiv.org/pdf/1704.02510)]  [[Code](https://github.com/togheppi/DualGAN.git)] 

* Unsupervised Deraining Where Contrastive Learning Meets Self-similarity [[PDF](https://arxiv.org/pdf/2203.11509)]  [[Code](https://github.com/yunguo224/NLCL.git)] 

## Diffusion-Based Methods
* CycleGAN-Turbo[[PDF](https://arxiv.org/pdf/2403.12036)]  [[Code](https://github.com/GaParmar/img2img-turbo.git)] 

* Dual Diffusion Implicit Bridges [[PDF](https://openreview.net/forum?id=5HLoTvVGDe)]  [[Code](https://github.com/suxuann/ddib.git)] 

* Cycle-diffusion[[PDF](https://arxiv.org/pdf/2210.05559)]  [[Code](https://github.com/ChenWu98/cycle-diffusion.git)] 

* UNIT-DDPM[[PDF](https://arxiv.org/pdf/2104.05358)]

* DDGAN[[PDF](https://arxiv.org/pdf/2112.07804)]  [[Code](https://github.com/NVlabs/denoising-diffusion-gan.git)]

* SYDIFF[[PDF](https://arxiv.org/pdf/2306.05178)]  [[Code](https://github.com/KAIST-Visual-AI-Group/SyncDiffusion)] 

## Other Emerging Models
* UNSB [[PDF](https://arxiv.org/pdf/2305.15086)]  [[Code](https://github.com/cyclomon/UNSB.git)] 

* Unpaired Image-to-Image Translation with Shortest Path Regularization[[PDF](https://openaccess.thecvf.com/content/CVPR2023/papers/Xie_Unpaired_Image-to-Image_Translation_With_Shortest_Path_Regularization_CVPR_2023_paper.pdf)]  [[Code](https://github.com/Mid-Push/santa.git)] 


# Contact
If you have any question, please feel free to concat Guolei Song (Email: kirsch0409lei@gmail.com).
