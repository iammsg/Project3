# **Traffic Sign Recognition** 
---
The goals / steps of this project are the following:
* Load the data set (see below for links to the project data set)
* Explore, summarize and visualize the data set
* Design, train and test a model architecture
* Use the model to make predictions on new images
* Analyze the softmax probabilities of the new images
* Summarize the results with a written report

[//]: # (Image References)

[image1]: ./Output_Images/Comparison_Sign0.png "Traffic Sign 0"
[image2]: ./Output_Images/Comparison_Sign40.png "Traffic Sign 40"
[image3]: ./Output_Images/Training_Set.png "Histogram of the Training Data"
[image4]: ./Output_Images/web_images.png "Traffic Signs from the Web"
[image5]: ./Output_Images/web_image_comparison.png "Traffic Signs from the Web : Classified"
[image6]: ./Output_Images/FeatureMapLayer1.png "1st Convolution Layer in the Network"
[image7]: ./Output_Images/FeatureMapLayer2.png "2nd Convolution Layer in the Network"

## Rubric Points
### Here I will consider the [rubric points](https://review.udacity.com/#!/rubrics/481/view) individually and describe how I addressed each point in my implementation.  

---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one. You can submit your writeup as markdown or pdf. You can use this template as a guide for writing the report. The submission includes the project code.

You're reading it! and here is a link to my [project code](https://github.com/iammsg/Project3/blob/master/Traffic_Sign_Classifier.ipynb)

### Data Set Summary & Exploration

#### 1. Provide a basic summary of the data set. In the code, the analysis should be done using python, numpy and/or pandas methods rather than hardcoding results manually.

I used the pandas library to calculate summary statistics of the traffic
signs data set:

* The size of training set is 34799
* The size of the validation set is 4410
* The size of test set is 12630
* The shape of a traffic sign image is 32x32x3
* The number of unique classes/labels in the data set is 43

#### 2. Include an exploratory visualization of the dataset.

For an exploration into the dataset, I plotted a few of the images so as to get an understanding of the different kinds of images, which are shown below.

![alt text][image1]
![alt text][image2]

I have also plotted a histogram of the different signs in the training set. This does show some indications of a skew in the data with some of the traffic signs being under-represented. This could possibly be an avenue of improving the fit, by augmenting the data with more instances of these signs.

![alt text][image3]

### Design and Test a Model Architecture

#### 1. Describe how you preprocessed the image data. What techniques were chosen and why did you choose these techniques? Consider including images showing the output of each preprocessing technique. Pre-processing refers to techniques such as converting to grayscale, normalization, etc. (OPTIONAL: As described in the "Stand Out Suggestions" part of the rubric, if you generated additional data for training, describe why you decided to generate additional data, how you generated the data, and provide example images of the additional data. Then describe the characteristics of the augmented training set like number of images in the set, number of images for each class, etc.)

I kept the preprocessing of the image data very simple and just normalized the data images. This step was essential for easier and better fitting, as this would allow the mean value to be zero and a tight sigma. This was accomplished by the formula $\frac{x-128}{128}$. 

This a step which can improved by perhaps converting to grayscale as the colors of the images do not matter. Another possibility would be to augment the dataset to get a larger trainig set.


#### 2. Describe what your final model architecture looks like including model type, layers, layer sizes, connectivity, etc.) Consider including a diagram and/or table describing the final model.

My final model consisted of the following layers:

| Layer         		| Description	        					              | 
|:---------------------:|:-------------------------------------------------------:| 
| Input         		| 32x32x3 RGB image   							          | 
| Convolution 5x5     	| 1x1 stride, same padding, outputs 28x28x6 	          |
| RELU					|												          |
| DropOut				| Keep probability 0.9							          |
| Max pooling	      	| 2x2 stride,  outputs 14x14x6  				          |
| DropOut				| Keep probability 0.9							          |
| Convolution 5x5	    | 1x1 stride, same padding, outputs 10x10x16              |
| RELU					|												          |
| DropOut				| Keep probability 0.9							          |
| Max pooling	      	| 2x2 stride,  outputs 5x5x16  				              |
| DropOut				| Keep probability 0.9							          |
| Flatten               | outputs 400                                             |
| Fully connected		| inputs 400, outputs 120 						          |
| RELU					|												          |
| DropOut				| Keep probability 0.9							          |
| Fully connected		| inputs 120, outputs 84 						          |
| RELU					|												          |
| DropOut				| Keep probability 0.9							          |
| Fully connected		| inputs 84, outputs 43						              |
| Softmax				| Outputs Probability of each Sign as a vector of size 43 |

 These layers assured that we were able to extract the right features out from each of the traffic signs.


#### 3. Describe how you trained your model. The discussion can include the type of optimizer, the batch size, number of epochs and any hyperparameters such as learning rate.

To train the model, I used a the Adam optimizer and it was able to provide good results. The hyperparameters were tuned as follows:
* The learning rate is 0.001
* The number of epochs is 40
* The batch size is 128

#### 4. Describe the approach taken for finding a solution and getting the validation set accuracy to be at least 0.93. Include in the discussion the results on the training, validation and test sets and where in the code these were calculated. Your approach may have been an iterative process, in which case, outline the steps you took to get to the final solution and why you chose those steps. Perhaps your solution involved an already well known implementation or architecture. In this case, discuss why you think the architecture is suitable for the current problem.

My final model results were:
* training set accuracy of 0.9948
* validation set accuracy of 0.95964 
* test set accuracy of 0.94521

I started with a standard LeNet architecture for my neural network as it has been proven to work for classying images with symbols in them. This architecture was effective to about a 70% accuracy with the raw data which improved to 80% after the data was normalized on the validation set. There were symptoms of over fitting as the training accuracy was in the mid 90s. To fix this, I added dropout layers after every activation function. These dropout layers prevent complex co-adaptations on training data and is a very efficient way of preventing over fitting. 

I tuned the learning rate and the number of epochs. Increasing the learning rate had a very detrimental effect with the fitting possibly to due to subtle features in the images. The high number of epochs was necessary to increasing the validation accuracy to an acceptable level. 

### Test a Model on New Images

#### 1. Choose five German traffic signs found on the web and provide them in the report. For each image, discuss what quality or qualities might be difficult to classify.

Here are the ten German traffic signs that I found on the web:

![alt text][image4] 

Most of these signs are pretty straightforward and should be easy enough to classify. The animal crossing sign will most likely be the most troublesome as the image of the animal is different when compared to our training data.

#### 2. Discuss the model's predictions on these new traffic signs and compare the results to predicting on the test set. At a minimum, discuss what the predictions were, the accuracy on these new predictions, and compare the accuracy to the accuracy on the test set (OPTIONAL: Discuss the results in more detail as described in the "Stand Out Suggestions" part of the rubric).

Here are the results of the prediction:

| Prediction       	                    |  Reality	        		   		      | 
|:-------------------------------------:|:---------------------------------------:| 
|No vehicles				            |  No vehicles                            |
|Children crossing				        |  Children crossing                      |
|No entry				                |  No entry                               |
|No passing				                |  No passing                             |
|Right-of-way at the next intersection	|  Right-of-way at the next intersection  |
|Bumpy road				                |  Wild animals crossing                  |
|General caution				        |  General caution                        |
|Bumpy road				                |  Bumpy road                             |
|Priority road				            |  Priority road                          |
|Right-of-way at the next intersection	|  Right-of-way at the next intersection  |


The model was able to correctly guess all but one of the traffic signs, which gives an accuracy of 90%. This compares favorably to the accuracy on the test set of 93%. As expected the Wild Animals crossing traffic sign did fail.

![alt text][image5] 

#### 3. Describe how certain the model is when predicting on each of the five new images by looking at the softmax probabilities for each prediction. Provide the top 5 softmax probabilities for each image along with the sign type of each probability. (OPTIONAL: as described in the "Stand Out Suggestions" part of the rubric, visualizations can also be provided such as bar charts)

The code for making predictions on my final model is located in the 17th cell of the Ipython notebook. The following tables outline the softmax probabilities for each of the signs we obtained from the web.

For Web Image 0 which is sign of No vehicles

| Probability |  Prediction           |
|:-----------:|:---------------------:|
|    1.0000   |  No vehicles          |
|    0.0000   |  No passing           |
|    0.0000   |  Turn right ahead     |
|    0.0000   |  Yield                |
|    0.0000   |  Speed limit (60km/h) |


For Web Image 1 which is sign of Children crossing

| Probability |  Prediction
|:-----------:|:--------------------------------------:|
|    1.0000   |  Children crossing                     |
|    0.0000   |  Right-of-way at the next intersection |
|    0.0000   |  Beware of ice/snow                    |
|    0.0000   |  Pedestrians                           |
|    0.0000   |  Bicycles crossing                     |


For Web Image  2 which is sign of No entry

| Probability |  Prediction           |
|:-----------:|:---------------------:|
|    1.0000   |  No entry             |
|    0.0000   |  Stop                 |
|    0.0000   |  No passing           |
|    0.0000   |  Beware of ice/snow   |
|    0.0000   |  Speed limit (20km/h) |


For Web Image  3 which is sign of No passing

|Probability  |  Prediction     
|:-----------:|:---------------------------------------------:|
|    0.9992   |  No passing                                   |
|    0.0008   |  End of no passing                            |
|    0.0000   |  No passing for vehicles over 3.5 metric tons |
|    0.0000   |  Priority road                                |
|    0.0000   |  No entry                                     |


For Web Image 4 which is sign of Right-of-way at the next intersection

|Probability  |  Prediction
|:-----------:|:--------------------------------------:|
|    1.0000   |  Right-of-way at the next intersection |
|    0.0000   |  Beware of ice/snow                    |
|    0.0000   |  Slippery road                         |
|    0.0000   |  Children crossing                     |
|    0.0000   |  Double curve                          |


For Web Image 5 which is sign of Bumpy road

|Probability  |  Prediction
|:-----------:|:-----------------------------:|
|    1.0000   |  Bumpy road                   |
|    0.0000   |  Traffic signals              |
|    0.0000   |  Dangerous curve to the right |
|    0.0000   |  Road work                    |
|    0.0000   |  Bicycles crossing            |


For Web Image  6 which is sign of General caution

| Probability  |  Prediction
|:------------:|:--------------------------:|
|    1.0000    |  General caution           |
|    0.0000    |  Traffic signals           | 
|    0.0000    |  Pedestrians               |
|    0.0000    |  Road narrows on the right |
|    0.0000    |  Road work                 |


For Web Image  7 which is sign of Bumpy road

| Probability |  Prediction
|:-----------:|:-----------------------------:|
|    0.9991   |  Bumpy road                   |
|    0.0009   |  Bicycles crossing            |
|    0.0001   |  Road narrows on the right    |
|    0.0000   |  Road work                    |
|    0.0000   |  Dangerous curve to the right | 


For Web Image  8 which is sign of Priority road

| Probability |  Prediction
|:-----------:|:------------------------------------:|
|    1.0000   |  Priority road                       |
|    0.0000   |  Speed limit (60km/h)                |
|    0.0000   |  End of all speed and passing limits |
|    0.0000   |  Speed limit (80km/h)                |
|    0.0000   |  Stop                                |


For Web Image  9 which is sign of Right-of-way at the next intersection

| Probability |  Prediction
|:-----------:|:--------------------------------------:|
|    1.0000   |  Right-of-way at the next intersection | 
|    0.0000   |  Beware of ice/snow                    |
|    0.0000   |  Double curve                          |
|    0.0000   |  Bicycles crossing                     |
|    0.0000   |  Children crossing                     |


### (Optional) Visualizing the Neural Network (See Step 4 of the Ipython notebook for more details)
#### 1. Discuss the visual output of your trained network's feature maps. What characteristics did the neural network use to make classifications?

I was able to derive the visual output of the two convolution layers in my trained network which are shown below:

![1st Convolution Layer][image6] 
![2nd Convolution Layer][image7] 

In both of these layers, it seems that the neural network is picking out the lines of various shades. As we go from Layer 1 to Layer 2, the complexity of the images increases, with it starting to pick out shapes.