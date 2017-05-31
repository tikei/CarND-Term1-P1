# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. First, I converted the images to grayscale, then I added Gaussian blur, to reduce noise.
Then I applied Canny transform to detect the edges in the image.
The next step was to apply a region of interest, essentially masking all areas in the image, where no road lines are expected.

The final step was to apply a Hough transform to only select the edges that represent lines. The parameters of the Hough transform were manually selected.  

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by:
First, separating the lines detected by the Hough transform into left and right lines. This was done based on the slope of the line.
Left lines have slope of less than -0.5 and right lines a slope greater than 0.5 (threshold of 0.5). The treshold was chosen after the Challenge video made the
algorithm plot lines close to horizontal.

After separating the lines into left and right buckets, I found the coefficients of the linear (quadratic for the Challenge) fit on the left and right using numpy.polyfit.
I then evaluated the line at the appropriate 2 points, after which the line was drawn.

For the Challenge I tried using a quadratic fit in order to draw the left line as a curve instead of straight line. The resulting video is in the test_videos_output directory. 

### 2. Identify potential shortcomings with your current pipeline

The Challenge showed a major shortcoming in a way that all parameters (region of interest, Hough parameters) are chosen manually.
When the car drove over a brighter patch of road, the algorithm failed to correctly identify the left lane line and instead was identifying the crash barrier wall. 


Another shortcoming is drawing straight lines in a road turn, which does not accurately represent the lane line.  



### 3. Suggest possible improvements to your pipeline

A possible improvement would be to have a moving average for the lines, so that temporary (short term) changes in the brightness conditions and background do not affect the accuracy of the plotted line. This means the current line parameters are combined with the average of the line parameters of the previous n images. This will cause a slight inaccuracy (delay) in the current line, but will improve the reaction to temporary noise.   

Another potential improvement could be to draw quadratic curves instead of lines. 

One can also think how to automatically select the Hough parameters and the region of interest vertices.
