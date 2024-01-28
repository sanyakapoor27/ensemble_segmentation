# Ensemble U-Net Segmentation

## Dataset
For this project, the electron microscopy dataset from [EPFL CVLAB](https://www.epfl.ch/labs/cvlab/data/data-em/) is used. The data contains images and masks in .TIF format representing  the hippocampus region of the brain. The mitochodria, as well as, synapses were annotated in the masks.

## Preprocessing
 The skimage library is used to read the .tif images and opencv to resize them to a consistent size of 256x256. As the images are all grayscale, an extra dimension is added at -1 axis to make sure there are no problems with the model implementation. A fuunction with a threshold of >128 pixels is also made to make sure all pixel values range between [0,1] for the mask.

 ## Ensemble Models
 The segmentation models library is used for this task. Two Unet models are imported with pre-trained VGG16 and VGG19 as encoders. As our dataset is grayscale, a convolutional layer is added to map the data to 3 channels. Both the models are compiled using dice loss, sigmoid activation function and adam optimizer with a learning rate of 0.0001. IoU and F1-score are used as evaluation metrics. 

 ## Prediction
 Both the models are saved and then loaded to predict on the test images. The predicted results are saved as a numpy array. Weights are assigned to the model predictions depicting the influence they will have on the final result. Tensordot is used to sum the predictions with the weights. Similarly, the IoU is also calculated using the ensemble prediction. We also develop a grid search for findind out the best prediction results by iterating over all weights within a range. These optimal weights are then used to predict and display the masks.
