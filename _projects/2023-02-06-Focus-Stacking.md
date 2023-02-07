---
title: 'Focus Stacking'
subtitle: 'A technique that combines multiple images taken at different focus distances to produce a final image with a greater depth of field.'
date: 2023-02-06 00:00:00
description: 
featured_image: '/images/focus-stacking/demo.png'
---
![](/images/focus-stacking/demo.png)


## What is Focus Stacking?
Focus stacking is a technique used in photography to increase the depth of field in an image by combining multiple images taken at different focus distances into a single image with greater sharpness and clarity from foreground to background. The process involves taking multiple shots of the same scene, each with a different focus distance, and then combining them into one single clear image. [(Detailed Photography Guide)](https://phlearn.com/magazine/focus-stacking-master-best-kept-secret-sharp-photos/)


## How to achieve Focus Stacking?
I researched several methods including findning maximum laplacian of gaussian (LoG) and using laplacian pyramid decomposition. Among of all methods, I find fusing images using laplacian pyramid decomposition generates the best results.

The **Laplacian Pyramid decomposition** is a technique used in image processing to represent an image at multiple levels of resolution or detail. The idea is to decompose an image into a series of increasingly smoothed and down-sampled versions, called a Gaussian Pyramid, and then subtract the corresponding levels of the Gaussian Pyramid from the original image to obtain a set of Laplacian images. Each Laplacian image represents the residual or the high-frequency details of the image that were lost during the smoothing and down-sampling process. 

![lpd](/images/focus-stacking/lpd.png)

The Wang and Chang's paper: [A Multi-focus Image Fusion Method Based on Laplacian Pyramid](http://www.jcomputers.us/vol6/jcp0612-07.pdf) introduces the method of using Laplacian Pyramid Decomposition to fuse images with different depth-of-focus. The method starts by decomposing each of the input images into a Laplacian Pyramid and then using the high-frequency details of each pyramid to create a weight map that indicates the focus level of each pixel. The weight maps are then used to combine the images into a single multi-focus image.

## Using Focus Stacking
[Github](https://github.com/bznick98/Focus_Stacking)

Inspired by the paper, I implemented focus stacking in Python. In addition to the paper, not only two images, but also multiple images can be fused together. In the latest version, the program also supports colored image.

```bash
# First clone the repo:
git clone https://github.com/bznick98/Focus_Stacking.git

# and go into project directory
cd Focus_Stacking

# (optional) Create a Python virtual environment for better package management. You can use conda, virtualenv, etc.

# Install required packages: OpenCV, Matlplotlib (TODO: add a requirements.txt)

# Run focus stacking program by:
python focus_stack.py PATH/TO/IMAGE/DIR/

# For more argument detail, see:
python focus_stack.py -h
```


## Rough Performance Evaluation
To evaluate the quality of final image, we can calculate the standard deviation of pixel values in each image. This metric will represents focusness of the image, higher is better.

|                  | Standard Deviation| 
|------------------|-------------------|
| Source Image 1   | 43.3302 |
| Source Image 2   | 40.4064 | 
| Source Image 3   | 41.2541 | 
| Final Result	   | **46.8988** |

![](/images/focus-stacking/demo.png)

From this rough measurement we can see that the final result has the highest standard deviation among all images, meaning it achieves the highest focusness. A future work is to come up with a better measurement of focusness and setup a testing set for measuring the performance of focus stacking algorithms.