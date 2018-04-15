# **Traffic Sign Recognition** 

In this project I've created and tuned neural network for German traffic signs recognition.


## Dataset Exploration

### Dataset Summary

As an input for this project I've received signs dataset which consists of:

1. Training dataset with total of 34799 sign pictures
1. Validation dataset with total of 4410 sign pictures
1. Test dataset with total of 12630 sign pictures

Each image in dataset has size of 32x32 pixels and 3 color channels.

Total number of sign classes in datatset is 43.

### Exploratory Visualization

Here is examples of some classes:

Class: 4. Name: "Speed limit (70km/h)". Samples: 1770.

![class 4 example][image_class4_examples]

Class: 26. Name: "Traffic signals". Samples: 540.

![class 26 example][image_class26_examples]

Class: 39. Name: "Keep left". Samples: 270.

![class 39 example][image_class39_examples]

Next bar chart shows samples distribution over the classes:

![samples distribution][image_samples_distribution]


## Design and Test a Model Architecture

### Preprocessing

First of all I've augment the data. I've used to functions to do it:

1. Small rotations. I've used random angle in range [-20,20] degrees.

    ![rotated image][image_rotation]
    
1. Small perspective transformations.
    
    ![perspective image][image_perspective]


I've tried two ways to augment the data:

1. Just increase data by adding random augmentation function for each image in input data.
2. Make number of samples in each class more or less equal by adding more modified images to the classes with less samples.

I've chosen this augmentation technics as it is natural for recognition algorithms to use data with not just strait sign picture, but also with sign pictures seen from other angles.

Finally, new dataset is normilized.


### Model Architecture

After some experiments with net architecture I've stopped on the next one:

1. Input layer (32x32x3 RGB image)

1. Convolution scope 1
    * convolution layer (5x5 stride, same padding, outputs 32x32x32)
    * RELU layer
    * dropout layer (0.1 dropout value was used during training)
    * max pooling layer (2x2 stride, outputs 16x16x32)

1. Convolution scope 2
    * convolution layer (5x5 stride, same padding, outputs 16x16x64)
    * RELU layer
    * dropout layer (0.1 dropout value was used during training)
    * max pooling layer (2x2 stride, outputs 8x8x64)

1. Convolution scope 3
    * convolution layer (5x5 stride, same padding, outputs 8x8x64)
    * RELU layer
    * dropout layer (0.1 dropout value was used during training)
    * max pooling layer (2x2 stride, outputs 4x4x64)

1. Fully connected scope 1
    * flatten layer (outputs 1024)
    * fully connected layer (outputs 256)
    * RELU layer
    * dropout layer (0.5 dropout value was used during training)

1. Fully connected scope 2
    * fully connected layer (outputs 128)
    * RELU layer
    * dropout layer (0.5 dropout value was used during training)

1. Logits scope
    * fully connected layer (outputs 43)

### Model Training

I've used Adam optimizer. To train my model I've tried:

1. Different net architecture (2-3 convolution layer, 2-3 fully connected layers, different number
    neurons for each layer)
1. Leaning rate from 0.0001 to 0.01
1. Number of epochs from 10 to 30
1. Batch size from 64 to 1024
1. Convolution layers dropout value from 0.7 to 1
1. Fully connected layers dropout value from 0.4 to 1
1. Different types of data augmentation

### Solution Approach

The best hyperparameters results for this model architecture:

* learning rate = 0.001
* epochs number = 30
* batch size = 128
* dropout value for convolution layers = 0.9
* dropout value for fully connected layers = 0.5

Final model results:

* training set accuracy - 0.997
* validation set accuracy - 0.975
* test set accuracy - 0.962

As the traffic signs recognition task is more complicated than the task performed by LeNet, it was obvious that bigger net should perform this task better. Just by adding more layers and increasing number of neurons I was able to achieve 96% accuracy on validation set. Dropout and data augmentation gave me additional 1.5%

At the same time future enlargment of the model was leading to accuracy decrease, due to underfitting.

## Test a Model on New Images

### Acquiring New Images

To check model performance I've downloaded few new signs from internet:

![new signs][image_newSigns]

While there are no issues with this signs lighting, the view angle is not as straight as on training data set. Also, signs here are not always horizontal. Lets see if data augmentation helped us to deal with this issues.

### Performance on New Images

| Estimated class | Real class |
|:---------------------:|:---------------------------------------------:|
| Speed limit (50km/h) | Speed limit (50km/h) |
| No entry | No entry |
| Vehicles over 3.5 metric tons prohibited | Vehicles over 3.5 metric tons prohibited |
| Speed limit (80km/h) | Speed limit (80km/h) |
| No passing | No passing |
| Speed limit (30km/h) | Speed limit (30km/h) |
| No vehicles | No vehicles |
| Speed limit (30km/h) | Speed limit (30km/h) |
| Stop | Stop |
| No passing for vehicles over 3.5 metric tons | Speed limit (50km/h) |
| Roundabout mandatory | Roundabout mandatory |
| General caution | General caution |
| Stop | Stop |

Total correct signs: 12/13 (92%)

### Model Certainty - Softmax Probabilities

