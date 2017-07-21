# **Finding Lane Lines on the Road** 

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./test_images_output/solidWhiteCurve.jpg "Pipeline Output"
[image2]: ./test_images_output_lanes/solidWhiteCurve.jpg "Pipeline Output with Lane Lines"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. For every image:
1. I converted the original image to grayscale.
2. I applied a Gaussian blur on the image using a kernel size of 5x5 pixels. 
3. I applied the Canny transform on the blurred image. Low/high thresholds of 80/200 were used.
4. A mask was applied to the gradient image. The mask was defined as a quadrilateral with vertices proportional to the dimensions of the image, forming an "almost triangle" with the bottom edge of the image as the base.
5. Hough lines were obtained using rho=1, theta=1deg, threshold=30, mininum line length=50, maximum line gap=150
![image1]

In order to draw a single line on the left and right lanes, I modified the draw_lines() function.
Lines were placed in two groups - left lane and right lane - depending on their gradient value. Those with positive gradients were placed in the "right lane" bucket, and negative gradient lines in the "left lane" bucket. For each lane, the average gradient and position were calculated. Using these values the lines were extrapolated to form the two lines on the left and right lanes.
![image2]

### 2. Identify potential shortcomings with your current pipeline

One potential shortcoming would be what would happen when the lane lines are not very straight. The pipeline assumes that taking the average of the gradient of each lane is a good representation of the lane lines. However, if the lanes are curving significantly, then it would make sense to create lane lines that are curved rather than just a straight average.

Another shortcoming could be when other cars (especially large trucks) in front are changing lanes, lane lines may be obstructed significantly, and the lane line estimation may be momentarily unavailable. There needs to be a way that lane lines could still be predicted based on previous estimations even when the visibility of the lines are lost briefly. 

Finally, when there are other visible "lines" or edges in the region of interest, these will impact the accuracy of the lane estimations, since these will also be considered as part of the lane lines. Their gradients and positions will influence the averages calculated as the lane lines.

### 3. Suggest possible improvements to your pipeline

A possible improvement would be in the draw_lines() function. Lines corresponding to the "left" or "right" lanes were split fairly arbitrarily - the sign of the gradient is not likely to be the conclusive way of determining whether a line should belong to the left or the right lane line. We could consider position, or maybe use a smarter clustering algorithm to group the lines into the two clusters. 

Another potential improvement could be to isolate the lines that actually belong to lane lines more effectively. This could be using a combination of a more refined masking area of interest, using the information about the position and gradient of each line to determine whether the line belongs to a lane line, etc.
