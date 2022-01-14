# Colorize Black-and-White images

This program is based on a 2016 paper published by Richard Zhang, Phillip Isola, and Alexei A. Efros, which can be found [here](https://arxiv.org/abs/1603.08511). Their project utilized the ImageNet database (or rather, a subset of it - one million [or so] of the fourteen million [or so] images) to create a pre-trained convolutional neural network.

Zhang et al. converted all the images they used from the BGR color space (used by the ImageNet database) to the Lab color space, which does a better job of mimicking how humans see color. Similar to the BGR (blue/green/red) color space, Lab also has three channels. The L channel encodes lightness intensity, the a channel encodes green-red, and the b channel encodes blue-yellow.

Since the L channel encodes only the intensity, it is used to represent the grayscale input of an image. From there, the network must predict both the a and b values. Once those predictions have been made, the L channel is combined with the returned ab channel, and the result is then converted back to BGR.

# Setup:

Download the three file "colorization_deploy_v2.prototxt", "colorization_release_v2.caffemodel", and "pts_in_hull.npy", all of which can be found [here](https://code.naturkundemuseum.berlin/mediaspherefornature/colorize_iiif/-/tree/master/experimental/model)
 
# Algorithm:

1) Load (possibly display) the grayscale image
2) Scale the pixel values so that they fall in the range [0, 1] instead of [0, 255]
3) Convert the image from BGR to Lab
4) Resize the image to 224 x 224 (required for the pre-trained model)
5) Extract the L channel and perform mean centering
6) Pass the L channel through the model, which will then return the predicted ab values
7) Resize the ab channel back to whatever size the original image was
8) Grab the L channel (from the original image, not the resized image)
9) Concatenate the L channel to the ab channel (this is the now-colorized image)
10) Convert the image from Lab back to BGR
11) Clip any values that fall outside the range [0, 1]
12) Convert the pixel values back to the range [0, 255]
13) Display the final result

The code is given in the Jupyter notebook ColorizeImage.ipynb

The final results are mixed. Some images turn out well, based on the model's training...

![BW01](https://user-images.githubusercontent.com/80790548/149422853-a0e521c6-a035-44a3-b9f0-893da9b75225.jpg)![Color01](https://user-images.githubusercontent.com/80790548/149422865-b05d8e4b-506e-4252-8804-c66dab7e8122.jpg)
![BW02](https://user-images.githubusercontent.com/80790548/149422876-e3dcecda-0589-4e18-bd71-338428c28d69.jpg)![Color02](https://user-images.githubusercontent.com/80790548/149422921-66f2188c-0681-4927-b9c5-bb20bd862a55.jpg)

...and others less so (my guess is that most of the left side of this picture was mistaken for foliage or reptile skin).

![BW04](https://user-images.githubusercontent.com/80790548/149423122-73b93cb1-ffaa-4480-bd2a-d2f2cd04c962.jpg)![Color04](https://user-images.githubusercontent.com/80790548/149423128-f5e2c830-ff51-4460-ac83-240848be2333.jpg)

By their own [admission](https://richzhang.github.io/colorization/), their colorization process isn't perfect. Instead of generating realistic results, it generates plausible ones which can fool humans about 32% of the time (significantly higher than previous methods).
