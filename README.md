# **Finding Lane Lines on the Road** 
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

This is the first Self-Driving Nanodegree (SDCND) Project, finding lane lines in a video. It is build on the [starter code provided by Udacity](https://github.com/udacity/CarND-LaneLines-P1). 
My solution can be found in my [Jupyter Notebook]().


## Processing Pipeline
0. Original image.
![](.\test_images\solidWhiteRight.jpg "Original image")
1. Convert to grayscale. I'm not averaging over the channels but instead just use the red channel to improve the value of the yellow lines. See below for an explanation and example.
![](.\example_images_writeup\pipeline1.jpg)
2. Slight blurring using Gaussian Blur to smooth out artifacts and noise.
![](.\example_images_writeup\pipeline2.jpg)
3. Canny Edge detection. Some try and error to find the parameters of 75 and 200. Within the limit of 1:2 to 1:3 that Canny recommended.
4. Do very rudamentry masking. The line classification should get rid of all unwanted lines.
5. Run Hough to find lines in the image.
6. Do line classification: Calculate the slope of the lines, choose the two highest slopes left and right. Only choose lines that have are within 80% of the slope of the highes and lowest slope.
7. Interpolate between the lines with similar slope. 
8. Overlay with original picture
	
## Shortcomings
#### Hough parameters
The hough function uses absolute values for threshold, min_line_len and max_line_gap. This works with the provided low resolution videos but totally breaks for the challenge video, which is a higher resolution. One solution would be the usage of relative values depending on the size of the image.
 ![](.\example_images_writeup\challenge_video_issue.jpg "Challenge video issue")

## Improvements
#### Grayscale conversion
I chose not to use the provided conversion to grayscale since it's just averaging over all channels. This isn't working very well on the yellow line in the second picture. Pure Yellow is [255,255,0] and in the picture it's a darker yellow at [235,201,77] (random sample). By just averaging we lose contrast here. One possibility would be a weighted average (e.g. squared) to give a bias to brighter monocromatic colors.
For this project it's more sensible to just use the red channel instead of averaging over all channels. This keeps most of the contrast without much computation. The red channel is the most useful because yellow is the strongest in the red channel. 
This is of course no universal approach. Lines of colors other than yellow, red, pink and white would be at a disadvantage to the point of beeing totally invisible.
#### Hough parameters

	