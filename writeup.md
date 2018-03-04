# **Finding Lane Lines on the Road** 
---

**Finding Lane Lines on the Road**

The goals/steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 6 steps.

- Converting the images to grayscale
- Applying Gaussian smoothing filter onto the grayscale image
- Applying the Canny algorithm to extract the edges 
- Using ROI filter reduce the noise, such as tree and other cars
- Apply the hough_line transformation to extract the lane 
- Draw the line on the initial image 


In order to draw a single line on the left and right lanes, I modified the draw_lines() function by using the average strategy. The workflow is following

1. For each line, according to the slope to decide that which line belongs to the left side, which belongs to the right one.
    - slope = (y2-y1)/(x2-x1)
    - slope > 0.0 , the line belongs to the left, store the two points (x1,y1) (x2,y2) into left set
    - slope < 0.0, the line is part of the right segments, store the two points (x1,y1) (x2,y2) into right set
2. Calculate the average position and average slope of each set. Then each line of the lane is determined.
3. Computing the endpoint on the top and bottom for left and right lines.
    - According the formula (y_ref-y_tar)/(x_ref-x_tar) = slope, (x_ref,y_ref) is the result shown above.
    - The target y_tar is determined by ROI and the size of the image shape.
    - Finally, the two-point (p_min as well as p_max) for each line are calculated.
4. According to endpoints from step 3. The two lines for the lane can be drawn onto the initial image. 
   

If you'd like to include images to show how the pipeline works, here is how to include an image: 

![alt text][image1] [image1]: ./examples/grayscale.jpg "Graysca


### 2. Identify potential shortcomings with your current pipeline

Using average strategy is easy but not robust. The end effect depends on lane itself and the setting of ROI.

The lane could be detected wrong when the line is not totally straight.
Now the lane is determined by the average position and average slope of lines. If the lane is not so straight, there will be a large difference between the slope of the front part of a line and the rear one. The average of the slope will be meaningless.


If ROI is set not correctly, more and more noise will be introduced. Then the lane detection will be more inaccurate.



### 3. Suggest possible improvements to your pipeline

A possible improvement would be that using the filter to reduce the extra line.
For instance, Kalman filter could be used to remove the line, which is not the part of the lane.

Another potential improvement could be to use extrapolate instead of average to enhance the robustness.

