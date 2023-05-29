---
title: 'Bilateral Neural Operators'
subtitle: 'Learning image retouching process using sequential neural operators in bilateral space.'
date: 2023-05-17 00:00:00
description: 
featured_image: '/images/bno/color-9.jpg'
---

![](/images/bno/demo-top.png)

### Problem Statement
**Image enhancement** is the process of making changes or improvements to a photograph to enhance its appearance. The inputs are usually low-quality or raw images (e.g. low-light, color-distorted or unprocessed raw sensor data), and preferably outputs a visually pleasing high-quality image for display. Most of the essential steps when human photographer enhances the image includes exposure correction, color style adjustment and white balance correction, etc.

Input Image      |  Result
:-------------------------:|:-------------------------:
![](/images/bno/demo-1-in.png)   |  ![](/images/bno/demo-1-out.png)
![](/images/bno/demo-2-in.png)   |  ![](/images/bno/demo-2-out.png)

<!-- <figure style="width:70%">
  <img src="/images/bno/demo-1.png" alt="demo-1"/>
  <center>
  <figcaption style="margin-top:5px">Input Image | Output Image </figcaption>
  </center>
</figure>

<figure style="width:70%">
  <img src="/images/bno/demo-2.png" alt="demo-2"/>
  <center>
  <figcaption style="margin-top:5px">Input Image | Output Image </figcaption>
  </center>
</figure> -->

Image enhancement is not only useful for photographic purposes, but also helps down-stream computer vision tasks like object detection and face recognition.

### Existing Methods
There are numerous existing methods ranging from traditional image processing techniques like histogram equalization, and more recent approaches based on deep learning. In this context, our emphasis lies on the deep learning methods due to their superior capabilities and broader applicability.

<details markdown="1"><summary>Bilateral-Grid Based Method</summary>

<a name="HDRNetTag"></a>

