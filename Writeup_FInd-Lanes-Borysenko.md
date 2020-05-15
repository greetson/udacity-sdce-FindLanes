# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./Output_Images/-0-original.png "Original Image"
[image2]: ./Output_Images/-1-Grayscale.png "Grayscale Image"
[image3]: ./Output_Images/-2-GaussianBlur.png "Smoothened image"
[image4]: ./Output_Images/-3-Canny.png "Canny Edges"
[image5]: ./Output_Images/-4-ROI-Canny.png "Canny Edges in ROI"
[image6]: ./Output_Images/-5-HoughLinesDetection.png "Hough Lines"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

The pipline was based on the information from Lesson 4: Computer vision fundamentals and contains following stages:
1. Convert image from colorfull gamma (RGB/BGR) to greyscale (to compute the gradient for edge detection)
2. Apply Gaussian smoothing to grayscale image - to suppress noise and spurious gradients by averaging
3. Apply Canny Edge Detection
4. Apply ROI mask - use only edges detected on the intended road surface
5. Extract Hough lines using hough Transform approach from Lesson 4: cv2.HoughLinesP() function
(6). Optional to use ROI masking for extrapolated lines
![image1]
![image2]
![image3]
![image4]
![image5]
![image6]

In order to draw a single line on the left and right lanes, draw_extrapolated_lines() function was added:
0. Receives a set of Hough lines as an input
1. Separates "left lanes" from "right lanes" depending on the slope value:
1.1 Created individual lists for left-hand-lanes and right-hand-lanes [lanes_left, lanes_right]
1.2 Taking into account opencv coordinate system, left-hand-lanes would have negative slove and right-hand-lanes would have positive slope. --> use as sorting criteria
2. Next step is to fit a line to all the segments of the left or right lane (consequently), Realised in extrapolate_lane() function:
2.1 Coordinates of all the lines added to a list to make a fit accross multiple data points
2.2 function returns 2 points that define start.point and end.point of fitted line.




### 2. Identify potential shortcomings with your current pipeline

Existing pipeline has quite a few shortcomings:
1. When lines segment are to short to be identified with given parameters of the Hough Transform function - no lines will be found - as a consequence for some frames there is no fitted line (flickering on the video stream)
2. Fur curved roads proposed approach would fail - extrapolating straight lines only
3. For high dynamic range of the lightning conditions many fake detections will apper (such as from shadows)
4. ROI is hard coded based on assumption of car movement, also ROI is depended on the resolution of the videp


### 3. Suggest possible improvements to your pipeline

Possible improvements:
1. To overcome shortcoming (1): create a "predictive line segment" that will save a rolling average of N extrapolated lines and plot it in case no HoughLines found for particular frame - this would allow to minimize flickering 
2.To overcome shortcoming (3): dynamic adjustment fo the Canny and HOugh functions required frame/by frame to account for the changes of the lighting conditions
