---
title: VFX Project - HDR
date: 2019-04-04 01:04:05
tags: Small Project
---
![](https://i.imgur.com/6blOFBm.jpg)

In this project, we implemented Paul Debevec's method to assemble an HDR image. For extension, we implemented MTB alignment algotirhm.

Link to Code: [Github](https://github.com/Petingo/HDR)

<!--more-->

## Run the code
Use python3 to run the code, and the `dataset` is the path to image resource.
```shell
cd code
python3 HDR.py ../dataset
python3 HDR-YUV.py ../dataset
```

[openCV](https://pypi.org/project/opencv-python/) and [exifread](https://pypi.org/project/ExifRead/) is necessory to run the program.
```shell
pip install opencv-python
pip install ExifRead
```


## HDR

For HDR image recovery, we implemented Paul Debevec's method.

### Using RGB channels
1. Separate the image into RGB channels.
2. For each channel, we randomly choose 100 pixels to find its response curve. For solving the over-determined linear system, we use numpy's `lstsq` function.
   ![](https://i.imgur.com/AmPK6G1.png)

3. Construct the raidance map for each channel and merge them together.  Then, we can get the HDR image.
   ![](https://i.imgur.com/6qSiETR.jpg)

4. Tone mapping

   We tried to implement the Gradient Domain algorithm for tone mapping, which should have worked fine if coded properly, but we failed to do so. (The code is still included.) We now use openCV's built-in tone mapping method to transfer the HDR image into a LDR one.

   ![](https://i.imgur.com/wwx05K5.jpg)

   However, though the quality is tolerable, some overexposed spots we can't eliminate appear in some cases.

### Using YCbCr channels
Then, we tried to convert the color space from RGB to YCbCr, and only reconstruct the HDR image of Y channel, which represents the luminance. For the chroma channels, we weighted average them by the luminance difference between the original photos and the recovered one. With this method, the color is better preserved, and the recovered HDR image seems more like the original photo. However, it sometimes results in strange HDR image which has too strong color saturation.
![](https://i.imgur.com/6blOFBm.jpg)

## Alignment
Here is an example of the result of our MTB alignment. The two HDR images are generated from the same image set, but the right one was processed after aligning.

![](https://i.imgur.com/SNhzjrN.png)


## What We Learned
The most interesting part of this project is how response curve is constructed. It is our first time to know how math, especially linear algebra, is so important for creating and implementing an algorithm.

## Team Member
Me & ZL Yu