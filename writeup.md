# **Finding Lane Lines on the Road** 
---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:

* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./test_images_output/grayscale.jpg "Grayscale"
[image2]: ./test_images_output/blur_image.jpg "Grayscale"
[image3]: ./test_images_output/edges_image.jpg "Grayscale"
[image4]: ./test_images_output/masked_image.jpg "Grayscale"
[image5]: ./test_images_output/masked_edge_image.jpg "Grayscale"
[image6]: ./test_images_output/line_image.jpg "Grayscale"
[image7]: ./test_images_output/result.jpg "Grayscale"
[image8]: ./test_images_output/result_solid_line.JPG "Grayscale"
[image9]: ./test_images_output/challenge_segmented.JPG "Grayscale"
[image10]: ./test_images_output/challenge_solid.JPG "Grayscale"

---

### Reflection

### 1. Draw left and right lanes with segmented lines. 

My pipeline consisted of 6 steps. 

Step 1: I converted the images to grayscale.

![alt text][image1]7/18/2017 11:27:55 AM 

Step 2: I applied a Gaussian Noise kernel to smoothing the images.

![alt text][image2]7/18/2017 11:34:40 AM 

Step 3: I applied Canny to the image to the Gaussian-smoothed grayscaled images and detected with thresholds on the gradient.

![alt text][image3]7/18/2017 11:41:36 AM 

Step 4: I defined a four sided polygon to mask the interested area of images.

![alt text][image4]7/18/2017 11:42:26 AM 

And the edges detected image with mask is shown as below.

![alt text][image5]7/18/2017 11:45:08 AM 

Step 5: I applied Hough transform to detect the lines of lanes, which will simply be an array containing the endpoints (x1, y1, x2, y2) of all line segments detected by the transform operation. And the lines were plotted in red.

![alt text][image6]7/18/2017 11:47:37 AM 

Step 6: I drew the segmented lines on the original images.

![alt text][image7]7/18/2017 12:02:43 PM 

### 2. Modification of draw_lines() function for drawing a single line on the left and right lanes.

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by the following.

* Initial two global variables for storing previous left and right lines of lane.
* Separate the segmented lines to left and right lines based on locations of endpoints and their slope.
* In case of segmented lines assigning to left or right lines, current line is determined by averaging the position of left or right lines with numpy.polyfit.
* The detected line is determined based on the current line and previous line on the same side. 
* In case of no segmented lines assigning to one of the two lines and the other line is detected with little difference in slope (<30%) comparing previous line on the same side, the failed detected line is determined based on the previous line on the same side.
* Extrapolate to the top and bottom of each lane based on the four sided polygon for masking.
* Plot the left and right lines on the original images.

![alt text][image8]7/18/2017 4:16:19 PM 


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when the both left and right lines are failed to detected at the same image.  

Another shortcoming could be that if there are too many noise near the line, the line cannot be determined correctly. 

For example:

For segmented line detection.
![alt text][image9]7/18/2017 4:30:44 PM 

For solid line detection.
![alt text][image10]7/18/2017 4:30:51 PM 

### 3. Suggest possible improvements to your pipeline

A possible improvement would be to build a model to estimate the left and right line based on the lines detected previously.
