#**Finding Lane Lines on the Road** 

The goals / steps of this project are the following:

* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./test_images_output/images.png "Image pipeline"

---

### Reflection

###1. Describe your pipeline. As part of the description, explain how you modified the `draw_lines()` function.

***Pipeline steps:***

* define region of interest
* convert the image to grayscale
* gaussian blur the image
* canny edge detection
* region of interest filter applied to the canny edges
* hough lines
* interpolate and average the lines

In order to draw a single line on the left and right lanes, ***I modified the `draw_lines()` function to do the following***:

* determine the slope/gradient of the line.
* if the slope was negative, and on the left hand side of the frame, it was considered to be part of the left lane.
* if the slope was positive, and on the right hand side of the frame it was considered to be part of the right lane.
* these slopes are the inverse of how they appear in images, because the y-axis is inverted (y=0 is at the top), hence why the left slope to look for is negative, and the right, positive.
* interpolate the x values (the y values are fixed) for each lane
* add interpolated values to the buffer (deque data type) for each lane
* if buffer has > 3 values in it then return the average of the x values in the buffer, otherwise the recently calculated interpolated x values are kept
* draw each lane

***The image below shows the pipeline steps***. Each image processed is in its own column. `Right-click > Open image in new tab` to see the full size image.

![alt text][image1]


###2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when

* if the brightness of the image where to dampen to such a level that edges cannot be found clearly from the grayscale images.

Another shortcoming could be

* on sharp corners/curves, as the interpolation is using linear functions, the curve would effectively have a secant cutting across it.


###3. Suggest possible improvements to your pipeline

A possible improvement would be to

* Have a non-linear function for the interpolation

Another potential improvement could be to

* Utilise other colour channels for edge detection, such as HSV (Hue Saturation Value), and add them to the `lines` that are passed to the `draw_lines()` function