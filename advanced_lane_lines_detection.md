## Advanced Lane Finding

---
**Goals**

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

[image1]: ./output_images/Original.jpg "Original"
[image2]: ./output_images/Undistorted.jpg "Undistorted"
[image3]: ./output_images/Original_frame.jpg "Original"
[image4]: ./output_images/Undistorted_frame.jpg " Undistorted"
[image5]: ./output_images/threshold.png "Threshold"
[image6]: ./output_images/straight.png "Straight"
[image7]: ./output_images/transform.png "Transform"
[image8]: ./output_images/masked.png "Masked"
[image9]: ./output_images/histogram.png "Histogram"
[image10]: ./output_images/window.png "Window"
[image11]: ./output_images/visualization.png "Visualization"
[image12]: ./output_images/output.png "Output"
[image13]: ./output_images/output2.png "Output2"


[video1]: ./output_video/project_video_output_final.mp4 "Video"

---

### PART 1: Camera Calibration


The code for this step is contained in the second code cell of the IPython notebook located in "advanced_lane_detection.ipynb" 

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 

#### Original Image
![alt text][image1]

#### Undistorted Image
![alt text][image2]

### Pipeline (single images)

#### 1. Distortion Correction

To demonstrate this step, I will describe how I apply the distortion correction to one of the test images like this one:

Original and Undistorted frame are shown in below images. After calibrating the camera, I used `cv2.undistort` function to undistort the original image.

#### Original Image
![alt text][image3]

#### Undistorted Image
![alt text][image4]

#### 2. Thresholding

I used a combination of color and gradient thresholds to generate a binary image. ~~First, I converted Original image to HLS colorspace. Then, I have splitted it into H,L and S channel and used S channel for color thresholding~~ I used R channel from RGB color space (because I was getting some errors in s channel) and combined it with x gradient thresholding.  Here's an example of my output for this step.  (note: this is not actually from one of the test images)

![alt text][image5]

#### 3. Perspective Transformation

I started with the straight road images to select the `src` and `dst` points. Based on that I have selected my source and destination points as shown below.



| Source        | Destination   | 
|:-------------:|:-------------:| 
| 705, 460      | 960, 0        | 
| 1110, 720     | 960, 720      |
| 205, 720      | 320, 720      |
| 560, 460      | 320, 0        |


I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

![alt text][image6]

My perspective transform result looks like following:

![alt text][image7]

#### 4. ROI

Then I did masking of transofrmed image to select some area. It looks like

![alt text][image8]

#### 5. Finding the Lines and Visualization

I used histogram technique to find the maximum pixels in the transformed image and used second order polynomial to find left and right lines.

The result looks like following:

Histogram image:

![alt text][image9]

Visualization of lines:

![alt text][image10]

![alt text][image11]


#### 6. Final Output

I implemented this step in last cell. This code merge original image with the above output. Here is an example of my result on a test image:

![alt text][image12]

Following output image shows Left and Right Curvature and distance of vehicle from the center of an image.

![alt text][image13]



---

### Pipeline (video)

#### 1. Video Output

Here's a [link to my video result](./output_video/project_video_output_final.mp4)

---

### Discussion

I learned a lot from this project. I found difficulties in shadow part of the video and I feel that I can do better in my pipeline which is independent of "COLOR". Also, the next thing I would like to save previous frames slope (Average Slope) values for left and right lanes. 

Also, I doubt that if my pipeline will work for night (very dark) images. I feel that I still need to improve my thresholding technique (which is more independent of colorspace) to make my pipeline robust!!