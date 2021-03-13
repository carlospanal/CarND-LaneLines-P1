# **Finding Lane Lines on the Road** 


---


The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[image1]: ./test_images_output/out_solidWhiteCurve.jpg "output"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

I created 2 pipelines. 
They work the same way, but the first one takes a list of images as an input instead of a single image and,
unlike the definitive pipeline, its region of interest is specifically designed for the size of training images.  
The first one was used for training images and includes a widget that lets you control the hough function parameters 
and also lets you move through the list of images.  
Initial values for parameters of this widget are the same as the ones chosen for both pipelines, and its use
doesn't affect the stored output of the involved training images.  

The definitive pipeline takes an image as an input and consists on the following steps:  

  1 - Convert intial RGB image to grayscale using grayscale()  

  2 - Apply smoothing to the image using gaussian_blur()  

  3 - Use Canny algorithm on the image to detect edges using canny()  

  4 - Define a region of interest, depending on the image size and using  np.array()  

  5 - Apply defined region to the output of canny() function using region_of_interest()  

  6 - Calculate Hough Lines using hough_lines()  
  
After creating this definitive pipeline, i started modifying draw_lines() so that function
was able to draw 1 averaged lines for each lane instead of all the detected hough lines.
Basic steps of this strategy:  

  1 - Group lines as left or right lines depending on their slope  
  2 - Use polyfit() and poly1d() to fit a line through each group of points  
  3 - Set starting and end points of each averaged lane considering the fitted line and image dimensions  
  
Regarding the challenge, I partially completed it by making these modifications:  

  1 - Define region of interest based on image dimensions instead of "hardcoding" it  
  2 - Define 2 global variables that store last averaged lines so the program doesn't break  
      when there is frame with no detected lines in some of the lanes.  
  3 - Set a minimal slope so horizontal lines are discarded.  
  
 The video of this challenge is still unstable from 0:04 to 0:05.  
 I tried some strategies like lowering the canny low and high threshold, but it had a negative effect on the previous videos.  


Output of the pipeline when applied to a training image:  
![alt text][image1]  

[link to folder containing videos on their final state](test_videos_output)  

[link to folder containing videos of different steps of the process](other_outputs)  


### 2. Identify potential shortcomings with your current pipeline


Changes on light instensity or road colors can break this pipeline, as happens with the last challenge video.   
Another problem is that this process is not considering the fact that there could be a much closer car on the same lane.  
In addition, the system is not robust against cracks on the road or in the car front glass.  


### 3. Suggest possible improvements to your pipeline

Averaged lines have some kind of vibration due to the movement of the car, a PID could be easly implemented to correct that.  
Another potential improvement, but a bit more complicated one, could be to divide the image in order to apply canny algorithm
with different values to different parts of the image depending on local changes on road colors.  
I tried to fix the problems of the optional challenge video lowering canny thresholds, as I explained before, but that 
just creates new problems on different videos and different moments of the same video.
