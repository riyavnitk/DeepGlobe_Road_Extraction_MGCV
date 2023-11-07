# Road Extraction using Image Segmentation and Deep Learning
## Introduction
We have used the DeepGlobe dataset for training our model. The dataset contains 803 images of size 2448x2448. We have used 600 images for training and 203 images for testing. The dataset contains 6 classes namely road, building, trees, crops, water and background. We have used only the road class for training our model. The dataset is available at https://competitions.codalab.org/competitions/18467#participate-get_starting_kit.
 The dataset contains the images and the corresponding masks. The masks are of the same size as the images and contain the pixel-wise class labels. The dataset also contains the corresponding geojson files which contain the coordinates of the road pixels. We have used the geojson files to generate the road masks. The geojson files are converted to road masks using the script geojson_to_mask.py. The script takes the geojson file and the corresponding image as input and generates the road mask. The script is available in the folder geojson_to_mask. The script generates the road mask of the same size as the image. The road mask contains the road pixels as 1 and the background pixels as 0. The road mask is then converted to a binary mask using the script binary_mask.py. The script takes the road mask as input and generates the binary mask. The script is available in the folder binary_mask. The binary mask contains the road pixels as 1 and the background pixels as 255. The binary mask is then converted to a 3 channel mask using the script binary_mask_to_3_channel_mask.py. The script takes the binary mask as input and generates the 3 channel mask. The script is available in the folder binary_mask_to_3_channel_mask. The 3 channel mask contains the road pixels as [0, 0, 255] and the background pixels as [255, 255, 255]. The 3 channel mask is then converted to a 2 channel mask using the script 3_channel_mask_to_2_channel_mask.py. The script takes the 3 channel mask as input and generates the 2 channel mask. The script is available in the folder 3_channel_mask_to_2_channel_mask. The 2 channel mask contains the road pixels as [0, 255] and the background pixels as [255, 255]. The 2 channel mask is then converted to a 1 channel mask using the script 2_channel_mask_to_1_channel_mask.py.

## Approach 1: U-Net
### U-Net Architecture
![U-Net Architecture](https://media.geeksforgeeks.org/wp-content/uploads/20220614121231/Group14.jpg)

The U-Net Architecture brings out the best of both worlds. It is a combination of the Fully Convolutional Network and the Encoder-Decoder Network. The U-Net Architecture is a Fully Convolutional Network with a contracting path and an expansive path. The contracting path is similar to the Encoder Network and the expansive path is similar to the Decoder Network. The contracting path consists of a series of convolutional layers followed by a max pooling layer. The expansive path consists of a series of convolutional layers followed by an upsampling layer. The output of the contracting path is concatenated with the output of the expansive path. The concatenated output is then passed through a series of convolutional layers to generate the final output. The U-Net Architecture is shown in the figure above.

### U-Net Architecture Results
The IoU score plots of the U-Net architecture is shown below.
![U-Net Architecture Results]('https://github.com/riyavnitk/DeepGlobe_Road_Extraction_MGCV/blob/master/images/UNetIoU.png')
The loss plots of the U-Net architecture is shown below.
![U-Net Architecture Results]('https://github.com/riyavnitk/DeepGlobe_Road_Extraction_MGCV/blob/master/images/UNetLoss.png')
The predictions of the U-Net architecture in identifying the road netowkr of one such ariel image is shown below.
![U-Net Architecture Predictions]('https://github.com/riyavnitk/DeepGlobe_Road_Extraction_MGCV/blob/master/images/UNetPred.png')

## Approach 2: DeepLabV3+
### DeepLabV3+ Architecture
![DeepLabV3+ Architecture](https://production-media.paperswithcode.com/models/Screen_Shot_2021-02-21_at_10.34.37_AM_kvOFts0.png)

The DeepLabV3+ Architecture is an extension of the DeepLabV3 Architecture. The DeepLabV3 Architecture is a Fully Convolutional Network with an Atrous Spatial Pyramid Pooling (ASPP) module. The ASPP module consists of a series of parallel convolutional layers with different dilation rates. The DeepLabV3+ Architecture is a Fully Convolutional Network with an Atrous Spatial Pyramid Pooling (ASPP) module and an Encoder-Decoder Network. The Encoder-Decoder Network consists of a series of convolutional layers followed by an upsampling layer. The output of the ASPP module is concatenated with the output of the Encoder-Decoder Network. The concatenated output is then passed through a series of convolutional layers to generate the final output. The DeepLabV3+ Architecture is shown in the figure above.

### DeepLabV3+ Architecture Results
The IoU score plots of the DeepLabV3+ architecture is shown below.
![DeepLabV3+ Architecture Results]('https://github.com/riyavnitk/DeepGlobe_Road_Extraction_MGCV/blob/master/images/DLV3IoU.png')
The loss plots of the DeepLabV3+ architecture is shown below.
![DeepLabV3+ Architecture Results]('https://github.com/riyavnitk/DeepGlobe_Road_Extraction_MGCV/blob/master/images/DLV3Dice.png')
The predictions of the DeepLabV3+ architecture in identifying the road netowkr of one such ariel image is shown below.
![DeepLabV3+ Architecture Predictions]('https://github.com/riyavnitk/DeepGlobe_Road_Extraction_MGCV/blob/master/images/DLV3pred.png')


# Conclusion
We can draw the following conclusions from the plots and the predictions:
- The DeepLabV3+ architecture outperforms the U-Net architecture in terms of the IoU score. The DeepLabV3+ architecture has an IoU score of 0.95 and the U-Net architecture has an IoU score of 0.73. 
- The DeepLabV3+ architecture manages to reach a higher IoU score and lower loss metric in just 3 epochs, whereas the U-Net architecture give a reasonable IoU score even after 100 epochs. 

The DeepLabV3+ architecture is able to identify the road network more accurately than the U-Net architecture. Both employ encoder-decoder architectures to solve the given image segmentation problem, but the DeepLabV3+ architecture is able to achieve better results than the U-Net architecture. The DeepLabV3+ architecture is able to achieve better results than the U-Net architecture because of the following reasons:
- We can exploy trasfer learning encoder architectures (like resnet) to get better results during the encoding process. The U-Net architecture just simply has a contractual convolution, followed by expansive convolution to achieve the encoding and decoding process.
- The DeepLabV3+ architecture employs an ASPP module which is able to capture the context of the image better than the U-Net architecture. The ASPP module consists of a series of parallel convolutional layers with different dilation rates. The ASPP module is able to capture the context of the image better because of the different dilation rates. The U-Net architecture does not employ an ASPP module.
- DeepLabV3+ is also heavier than U-Net. The DeepLabV3+ architecture has 41,259,560 parameters and the U-Net architecture has 31,030,922 parameters. The DeepLabV3+ architecture has more parameters than the U-Net architecture because of the ASPP module. The ASPP module consists of a series of parallel convolutional layers with different dilation rates. The ASPP module has more parameters than the U-Net architecture because of the different dilation rates. The U-Net architecture does not employ an ASPP module.