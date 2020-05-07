#**Traffic Sign Recognition** 

**Build a Traffic Sign Recognition Project**

The goals / steps of this project are the following:
* Explore the data set
* Design, train and test a model architecture
* Use the model to make predictions on new images

### Data Set Summary & Exploration

#### 1. Provide a basic summary of the data set and identify where in your code the summary was done. In the code, the analysis should be done using python, numpy and/or pandas methods rather than hardcoding results manually.

- Number of training examples = 34799
- Number of valid examples= 4410
- Number of testing examples = 12630
- Image data shape = (32, 32, 3)
- Number of classes = 43

### Design and Test a Model Architecture


Although colors in the traffic sign are important in real world for
people to recoganize different signs, traffic signs are also different
in their shapes and contents. We can ignore colors in this problem
because signs in our training set are differentiable from their
contents and shapes, and the network seems having no problem to learn
just from shapes.

Therefore, My preprocessing phase normalizes images from [0, 255] to
[0, 1], and grayscales it. You can see the grayscale effects in cell
10.

The train, valid and test data are prepreocessed in cell 9. I use
cross validation to split training data. The code to split the data
is in function `train` (see cell 15).

To cross validate my model, I randomly split the given training sets
into training set and validation set. I preserved 10% data for
validation. `sklearn` has the handy tool `train_test_split` to do the
work.

The code is in function `classifier` (see cell 11).

I adapted LeNet architecture: Two convolutional layers followed by one
flatten layer, drop out layer, and three fully connected linear
layers.

1. convolution 1: 32x32x1  -> 28x28x12 -> relu -> 14x14x12 (pooling)
2. convolution 2: 14x14x12 -> 10x10x25 -> relu -> 5x5x25   (pooling)
3.       flatten: 5x5x25   -> 625
4.      drop out: 625      -> 625
5.        linear: 625      -> 300
6.        linear: 300      -> 150
7.        linear: 150      -> 43

The code for training the model is in cell 15 and 16.

I train the model in 10 iterations (epochs), and each iteration is
trained with 64 batch size. Adam optimizer is used with learning rate
0.001.


The code for calculating the accuracy of the model is in cell 16, 17, 18.

My final model results were:
* training set accuracy of 0.99 (overfitting the cross validation)
* validation set accuracy of 0.937
* test set accuracy of 0.93


The first model is adapted from LeNet architecture. Since LeNet
architecture has a great performance on recognizing handwritings, I
think it would also work on classifying traffic signs.

I used the same parameter given in LeNet lab. Its training accuracy
initially was around 90%, so I thought the filter depth was not large
enough to capture images' shapes and contents. Previously the filter
depth was 6 for the first layer and 12 for the second. I increased
to 12 and 25. The accuracy increased to around 93%.

I then added a drop out layer, which is supposed to used to prevent
overfitting, but I found a drop out layer could sometimes increase the
accuracy to 95%.

I also tuned `epoch`, `batch_size`, and `rate` parameters, and settled at

- `epoch` 10
- `batch_size` 64
- `learning rate` 0.001

I have my explainations of the effect of the drop out layer after I've
seen some of the training data. Some images are too dark to see the
sign, so it seems that these images act as noises in the training data
and drop out layer can reduce the negative effects on learning.

The final accuracy in validation set is around 0.95.

### Test a Model on New Images

The chosen signs are visualized in cell 20.

I want to see how the classifier performs on similar signs. The
General Caution and Traffic signals: they both look like a vertical bar
(see the visualization) when grayscaled. And pedestrains and child
crossing look similar in low resolution.


The code for making predictions on my final model is in cell 22. The
result is explained and virtualized in detail in cell 28.

The accuracy on the new traffic signs is 63.6%, while it was 93% on
the test set. This is a sign of underfitting. By looking at the
virtualized result, I think this can be addressed by using more image
preprocessing techniques on the training set.


In the submitted version, the model can correctly guess 7 out of 11
signs. The accuracy is 63.6%. However, it can sometimes predict
correctly 10 out of 11 images.

By looking at the virtualized data. The predictions of pedestrains,
children crossing, and speed limit 60km/h are actually close
enough. This is actually consistent to my various
experiments. Sometimes the prediction accuracy can be as good as
90%. I think to get the consistent correctness, I need more good
data. One simple thing to do might be to preprocess the image by
brightening dark ones.
