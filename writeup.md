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
[image1]: ./writeup_imgs/binary.png "Binary image"
[image2]: ./writeup_imgs/bird-eye-view.png "Bird eye view"
[image3]: ./writeup_imgs/channels.png "Different channels visualization"
[image4]: ./writeup_imgs/chess-corners.png "Chessboard corners"
[image5]: ./writeup_imgs/final.png "Final fit"
[image6]: ./writeup_imgs/line-detection.png "Line detected"
[image7]: ./writeup_imgs/undistored-chessboard.png "Undistorted chessboard"
[image8]: ./writeup_imgs/undistored.png "Undistorted image"

### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

The code for this step is contained in cells 2 and 3 of the IPython notebook located in "./Advanced_Lane_Finding_Project.ipynb".  

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection. Here is an example of the corners found on the images.

![alt text][image4]

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function  and obtained this result:

![alt text][image7]

### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.

To demonstrate this step, I will describe how I apply the distortion correction to one of the test images like this one:

![alt text][image8]

#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

First, for different color spaces, I check on a grayscale visualization which channels had the best potential for line detection.

TODO
![alt text][image3]

At the end, I used saturation channel from HLS for color features and combined it with the gradient of the Y of the YUV space(Codes goes from cell 9 to 15).

![alt text][image1]

#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

On cell 6 I check several src point location looking a good filter. Here are the source and destination points:

| Source        | Destination   |
|:-------------:|:-------------:|
| 530, 450      | 100, 100        |
| 190, 660      | 100, 620      |
| 1090, 660     | 1180, 620      |
| 1090, 450      | 1180, 100        |

I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

![alt text][image2]

#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

I used sliding window technique show in the class to select pixels over the line. Then I use np.polyfit to fit a second-order polynomial.

![alt text][image6]

#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

In cell 20, method update_curvature, I use Numpy operations to compute each line curvature.

#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

The code is in cell 21, method detect_lines after the fit is updated, lines are draw on an empty canvas, using the inverse transformation image is warp back, and then combine with the undistorted picture.

![alt text][image5]

---

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](./test_videos_output/project_video.mp4)

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

Here I'll talk about the approach I took, what techniques I used, what worked and why, where the pipeline might fail and how I might improve it if I were going to pursue this project further.  
