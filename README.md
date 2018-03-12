## Advanced Lane Finding

[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

[img_raw]: ./imgs/img_raw.png "Raw Image with Chessboard"
[img_undist]: ./imgs/img_undist.png "Undistorted Image with Chessboard"
[img_hsl]: ./imgs/img_hsl.png "Image on HSL Color Space"
[img_warped]: ./imgs/img_warped.png "Warped Image to Bird-eye's View"
[img_proc]: ./imgs/process.png "Process Flow"
[img_roi]: ./imgs/roi.png "ROI on the Image"
[img_curve_fit]: ./imgs/curve_fit.png "Curve Fitting"
[img_sliding_window]: ./imgs/sliding_window.png "Sliding Window for Line Detection"

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

![][img_roi]

![][img_warped]

### Image Preprocessing
The image processing method is consisted of two parts:
- Filter out lane line by converting to HSL color space
- Filter out vertical lines by Sobel filter on x-direction

The image was converted to HSL color space, shown as below, due the study in first project that the yellow and white lane lines are easier to be detected than RGB. Vertical lines were detected by Sobel filter but with some noises. The ROI implemented in the first project was applied to filter out irrelevant noises.

ROI: point_left_up = (0.25 * cols, 0), point_right_bottom = (0.75 * cols, rows)

![][img_hsl]

### Lane Detection & Curvature Estimation
The whole process is shown in the below image, and the detailed information will be discussed later.

![][img_proc]

The lane detection was achieved by the method proposed by the lesson which impressed me by using sliding window. It used the peaks in the histogram respect to columns as a start point, and seek up to find the center of the pixels in the window. After that, there are a point sets which can be used to find the fittest 2nd order polynomial function to these points.
In order to deal with some false detection, the curvatures were recorded as history to calculate the confidence of this round's result. What I implemented is to compare the RMS of the differences between polynomial function's parameter with history. If the sum of RMS is bigger than a thresold, then this round's result will NOT be cached and output the previous result.

![][img_sliding_window]
![][img_curve_fit]

### Show Detected Result
I've tested my algorithm on 'project_video.mp4' which looks not bad, but it messed up in 'challenge_video.mp4'. I think it might be some parameters tuning, such as ROI range, which I do not have enough time to figure out how to improve my algorithm.
