
---

**Finding Lane Lines on the Road**

---


### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

The goal of this project, is to create a pipeline that finds lane lines on the road. The different steps needed to create our pipeline are listed below:

* Convert image to grayscale for easier manipulation
* Apply Gaussian Blur to smoothen edges
* Apply Canny Edge Detection on smoothed gray image
* Trace Region Of Interest and discard all other lines identified by our previous step that are outside this region
* Perform a Hough Transform to find lanes within our region of interest and trace them in red
* Separate left and right lanes
* Interpolate line gradients to create two smooth lines

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by using the slope-intercept form to genrate the connecting lines.
To be able to trace a full line and connect lane markings on the image, we must be able to distinguish left from right lanes.
If we observe the edge-detected image, we can derive the gradient (i.e slope) of any left or right lane line:
* left lane: as x value (i.e. width) increases, y value (i.e. height) decreases: slope must thus be negative
* right lane: as x value (i.e. width) increases, y value (i.e. height) increases: slope must thus be positive
We can therefore define a function that separates lines into a left and right lane. We must be careful when the denominator of the gradient (the dx in dy/dx) is 0, and ignore any line with such line.

To trace a full line from the bottom of the screen to the highest point of our region of interest, we must be able to interpolate the different points returned by our Hough transform function, and find a line that minimizes the distance across those points. This is done by generating a equation of line through polynomial functions in python.

---

### 2. Identify potential shortcomings with your current pipeline

One potential shortcoming would be what would happen when we detect curved lanes. This scheme will not work well on curved lanes. 

Another shortcoming could be of inaccurately tuned Hough transform parameters. Thigns may get bad if there are drastic changes in images.

---

### 3. Suggest possible improvements to your pipeline
This initial implementation worked passably well for the two basic videos, but does not work good on the challenge quiz. 
A possible improvement would be to have some kind of interpolation between subsequent frames to discard lines that deviate too much

Another potential improvement could be to smoothen the lane detection between subsequent frames through averaging.
