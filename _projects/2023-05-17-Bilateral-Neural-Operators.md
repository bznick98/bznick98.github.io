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
There are plenty of existing methods ranging from traditional image processing techniques like histogram equalization, to more recently deep learning approaches. Here we are focusing on some of the recent deep learning approaches since they show greater capability and generalizability.

<details markdown="1"><summary>Bilateral-Grid Based Method</summary>

<a name="HDRNetTag"></a>

Bilateral grid comes from the paper [HDRNet](https://groups.csail.mit.edu/graphics/hdrnet/). It is a data structure that represents an image transformation. Comparing to processing full resolution image, processing in bilateral space is **faster** and **edge-aware**. A bilateral grid is predicted using downsampled image (thus faster), and final output is [sliced](https://people.csail.mit.edu/sparis/publi/2007/siggraph/Chen_07_Bilateral_Grid.pdf) from the predicted grid using the full-res image, without the need to feed the full-res image into neural network. Because of this, it consumes less memory space compared to other methods processing using full resolution image.

</details>

<details markdown="1"><summary>GAN Based Method</summary>

GAN-based approach like [Pix2Pix](https://phillipi.github.io/pix2pix/) are really powerful for image generation or style transfer, but it lacks the capability to generate a visually smooth image. The results often comes with artifacts and it is generally unacceptable for photography applications. In addition, the model size of generative model is often large and cannot achieve real-time.

</details>

<details markdown="1"><summary>3D-LUT Based Method</summary>

LUT-based approaches like [AdaInt](https://arxiv.org/pdf/2204.13983.pdf) and [3D LUT](https://arxiv.org/pdf/2009.14468.pdf) is a fast and visually smooth approach for image enhancement. It learns a 3D Look-up Tables (LUTs) for transfering the color in one space to the other. The learned Look-up Tables (LUTs) are a form of compact representation of color transfer just like bilateral grid. The downside of 3D LUT based methods is that they don't offer interpretable process of how model is modifying the photo, and the model size is relatively large compared to Neural Operators.

</details>

<details markdown="1"><summary>Sequential Retouching Method</summary>

[Neural Operator](https://arxiv.org/pdf/2207.08080.pdf) is a novel approach for image enhancement process. Specifically, it learns how human photographers process images by sequentially applying operators on an image. It's sequential processing makes the model more interpretable. However, the original paper's approach will suffer from the runtime performance if the input image is too large.

</details>

### Proposed Method
Instead of a black-box image transformation, I would like to approach this problem with a more interpretable solution like [Neural Operator](https://arxiv.org/pdf/2207.08080.pdf) did. By applying sequential operations to the input image, we can tackle the image restoration task stage by stage. We constrain the intermidiate operators to approximate common photographic operators like **exposure correction**, **black clipping**, **vibrance adjustment** and **white balance adjustment**, etc. 

The original **Neural Operator** method achieves great results on MIT-Adobe 5K dataset with a relatively small model size. However, it is contrained to be dependent on input image size because of its model architecture directly operates on the full resolution image. It requires huge amounts of memory and runtime for hi-res inputs like 3840x2160.

<figure style="width:70%">
  <img src="/images/bno/neural_ops_arch.jpg" alt="original neural op arch"/>
  <center>
  <figcaption style="margin-top:5px">Fig1. Original Neural Operators</figcaption>
  </center>
</figure>

To ameliorate this problem, we consider replacing each neural operator with a modified lightweight [**HDRNet**](#HDRNetTag). This method avoided processing the full-res image "in network". Instead, it process a greatly down-sampled image and only process full-res image when applying the color transformation, which is a relatively low-cost and less-dependent on input resolution.

The **proposed method** combines the both benefits of HDRNet and Neural Operator. For each of the neural operator in the original architecture, I replaced it with a simplified HDRNet, which learns a affine color transformation bilateral grid. Therefore, the high-res inputs will not involve with network inference anymore, but only used in the **slice** step. A detailed model architecture description can be found [here](#model-architecture).

<figure style="width:90%">
  <img src="/images/bno/bilateral_ops_arch.jpg" alt="bilteral neural op arch"/>
  <center>
  <figcaption style="margin-top:5px">Fig2. Bilateral Neural Operators</figcaption>
  </center>
</figure>

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

<figcaption style="text-align: center;font-size: 50%;margin-top:3px;">(- means data not available)</figcaption>

I cannot reproduce the exact same results using the dataset Neural Ops authors provided (because the author only provides partial training dataset), but I did try to reproduce the best I can using the dataset I generated. Using our training data, we're able to achieve similar results for our model, and also runs faster and consumes less memory space.

I also measure runtime performance of NeuralOps and BilateralOps (ours) along with its CUDA memory usage.

|  Model/Resolution   | 500x300 | 1200x900 | 2500x1200 | 4000x3000 | 
|---------------------|---------|----------|-----------|-----------|
| NeuralOp            |  8.26 ms (121 FPS) <br/> **262 MB**   |  44.41 ms (22 FPS) <br/> **1.61 GB**    |  121.14 ms (8 FPS) <br/> **4.43 GB**  |  N/A <br/> **N/A**  |
| **BilateralOp (ours)**  |  6.78 ms (147 FPS) <br/> **55 MB**   | 32.86 ms (30 FPS) <br/> **266 MB**    | 87.05 ms (11 FPS) <br/> **702 MB**    | 350.22 ms (2 FPS) <br/> **2.70 GB**  |

<figcaption style="text-align: center;">Table2. Runtime compared on different input resolution.</figcaption>

<figcaption style="text-align: center;font-size: 50%;margin-top:3px;">(N/A means model runs out-of-memory for given input resolution, time is measured per inference)</figcaption>

We can see from the table that our method is faster than the Neural Op, and it is able to handle large input resolution and consumes relatively smaller memory space.

### Visual Comparison
Images are arranged from left to right as: `Input`, `Output`, `Target`, `Diff(Output,Target)`
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