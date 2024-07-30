# Training basic CNNs from scratch

## 1.1 Baseline Model

Here I've defined a base model which has two convolution layers with 5x5 kernels each followed by a Max Pooling layer then 2 linear layers with the last having the same shape as the number of classes in the dataset. The model achieves a validation accuracy of 69%.

## 1.2 Model with smaller Filters and more Convolutions

Research has shown that using smaller filters with more convolution layers gives some improvement in model performance like in VGGNet. Here I test that by modifying the base model by using 2 successive convolution layers with a kernel size of 3x3 followed by a Max Pooling layer. Everything else is kept the same.

From the results, I observed that the model achieved a slightly better performance with a validation accuracy of 73% agreeing with established findings.

## 1.3 Regularization Using Dropout

Regularization has been shown to help models learn better and a popular one is Dropout. However, there's no exact science to applying dropout in CNNs. Older [papers](https://arxiv.org/pdf/1207.0580) suggest applying dropout of p = 0.3 - 0.5 only after the last convolution and after linear layers while [newer research](http://mipal.snu.ac.kr/images/1/16/Dropout_ACCV2016.pdf) suggest also adding a small dropout of p = 0.1 to 0.2 after each convolution layer. Here I investigate both approaches keeping the model same as in 1.4 but adding dropout as required.

From the results, I observed that adding dropout does improve performance with both approaches; however adding dropout after each convolution and linear layer achieved better performance with a validation accuracy of 76% as opposed to only after the last convolution layer with 74%.

# 2. Image Augmentation

## 2.1 Augmentations Used

To make the model robust, I used the following image augmentations:

- Horizontal Flip
- Random Crop
- Color Jitter

These augmentations were applied randomly during training to help the model generalize better to unseen data.

## 2.2 Impact on Model Performance

Using these augmentations, the model's validation accuracy increased by 3%, reaching 79%.

# 3. Generating Saliency Maps

## 3.1 What are Saliency Maps?

Saliency maps help in understanding what part of an image the model is focusing on when making a prediction. By visualizing these maps, we can gain insights into the model's decision-making process.

## 3.2 Methodology

To generate saliency maps, I used the occlusion sensitivity method. This involves sliding a square block over the image, masking some part of it by setting the pixels within the block to zero. The model's prediction probability is observed to see how it changes with different parts of the image occluded.

## 3.3 Generating Saliency Maps for some Classes

### 3.3.1 Example 1 - Cat

I got an image of a cat from the web and generated the saliency map for it. The heatmap generated shows that the model's prediction probability remains unchanged when the clouds around the cat are occluded and reduces most when part of the cat's face is occluded. Puzzlingly, occluding some parts of the cat's body increases the model's confidence in its prediction.

### 3.3.2 Example 2 - Dog

I got an image of a dog from the web and generated the saliency map for it. The model accurately classifies the original dog image with a probability of 76%. The heatmap generated shows that the model's prediction probability remains unchanged when the background around the dog is occluded and reduces the most when the body and ears of the dog are occluded. Also surprisingly, the model's prediction probability increases when part of the dog's face is occluded.

### 3.3.3 Example 3 - Mailbox

I got an image of a mailbox from the web and generated the saliency map for it. The model accurately classifies the original mailbox image with a probability of 66%. The heatmap generated shows that the model's prediction probability drops considerably - to 50% when the front of the mailbox is occluded, showing that the model focuses on specific mailbox markers. Also, when the surrounding bush or the diagrams on the mailbox are occluded, the model's prediction probability increases showing that the image becomes less confused by the background in the image and is more confident in its predictions for the mailbox.