Next is top 5 probabilities for each image. We can see, that for the most pictures net is pretty sure
about it's assumption. There is only one image where we can see that all probabilities are less than 0.35. That's the only one image with incorrect estimation. We can see that top 5 possible classes for this image are more or less similar - red border, white inside with some black picture.

1. Speed limit (50km/h)

    ![new sign 1][image_newSign1]
    ![new sign 1 probabilities][image_newSign1p]

1. No entry

    ![new sign 2][image_newSign2]
    ![new sign 2 probabilities][image_newSign2p]


1. Vehicles over 3.5 metric tons prohibited

    ![new sign 3][image_newSign3]
    ![new sign 3 probabilities][image_newSign3p]


1. Speed limit (80km/h)

    ![new sign 4][image_newSign4]
    ![new sign 4 probabilities][image_newSign4p]


1. No passing

    ![new sign 5][image_newSign5]
    ![new sign 5 probabilities][image_newSign5p]

1. Speed limit (30km/h)

    ![new sign 6][image_newSign6]
    ![new sign 6 probabilities][image_newSign6p]

1. No vehicles

    ![new sign 7][image_newSign7]
    ![new sign 7 probabilities][image_newSign7p]

1. Speed limit (30km/h)

    ![new sign 8][image_newSign8]
    ![new sign 8 probabilities][image_newSign8p]

1. Stop

    ![new sign 9][image_newSign9]
    ![new sign 9 probabilities][image_newSign9p]

1. Speed limit (50km/h)

    ![new sign 10][image_newSign10]
    ![new sign 10 probabilities][image_newSign10p]

1. Roundabout mandatory

    ![new sign 11][image_newSign11]
    ![new sign 11 probabilities][image_newSign11p]

1. General caution

    ![new sign 12][image_newSign12]
    ![new sign 12 probabilities][image_newSign12p]

1. Stop

    ![new sign 13][image_newSign13]
    ![new sign 13 probabilities][image_newSign13p]


### Neural Network Visualization

Here is an example of layer 1 activations on image of sign 50 km/h limit. We can see here that some neurons activates on red border color, others on color transition from left to right, while some other neurons seems to like sign background and doesn't like sign itself.

![layer 1 activations][image_conv1Act]


[image_class4_examples]: ./writeup_imgs/class4_examples.png "class 4 example"
[image_class26_examples]: ./writeup_imgs/class26_examples.png "class 26 example"
[image_class39_examples]: ./writeup_imgs/class39_examples.png "class 39 example"
[image_samples_distribution]: ./writeup_imgs/samples_distribution.png "class 39 example"
[image_rotation]: ./writeup_imgs/rotation.png "rotation example"
[image_perspective]: ./writeup_imgs/perspective.png "perspective example"
[image_newSigns]: ./writeup_imgs/newSigns.png "new signs"
[image_newSign1]: ./writeup_imgs/newSign1.png "new sign 1"
[image_newSign1p]: ./writeup_imgs/newSign1p.png "new sign 1 probabilities"
[image_newSign2]: ./writeup_imgs/newSign2.png "new sign 2"
[image_newSign2p]: ./writeup_imgs/newSign2p.png "new sign 2 probabilities"
[image_newSign3]: ./writeup_imgs/newSign3.png "new sign 3"
[image_newSign3p]: ./writeup_imgs/newSign3p.png "new sign 3 probabilities"
[image_newSign4]: ./writeup_imgs/newSign4.png "new sign 4"
[image_newSign4p]: ./writeup_imgs/newSign4p.png "new sign 4 probabilities"
[image_newSign5]: ./writeup_imgs/newSign5.png "new sign 5"
[image_newSign5p]: ./writeup_imgs/newSign5p.png "new sign 5 probabilities"
[image_newSign6]: ./writeup_imgs/newSign6.png "new sign 6"
[image_newSign6p]: ./writeup_imgs/newSign6p.png "new sign 6 probabilities"
[image_newSign7]: ./writeup_imgs/newSign7.png "new sign 7"
[image_newSign7p]: ./writeup_imgs/newSign7p.png "new sign 7 probabilities"
[image_newSign8]: ./writeup_imgs/newSign8.png "new sign 8"
[image_newSign8p]: ./writeup_imgs/newSign8p.png "new sign 8 probabilities"
[image_newSign9]: ./writeup_imgs/newSign9.png "new sign 9"
[image_newSign9p]: ./writeup_imgs/newSign9p.png "new sign 9 probabilities"
[image_newSign10]: ./writeup_imgs/newSign10.png "new sign 10"
[image_newSign10p]: ./writeup_imgs/newSign10p.png "new sign 10 probabilities"
[image_newSign11]: ./writeup_imgs/newSign11.png "new sign 11"
[image_newSign11p]: ./writeup_imgs/newSign11p.png "new sign 11 probabilities"
[image_newSign12]: ./writeup_imgs/newSign12.png "new sign 12"
[image_newSign12p]: ./writeup_imgs/newSign12p.png "new sign 12 probabilities"
[image_newSign13]: ./writeup_imgs/newSign13.png "new sign 13"
[image_newSign13p]: ./writeup_imgs/newSign13p.png "new sign 13 probabilities"
[image_conv1Act]: ./writeup_imgs/conv1_act.png "layer 1 activations"
