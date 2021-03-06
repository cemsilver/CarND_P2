## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

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

[image1]: ./Writeup_images/camera_undistort.png "Undistorted"
[image2]: ./Writeup_images/ud_testimage1.jpg "Road Transformed"
[image3]: ./Writeup_images/binary_result.JPG "Binary Example"
[image4]: ./Writeup_images/perspective_transform.JPG "Warp Example"
[image5]: ./Writeup_images/fit_polynomial.JPG "Fit Polynomial"
[image6]: ./Writeup_images/search_around_poly.JPG "Pipeline image"
[image7]: ./Writeup_images/pipeline.JPG "Pipeline image"
[video1]: ./output_videos/pipeline_project_video.mp4 "Video"

## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points

### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---

### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Advanced-Lane-Lines/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!

### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

The code for this step is contained in the first code cell of the IPython notebook located in "CarND_AdvanceLaneFinding.ipynb". 

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 

![alt text][image1]

### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.

I used `undistort_image()` function I defined in part "1 Camera calibration & distortion corrected image" row 43-46, in order to apply distortion correction. Result can be seen below:
![alt text][image2]


#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

I used a combination of color and gradient thresholds to generate a binary image. Here's an example of my output for this step.  

![alt text][image3]

#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

The code for my perspective transform includes a function called `perspective_transform()`, which is on part 3 Perspective transform of my .ipynb file.  The `perspective_transform()` function takes as inputs an image (`img`), as well as source (`src`) and offset. Instead of giving the  destination (`dst`) point as an input to the function, I calculated it from (`src`) points and (`offset`) paramters as shown here: 
    (`dst = np.float32([[offset, 0],
                      [w-offset, 0],
                      [offset, h],
                      [w-offset, h]])
                      `)
                      
I verified that my perspective transform was working as seen in the image below.

![alt text][image4]

#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

Then I applied `fit_polynomial()`function to the warped image and fit my lane lines with a 2nd order polynomial kinda like this:

![alt text][image5]

After that, as shown on the part 4.2 Skipping windows and searching around previous detected line, I draw the area between two lane lines.
![alt text][image6]

#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

I did this in lines given in part 5. Determining curvature of the lane & vehicle position.

#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

I implemented this step in part 6 as Combination of steps Also I defined a function called `pipeline()` to make it more convenient to run with images and video.  Here is an example of my result on a test image:

![alt text][image7]

---

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](./output_videos/pipeline_project_video.mp4)

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

The pipeline tends to work unstable when the car passes too many shaded areas like nearby trees.
In order to make it more robust, perspective transform can be worked through a little bit more.
