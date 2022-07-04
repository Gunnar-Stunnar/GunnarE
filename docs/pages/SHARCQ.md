# DeepSEA-SHARCQ

> **D**eep (learning) for **S**implified, **E**nd-to-end **A**utomation
>Developing on
>**S**lice **H**istology **A**lignment **R**egistration and **C**ell **Q**uantification

<div>
    <img src="https://gunnar-stunnar.github.io/GunnarE/media/Sharcq/SHARCQ.png" width="50%" style="margin-left:auto; margin-right:auto;display:block;">
</div>

## Overview

[SHARCQ Paper](#original-paper)
>Advancements in genetically distinct cell-labeling have begun to outpace the analysis required to quantify
>it. Particularly for whole-brain mapping, robust and automated methods for quick post-imaging analysis
>have become increasingly necessary to process large amounts of data contained in images. To address the
>need for processing high-throughput imaging data, we developed a tool to count fluorescent cells by brain
>region using the digitized Allen Brain Atlas and the modified Franklin–Paxinos Atlas. This tool is called
>SHARCQ (Slice Histology Alignment, Registration, and Cell Quantification).

<!-- SHARCQ is a tool that allows researchers to identify locations and boundries of regions in mouse brains that correlate with standards. How it does this is by standardizing the mouse brain slices into an Atlas Space where the locations and boundries have been outlined.   -->

SHARCQ is developed by my friend and colleage Kristoffer Lauridsen. As stated, the field of Neroscience is moving at an extremly fast rate and methods for analytics are not there to help quantify mass amounts of data. The original SHARCQ tool was built to align images and count cells by hand. As the volume of data grows this is found to be infeaseble unless supported by a large team. That is where the project DeepSEA-SHARCQ came about. The goal is to automate as much of the processes that was being done by hand using machine learning and efficient data designs. I advanced the original SHARCQ process and developed new strategies to fully automate the brain image analysis.       


# Technology Used

## Deep Slice
[Deep Slice Github](https://github.com/PolarBean/DeepSlice)



## Neural Best Buddies (NBB)

<div>
    <img src="https://gunnar-stunnar.github.io/GunnarE/media/Sharcq/nbb.png" width="50%" style="margin-left:auto; margin-right:auto;display:block;">
</div>

One of the major challenges faced was aligning brain images with a standard model (Atlas Space). The method developed in SHARCQ was to go image by image and select landmark points that will morph the mouse brain to the standard model. I identified and incorporated Neural Best Buddies (NBB) which is a method that currently uses vision models (VGG19) to identify key landmarks between images in a corresponding domain.   

<div style="overflow:auto; ">
    <img src="https://gunnar-stunnar.github.io/GunnarE/media/Sharcq/MouseBrain.png" width="30%" height="100%" style="float:right; display:block;">
    <img src="https://gunnar-stunnar.github.io/GunnarE/media/Sharcq/AtlasBrain.png" width="30%" style="float:left; display:block;"/>
</div>

### Transforming Mouse to Atlas

The next challange was transforming/morphing the Mouse Brain to the Atlas space. At first I tried Homography, a method that adjusts a 2d plane in 3d space to create a squeezing, rotation, and other various linear transforms. This was decent but not meeting out expectation for a quality Image overlay. Below is an overview of what Homography is trying to accomplish.  

<div>
    <img src="https://gunnar-stunnar.github.io/GunnarE/media/Sharcq/homography.jpeg" width="50%" style="margin-left:auto; margin-right:auto;display:block;">
</div>

Working through the math we were certain we needed a non-linear method for image morphing. I came across a method of using a triangular mesh that would transform over triangles from one mesh to another linearly reconstructing the mesh in a different space. Below is a picture of the triangle mesh being placed over the Mouse Brain. 

<div style="overflow:auto; ">
    <img src="https://gunnar-stunnar.github.io/GunnarE/media/Sharcq/MouseTriangle.png" width="30%" height="100%" style=" display:block; margin-left:auto; margin-right:auto;">
</div>

We already had the Homography figured out so transforming the triangles to the other mesh was easy. You can see the final output of the whole registration in the images below. The bottom left is the original mouse brain, moving up and over is the flow from transform to overlay. 


<div style="overflow:auto; ">
    <img src="https://gunnar-stunnar.github.io/GunnarE/media/Sharcq/Fulltrans.png" width="100%" height="100%" style=" display:block; margin-left:auto; margin-right:auto;">
</div>

## Cell Count
The next challange we faced was counting cells in the images. We used florescent markers on some experiments to mark the cells bodys and on other experiments the cell membrane was marked using a different technique. The method of marking the cell bodies was easy to identify as they were just blobs in the image. We used algorithms like Connected Components and Watershed to count the cells accurately with marginal error. 

[useful tutorial on cell counting](https://pyimagesearch.com/2015/11/02/watershed-opencv/)


<!-- ![image alt ><]() -->


## Original Paper

```pdf
	https://gunnar-stunnar.github.io/GunnarE/media/papers/ENEURO.0483-21.2022.full.pdf
```