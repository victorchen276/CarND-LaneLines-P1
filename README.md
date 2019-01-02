# **Finding Lane Lines on the road** 


---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[outputimage]: ./test_images_output/output.png "Test Image Output"
[houghimage]: ./test_images_output/output1.png "Hough Output"

---
Source Code: 

	Project1.ipynb

Output:

	Test images output is under test_images_output folder
	Test videos output is under test_videos_output folder

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. 

1. Convert image to grayscale
2. Apply gaussian blur to smooth the edge
3. Apply Canny edge transform
4. Apply region of interest mask
	- Lane line only appears on the lower part of image. 
5. Preform Hough Transform Line Detection
	- At this stage, we have mutilpe lines on image. When there is shadows in the middle of ROI, Hough Transfrom algtrithom returns false lines between actual left and right lane lines(image 3).
	 ![alt text][houghimage]


In order to draw a single line on the left and right lanes, I modified the draw_lines() function by folllowing steps.

1. ignore both vertical and horizontal line
2. seperate left lane and right lane. If the slope of a line is negative, it belongs to left lane. If it is positive, it is on right lane.
3. Apply the fitting polynomial of degree 1 to points of lines on both left and right lane. resuls is slope and intercept of line.  
4. We draw the highlight line from bottom of image to the 65% of its height. therefore, we have two y values. With the slopes and intercepts from step 3, we can calculate 2 points of a line. By this method, we can draw a single highlighted line on top of left and right lane line.

###Output: 
![alt text][outputimage]


### 2. Identify potential shortcomings with your current pipeline


One shortcoming is numpy's polyfit fails to find a line in some frames of chanllage video. It makes the lane line detection is not smoothing when there is shadows and bright spots in the region of interest.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to usd RANSAC algorithm to eliminate outliers.

Another potential improvement could be to apple the region of interest mask at the first step. It can reduce the computational cost.

