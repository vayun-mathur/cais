# ASL classifier
By Vayun Mathur, vayunmat@usc.edu

This project is a resnet to classify images of sign language fingerspelling into the letters they represent, trained using a pre-trained model.

## Dataset
https://www.kaggle.com/datasets/grassknoted/asl-alphabet/data

The above dataset was use for training this model. It contains 87,000 images of dimensions 200x200, split equally across 29 classes.

I preprocessed the data with 3 transformations:

1. RandomResizedCrop was used to create more variation in the dataset since there are many images that were almost identical to each other
2. RandomHorizontalFlip was used because as far as I could tell, all images were using the right hand, but fingerspelling can also be done with the left hand. Horizontal flip was done to add variation in handedness so that it is effective with either hand.
3. Normalize was used because I was doing transfer learning with a resnet model and the resnet model requires this preprocessing step.

## Model Development and Training

Before training the model, I re-created the train, validation, and test datasets because the dataset provided contained only 1 test image per class. The resulting train dataset has 6960 images, validation has 1740, and test has 2610.

When developing the model, I optimized 3 hyperparameters:

1. learning rate = 0.001 - I also tried 0.03, 0.01, 0.003, and 0.0003, but 0.001 worked significantly better than any of the others in a 2 epoch test
2. scheduler step size = 7 epochs - I observed overfitting in the 9th epoch before setting this, so I set this to decrease learning rate a bit before that epoch
3. gamma = 0.1 - I tried 0.3 but it didn't change anything, and 0.1 worked so I used that.

I set batch size to 64 so that the model wouldn't ever waste time loading data to the gpu and not computing. I didn't set it higher because I had cut down the size of the dataset already, and it had no effect on training speed

## Model Evaluation/Results

F1 score: 90.2%

Accuracy: 90.1%

Precision: 90.6%

Recall: 90.2%

#### Confusion Matrix:

![Alt Text](/download.png)

## Discussion

Given the results (>90% on all metrics), the model is effectively classifying the different fingerspelled letters. Looking at the confusion matrix, the one place where this model is getting confused most are n and m, which do look quite similar as it is.

The one major limitation holding this project back from real social good is that fingerspelling is only a small portion of sign language, making up only 8.7% of ASL signing.

However, were I to train a model that had image data for all words in sign language, such a model could be used in a number of applications. Perhaps the most important application of such a model would be making it easier for deaf people to communicate to non-deaf people through a device that turns signing into speech. However, I believe that the model I have made would not work for this purpose because many words in sign language require movement, which cannot be captured in images alone.

## Citation

I got the train loop from the [pytorch documentation](https://pytorch.org/tutorials/beginner/introyt/trainingyt.html)