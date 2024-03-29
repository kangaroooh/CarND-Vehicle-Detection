**Vehicle Detection Project**

The goals / steps of this project are the following:

* HOG feature extraction
* train Linear SVM Classifier
* sliding window search


[//]: # (Image References)
[false_pos]: ./report_images/false_positive.png
[sliding]: ./report_images/sliding_windows.png
[works]: ./report_images/works.png
[video1]: ./project_video.mp4

### Histogram of Oriented Gradients (HOG)

#### 1.Extracting HOG features from the training images.

The code for this step is contained in the `second code cell` of the IPython notebook. 

I started by reading in all the `vehicle` and `non-vehicle` images.  

I then explored different color spaces and different `skimage.hog()` parameters (`orientations`, `pixels_per_cell`, and `cells_per_block`). 

I chose those parameters by try training the classifier and see how it goes well on test images (is it able to detect the cars) along with speed. Using `YCrCb` color space and `orientations=8`, `pixels_per_cell=(8, 8)` and `cells_per_block=(2, 2)`


#### 2. HOG parameters selections.

I tried various color spaces on the classifier and I found that `YCrCb` works best on the test images, so I settled down with it. The `orientation` that is larger than 8 does not seem to be a good choice on this since it does not improve the classifier that much. The `pixels_per_cell` and `cells_per_block` that I chose gives good intuition on eyes and magnify the classification rate to ~98 


#### 3. Training a classifier.

I trained a linear SVM using the `third cell` blocks in the jupyter notebook file. Using the training data which uses `spatial_feat`, `hist_feat`, and `hog_feat`. The data is scaled random split to train/test by `StandardScaler()` and `train_test_split()` before passed to the classifier.


### Sliding Window Search

#### 1. Sliding window search & overlapping windows?

I implemented the sliding window approach in the `4th cell` block of the jupyter notebook file. I chose 3 groups of windows where the first two are to detect the cars that just entered in the frame (lower left/right corners), so the block will be larger. Another group of windows is in the center of the image along the horizontal line where the cars getting smaller, so I make the windows smaller and more overlapping. Actually, I am more happy with more overlapping windows, but it costs more computation time and the current preferences are okay.


![alt text][sliding]

#### 2. Some examples of test images.

The parameters on HOG features can be fine-tuned for better classification as aforementioned above. I also made a mechanism which helps reducing the false positives on the following sections


![alt text][works]
---

### Video Implementation

#### 1. Provide a link to your final video output.
Here's a [link to my video result](./out_project_video.mp4)


#### 2. Filtering of false positives and some method for combining overlapping bounding boxes.

On the 4th cell code, I created a heatmap threshold it, then used `scipy.ndimage.measurements.label()` to identify blobs in the heatmap. Given an assumption that each blob corresponded to a vehicle, I constructed bounding boxes to cover the area of each blob detected.  

Here's an example result showing the heatmap from a series of frames of video, the result of `scipy.ndimage.measurements.label()` and the bounding boxes:

### Here are six frames and their corresponding heatmaps and resulting of bounding boxes:

![alt text][false_pos]


---

### Discussion

- I see that the sliding windows size can set its aspect ratio to wide-screen which will make the overlapping size much easier and easier to detect the near cars 
- the bounding boxes are still shaky, i believe using averaging over time can solve the problem
- the classifier is not accurate enough, there are still some mis-classification on the project video even the classifier got ~98% accuracy, might need to solve for overfitting ot using CNN to solve it.
