## Writeup


---

**Advanced Lane Finding Project**

The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points

### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---

### Writeup / README


### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

The code for this step is contained under **1. Camera Calibration** heading in the IPython notebook located under root dir as "P2.ipynb".

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 

![Camera Calibration](https://github.com/jay-thakur/Advanced-Lane-Finding/blob/master/output_images/camera_calibration.JPG)

### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.

Below are steps to demonstrate this - 
1. Prepared objpoints & imgpoints
2. Computed camera calibration and distortion coefficients on object points and image points.
3. Applied distortion correction to obtain the result.

I will describe how I apply the distortion correction to one of the test images like this one:
![distortion-corrected](https://github.com/jay-thakur/Advanced-Lane-Finding/blob/master/output_images/camera_calibration.JPG)


#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

This is header **2. Image Thresholding** in notebook. I used a combination of color and gradient thresholds to generate a binary image. Here's an example of my output for this step.

![Thresholded binary](https://github.com/jay-thakur/Advanced-Lane-Finding/blob/master/output_images/binary_threshold.png)

#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

The code for my perspective transform includes a function called `birds_eye_view()`, which is header **3 . Perspective Transform** in notebook.The `birds_eye_view()` function takes as inputs an image (`img`), I choose the `src` and destination (`dst`) points in the following manner with offset 200:

```
src = np.float32([
        [  588,   446 ],
        [  691,   446 ],
        [ 1126,   673 ],
        [  153 ,   673 ]])

dst = np.float32([[offset, 0], [img_size[0] - offset, 0], [img_size[0] - offset, img_size[1]], [offset, img_size[1]]])
```

I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

![Perspective Transform](https://github.com/jay-thakur/Advanced-Lane-Finding/blob/master/output_images/bird_eye_view.png)

#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

In header **4. Lane Detection & Fit with Polynomial** of Notebook I fit my lane lines with a 2nd order polynomial with `find_lane_pixels() & fit_polynomial()`. The output looks like this -

![Identified lane Pixels](https://github.com/jay-thakur/Advanced-Lane-Finding/blob/master/output_images/poly_fit.png)

#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

I did this in header **5. Radius of Curvature & Distance from Center** in notebook. 

#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

I implemented this step in header **6. Plotting Identified Lanes** in the function `plot_identified_lanes()`.  Here is an example of my result on a test image:

![alt text](https://github.com/jay-thakur/Advanced-Lane-Finding/blob/master/output_images/plotted_lane.png)

---

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](https://github.com/jay-thakur/Advanced-Lane-Finding/blob/master/project_video_output.mp4)

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

I used the same technique which has beenn described in lecture. Sliding window implementaion was really challenging for me. I hardcoded the src points for the perspective transform, by manually obtaining the src points based on the first frame lanes position. Here I can improve by automatically updating the src points each frame. This could improve the performance of the lane detection for the harder challenge video.  
