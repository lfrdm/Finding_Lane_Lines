# **Finding Lane Lines on the Road** 

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report

[//]: # (Image References)

[image0]: ./test_images/solidYellowCurve2.jpg "Original image"
[image1]: ./examples/solidYellowCurve2_1.jpg "Colorsegmentation"
[image2]: ./examples/solidYellowCurve2_2.jpg "Grayscale"
[image3]: ./examples/solidYellowCurve2_3.jpg "Canny edge"
[image4]: ./examples/solidYellowCurve2_4.jpg "Trapeze mask"
[image5]: ./examples/solidYellowCurve2_5.jpg "Hough lines"
[image6]: ./examples/solidYellowCurve2_6.jpg "Final output"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.


Original example image for visualzation purposes (solidYellowCurve2.jpg of /test_images):
![alt text][image0]

My pipeline consisted of the following 7 steps:
1. **Colorsegmentation:** Color selection was carried out to filter only the yellow and white color of the lane lines. This [source](http://docs.opencv.org/3.0-beta/doc/py_tutorials/py_imgproc/py_colorspaces/py_colorspaces.html) of OpenCV was modified to first create a mask for the white colors in RGB colorspace and second after a conversion from BGR to HSV colorspace a mask for yellow colors was created. Finally, both masked images were combined, showning only white and yellow pixels. In HSV colorspace the color yellow has a hue value of around 100 (30°).
![alt text][image1]
2. **Grayscale conversion:** A conversion to grayscale was carried out to be able to use canny edge algorithm on all combined color channels.
![alt text][image2]
3. **Gaussian blur:** The image was blurred with a gaussian kernel of size 3 as a preprocessing for the edge detection to smear the gradients over a wider region.
4. **Canny edge detection:** The algorithm detects edges in the blurred image with a given lower and upper threshold for the edges gradient. It returns a binary image of edges and background.
![alt text][image3]
5. **Trapeze mask:** A mask was applied to a region in the image, where the lines are very likely to occur and other regions are set to 0.
![alt text][image4]
6. **Hough lines detection:** Hough lines detector was used to detect lines (red below, overlayed on the original image) in the binary image of a certain length and with an allowed gap length.
![alt text][image5]
7. **Draw lines function:** The draw lines function uses the previously detected lines to differntiate these lines in left and right lane markings and accordingly use [numpy.polyfit](https://docs.scipy.org/doc/numpy/reference/generated/numpy.polyfit.html) function to fit the best line with linear regression algorithm. The returned linear function is used to calculate start- and endpoints of the found lines and draw these in the image. If no lines are detected, no lines will be shown. The final output image is shown below.
![alt text][image6]

### 2. Identify potential shortcomings with your current pipeline

In the given example images and videos (even challenge) no shortcomings occured. But, there are many possible situations, where my pipeline could fail: 
* bad weather conditions, due to noisy images
* big occlusions by objects, especially white and yellow cars, due to falsely detected lines in other objects
* bad lighting conditions, due to lower gradients

Overall it may fail due to overfitting on the examples.


### 3. Suggest possible improvements to your pipeline

Possible improvements could be:
* Test on bigger dataset and change parameters accordingly

