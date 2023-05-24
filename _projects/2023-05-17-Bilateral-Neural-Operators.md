---
title: 'Bilateral Neural Operators'
subtitle: 'Learning image retouching process using sequential neural operators in bilateral space.'
date: 2023-05-17 00:00:00
description: 
featured_image: '/images/bno/demo-1.png'
---

### Problem Statement
**Image retouching** is the process of making changes or improvements to a photograph to enhance its appearance. The inputs includes but not limit to compressed jpeg images and raw sensor data from camera, and preferably outputs an 8-bit sRGB jpeg image for display. Most of the methods enhance the image includes but not limit to exposure correction, color style adjust and white balance correction.

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

<details markdown="1"><summary>Bilateral Grid Based</summary>

<a name="HDRNetTag"></a>

[HDRNet](https://groups.csail.mit.edu/graphics/hdrnet/) originated from the technique (or data structure) named **Bilateral Grid**. Comparing to processing full resolution image, processing in bilateral space is faster and edge-aware. Bilateral grid is predicted using downsampled image (thus faster), and final output is [sliced](https://people.csail.mit.edu/sparis/publi/2007/siggraph/Chen_07_Bilateral_Grid.pdf) from the predicted grid using the full-res image to produce, without needing to feed the full-res image into the network. The benefit of this bilateral grid based approach is that it works fast and the runtime is less dependent on the input resolution.

</details>

<details markdown="1"><summary>GAN Based</summary>

GAN-based approach like [Pix2Pix](https://phillipi.github.io/pix2pix/) are really powerful for image generation or style transfer, but it lacks the capability to generate a visually smooth image. The results often comes with artifacts and it is generally unacceptable for photography applications. In addition, the model size of generative model is often large and cannot achieve real-time.

</details>

<details markdown="1"><summary>3D LUT Based</summary>

LUT-based approaches like [AdaInt](https://arxiv.org/pdf/2204.13983.pdf) and [3D LUT](https://arxiv.org/pdf/2009.14468.pdf) is a fast and visually smooth approach for image enhancement. It learns a 3D Look-up Tables (LUTs) for transfering the color in one space to the other. The learned Look-up Tables (LUTs) are a form of compact representation of color transfer just like bilateral grid. The downside of 3D LUT based methods is that they don't offer interpretable process of how model is modifying the photo, and the model size is relatively large compared to Neural Operators.

</details>

<details markdown="1"><summary>Sequential Retouching</summary>

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

To ameliorate this problem, we consider  to replace each neural operator with a modified lightweight [**HDRNet**](#HDRNetTag). This method avoided processing the full-res image "in network". Instead, it process a greatly down-sampled image and only process full-res image when applying the color transformation, which is a relatively low-cost and less-dependent on input resolution.

The **proposed method** combines the both benefits of HDRNet and Neural Operator. For each of the neural operator in the original architecture, I replaced it with a simplified HDRNet, which learns a affine color transformation bilateral grid. Therefore, the high-res inputs will not involve with network inference anymore, but only used in the **slice** step.

<figure style="width:90%">
  <img src="/images/bno/bilateral_ops_arch.jpg" alt="bilteral neural op arch"/>
  <center>
  <figcaption style="margin-top:5px">Fig2. Bilateral Neural Operators</figcaption>
  </center>
</figure>

### Results (Not Finalized)

Based on the results of Neural Operator paper, the proposed method achieves similar **PSNR**/**SSIM**/**ΔE** score, with a runtime faster than Neural Operator when input resolution is large (more like in real scenarios).

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
| NeuralOp            |  24.32    |  0.907     | 9.795   |  0.045 | **28,108**   |
| BilateralOp (ours)  |  24.22    |  0.906       |  9.830    | 0.043     |  69,012      |

<figcaption style="text-align: center;">Table1. Metrics compared on FiveK Dark Dataset.</figcaption>
<figcaption style="text-align: center;">(- means data not available)</figcaption>


### Visualization Comparison
Images are arranged as **[ Input | Output | Target | Difference ]**
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