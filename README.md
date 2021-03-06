# Captcha identifier

### A neural network implemented in python capable of identifying the captcha characters for the UMass CS Webmail.
#### Python libraries required : Numpy, OpenCV
   
### No additional ML libraries such as tensorflow, theano, etc needed.

## Components
### [https://github.com/itachi-rg/captcha\_chrome\_plugin](https://github.com/itachi-rg/captcha_chrome_plugin)
An extension or plugin for the chrome web browser which extracts the captcha image and sends it to the server for identification. Once it receives the response, it populates the captcha text box with the captcha characters.

### [https://github.com/itachi-rg/captcha\_app\_engine](https://github.com/itachi-rg/captcha_app_engine)
The initial proxy server which receives the HTTP request from the chrome plugin. This just redirects the request to the actual server which does the processing of the image.

### [https://github.com/itachi-rg/captcha\_python\_server](https://github.com/itachi-rg/captcha_python_server)
This main server receives the request from the proxy server, uses the trained neural network to predict the characters for the captcha image in the request and returns the reponse.

### [https://github.com/itachi-rg/captcha](https://github.com/itachi-rg/captcha)
The main project which contains the implementation of a generic neural network program and its usage to identify the characters in the captcha image. The neural network is implemented using numpy and does not use built in neural networks using frameworks such as tensorflow, theano, etc. 

## Building the neural network and training it to identify the captcha images

### Step 1 : Run captcha_pre_process.py

This will divide the captcha images into individual characters using k-means clustering algorithm.

```
python .\captcha_pre_process.py

File --------------> 1.png
classified\dividedCluster\9\0.png
classified\dividedCluster\C\0.png
classified\dividedCluster\S\0.png
classified\dividedCluster\F\0.png

```
Here the 1.png containing the captcha image for '9CSF' is divided into separate images and each image is stored in the respective folder for that character.

This is done for all the images in the folder 'fullimages'. Currently 500.

The input characters are taken from classified\class.txt

Currently the divided images are already present. But in case any changes are done to the image processing aspect, then need to delete the folder and run this script.

### Step 2 : Run captcha_process.py

```
python .\captcha_process.py

[90  0  0 ...,  0  0  0]
[91  0  1 ...,  0  0  0]

```
This script will read the pixel values of the images and create instance vectors for each image.

The first character of each instance represents the ASCII value of the character.

The vectors are stored in the 'training_data.csv' file.

### Step 3 : Training

