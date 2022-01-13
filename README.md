# Colorize Black and White images

This program is based on a 2016 paper published by Richard Zhang, Phillip Isola, and Alexei A. Efros, which can be found [here](https://arxiv.org/abs/1603.08511). Their project utilized the ImageNet database (or rather, a subset of it - one million of the more than fourteen million images) to create a pre-trained convolutional neural network.

Zhang et al. converted all the images they used from the RGB color space to the Lab color space, which does a better job of mimicking how humans see color. Similar to the RGB (red/green/blue) color space, Lab also has three channels. The L channel encodes lightness intensity, the a channel encodes green-red, and the b channel encodes blue-yellow.

Since the L channel encodes only the intensity, it is used to represent the grayscale input of an image. From there, the network must predict both the a and b channels. Once those predictions have been made, the L channel is recombined with the ab channels, and the result is then converted back into RGB.

The final result is a believable (if not entirely accurate) colorization of the original grayscale image.
