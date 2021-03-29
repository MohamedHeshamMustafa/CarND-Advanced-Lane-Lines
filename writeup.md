## Writeup Template

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

[//]: # (Image References)

[image1]: ./examples/undistort_output.png "Undistorted"
[image2]: ./test_images/test1.jpg "Road Transformed"
[image3]: ./examples/binary_combo_example.jpg "Binary Example"
[image4]: ./examples/warped_straight_lines.jpg "Warp Example"
[image5]: ./examples/color_fit_lines.jpg "Fit Visual"
[image6]: ./examples/example_output.jpg "Output"
[video1]: ./project_video.mp4 "Video"

## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points

### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---

### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.
You're reading it!

### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.
  I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function 

### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.

To demonstrate this step, I will describe how I apply the distortion correction to one of the test images:
I used `cv2.undistort` which take a dsitorted img and a camera matrix, and the dist coeff and returns undistorted img
Examples of Images after and before distortion can be seen in my Ipython notebbok section named (Apply Distortion Correction)

#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

I used a combination of color and gradient thresholds to generate a binary image where I used `abs_sobel_shreshold`
, `Magnitude of Gradient` and `Direcotion of the gradient` where I combined them in `combined_threshold` to get a better lane detection , Note(The input for those gradient was an image in HLS color domain where the input where S channel) 
code of this section can be seen in my Ipython notebook section (Gradient & color threshold)

#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

The code for my perspective transform includes a function called `Image_Wrapping()`, which appears in (Prespective Transform & Image Wrapping section) in the oslution `Image_Wrapping()` function takes as inputs an image (`img`), as well as source (`src`) and destination (`dst`) points.I chose the hardcoded the source and destination points where I assumed a fixed places for the points, this code can be seen in my section named (Prespective Transform & Image Wrapping)
examples of out put images can be seen in output_images folder and also appears in my notebook solution

#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?
 in section (Sliding Window and find lane fitting) I used the function `fit_polynomial()` and `find_lane_pixels` that takes a histogram of the bottom half of the image then Create an output image to draw on and visualize the result `out_image`, it Find the peak of the left and right halves of the histogram, These will be the starting point for the left and right lines
After that it Set height of windows - based on nwindows above and image shape then dentify the x and y positions of all nonzero pixels in the image then calculate Current positions to be updated later for each window in nwindows,
The function also Step through the windows one by one and Identify window boundaries in x and y (and right and left)
Also Extract left and right line pixel positions.
As for `fit_polunomial()`  Find our lane pixels first leftx, lefty, rightx, righty, out_img
 from `find_lane_pixels(b)` then Fit a second order polynomial to each using `np.polyfit`

#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

I did this in section (Calculate lane curvature) that defines a `measure_curvature_pixels()` that Calculates the curvature of polynomial functions in meters and tested my curvature in (Test Curvature) section
#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.
in section (Final Step, Draw Line) you will see some photos I tested on the final pipeline 
examples photo of this section can be found in both Ipython (project.ipynp) and also my output_images folder
### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](./out_test_video.mp4)

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

Here I'll talk about the approach I took, what techniques I used, what worked and why, where the pipeline might fail and how I might improve it if I were going to pursue this project further. 
one of the things I noticed is that when applying threshold in (gradient and color threshold) section is that using HLS color space is better that it helps more in identifying the white lanes in the image.
The probelm I faced was detecting the curvature of the lane better and I guess it can be improved in the future trials.
My pipeline will likely fail if the road has a big curvature angle
one thing will make it better in this case is to use DL techniques to avoid such things