```
python .\captcha_model_build.py train
Starting training
Learning Rate :  0.1
Number of hidden layers :   1
Total iterations  50000
Print error interval  10
Number of instances ( m )  4000
Number of input nodes ( n )  2701
Hidden Layer  1  : Number of hidden nodes ( h )  2700
Number of output nodes ( r )  2700
 Initial Layer [  0  ]
 [[ 0.  0.  0. ...,  0.  0.  0.]
 [ 0.  0.  0. ...,  0.  0.  0.]
 [ 0.  0.  0. ...,  0.  0.  0.]
 ...,
 [ 0.  0.  0. ...,  0.  0.  0.]
 [ 0.  0.  0. ...,  0.  0.  0.]
 [ 0.  0.  0. ...,  0.  0.  0.]]
 Initial Layer [  1  ]
 [[ 0.  0.  0. ...,  0.  0.  0.]
 [ 0.  0.  0. ...,  0.  0.  0.]
 [ 0.  0.  0. ...,  0.  0.  0.]
 ...,
 [ 0.  0.  0. ...,  0.  0.  0.]
 [ 0.  0.  0. ...,  0.  0.  0.]
 [ 0.  0.  0. ...,  0.  0.  0.]]
 Initial Layer [  2  ]
 [[ 0.  0.  0. ...,  0.  0.  0.]
 [ 0.  0.  0. ...,  0.  0.  0.]
 [ 0.  0.  0. ...,  0.  0.  0.]
 ...,
 [ 0.  0.  0. ...,  0.  0.  0.]
 [ 0.  0.  0. ...,  0.  0.  0.]
 [ 0.  0.  0. ...,  0.  0.  0.]]
 Initial Synapse [  0  ]
 [[ 0.21689063  0.38360781  0.57063898 ...,  0.72053871  0.83261106
   0.51696751]
 [ 0.8991828   0.07096534  0.49813058 ...,  0.75074914  0.49703711
   0.34086053]
 [ 0.37246436  0.95565204  0.52275393 ...,  0.94948909  0.16550372
   0.00924244]
 ...,
 [ 0.69369789  0.77546224  0.36781348 ...,  0.57011667  0.99457301
   0.09044894]
 [ 0.92904001  0.59270978  0.08220815 ...,  0.80061273  0.77968066
   0.57666717]
 [ 0.61966872  0.75889115  0.84394561 ...,  0.90526759  0.82381707
   0.23313146]]
 Initial Synapse [  1  ]
 [[ 0.66647049  0.21075501  0.94575418 ...,  0.83105532  0.55946156
   0.41776201]
 [ 0.99101232  0.76120872  0.37642524 ...,  0.37257107  0.18740529
   0.0915045 ]
 [ 0.16953221  0.06314364  0.99269919 ...,  0.95472759  0.26694058
   0.18264797]
 ...,
 [ 0.04805408  0.53239683  0.63516114 ...,  0.10968724  0.30970068
   0.45559373]
 [ 0.02574869  0.61229518  0.32267765 ...,  0.71098795  0.38318362
   0.4044322 ]
 [ 0.65860026  0.98768124  0.49763725 ...,  0.7540536   0.68385173
   0.83450639]]
Error === 74.53   === iterations  0  === Percentage training completed 0.00  === Elapsed time 14.97
Saving full object to file  full_nn_object.pkl
C:\captcha\neurallib.py:37: RuntimeWarning: overflow encountered in exp
  return 1/(1+np.exp(-x))
Error === 31.54   === iterations  10  === Percentage training completed 0.02  === Elapsed time 135.06
Saving full object to file  full_nn_object.pkl
Error === 49.37   === iterations  20  === Percentage training completed 0.04  === Elapsed time 128.05
Error === 30.64   === iterations  30  === Percentage training completed 0.06  === Elapsed time 112.11
Saving full object to file  full_nn_object.pkl
Error === 33.87   === iterations  40  === Percentage training completed 0.08  === Elapsed time 123.68
Error === 5.67   === iterations  260  === Percentage training completed 0.52  === Elapsed time 134.90
Error === 3.45   === iterations  270  === Percentage training completed 0.54  === Elapsed time 120.66
Error === 2.73   === iterations  280  === Percentage training completed 0.56  === Elapsed time 121.95
Saving full object to file  full_nn_object.pkl
Error === 3.63   === iterations  290  === Percentage training completed 0.58  === Elapsed time 133.34
Error === 1.18   === iterations  590  === Percentage training completed 1.18  === Elapsed time 141.73
Saving full object to file  full_nn_object.pkl
Error === 0.98   === iterations  600  === Percentage training completed 1.20  === Elapsed time 146.92
Saving full object to file  full_nn_object.pkl
Saving only synapses to  model.pkl

```

This script will train the neural network over the given number of iterations. 
Gradient descent algorithm is used to minimize the error in classification.

The trained neural network is stored in the model.pkl file.

### Step 4 : Training data Validation 

This will check the accuracy of prediction of the training instances using the trained neural network.

```
python .\captcha_model_build.py validate_train

Validating
Reading from file  model.pkl

Predicted   AS4B
Correct     AS4B


                Single Character
                        Correct count : 332
                        Total count   : 384

                        Percentage    : 86.45833333333334

                Captcha
                        Correct count : 62
                        Total count   : 96

                        Percentage    : 64.58333333333334

Predicted   PMVB
Correct     PMVB


                Single Character
                        Correct count : 336
                        Total count   : 388

                        Percentage    : 86.5979381443299

                Captcha
                        Correct count : 63
                        Total count   : 97

                        Percentage    : 64.94845360824742

```

### Step 5 : Testing

This will test the accuracy of prediction of the trained neural network on the test images.

```
python .\captcha_model_build.py validate_test

Test set validation
Reading from file  full_nn_object.pkl

Predicted   7AR4
Correct     7NR4


                Single Character
                        Correct count : 341
                        Total count   : 396

                        Percentage    : 86.11111111111111

                Captcha
                        Correct count : 63
                        Total count   : 99

                        Percentage    : 63.63636363636363

Predicted   P7CA
Correct     P7CA


                Single Character
                        Correct count : 345
                        Total count   : 400

                        Percentage    : 86.25

                Captcha
                        Correct count : 64
                        Total count   : 100

                        Percentage    : 64.0

```