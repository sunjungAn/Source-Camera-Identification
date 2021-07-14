# Source Camera Identification

## 1. Traditional Convolutional Neural Networks



#### Bondi

+ firstly introduced deep learning-based schemes for source camera identification
+ path-breaking method 



#### Freire-Obregon

+ Focusing on image captured by mobile phones

+ using L-ReLU activation function

  > better than ReLU



#### Huan

+ presented an architecture similar to the one proposed by Bondi
+ using Batch Normalization
+ improve over the accuracy by Freire-Obregon



#### Yao

+ robust against JPEG compression and noise adding
+ not resistant to re-scaling operation



#### Chen

+ investigated the use of ResNet(with 26 layers)
+ for brand(99.12%), model(94.73%), device-level(45.81%) identification
+ ResNet has better than AlexNet, GoogleNet, scheme Bondi



#### Ding

+ extended method by combining ResNet with a multi-task learning strategy.
+ integrated (brand, model, sensor-level)into one framework



#### Bayar and Stamm 

+ presented two different schemes to address the open-set problem for camera model identifiation.
+ aim to judge whether the device that captured the query image known or unknown.



#### Mayer and Stamm

+ tried to measure similarity among images by using a siamese network



## 2. Data Enhancement

+ including data augmentation and pre-processing
+ improve the performance

**bondi**

+ normalized the images by subtracting

**Kamal**

+ random crops, random rotation, image manipulation, images addition, empirical mode decomposition



The results demonstrated that adding more images has a great impact to performance

using manipulated image enhance the robustness of the CNN model



### Preprocessing

+ improve the signal-to-noise ratio(SNR)

**Reduce the interference of the image contents**

1. based on the idea of applying a denoising filter F  to the input image I, and then subtracting the result of that operation from I itself, thus obtaining the noise N

   **N = I - F(I)**

+ tuama
  + chose a wavelet-based denoiser
  + filters based on Photo Response Non Uniformity(PRNU) for source camera identification
+ Bayer and Stamm
  + evaluated the effect of MFR



2. based on the idea that the noise can be easily obtained by using a spatial filter G:

   **N = I * G**

+ tuama
  + tested the effectiveness of an high-pass filter(HP)
  + HP filters better results than wavelet-based denoiser when used in CNN-based shemes
+ Ding
  + evaluated the case of gaussian filter

+ Yang
  + presented self-learning filters (improve SNR)
+ Bayar and Stamm 
  + proposed a novel constrained convolution(learned high-pass within given bound)
+ Wang 
  + used local binary pattern (LBP) to code the image
    1. LBP coding operation 
    2. fed into CNN architecture

**Self-learning, filters, contrained convolutions, LBP coding are reported to outperform high-pass filters**



+ Zuo
  + evaluated how the performance of CNN models are impacted by the use of two preprocessing techniques
  + results indicate that the CNN model without pre-procession provides better performances
+ analysis of data enhancement techniques used in source camera identification tasks highlight an important difference with respect to classical DE methods used in machine learning.

+ most DE technique aims to reduce the influence of image contents by filtering out information deemed not useful



## 3. Fusion and Ensemble

aim to enhance performance by fusing multiple models and features together





## 4. Patch Selection

choosing different areas in the image could have some effect on the performance.







