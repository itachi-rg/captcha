# captcha

##Step 1 : Run captcha_pre_process.py

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

##Step 2 : Run captcha_process.py

```
python .\captcha_process.py

[90  0  0 ...,  0  0  0]
[91  0  1 ...,  0  0  0]

```
This script will read the pixel values of the images and create instance vectors for each image.

The first character of each instance represents the ASCII value of the character.

The vectors are stored in the 'processed_input_cluster.csv' file.

Step 3 : Training

python .\captcha_model_build.py traincluster

traincluster
Learning Rate :  0.05
Number of hidden layers :   1
Total iterations  50000
Print error interval  10
Number of instances ( m )  2000
Number of input nodes ( n )  2701
Number of hidden nodes ( h )  1800
Number of output nodes ( r )  5


Error === 53.78   === iterations  0  === Percentage training completed 0.00  === Elapsed time 1.79
Saving to file  temp_data.pkl
Error === 50.00   === iterations  10  === Percentage training completed 0.02  === Elapsed time 36.27
Saving to file  temp_data.pkl
Error === 50.00   === iterations  20  === Percentage training completed 0.04  === Elapsed time 18.59
Error === 48.67   === iterations  30  === Percentage training completed 0.06  === Elapsed time 16.23
Saving to file  temp_data.pkl
Error === 47.31   === iterations  40  === Percentage training completed 0.08  === Elapsed time 17.12
Saving to file  temp_data.pkl
Error === 46.11   === iterations  50  === Percentage training completed 0.10  === Elapsed time 20.98
Saving to file  temp_data.pkl
Error === 45.18   === iterations  60  === Percentage training completed 0.12  === Elapsed time 20.12
Saving to file  temp_data.pkl
Error === 45.24   === iterations  70  === Percentage training completed 0.14  === Elapsed time 20.07


This script will train the neural network over the given number of iterations. 
Gradient descent algorithm is used to minimize the error in classification.

The output model is stored in the temp_data.pkl file.

Step 4 : Training data Validation 

This will check the accuracy of prediction of the training instances using the model.

python .\captcha_model_build.py validatecluster

Validating
Reading from file  temp_data.pkl

Predicted   9522
Correct     9CSF
char correct count  1  : char total_count  4  captcha correct count  0  percent  25.0
Predicted   22C9
Correct     V96V
char correct count  1  : char total_count  8  captcha correct count  0  percent  12.5
Predicted   2229
Correct     WKEP
char correct count  1  : char total_count  12  captcha correct count  0  percent  8.333333333333332
Predicted   222C
Correct     VMAB
char correct count  1  : char total_count  16  captcha correct count  0  percent  6.25
Predicted   9222
Correct     EGNA
char correct count  1  : char total_count  20  captcha correct count  0  percent  5.0
Predicted   5C92
Correct     943E
char correct count  1  : char total_count  24  captcha correct count  0  percent  4.166666666666666



Step 5 : Testing

This will test the model on the test images.

python .\captcha_model_build.py testcluster

Test cluster
Reading from file  temp_data.pkl
Predicted   2955
Correct     WFDB
char correct count  0  : char total_count  4  captcha correct count  0  percent  0.0
Predicted   2229
Correct     PHV8
char correct count  0  : char total_count  8  captcha correct count  0  percent  0.0
Predicted   2922
Correct     E6N9
char correct count  0  : char total_count  12  captcha correct count  0  percent  0.0
Predicted   2222
Correct     H42X
char correct count  1  : char total_count  16  captcha correct count  0  percent  6.25
Predicted   2225
Correct     VPF8
char correct count  1  : char total_count  20  captcha correct count  0  percent  5.0