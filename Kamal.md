## pre-knowledge

+ focus has shifted towards detecting intrinsic camera features



## Datasets & Preprocessing



### Dataset -1 

Camera Model Identification Dataset provided for the IEEE Signal Processing Cup 2018

+ 10 different camera models having 275 images for each
+ without any labels

Collected from Flickr during the open competition phase of SP Cup 2018

![image-20210721092704152](C:\Users\User\AppData\Roaming\Typora\typora-user-images\image-20210721092704152.png)

+ post-processing:  

  + JPEG-Compression with quality factor 90% and 70%
  + Resizing by a factor of 0.5, 0.8, 1.5 and 2.0
  + Gamma-Correction using r as 0.8 and 1.2
  + **EMD** 

  

### Dataset -2

Dresden Image Database 

+ 27 different camera models





## Methods

1. select patches of size 256x256
2. train DenseNet-201 architecture with patches of size 256x256
3. extract features from second to the last layer  **(1x1920)**
4. extract all non-overlapping patches of size 128x128 and 64x64 from eatch training image **(4x1920) (16x1920)**
5. Averaging **(4x1920) → (1x1920)  (14x1920) →(1x1920)**
6. concatenate the feature vectors **(3x1920)**
7. SE block
8. classification block



<img src="C:\Users\User\AppData\Roaming\Typora\typora-user-images\image-20210721094937508.png" alt="image-20210721094937508" style="zoom:67%;" />



### 1. Select patches

only use patches of good quality to train our network

patch's quality is computed as:

<img src="C:\Users\User\AppData\Roaming\Typora\typora-user-images\image-20210721095411173.png" alt="image-20210721095411173" style="zoom:67%;" />

**On experiment**

a = 0.7, B = 4, r = ln(0.01), P = patch

µc = mean and σc = standard deviation,



**select 20 patches of size 256x256 with highest Q values**





### 2. EMD

<img src="C:\Users\User\AppData\Roaming\Typora\typora-user-images\image-20210721095941338.png" alt="image-20210721095941338" style="zoom:67%;" />

**input signal is decomposed into IMFs and Residue**



**2D EMD**

<img src="C:\Users\User\AppData\Roaming\Typora\typora-user-images\image-20210721100039769.png" alt="image-20210721100039769" style="zoom: 67%;" />



+ used **FastRBF** to interpolate upper and lower envelopes of scattered local maxima and minima from I(m,n). The mean of envelopes is then subtracted form the image **to get the IMF**
+ **FastRBF**

<img src="C:\Users\User\AppData\Roaming\Typora\typora-user-images\image-20210721101554913.png" alt="image-20210721101554913" style="zoom:67%;" />

**EMD essentially works as a denoising scheme by removing random high-frequency noise-components from the image data** --> **used Python's PyEMD**





## Model Architecture



### DenseNet

<img src="C:\Users\User\AppData\Roaming\Typora\typora-user-images\image-20210721103608359.png" alt="image-20210721103608359" style="zoom: 80%;" />

<img src="C:\Users\User\AppData\Roaming\Typora\typora-user-images\image-20210721103637618.png" alt="image-20210721103637618" style="zoom: 50%;" />

+ consist of 4 dense blocks, each with a **growth-rate of 32**

  <reference>

  <img src="https://user-images.githubusercontent.com/57055431/111908404-7bb62d00-8a9c-11eb-979a-ac967a998270.jpg" alt="img" style="zoom: 67%;" />

  

+ to preserve image information throughout the network, the output of each layer is propagated to all of the layers in front of it.





### Squeeze and Excitation Block(SE)

The aim of this module is to improve the representational power of a network by explicitly modeling the interdependencies between the channels of its output.



the SE block performs feature recalibration, through which it can learn to use global information to selectively emphasize informative features and suppress less useful ones, without changing the dimensions of the feature vector



<img src="C:\Users\User\AppData\Roaming\Typora\typora-user-images\image-20210721105731691.png" alt="image-20210721105731691" style="zoom: 80%;" />

<img src="C:\Users\User\AppData\Roaming\Typora\typora-user-images\image-20210721110045075.png" alt="image-20210721110045075" style="zoom: 50%;" />

+ recalibration 
+ the feature maps are then re-weighted to generate the output of the SE block which can ten be fed directly to the classification layers



### Classification block

1. (3x1920) produced at the output of the SE block is then passed through a Dropout layer with 30%

2. Global Average Pooling  -> (1x1920)
3. Softmax -> 10 classes





## Experiments & Results



### phase -1(Experiment)

+ Dataset-1 20 patches of size 256x256
+ train: 85%, validation: 15%
+ Stochastic Gradient Descent (momentum of 0.9, learning rate = 10e-3)
+ the learning rate is decreased by a factor of 10e-1 if the validation loss have not decreased in 2 successive epochs(when learning rate is reduced to 10e-7, training is stopped)



### phase -1(Result)

+ Score = 0.7 x (Accuracy of Unaltered Images) + 0.3 x (Accuracy of Manipulated Images)

<img src="C:\Users\User\AppData\Roaming\Typora\typora-user-images\image-20210721153513315.png" alt="image-20210721153513315" style="zoom:50%;" />

+ higher accuracy is produced for larger input sizes
+ residual camera model information left after cropping an image to this size are minimal



<img src="C:\Users\User\AppData\Roaming\Typora\typora-user-images\image-20210721154152944.png" alt="image-20210721154152944" style="zoom: 67%;" />

+ EMD augmented images in increasing the accuracy
+ this IMF likely to have some correlations to the device-level features
  + device-level features degrades its performance





### phase -2(Experiment)

+ Dataset -2 20 patches of size 256x256
+ transfer learning on Phase-1 (initial weights of the network from Phase-1)
+ not used our full pipeline for the dataset (not SE layer)
+ trained for the 27 classes



### phase -2(Result)

+ overall accuracy of over 99% is achieved for 19 camera models by the 1st network of Phase-1
+ Though the training dataset is very small compared to the dataset of Phase-1, but still DenseNet201 is able to detect the camera models very accurately because of the larnt features from Phase-1.





### phase -3(Experiment)

+ used all images from Phase-1
+ EMD data has not been included 
+ sub-divided these data into 4 classes (Unaltered, Resized, JPEG Compressed and Gamma Corrected)

**used 128x128 sized patches for training due to much higher prediction accuracy compared to other sizes.**





### phase -3(Result)

+ tried to identify the 4 types of image-manipulations used on the images of Dataset-1

  <img src="C:\Users\User\AppData\Roaming\Typora\typora-user-images\image-20210721174200652.png" alt="image-20210721174200652" style="zoom:50%;" />

+ these result have been obtained by using only the DenseNet 201 architecture and 128x128 patch size.





**features learned have some sort of orthogonality among them **

**--> these features may be used in other image forensic tasks outside the premise of our current work**

