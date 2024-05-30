---
layout: page
title: DeepGlobe Land Cover Classification 
description: Classifying land coverage using remote sensing satelite data.
img: assets/img/land.jpg
importance: 1
toc:
  beginning: true

category: AI
---

> Code: [GitHub Repo](https://github.com/hrishikeshh/DeepGlobe-Land-Cover-Classification)

---

## 1. Background

> This project presumes some prior high level understanding of machine learning, deep neural networks, and working in Python, Keras, and TensorFlow 2.0.

**Satellite Imagery**: Satellite and remote sensing have seen great advances in recent years with one of the key catalyst being the price compression in launch costs from private space launch companies. This has allowed a wider cast of entities and organizations to deploy more satellites. The quality of imagery available to the general public, such as those from Sentinel-2, is also increasing; although still a ways from privately paid satellite services. Sentinel-2 has a typical pixel resolution of 10m band which means that each pixel represents 10m x 10m area.

**CNNs**: Convolutional Neural Networks are the state of the art neural network architecture for image recognition and computer vision tasks. There are many different variations of CNNs but they typically consist of convolution, pooling, and dense connected layers. CNNs perform much better than regular feed-forward neural networks in images due to its ability to reduce the resolution (dimensions) of an image onto feature maps while still encoding the key features of that image. This greatly reduces the complexity and computational need of the network. A typical CNN architecture is shown below.


<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/deepglobe/cnn-structure.webp" title="Typical structure of CNN" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    A typical Convolutional Neural Network. Source: HML
</div>


**FCNs**: Typical CNNs perform great for classifying entire images but have difficulty classifying objects and segments within images. This is due to the fact that the convolution and pooling layers essentially encode the key features of the image down into ever smaller feature maps then feed that into fully connected layers, before a final Sigmoid or Softmax activation layer for final classification. The encoded features or final classifications cannot be easily mapped back to the original image.

This is where *Long et al. (2015)’s Fully Convolutional Network (FCN)* come in. FCNs are just like CNNs except they have a series of convolutional and upsampling layers to mirror the convolutional and pooling layers in place of the dense connected layers. This allows the network to encode (downsample) and then decode (upsample) back into the original image’s resolution and spatial dimensions which allow for pixel-wise classification. If we edit the typical CNN layer from earlier, we might reach a configuration of something that looks like below.



<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/deepglobe/cnn-int.webp" title="CNN" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Fig: What a Fully Convolutional Network might look like. Note that this is not a true FCN but helps in gaining some intuition into the how and why. Source: HML
</div>


**Common Types of Computer Vision Problems:** For this project, I will be working with a type of computer vision problem called **semantic segmentation**. There are many other types of problem sets like object detection, instance segmentation, and others which warrant their own discussions at a later date.

Semantic Segmentation is determining which pixels in an image belongs to which class as shown below. For these problems, pixel-wise classification and localization of objects and edges within an image are important and therefore are good uses of FCNs.





<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/deepglobe/segmentation.webp" title="segmentation" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Fig: Semantic Segmentation Example. Source: Jeremy Jordan
</div>


## 2. Dataset

> The [DeepGlobe Land Cover Classification Challenge](https://competitions.codalab.org/competitions/18468) and hence, the dataset are governed by [DeepGlobe Rules](http://deepglobe.org/docs/DeepGlobe_Rules_3_2.pdf), The [DigitalGlobe’s Internal Use License Agreement](http://deepglobe.org/docs/CVPR_InternalUseLicenseAgreement_07-11-18.pdf), and [Annotation License Agreement](http://deepglobe.org/docs/Annotation%20License%20Agreement.pdf).

> Data Source : [DeepGlobe Land Cover Classification](https://competitions.codalab.org/competitions/18468)


**Training Data**: The dataset comprised of **803 satellite imagery** in RGB of size **2448x2448** with one set comprised of satellite images and the other set comprised of labeled masks. Each satellite image is at the 50cm pixel resolution band collected by DigitalGlobe’s satellite. 50cm pixel resolution is quite high by typical publicly accessible satellite imagery resolution standards which are usually around the 5m to 10m pixel resolution band.

**Test Data:** The dataset also contained **171 validation** and **172 test images**, both without labeled masks. I ended up combining these 2 groups into a testing dataset and randomly sampled a subset of the training data for validation.

Here are some sample training images:


<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/deepglobe/sample-img.webp" title="Sample data" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Fig: Sample of 8 training satellite images and their corresponding Ground Truth Mask labels.
</div>


Each satellite image can have up to 7 different segmentation classes where each class is denoted by a different color in the Ground Truth Mask labels. The table below shows the seven different classes, their RGB channel values, and associated color in the Ground Truth Mask labels.









<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/deepglobe/table.webp" title="table" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Fig: Breakdown of the 7 segmentation classes.
</div>

## 3. Neural Network Architectures

After some research and testing, I settled on implementing two different network architectures for my problem set.

### U-Net

The first network architecture is [Ronneberger et al. (2015)’s U-Net](https://arxiv.org/abs/1505.04597). The U-Net was originally developed for segmenting biomedical images but have since then been widely used for segmenting many types of images by the ML community. The architecture is shown below where the convolutional and max pooling layers perform an encoder operation on the image and the convolutional and upsampling layers perform the corresponding decoder operation into a full resolution output segmentation map. It also implements long skip connections (grey arrows) which concatenate feature maps from correspondingly sized layers in the encoder and decoder layers to help with localization of objects and edges within the image.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/deepglobe/u-net.webp" title="table" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Fig: Original U-Net architecture. Source: Ronneberger et al. (2015)’s U-Net
</div>



### U-Net with ResNet50 Encoder Backbone

The second architecture uses transfer learning of a ResNet50 architecture trained on ImageNet to replace the encoder portion of the U-Net. An example of what such an architecture may look like is shown below. In theory, such an architecture can have good performance since it would already be trained on high level features in images from ImageNet and could reduce the amount of training needed than an entirely new untrained architecture.


<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/deepglobe/u-net-2.webp" title="table" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Fig: Sample ResNet50 encoder and U-Net decoder architecture. Source: Neven, Robby & Goedemé, Toon
</div>


## 4. Code

I will just go over some of the key steps and reasoning in my code process but the code for the entire project and how to set it up can be found in my [GitHub Repo](https://github.com/hrishikeshh/DeepGlobe-Land-Cover-Classification). I chose to use Google Colab to take advantage of their GPUs for training. I developed the neural networks in Keras and TensorFlow 2.0 building on inspiration from [Zizhaozhang’s U-Net](https://github.com/zizhaozhang/unet-tensorflow-keras/blob/master/model.py) and [Nikhilroxtomar’s ResNet_U-Net](https://github.com/nikhilroxtomar/Semantic-Segmentation-Architecture/blob/main/TensorFlow/resnet50_unet.py) implementations.

### Preprocessing

The masks first need to be One-Hot Encoded from RGB channels into their respective channels per class like shown in the image below.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/deepglobe/hot-encode.webp" title="table" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Fig: One-Hot Encode of masking images for semantic segmentation. Source: Jeremy Jordan
</div>

Since each image was 2448x2448x3 and the corresponding masks are 2448x2448x7 (each channel per class), each training sample are quite large so I had to cut them into smaller patches of 612x612 instead.

We also needed a custom data generator to yield an, or batches of, image(s) during training instead of loading all training dataset into RAM all at once.


### Training

During training, our metric and loss functions are the Dice Coefficient and Dice Coefficient Loss respectively.To get a better understanding of how the U-Net is built and trained and for the second ResNet50_U-Net transfer learning model implementation please see the GitHub repo.


## 5. Results

After training, the ResNet50_U-Net had a validation dice coefficient of 0.803 and the U-Net had a dice coefficient of 0.784.

Here were some of the validation results showing the original satellite image, the Ground Truth Mask, the predicted Mask from the RestNet50_U-Net architecture, and the predicted Mask from the U-Net architecture. The smaller squares below the initial first level show the activations of each of the 7 classes for the RestNet50_U-Net architecture.



<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/deepglobe/result-1.webp" title="table" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Fig: Sample results comparing Original Satellite Image, Ground Truth Mask, Predicted ResNet50_U-Net Model, and Predicted U-Net Model (top). Class activations for each of the 7 classes for the RestNet50_U-Net architecture (bottom).
</div>


Here are a couple of just the U-Net architecture.







<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/deepglobe/result-2.webp" title="table" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Fig: Sample results comparing Original Satellite Image, Ground Truth Mask, and Predicted U-Net Model (top). Class activations for each of the 7 classes for the U-Net architecture (bottom).
</div>


## 6. Final Thoughts

I hope that sharing this post and the source code can help others in the future whom may need it as reference for similar problem sets.

One area of improvement for this project would be to try using an architecture specifically designed and pre-trained on semantic segmentation tasks for transfer learning instead of using the ResNet50 which is typically used for multi-class image classifications. Additionally, variations of DeepLab introduced by Chen et al. in Rethinking Atrous Convolution for Semantic Image Segmentation⁷ have found to perform extremely well on image segmentation tasks. It may bode well to give the DeepLabv3 architecture a try.

Lastly, I would like to apply the knowledge gained here to more specific problems in the climate and sustainability domain like:

  - Forest and tree mapping to monitor deforestation
  - Ice-cap monitoring
  - Smart city/urban planning
  - Endangered wildlife monitoring

---