# Self-Driving Car Engineer Nanodegree

## Project: Vehicle Detection and Tracking



---

### Vehicle Detection Project


###### The goals / steps of this project are:

- Perform a Histogram of Oriented Gradients (HOG) feature extraction on a labeled training set of images and train a classifier Linear SVM classifier.
- Optionally, you can also apply a color transform and append binned color features, as well as histograms of color, to your HOG feature vector.
- Implement a sliding-window technique and use your trained classifier to search for vehicles in images.
- Run your pipeline on a video stream (start with the test_video.mp4 and later implement on full project_video.mp4) and create a heat map of recurring detections frame by frame to reject outliers and follow detected vehicles.
- Estimate a bounding box for vehicles detected.

### Histogram of Oriented Gradients (HOG)




By trial and error I ended up using the following parameters:
- LUV color space.
- Number of orient = 12
- Numbers of pixels per cell = 8
- Number of cells per block = 1

These parameters are shown in the second code cell line 110. The hog function is found in lines(10 - 27). The image bellow shows HOG visualization of a car and a not-car picture:
 <img src="writeup_pics/hog.png" width="461" alt="Combined Image" /> 


**Hog Implementation:**
- The dataset is obtained in the third code cell. 
- Features were scaled, which is quite important for any machine learning algorithm. (fourth code cell)
- The dataset is split into a training and test set, where the test set is 20% of all the data. (fourth code cell)
- I used linear support vector classifier implemented as part of scikit-learn. 
- The final accuracy obtained on the test set was **0.9896**. (fifth code cell)


### Sliding Window Search



The sliding window function is in lines 76-101 in the second code cell.
Instead of searching the hole frame the Y axis of the image was restricted to [400, 560].

 <img src="images/cut.png" width="961" alt="Combined Image" /> 
 <p style="text-align: center;"> Search area (above)</p>

 <img src="images/box.png" width="461" alt="Combined Image" /> 
 <p style="text-align: center;"> Sliding boxes drawn on an image (above)</p>
 
By trial and error I endedup using the following parameters:
- Window size = (64,64)
- 70% overlap.

**Examples of test images:**

The bounding boxes of all already detected cars are used when the heatmap is calculated. Those bounding boxes are regarded as if the car has been identified on that spot. That helps avoid flicker and loosing of already identified cars (lines 10-19 in the sixth code cell). Bellow are examples of images and theri resulting heatmap.
 <img src="images/hm.png" width="961" alt="Combined Image" /> 


Increasing window size gave b
etter results. The following pictures were obtained with window size = 64X64:
 <img src="images/badex.png" width="961" alt="Combined Image" /> 
 
 
Increasing window size gave better results. The following pictures were obtained with window size = 70X70:
 <img src="images/ex.png" width="961" alt="Combined Image" /> 


### Video Implementation



#### The pipeline should perform reasonably well on the entire project video (somewhat wobbly or unstable bounding boxes are ok as long as the vehicles are identified most of the time with minimal false positives.)

Video pipeline is in the sixth code cell. The sliding window search plus classifier has been used to search for and identify vehicles in the videos provided. Video output has been generated with detected vehicle positions drawn bounding boxes, circles, on each frame of video.

### [Output Video](https://youtu.be/ewOWi-mj-Y8)


**Filter for false positives and combining overlapping bounding boxes**

By increasing the window size (***xy_window***, line 6 in the sixth code cell), many false positives were removed. In the figure bellow, the calculations for the image on the left is done using ***xy_window=(70,70)***, ***xy_window=(70,70)***, and for the image on the right.
 <img src="images/wr.png" width="961" alt="Combined Image" /> 
 <p style="text-align: center;">  Example for results when changing window size (above)</p>


### Discussion

- Although cars are detected most of the video, bounding boxed are quite wobbly.
- Despite my attempts to remove outliers some still seem to appear.

A smoothing algorithm (averaging results overtime) will remove these 2 problems.
