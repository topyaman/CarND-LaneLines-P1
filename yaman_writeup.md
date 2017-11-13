# **Finding Lane Lines on the Road** 


**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./images/SolidWhiteRight1.jpg

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of several steps:

          Grayscaling the frame
          Reducing the noise in the grayscaled frame
          Determining the edges within the frame
          Drawing the lines of the lane using the information provided by the detected edges
          
In order to draw a single line on the left and right lanes, I modified the draw_lines() function by first capturing the relevant data for the right and left hand lines. This was done by determining whether the detected line had a positive or negative slope. Remember that the y-axis is inverted, so lines with positive slopes are actually the right hand line, and lines with negative slopes are the left hand line. 

The slope and y-axis intercept of each line was then calculated and stored in one of four different arrays(left/right slope, left/right intercept). This could have been reduced to two, two axis arrays but for the sake of clarity and simplicity four arrays were used.

The average slope and average y-axis intercept of the current and all prevously detected right hand lines was computed. The averaged slope and y-axis intercept where then used to interpolate the lines extending along the region of interest.

Using the equation of a line: y = mx + b where: 
          m = average of all left/right line slopes
          b = average of all left/right line y-axis intercepts
          y = the min and max of the y-axis regions of interest, in this case 315 and 540.
 
The x intercept for our interpoloated line was computed. This process was repeated for both the left and right lines.

![image1]:

### 2. Identify potential shortcomings with your current pipeline

There are several shorcomings of the current pipeline. First, because all historical slope and y-axis intercept data is included when interpolating the full line, the pipeline will take to produce a line that properly convergers to the actual line in the image in the event of a sudden change in the orientation of the lane(such as a bend or corner turn)

Also, the parameters have been turned to the type of lane markings found in this video, but if the lane markings where to change, it could have significant implications on the accuracy of the pipeline.


### 3. Suggest possible improvements to your pipeline

An imporvement would be to only consider the slope and y-axis data from lines detected in the current frame when attempting to interpolate a line of the full line of interest. This was attempted and while results did trend well, it did however cause jitter in the line that was in some instances very significant due to the lack of historical information that would help the algorithm determine where the line may have been in previous frames. Bounding the limits of the average slope and y-axis could alieat this but severly restrict the operating window the pipeline.