Bilateral grid comes from the paper [HDRNet](https://groups.csail.mit.edu/graphics/hdrnet/). It is a data structure that represents an image transformation. Comparing to processing full resolution image, processing in bilateral space is **faster** and **edge-aware**. A bilateral grid is predicted using downsampled image (thus faster), and final output is [sliced](https://people.csail.mit.edu/sparis/publi/2007/siggraph/Chen_07_Bilateral_Grid.pdf) from the predicted grid using the full-res image, without the need to feed the full-res image into neural network. Because of this, it consumes less memory space compared to other methods processing using full resolution image.

</details>

<details markdown="1"><summary>GAN Based Method</summary>

GAN-based approaches like [Pix2Pix](https://phillipi.github.io/pix2pix/) are indeed highly effective for tasks such as image generation and style transfer. However, one limitation is their tendency to produce images with visible artifacts, resulting in a lack of visual smoothness. This drawback makes them generally unsuitable for applications in photography where high-quality results are required. Furthermore, generative models, including Pix2Pix, often have large model sizes that hinder real-time performance, making them less practical for certain real-time applications.

</details>

<details markdown="1"><summary>3D-LUT Based Method</summary>

LUT-based approaches, such as [AdaInt](https://arxiv.org/pdf/2204.13983.pdf) and [3D-LUT](https://arxiv.org/pdf/2009.14468.pdf), provide a fast and visually smooth method for image enhancement. These approaches utilize 3D Look-up Tables (LUTs) to efficiently transfer colors from one color space to another. The learned LUTs serve as a compact representation of color transfer, similar to bilateral grids. However, one limitation of 3D LUT-based methods is their lack of interpretability regarding how the model modifies the photo. Additionally, these methods often have relatively larger model sizes compared to Neural Operators.

</details>

<details markdown="1"><summary>Sequential Retouching Method</summary>

<a name="NeuralOpsTag"></a>

[Neural Operator](https://arxiv.org/pdf/2207.08080.pdf) is an innovative approach for image enhancement that aims to mimic the sequential processing performed by human photographers. By learning to apply operators in a sequential manner, the model gains interpretability, allowing users to understand the image processing steps. The trend of performing image restoration tasks in a stage-by-stage fashion has been observed in recent papers, including works like [HINet](https://openaccess.thecvf.com/content/CVPR2021W/NTIRE/papers/Chen_HINet_Half_Instance_Normalization_Network_for_Image_Restoration_CVPRW_2021_paper.pdf) and [MAXIM](https://openaccess.thecvf.com/content/CVPR2022/papers/Tu_MAXIM_Multi-Axis_MLP_for_Image_Processing_CVPR_2022_paper.pdf). These approaches adopt a multi-stage architecture, where each stage focuses on addressing specific aspects of the restoration process. However, it's worth noting that the original Neural Operator implementation may encounter runtime performance issues when dealing with large input images. Processing larger images could potentially result in slower execution times or resource limitations.

</details>

### Proposed Method
Rather than relying on a black-box image transformation approach, adopting a more interpretable solution similar to the [Neural Operator](#NeuralOpsTag) is a viable approach for tackling the image restoration task. By applying sequential operations to the input image, the restoration process can be broken down into stages, allowing for better interpretability and control over the transformation. To mimic how human photographers edit photos, the intermediate operators can be constrained to approximate common photographic operations such as **exposure correction**, **black clipping**, **vibrance adjustment**, **white balance adjustment**, and more. This approach not only provides a clearer understanding of how the image is being modified but also allows for fine-grained adjustments to achieve the desired restoration outcome.

The original Neural Operator method has shown impressive results on the MIT-Adobe 5K dataset while maintaining a relatively small model size. However, its model architecture directly operates on the full-resolution image, leading to a constraint on the input image size. This limitation becomes particularly challenging when dealing with high-resolution inputs. (e.g. 4K)

<figure style="width:70%">
  <img src="/images/bno/neural_ops_arch.jpg" alt="original neural op arch"/>
  <center>
  <figcaption style="margin-top:5px">Model Architecture of Neural Operators</figcaption>
  </center>
</figure>

To address the challenge of processing high-resolution images in the original Neural Operator method, one possible approach is to replace each neural operator with a modified lightweight [**HDRNet**](#HDRNetTag). This alternative method avoids processing the full-resolution image "in network." Instead, it initially operates on a significantly down-sampled version of the image, reducing the computational burden. The full-resolution image is only processed when applying the color transformation ([slicing]((https://people.csail.mit.edu/sparis/publi/2007/siggraph/Chen_07_Bilateral_Grid.pdf))), which is a relatively low-cost operation and less dependent on the input resolution. By adopting this strategy, the computational and memory requirements for processing high-resolution images can be alleviated, making the method more practical and efficient.

A more detailed description of propsed model architecture that combines both the advantages of Neural Ops and HDRNet can be found [here](#model-architecture).

<figure style="width:90%">
  <img src="/images/bno/bilateral_ops_arch.jpg" alt="bilteral neural op arch"/>
  <center>
  <figcaption style="margin-top:5px">Model Architecture of Bilateral Neural Operators</figcaption>
  </center>
</figure>

### Dataset
A commonly used dataset for evaluating image enhancement process is [MIT-Adobe-5K](https://data.csail.mit.edu/graphics/fivek/). This dataset consists of 5,000 raw images in 16-bit TIFF format. In order to make a fair comparison with the Neural Ops paper, we decided to use the exact processed dataset they used, which was originally provided by [CSRNet](https://github.com/hejingwenhejingwen/CSRNet) (referred to as FiveK-Dark).

For training set, the Neural Ops paper selected 4,500 images from the dataset, while the remaining 500 images were reserved for testing. All the images in the dataset were cropped to a resolution of 500x300 or 300x500, depending on the orientation of the image. The training images are in the format of 16-bit TIFF raw files, while the target images are in 8-bit sRGB JPEG format.

Additionally, we intend to evaluate our models on low-light image datasets such as [LOL](https://daooshee.github.io/BMVC2018website/) and [VE-LOL](https://flyywh.github.io/IJCV2021LowLight_VELOL/). In these datasets, the input images are captured under low-light conditions, while the corresponding target images are high-quality images with correct exposure.

### Results (Not Finalized)

Based on the results of Neural Operator paper, the proposed method achieves similar **PSNR**/**SSIM**/**ΔE** score, with a runtime faster than Neural Operator when input resolution is large (like in real scenarios).

|    Model            | PSNR    | SSIM     | ΔE    | LPIPS | #params      |
|---------------------|---------|----------|-------|--------------|--------------|
| White-Box           |  18.59    |  0.797     | 17.42   | - |  8,561,762   |
| Distort & Recover   |  19.54    |  0.800     | 15.44   | - | 259,263,320 |
| HDRNet              |  22.65    |  0.880     | 11.83   | - | 482,080     |
| DUPE                |  20.22    |  0.829     | 16.63   | - | 998,816     |
| MIRNet              |  19.37    |  0.806     | 16.51   | - | 31,787,419  |
| Pix2Pix             |  21.41    |  0.749     | 13.26   | - | 11,383,427  |
| 3D-LUT              |  23.12    |  0.874     | 11.26   | - | 593,516     |
| CSRNet              |  23.86    |  0.897     | 10.57   | - | 36,489      |
| NeuralOp (from paper)     |  **24.32**    |  **0.907**     | **9.795**   |  0.045 | **28,108**   |
| NeuralOp (reproduced)     |  24.04    |  0.898     | 10.333   |  0.048 | **28,108**   |
| **BilateralOp (ours)**  |  24.22    |  0.906       |  9.830    | **0.043**     |  69,012      |

<figcaption style="text-align: center;">Table1. Metrics compared on FiveK Dark Dataset.</figcaption>

<figcaption style="text-align: center;font-size: 50%;margin-top:3px;">(- means not measured)</figcaption>

I cannot reproduce the exact same results using the dataset Neural Ops authors provided (because the author only provides partial training dataset), but I did try to reproduce the best I can using the dataset I generated. Using our training data, we're able to achieve similar results for our model, and also runs faster and consumes less memory space.

I also measured runtime performance of NeuralOps and BilateralOps (ours) along with its CUDA memory usage.

|  Model/Resolution   | 500x300 | 1200x900 | 2500x1200 | 4000x3000 | 
|---------------------|---------|----------|-----------|-----------|
| NeuralOp            |  8.26 ms (121 FPS) <br/> **262 MB**   |  44.41 ms (22 FPS) <br/> **1.61 GB**    |  121.14 ms (8 FPS) <br/> **4.43 GB**  |  N/A <br/> **N/A**  |
| **BilateralOp (ours)**  |  6.78 ms (147 FPS) <br/> **55 MB**   | 32.86 ms (30 FPS) <br/> **266 MB**    | 87.05 ms (11 FPS) <br/> **702 MB**    | 350.22 ms (2 FPS) <br/> **2.70 GB**  |

<figcaption style="text-align: center;">Table2. Runtime/Memory compared on different input resolution.</figcaption>

<figcaption style="text-align: center;font-size: 50%;margin-top:3px;">(N/A means model runs out-of-memory for given input resolution, time is measured per inference)</figcaption>

We can see from the table that our method is faster than the Neural Op, and it is able to handle large input resolution and consumes relatively smaller memory space.

### Visual Comparison
Images are arranged from left to right as: `Input`, `Output`, `Target`, `Diff`. `Diff` is calculated from absolute error between `Output` and `Target`.  
###### Scene 1
<figure style="width:70%">
  <img src="/images/bno/color-5.jpg" alt="color-5"/>
  <center>
  <figcaption style="margin-top:5px">Our Result</figcaption>
  </center>
</figure>

<figure style="width:70%">
  <img src="/images/bno/neurop-5.jpg" alt="neurop-5"/>
  <center>
  <figcaption style="margin-top:5px">Original Neural Ops Result</figcaption>
  </center>
</figure>

###### Scene 2
<figure style="width:70%">
  <img src="/images/bno/color-6.jpg" alt="color-6"/>
  <center>
  <figcaption style="margin-top:5px">Our Result</figcaption>
  </center>
</figure>

<figure style="width:70%">
  <img src="/images/bno/neurop-6.jpg" alt="neurop-6"/>
  <center>
  <figcaption style="margin-top:5px">Original Neural Ops Result</figcaption>
  </center>
</figure>

###### Scene 3
<figure style="width:70%">
  <img src="/images/bno/color-9.jpg" alt="color-9"/>
  <center>
  <figcaption style="margin-top:5px">Our Result</figcaption>
  </center>
</figure>

<figure style="width:70%">
  <img src="/images/bno/neurop-9.jpg" alt="neurop-9"/>
  <center>
  <figcaption style="margin-top:5px">Original Neural Ops Result</figcaption>
  </center>
</figure>

### Model Architecture

Our model uses multi-stage network architecture like Neural Ops, where it solves the image restoration problem step by step. For each stage, it learns a bilateral grid for input/output color transformation. The benefit of using bilateral grid instead of using sets of convolution layers is that bilateral grid is less dependent to full input resolution.

For a single stage, it first downsizes the input resolution to a low-res image (e.g. 128x128), and uses low-res image to predict a 16x64x64 bilateral grid. Each cell in this grid contains the slope and bias for corresponding local affine color transformation. For full resolution input, we predict guidemap with a same spatial dimension for slicing through the learned grid. Slice operation will return a full resolution output and we can feed the output to the next stage if needed.

<figure style="width:90%">
  <img src="/images/bno/bilateral_ops_arch.jpg" alt="bilteral neural op arch"/>
  <center>
  <figcaption style="margin-top:5px">Fig2. Model Architecture for Bilateral Neural Operators</figcaption>
  </center>
</figure>

### Implementation
The **training** process is separated into two major stages.
- Stage 1: Initialize individual operators
  - We first pre-train each individual operators to approximate photographic operators like, black-clipping (**BC**), exposure correction (**EX**), white balance (**WB**) and vibrance adjustment (**VB**). For each type of operators, we generate a sets of images that are only adjusted by that corresponding operation in *Adobe Lightroom* similar to Neural Op's approach.
- Stage 2: Jointly train model with initialized operators
  - Our proposed model includes same operator as the ones initialized in the previous step. We first load the weights from pre-trained operators and then starts training the entire model.