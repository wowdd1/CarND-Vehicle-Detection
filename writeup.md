##Writeup Template
###You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

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
[image1]: ./output_images/download1.png
[image2]: ./output_images/download2.png
[image3]: ./output_images/download3.png
[image4]: ./output_images/download4.png
[image5]: ./output_images/download5.png
[image6]: ./output_images/download6.png
[image7]: ./examples/output_bboxes.png
[video1]: ./project_video.mp4

## [Rubric](https://review.udacity.com/#!/rubrics/513/view) Points
###Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
###Writeup / README

####1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Vehicle-Detection/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!

###Histogram of Oriented Gradients (HOG)

####1. Explain how (and identify where in your code) you extracted HOG features from the training images.

there is a class called FeatureExtractor, in the __init__ function i use hog function(skimage.feature) for extracted HOG features

####2. Explain how you settled on your final choice of HOG parameters.

i use some good parameters from the classroom code

####3. Describe how (and identify where in your code) you trained a classifier using your selected HOG features (and color features if you used them).

I trained a linear SVM using LinearSVC from skimage package, i call the fit method of LinearSVC, pass in the training data, i use hog and color features for training the svm classifier

###Sliding Window Search

####1. Describe how (and identify where in your code) you implemented a sliding window search.  How did you decide what scales to search and how much to overlap windows?

the sliding window is in the detect_vehicles and detections_for_scalefunction of VehicleTracker class,
for every image, it be scaled by some factor(.3, .5, .65, .8), then use a 64 x 64 window search the vehicle start from some Y that also be scaled(.6, .57, .56, .55), then record that postion if there is a vehicle in that window

####2. Show some examples of test images to demonstrate how your pipeline is working.  What did you do to optimize the performance of your classifier?
i do not search all areas of the image, but start from some Y, because the  vehicle would not show in the top area of the image

here is some test images result:

![alt text][image1]
![alt text][image2]
![alt text][image3]
![alt text][image4]
![alt text][image5]
![alt text][image5]

---

### Video Implementation

####1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (somewhat wobbly or unstable bounding boxes are ok as long as you are identifying the vehicles most of the time with minimal false positives.)
Here's a [link to my video result](./project_video_annotated_vehicles.mp4)


####2. Describe how (and identify where in your code) you implemented some kind of filter for false positives and some method for combining overlapping bounding boxes.
for fix false positives, i record heatmap for every frame, if a slide widonw be predicted as vehicle in ther current frame, but on the next frames it be predicted not a vehicle, then the value in the heatmap will small, so a frame will be compare with previous frames, after that, call label on the heatmap, it will tell us where is the vehicle, and the false positives will not be included

---

###Discussion

####1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?
if the vehicle not appear in the slide window search area, it will not be detected, so need search more area in the image, but the speed of search process will go down, there are some other approach for vehicle detection, like SSD multibox, YOLO, the speed and accuracy is better than slide window approach
