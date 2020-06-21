# **Finding Lane Lines on the Road** 

---

**Project Name: Finding Lane Lines on the Road**

**Project Goals:**
* Make a pipeline that finds lane lines on the road
* The code should work for both images and videos
* The lanes lines identified should be close to the real lane lines visually

[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"
[image2]: ./examples/laneLines_thirdPass.jpg "Final"

---

## Reflection

### 1. Pipeline description. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps:
 - First, I converted the images to grayscale.
 
 ![Grayscale Image][image1]
 
 - Then I applied a Gaussian Noise kernel to blur the gray image.
 - Next, I use Canny Edge Detection to generate an image with edges only. I also used a predefine set of vertices to black out anything outside a polygon.
 - Then I applied Hough Transform to generate lines which were later drawn on the image with edges to return an image with lines.
 - Finally, I combined image with lines and original image under a proper weighted ratio to get the finally image I want.
 
 ![Grayscale Image][image2]
 
In order to draw a single line on the left and right lanes, I modified the draw_lines() function by calculating and comparing the slope of each lines generate by Hough Transform. If the slope of the line is within the left lane's slope range, then it is appended to left lane lines. If the slope of the line is within the right lane's slope range, then it is appended to right lane lines. If the line is neither left lane line or right lane line, then it is dropped. After collecting all left and right lane lines, I averaged the coordinates of all lines in either left or right lane line set and generated one single line for each set. The result of this is two sets of first order polynomial coefficients (m and b), one for left lane and one for right lane. I used these coefficients together with the desired end points (fixed y coordinates) to draw two solid lines on the original image. See definition of function draw_lines() for details.

### 2. Identify potential shortcomings with your current pipeline

One potential shortcoming would be what would happen when there are outliers in the output image from Canny Transform. The Hough Transform will fail because it will consider the outliers as important data points, resulting in incorrect line generation

Another shortcoming could be that when the user would like to tweak the parameters of Gaussian Noise kernel, Canny Transform or Hough Transform, there would be no way be cause all the parameters are fixed in the source code without any interface to the user.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to improve the robustness of the code by introducing advanced computer vision techniques such as RANSAC to handle outliers

Another potential improvement could be to parameterize the code and make interfaces so that users have more flexibility to interact with the code.
