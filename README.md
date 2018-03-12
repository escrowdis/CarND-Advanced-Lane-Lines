## Advanced Lane Finding

[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

[img_raw]: ./imgs/img_raw.png "Raw Image with Chessboard"
[img_undist]: ./imgs/img_undist.png "Undistorted Image with Chessboard"
[img_hsl]: ./imgs/img_hsl.png "Image on HSL Color Space"
[img_warped]: ./imgs/img_warped.png "Warped Image to Bird-eye's View"
[img_proc]: ./imgs/process.png "Process Flow"


The Project
---

The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

---

This project was implemented before in school, which I can finish this faster than other projects. There are some parts need to be done.

### Camera Calibration
In order to fine corners in chessboard, OpenCV's functions, findChessboardCorners and calibrateCamera, were applied to estimate the distortion coefficients and distance of the camera. The calibration was saved as 'calib_file.p' for later use which only need to calculated once. This assumption is satisfied if the data was recorded after camera calibration ASAP, or the calibration file may failed to calibrate the camera intrinsic parameters perfectly.

| Raw | Undistorted|
|-|-|
|![][img_raw]|![][img_undist]|

### Perspective Transform
In order to calculate the curvature of lane line, the perspective of the image must be changed to bird-eye's view. The Transformation matrix was estimated by OpenCV's getPerspectiveTransform. The paramters used in getPerspectiveTransform was referred to the lane line detection project which made straight line look straight on bird-eye's view.

![][img_warped]
![][img_hsl]

### Image Preprocessing
The image was processed by two parts:
- Filter out lane line by converting to HSL color space
- Filter out vertical lines by Sobel filter on x-direction

The image was converted to HSL color space due the study in first project which can filter out lane line better than RGB.  Vertical lines were found but with some noised. The ROI implemented in project 1 was applied to filter out irrelevant noises.

### Lane Detection & Curvature Estimation
The lane detection was achieved by the method proposed by the lesson which impressed me by using sliding window. In order to deal with some false detection, the curvatures were recorded as history to calculate the confidence of this round's result. What I implemented is to compare the RMS of the differences between polynomial function's parameter with history. If the sum of RMS is bigger than a thresold, then this round's result will NOT be cached and output the previous result.

![][img_proc]

### Show Detected Result
I've tested my algorithm on 'project_video.mp4' which looks not bad, but it messed up in 'challenge_video.mp4' which I do not have enough time to figure out how to improve my algorithm.
