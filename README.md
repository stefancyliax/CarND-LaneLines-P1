**Finding Lane Lines on the Road** 
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

This is the first Self-Driving Nanodegree (SDCND) Project, finding lane lines in a video. It is build on the [starter code provided by Udacity](https://github.com/udacity/CarND-LaneLines-P1). 
My solution can be found in my [Jupyter Notebook]().


## Processing Pipeline
1. Convert to grayscale. Since the provided function only averages over the channels, I chose to just use the red channel as the greyscale image. This proved to be better performing than the grayscale conversion.
2. Slight blurring using Gaussian Blur to smooth out artifacts and noise.
3. Canny Edge detection.
4. Some rudimentary masking. The line classification should get rid of all unwanted lines.
5. Run Hough to find lines in the image.
6. Do line classification: Calculate the slope of the lines, choose the two highest slopes left and right. Only choose lines that have are within 80% of the slope of the highes and lowest slope.
7. Calculate mean slope and intercept and draw line between highest point and lower edge of the image.
8. Overlay with original picture.
![Pipeline](https://github.com/stefancyliax/CarND-LaneLines-P1/blob/master/output_images/Pipeline.png)
![Final Image](https://github.com/stefancyliax/CarND-LaneLines-P1/blob/master/output_images/Pipeline_final.png)

#### Videos (on Youtube)
[![IMAGE ALT TEXT HERE](http://img.youtube.com/vi/biHSV7n7X9A/0.jpg)](http://www.youtube.com/watch?v=biHSV7n7X9A)
[![IMAGE ALT TEXT HERE](http://img.youtube.com/vi/aPUVaNXrMkg/0.jpg)](http://www.youtube.com/watch?v=aPUVaNXrMkg)

Challenge Video: (High contrast algorithm is used here)
[![IMAGE ALT TEXT HERE](http://img.youtube.com/vi/B_TfCjRA9TE/0.jpg)](http://www.youtube.com/watch?v=B_TfCjRA9TE)

	
## Shortcomings
#### Classification of lines
The classification of lines falls short when there are lines on the road surface. Here a better classification would be needed, that isn't just dependent on the steepest slope. One possibility would be a plausibility check of the slope. For example only allowing the slope to be between 0.5 and 0.9.
#### Hough parameters
The hough function uses absolute values for threshold, min_line_len and max_line_gap that were derived from the provided low resolution videos. They don't work very well on a higher resolution video like the challenge video. One possible solution would be the usage of relative values depending on the size of the image.
#### Smoothing of line
in the videos the detected lane lines are jittering quite bad, especially in the challenge video. A smoothing between the frames would solve this. 


## Improvements
#### Grayscale conversion 
I chose not to use the provided conversion to grayscale since it's just averaging over all channels. This isn't working very well on the yellow line, especially on light road surfaces like in the challenge video.
By just averaging we lose all contrast here. One possibility would be a weighted average (e.g. squared) to give a bias to brighter monocromatic colors. See below for an algorithm enhancing the contrast.

For this project it's more sensible to just use the red channel as the grayscale image. This keeps most of the contrast without much computation. The red channel is the most useful because yellow is strongest in the red channel. 
This is of course no universal approach. Lines of colors other than yellow, red, pink and white would be at a disadvantage to the point of beeing totally invisible.


![](https://github.com/stefancyliax/CarND-LaneLines-P1/blob/master/output_images/grayscale_conversion.png)

#### Increasing the contrast of the lines
The challenge video is hard because there is very little contrast between the yellow line and the light surface. Besides having lots of shadows, tire marks and other noise.

I added a experimental line of code to the pipeline that increases the contrast of the picture, but slows the processing down significantly. The render time went from 18 seconds to 15 minutes. There may be a faster implementation though.

The code lowers the value of every pixel below the value of 230. This threshold is deliberatly choosen to separate the light road surface from the yellow line in the challenge video. See the effect in the picures above. The result of the algorithm is on the right.



	
