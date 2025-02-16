# UAV-Attack-Detection

IN this project, me and my colleague wanted to apply some knowledge of Artificial Intelligence to Cybersecurity, and ended up building an a model that recognize when UAV (Unmanned Aerial Vehicle) are under attack. This README will explain the results we obtained using various models and the knowledge we gained from this work. IPYNB script are provided in order to replicate the work.


# Dataset

The hardest thing to do was working on the dataset. We took it from (here)[https://github.com/uamughal/UAVs-Dataset-Under-Normal-and-Cyberattacks].
As you can see, the dataset comprises 5 classes (Bening, DoS, Replay Attack, Evil Twin, FDI) and for each one of them there are 2 type of features, Cyber(e.g. 'frame.number', 'wlan.duration') and Physical ones (e.g. 'pitch','roll','yaw'). After doing some study on the data, we decided to remove the FDI (False Data Injection) class because the physical and Cyber features where completely different from the ones of other class, and so making it impossible for us to build a coherent model.

In total, for every class, we had 37 Cyber-Features and 16 Physical Features. Later in the study, we discovered that not all the classes have the same features. As it is the case for the data belonging to the Evil Twin attack, so we decided to create two different dataset:

- 1st Dataset --> Only Classes with common features (Bening, DoS, Replay) , 37 + 16 features in total
![image](https://github.com/user-attachments/assets/637f4456-3482-4753-9608-592057d5fab5)

- 2nd Dataset --> All the classes, but just the common features. So now we work on Bening, DoS, Replay, Evil twin with just 17 features in total
![image](https://github.com/user-attachments/assets/f471ed14-97c6-42e0-87ee-5aeddb1b9d78)



METTERE FOTO DISTRIBUZIONE CLASSI DEI DUE DATASET

# How to create one single dataset from 2 groups of features?
Since cyber and phyisical features needed to be put in one single dataset in order for the model to be trained on, we itered on all the timestamps of the cyber and physical features (yes, every tuple had a timestap) and for every physical tuple, we addedd those features to the closest timestamp in the cyber tuple. In the end, we had 51 feaures rich tuples (37 +16 = 53, 53 - 2 = 51; we removed the 'timestamp' and 'barometer' features)


# Models
Since it was an educational work, we applied 3 models to these three datasets:
- Decision Tree
- Näive Bayesian
- Multi-Layer Feed Forward Neural Network


# Additional work
For every model, we applied two different types of normalization (z-score and Min-Max) and also performed PCA (Principal Component Analysis).


# Dataset 1 Analysis

METTERE TABELLA VALORI ACCURACY

![image](https://github.com/user-attachments/assets/1ca48f34-1bb9-4899-a40b-c5a490970234)


why min - max and Z-score are the same for Decision Tree and Naive Bayesian. Decision tree doesnt weights the features, so normalization doesn't change anything. Also NaiveBayesian already uses mean and standard deviation for calculating the probabilities, so measures are already normalized.
why instead for MMFFNN the accuracy increases  using z-score. here in this model weights are applied to feature so evidently the use of z-score normalization works better because some outliers perturbs the min max method.

ALso, Naive Bayesian perfroms poorly because of the assumption it makes about the feature: THEY ARE ALL INDEPENDENT. Evidently, this is not the case as we can see from slide 19, there are many featuresa that are coorelated in the dataset 1.
ALso, Naive Bayes model suffers from the curse fo diensionality because it calculates the propability for each feature independently, and here we have 51 features.

