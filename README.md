# Advanced Lane Finding
---

![](https://github.com/Luzhongyue/Advanced-Lane-Lines/blob/master/output_images/video.png)
[video](https://github.com/Luzhongyue/Advanced-Lane-Lines/blob/master/project_video_output.mp4)

## Overview

In this project, the goal is to write a software pipeline to identify the lane boundaries in a video.

The steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

## Camera Calibration

The code for this step is contained in the first thre code cell of the IPython notebook located in **Advanced_Lane_Lines.ipynb**  which is 
done with the provided images in the **camera_cal** folder.

The process as bellow:

(1)preparing "object points",  I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same 
for each calibration image. 

(2)Input the images

(3)Transform to gray spaces

(4)Use **cv2.findChessboardCorners()** to find the corners on the chessboard as imgpoints.

(5)using the **cv2.calibrateCamera()** to compute the camera calibration and distortion coefficients

I then used the output objpoints and imgpoints to compute the camera calibration and distortion coefficients using the **cv2.calibrateCamera()** function,then use this distortion correction to the test image using the **cv2.undistort()** function.

The result as below:

![](https://github.com/Luzhongyue/Advanced-Lane-Lines/blob/master/output_images/chessboard.jpg)

## Pipeline(single image)

### 1.Distortion Correction

Using the paremeter from  the function **correction_cofficient()** and  **cv2.undistort()** to get the undistort image,the image as showed 
as below:

![](https://github.com/Luzhongyue/Advanced-Lane-Lines/blob/master/output_images/undistort.jpg)

### 2.Color Threshold and Perspective Transform

As is known to us all, The road alternated between light and shade, with occasional shading, but the yellow and white lane lines were 
always there. The white lane lines in the image can be segmented according to the L (brightness) channel in the HSL model, and the yellowlane lines in the image can be segmented according to the b (blue and yellow) channel in the Lab model. Then the two segmentation resultsare combinedand superimposed on one picture to obtain two complete lane lines.Lastly, I did perspective transform after applying my thresholds on the image. This was done with the **getPerspectiveTransform()** and **warpPerspective()** funtions.
The result as shown as below:

![](https://github.com/Luzhongyue/Advanced-Lane-Lines/blob/master/output_images/test1_output.png)
![](https://github.com/Luzhongyue/Advanced-Lane-Lines/blob/master/output_images/test2_output.png)
![](https://github.com/Luzhongyue/Advanced-Lane-Lines/blob/master/output_images/test3_output.png)
![](https://github.com/Luzhongyue/Advanced-Lane-Lines/blob/master/output_images/test4_output.png)
![](https://github.com/Luzhongyue/Advanced-Lane-Lines/blob/master/output_images/test5_output.png)
![](https://github.com/Luzhongyue/Advanced-Lane-Lines/blob/master/output_images/test6_output.png)


### 3.Lane-line Pixels Detection and Fit Their Positions with a Polynomial

the function **find_lane_pixels()** use a slide window to find the lane-line pixels. after get the lane_pixels, use **np.polyfit()** to get polynomial paratmeters. To visualize the search result and fit polynomial, use **fit_polynomial()** function or **search_around_poly()** function. 
The visualized result as showed respectively as below:

![](https://github.com/Luzhongyue/Advanced-Lane-Lines/blob/master/output_images/test6_rb.png)
![](https://github.com/Luzhongyue/Advanced-Lane-Lines/blob/master/output_images/test6_g.png)

### 4.Measuring Curvature

The function **curvature()** used to calculate radius. Firstly, transform the lane piont from pixels to meters, then fit these data use 
**np.polyfit()** funcion. After geting the polynomial parameter, use the function **R = (1+(2Ay+B)^2)^3/2 / (|2A|)** to calculate the 
curvature. 

### The Position of the Vehicle with Respect to Center

The position of the vehicle with respect to the center of the lane is calculated with function **position_from_center()**. Firstly, 
tranfer pixel to meter, then compare the lane center with picture center to get offset. 

### 5.Impose Lane Boundaries on Original Image

Using functions **cv2.getPerspectiveTransform()** and **cv2.warpPerspective()** to waraped the image from bird_view to normal. Lastly, using **cv2.putText()** to display the curvature and the position of the vehicle with respect to the center.
The result showed below:

![](https://github.com/Luzhongyue/Advanced-Lane-Lines/blob/master/output_images/test6_v.png)

## Pipeline(video)

Here is a link of my video result:![project_video_output](https://github.com/Luzhongyue/Advanced-Lane-Lines/blob/master/project_video_output.mp4)


