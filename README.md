# face-similarities
Using
Requirements
Python >= 3.6
Usage
Clone the github repo

git clone https://github.com/anujkhare/face-similarity-pytorch.git
cd face-similarity-pytorch
Install requirements:

pip install -r requirements.txt
Run the inference script:

python predict.py -img1 <path-to-image-1> -img2 <path-to-image-2>
Run python pre
dict.py --help for more options.

Training
Use the jupyter notebook train-siamese.ipynb.

Details
Approach
I used a Siamese network along with contrastive loss for learning (dis)similarity between image pairs.

Refer to src for details and the code.

Data split
Only used the LFW data set.

Randomly separate all images of some people for the test set
From all images in the training part, split out training/validation image sets
The held-out set will contain people that the model would have never seen during training/validation.

Pre-processing / augmentations
Use dlib face detection to crop a tight bbox around the face. Expand the b-box by 20% on all sides to incorporate more context.
Apply some random transformations (horizontal flip, rotations)
Resize and/or pad to 160 * 160
Labels and Sampling
Given two images, the label is defined as:

0: if the images belong to different people
1: if the images belong to the same person
We'll pick up pairs of images from the given set using the following strategy:

Positive pair: pick the next positive pair from all the available pairs
Negative pair: for one of the images picked for the positive pair, find a random negative image
Below are some sample pre-processed pairs:
![image](https://github.com/user-attachments/assets/b7d15e7d-e453-428d-a088-0baabf8194ef)

image

Each row is a pair, every alternate row starting with the first is a positive pair.

Choosing a threshold
Find the predicted distances on validation set pairs
Choose a distance threshold that maximizes the F1
Sample Outputs
From the train set:
![preds_train](https://github.com/user-attachments/assets/e0e83e03-3f6d-45e9-9b71-53333bb6d783)

Train preds

From the val set:

Val preds

From the test set:

Test preds

Notes
The model is overfitting significantly!
Loss curves:

Train loss Val loss

Usinng threshold=1.5:

Data split	F1	N-pairs
Train	98.74%	12000
Val	93.81%	12000
Test	57.61%	420
Contrastive loss vs Triplet loss
The FaceNet paper as well as the implementation in OpenFaces use a "triplet loss" for the metric learning. I chose

Better sampling strategies
Presently, I am sampling all possible positive pairs and an equal number of randomly chosen negative pairs.

Better sampling strategies or online hard example mining would improve results.

Pre-trained models
I didn't use any pre-trained models since accuracy was not the main focus here. If needed, VGG-Face seems like a great choice!
