# Fully convolutional networks for image denoising

Here, I've trained an autoencoder that takes in a noisy image and outputs a denoised version of the image.

## 1.1 Loading the Dataset

Here I create a dataset class which returns a noisy image and its ground truth label.

## 1.2 Model Architecture

The Autoencoder consists of 2 parts - the encoder and decoder.

- The encoder, which is used for feature extraction, takes in the input image of size 3x64x64. It consists of three blocks of convolutional layers using a kernel size of 3x3 with increasing depth (64 -> 128 -> 256) followed by max pooling, which progressively reduces the spatial dimensions and increases the depth of the feature maps.

- The decoder consists of three blocks of transpose convolutional layers using a kernel size of 3x3 with decreasing depth (256 -> 128 -> 64) followed by unpooling, which progressively restores the spatial dimensions and reduces the depth to produce the final output with the same size as the input image 3x64x64.

## 1.3 Training the Model

**Loss Function**

Following [1], I've used the **Mean Squared Error (MSE)** as the loss function of the autoencoder network. It compares each pixel value of the ground truth to the output of the network. It is simple to compute and also implicitly penalizes larger errors, thus allowing for some form of regularization.

**Training and Evaluation**

I've used the **Structural Similarity Index (SSIM)** [2] to evaluate the model. It is a perceptual metric that measures the similarity between two images. SSIM considers changes in structural information, making it more aligned with human visual perception. SSIM values range from -1 to 1, where 1 indicates perfect similarity between the images.

## 1.4 Results

The model achieves an SSIM of 0.887 and 0.897 on the train set and validation set respectively. Evaluating on the test set, the model achieves an SSIM of 0.895, showing there's no overfitting.

Visually inspecting the results, I observed that the denoised images have significantly reduced noise while preserving the structural details of the original images.
