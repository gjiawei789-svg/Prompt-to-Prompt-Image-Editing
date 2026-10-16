# 文本驱动的图像风格迁移：通过 Stable Diffusion 模型、Prompt-to-Prompt 与 Null-Text Inversion 实现精确视觉操控

> *Stable Diffusion* Implementation, our method is built on Prompt-to-Prompt and Null-Text
### [Project Page](https://prompt-to-prompt.github.io)&ensp;&ensp;&ensp;[Paper](https://prompt-to-prompt.github.io/ptp_files/Prompt-to-Prompt_preprint.pdf)

##  简介
近年来，随着生成式人工智能技术的快速发展，文本引导的图像生成模型（如 Stable Diffusion）在内容创作、数字艺术和广告设计等领域展现出巨大潜力。然而，如何对已有图像（包括真实照片）进行精确、可控的语义级编辑，仍是当前研究中的关键挑战。传统方法（如基于 GAN 的图像编辑）往往难以在修改风格或内容的同时保留原图的结构细节。例如，将红色苹果图片改为绿色时，可能意外改变其形状、位置甚至背景布局。

本项目聚焦于文本驱动的图像风格迁移，旨在融合 Stable Diffusion、Prompt-to-Prompt（P2P） 和 Null-Text Inversion（NTI） 等前沿技术，构建一个高保真、可编辑且无需微调的图像操控框架。本研究不仅推动生成模型向可控编辑方向发展，也为数字内容创作和个性化图像处理等实际应用提供有效工具。

## 研究目标
我们的目标是在 Stable Diffusion 架构下集成 P2P 与 NTI，实现真实图像的高保真重建与精确风格迁移。

具体目标：

* 实现真实图像的高质量反演，使其能够嵌入扩散模型的潜空间；

* 利用 Prompt-to-Prompt 技术，仅通过修改文本提示即可控制图像的局部或全局风格；

* 通过 Null-Text Inversion 优化重建质量，使编辑后的图像既符合新的风格语义，又严格保留原始结构。

主要工作内容：
* 数据集获取与探索：收集并整理多种类型图像数据，包括来自 COCO val2017 数据集的自然场景图像和自采集真实照片。对各类图像的结构复杂度、语义清晰度和风格多样性进行初步分析。

* 数据预处理与实验环境构建：对图像进行标准化处理（如尺寸统一、格式转换），并搭建基于 Stable Diffusion 的基础实验平台。

* 模型实现与关键技术集成：

* * 实现 Null-Text Inversion（NTI），完成从真实图像到潜在噪声的高保真映射；

集成 Prompt-to-Prompt（P2P） 机制，通过注入源图像的交叉注意力图实现空间布局保持。

* 模型评估与效果对比
结合定量与定性方法评估系统性能：

DINO 特征相似度： 衡量编辑前后物体姿态与布局的一致性（越高越好）；

LPIPS 感知距离： 评估图像细节的自然度与伪影程度（越低越好）；

CLIP 语义对齐分数： 判断生成结果是否符合新提示词语义（如“水墨画”“卡通风格”等，越高越好）。

同时，在不同数据子集上比较编辑效果，验证方法的泛化能力。

## Setup

This code was tested with Python 3.8, [Pytorch](https://pytorch.org/) 1.11 using pre-trained models through [huggingface / diffusers](https://github.com/huggingface/diffusers#readme).
Specifically, we implemented our method over  [Latent Diffusion](https://huggingface.co/CompVis/ldm-text2im-large-256) and  [Stable Diffusion](https://huggingface.co/CompVis/stable-diffusion-v1-4).
Additional required packages are listed in the requirements file.
The code was tested on a RTX3090 24GB but should work on other cards with at least **23GB** VRAM.

You can set up the conda environment (with python3.8+CUDA11.1) using the command below:

``` 
pip install -r requirements.txt
```

## QuickStart

To quickly generate results with **different styles and different extents**, you can use the following command:

``` 
bash bash/exp_styles.sh
``` 
To quickly generate results with **editable style transfer**, you can use the following command:

``` 
bash bash/exp_modify.sh
``` 

Alternatively, you can use the following command to generate results with specific parameters:

``` 
python cartoon_styles.py --description "A handsome man in short hair" --path example_images/portrait5.jpg --blend man --self_ratio 0.4
``` 
The `--description` parameter allows you to provide a description of the image. The `--path` parameter allows you to specify the path of the image. The `--blend` parameter allows you to specify the part of the image that you want to modify. The `--self_ratio` parameter allows you to control the strength of the style, with a range of $0.2$ to $0.8$, where a smaller value results in a stronger style.


## Result

The generated images from the scripts above will be stored in the `\tmp` folder. We also provide some pre-generated results in the `\example_results` folder.

### One Model for All Styles
In these images, we show the results of generating multiple styles using a single model. These styles include different cartoon and art styles. By using a single model, we are able to generate multiple different styles in one run without the need to re-train the model or change parameters.


### Not Only Faces
This image shows that our model is not only able to perform style transfer on faces, but also on other types of images. In this image, we used a landscape image and transformed it into different style. This demonstrates the wide applicability of our model, which can be used for a variety of different types of images.


### Editable Style Transfer
This image shows that our style transfer model is editable. We can select any portion of the transformed image and adjust its style. In this image, we selected a portion of the face and applied a personal modification over it.


### Different Extent
In this image, we show that the strength of the generated style can be controlled. We demonstrate this by generating multiple images with the same source image and style, but with different levels of style intensity. The lefter image has a weaker style, while the righter image has a stronger style. This shows that our model is able to generate a range of different styles with varying levels of intensity.

