# srez

Image super-resolution through deep learning. This project uses deep learning to upscale 16x16 images by a 4x factor. The resulting 64x64 images display sharp features that are plausible based on the dataset that was used to train the neural net.

Here's an random, non cherry-picked, example of what this network can do. From left to right, the first column is the 16x16 input image, the second one is what you would get from a standard bicubic interpolation, the third is the output generated by the neural net, and on the right is the ground truth.

![Example output](batch004200_out.jpg)

As you can see, the network is able to produce a very plausible reconstruction of the original face. As the dataset is mainly composed of well-illuminated faces looking straight ahead, the reconstruction is poorer when the face is at an angle, poorly illuminated, or partially occluded by eyeglasses or hands.


# How it works

In essence the architecture is a DCGAN where the input to the generator network is the 16x16 image rather than a multinomial gaussian distribution.

In addition to that the loss function of the generator has a term that measures the L1 difference between the 16x16 input and downscaled version of the image produced by the generator.

The adversarial term of the loss function ensures the generator produces plausible faces, while the L1 term ensures that those faces resemble the low-res input data. We have found that this L1 term greatly accelerates the convergence of the network during the first batches and also appears to prevent the generator from getting stuck in a poor local solution.

Finally, the generator network relies on ResNet modules as we've found them to train substantially faster than more old-fashioned architectures. The adversarial network is much simpler as the use of ResNet modules did not provide an advantage during our experimentation.


## Dataset-LFW
    cd datasets
    python3 create_datasets.py
then copy the images to folder dataset

# Training the model
    python3 srez_main.py --run train
# Demo
    python3 srez_main.py --run demo

# original code
this code is forked from [here](https://github.com/david-gpu/srez)




