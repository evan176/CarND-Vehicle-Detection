## Writeup Template
### You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

---

**Vehicle Detection Project**

The goals / steps of this project are the following:

* Perform a Histogram of Oriented Gradients (HOG) feature extraction on a labeled training set of images and train a classifier Linear SVM classifier
* Optionally, you can also apply a color transform and append binned color features, as well as histograms of color, to your HOG feature vector. 
* Note: for those first two steps don't forget to normalize your features and randomize a selection for training and testing.
* Implement a sliding-window technique and use your trained classifier to search for vehicles in images.
* Run your pipeline on a video stream (start with the test_video.mp4 and later implement on full project_video.mp4) and create a heat map of recurring detections frame by frame to reject outliers and follow detected vehicles.
* Estimate a bounding box for vehicles detected.

[//]: # (Image References)
[car_image]: ./examples/car.png
[notcar_image]: ./examples/not_car.png
[car_hog]: ./examples/car_hog.png
[notcar_hog]: ./examples/notcar_hog.png
[car_ycbcr]: ./examples/car_ycbcr.png
[notcar_ycbcr]: ./examples/notcar_ycbcr.png
[car_rgb]: ./examples/car_rgb.png
[notcar_rgb]: ./examples/notcar_rgb.png
[car_hsv]: ./examples/car_hsv.png
[notcar_hsv]: ./examples/notcar_hsv.png
[search_map]: ./examples/search_map.png
[corresponding]: ./examples/corresponding.png
[corresponding2]: ./examples/corresponding2.png
[corresponding3]: ./examples/corresponding3.png
[corresponding4]: ./examples/corresponding4.png
[corresponding5]: ./examples/corresponding5.png
[corresponding6]: ./examples/corresponding6.png
[video1]: ./project_video.mp4

## [Rubric](https://review.udacity.com/#!/rubrics/513/view) Points
### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Vehicle-Detection/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!

### Histogram of Oriented Gradients (HOG)

#### 1. Explain how (and identify where in your code) you extracted HOG features from the training images.

The code for this step is contained in the first code cell of the IPython notebook (or in lines # through # of the file called `some_file.py`).  

I started by reading in all the `vehicle` and `non-vehicle` images.  Here is an example of one of each of the `vehicle` and `non-vehicle` classes:

![alt text][car_image]
![alt text][notcar_image]

I then explored different color spaces and different `skimage.hog()` parameters (`orientations`, `pixels_per_cell`, and `cells_per_block`).  I grabbed random images from each of the two classes and displayed them to get a feel for what the `skimage.hog()` output looks like.

Here is an example using the `YCrCb`, `RGB`, `HSV` color space and HOG parameters of `orientations=8`, `pixels_per_cell=(8, 8)` and `cells_per_block=(2, 2)`:


![alt text][car_hog]
![alt text][notcar_hog]
![alt text][car_ycbcr]
![alt text][notcar_ycbcr]
![alt text][car_rgb]
![alt text][notcar_rgb]
![alt text][car_hsv]
![alt text][notcar_hsv]

#### 2. Explain how you settled on your final choice of HOG parameters.

I tried various combinations of parameters and decide to concatenate all of above color space, HOG (orident=9, pixels_per_cell=8, cell_per_block=2) and bin spatial (32, 32). After this concatenation, I normalize features and using sklearn.StandardScaler to scale range. In order to solve high dimension problem, I use PCA to reduce dimensiton to 300. The sum of pca explained_variance_ratio is 0.89.

#### 3. Describe how (and identify where in your code) you trained a classifier using your selected HOG features (and color features if you used them).

I trained a logistic regression to predict car.

### Sliding Window Search

#### 1. Describe how (and identify where in your code) you implemented a sliding window search.  How did you decide what scales to search and how much to overlap windows?

I decided to search the half of image because car only show one the down part. There are 3 window size: 64, 96, 128.

#### 2. Show some examples of test images to demonstrate how your pipeline is working.  What did you do to optimize the performance of your classifier?

After searching of image, I create the classified result as heatmap.
---

### Video Implementation

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (somewhat wobbly or unstable bounding boxes are ok as long as you are identifying the vehicles most of the time with minimal false positives.)
Here's a [link to my video result](./project_out.mp4)


#### 2. Describe how (and identify where in your code) you implemented some kind of filter for false positives and some method for combining overlapping bounding boxes.

I recorded the positions of positive detections in each frame of the video.  From the positive detections I created a heatmap and then thresholded that map to identify vehicle positions.  I then used `scipy.ndimage.measurements.label()` to identify individual blobs in the heatmap.  I then assumed each blob corresponded to a vehicle.  I constructed bounding boxes to cover the area of each blob detected.  

Here's an example result showing the heatmap from a series of frames of video, the result of `scipy.ndimage.measurements.label()` and the bounding boxes then overlaid on the last frame of video:

### Here are six frames and their corresponding heatmaps and labels:
![alt text][corresponding]
![alt text][corresponding2]
![alt text][corresponding3]
![alt text][corresponding4]
![alt text][corresponding5]
![alt text][corresponding6]
