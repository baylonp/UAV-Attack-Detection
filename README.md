


# UAV-Attack-Detection

In this project, me and [my colleague](https://github.com/Davide-Di-Rocco-88) wanted to apply some knowledge of Artificial Intelligence to Cybersecurity, and ended up building a model that recognizes when UAV (Unmanned Aerial Vehicles) are under attack. This README will explain the results we obtained using various models and the knowledge we gained from this work. IPYNB scripts are provided in order to replicate the work.


# Table Of Content

- [Introduction to Legislative Aspects](https://github.com/baylonp/UAV-Attack-Detection/blob/main/README.md#introduction-to-legislative-aspects)

- [Dataset](https://github.com/baylonp/UAV-Attack-Detection#dataset)
  - [How to create one single dataset from 2 groups of features?](https://github.com/baylonp/UAV-Attack-Detection#how-to-create-one-single-dataset-from-2-groups-of-features)
- [Models](https://github.com/baylonp/UAV-Attack-Detection?tab=readme-ov-file#models)
- [Additional Work](https://github.com/baylonp/UAV-Attack-Detection?tab=readme-ov-file#additional-work)
- [Dataset 1 Analysis](https://github.com/baylonp/UAV-Attack-Detection?tab=readme-ov-file#dataset-1-analysis)
  - [PCA (Principal Component Aalysis)](https://github.com/baylonp/UAV-Attack-Detection?tab=readme-ov-file#pca-analysis)
- [Dataset 2 Analysis](https://github.com/baylonp/UAV-Attack-Detection?tab=readme-ov-file#dataset-2)
- [Feauture Importance](https://github.com/baylonp/UAV-Attack-Detection?tab=readme-ov-file#feature-importance)
- [Conclusions](https://github.com/baylonp/UAV-Attack-Detection?tab=readme-ov-file#conclusion)
- [References](https://github.com/baylonp/UAV-Attack-Detection/blob/main/README.md#references)


# Introduction to Legislative Aspects

Over the years, drone technology has become widespread and omnipresent in our society. Initially designed and developed for military purposes, these devices are now essential in key sectors such as agriculture, surveillance, and humanitarian emergencies.

Major companies like Amazon are testing rapid delivery services using drones, while in agriculture, drones have proven to be highly efficient in monitoring crops. However, their use has raised, and continues to raise, numerous political, social, and legal questions.

As a result, a legislative intervention was necessary—one that would not be limited to individual states but would lay the foundations for harmonized regulation.
At the international level, several organizations and treaties regulate drone use, including the **International Civil Aviation Organization (ICAO)**, which is currently developing an integrated traffic management system for drones (the so-called UTM - Unmanned Traffic Management), and the **European Union Aviation Safety Agency (EASA)**. The latter’s main regulation, Regulation (EU) 2019/947, which came into force in 2019, distinguishes between different categories of drone use: open, specific, and certified, depending on the level of risk to the public. This tiered system allows, among other things, for a distinction between drones used for recreational purposes (hobby flying) and those used for commercial or industrial applications.

At the **national level**, drone use is governed by Regulation (EU) 2019/947, supplemented by the Italian UAS-IT Regulation, adopted by ENAC (the Italian Civil Aviation Authority). This authority defines drones as Remotely Piloted Aircraft (RPA) and sets specific rules regarding:

- The maximum flight altitude, generally set at 120 meters to ensure the safety of populated areas (though this is not an absolute limit);
- The requirement to always keep the drone within the pilot's direct line of sight (VLOS – Visual Line of Sight), applicable to the open category;
- No-fly zones, including airports, military bases, and densely populated urban areas;
- The mandatory registration of drones weighing more than 250 grams and the requirement to obtain a license for commercial drone pilots and for those operating in the open category.

In parallel, Regulation (EU) 2019/945 establishes technical and safety requirements for the manufacturing and sale of drones within the European market, introducing a classification system from C0 to C6 and making CE marking mandatory from 2024.


Additionally, regarding privacy and personal data protection, the General Data Protection Regulation (GDPR) mandates that drone operators, especially those using drones for commercial purposes, comply with strict criteria such as data minimization and obtaining consent from individuals being recorded when applicable (particularly when footage is used for commercial distribution).
In conclusion, it is clear that drones represent a technology with immense potential. However, while they can bring extraordinary benefits, they also pose risks of misuse and abuse, making it crucial to accompany their adoption with public debate and a clear, shared regulatory framework. Only in this way will it be possible to fully harness the opportunities offered by this innovation while ensuring the protection of fundamental rights and collective security.

# Dataset

The hardest thing to do was working on the dataset. We took it from [here](https://github.com/uamughal/UAVs-Dataset-Under-Normal-and-Cyberattacks).
As you can see, the dataset comprises 5 classes (Bening, DoS, Replay Attack, Evil Twin, FDI) and for each one of them there are 2 types of features, Cyber(e.g. 'frame.number', 'wlan.duration') and Physical ones (e.g. 'pitch','roll','yaw'). After doing some study on the data, we decided to remove the FDI (False Data Injection) class because the physical and Cyber features where completely different from the ones of other class, and so making it impossible for us to build a coherent dataset.

In total, for every class, we had 37 Cyber-Features and 16 Physical Features. Later in the study, we discovered that not all the classes had the same features, as it is the case for the data belonging to the Evil Twin attack, so we decided to create two different dataset:

- **1st Dataset** --> Only Classes with common features (Bening, DoS, Replay) , 37 + 16 features per tuple in total
  
![image](https://github.com/user-attachments/assets/637f4456-3482-4753-9608-592057d5fab5)

- **2nd Dataset** --> All the classes, but just the common features. So now we work on Bening, DoS, Replay, Evil twin with just 17 features in total
  
![image](https://github.com/user-attachments/assets/f471ed14-97c6-42e0-87ee-5aeddb1b9d78)




# How to create one single dataset from 2 groups of features?
Since cyber and phyisical features needed to be put in one single dataset in order for the model to be trained, we iterated on all the timestamps of the cyber and physical features (yes, every tuple had a timestap) and for every physical tuple, we addedd those features to the closest timestamp in the cyber tuple. In the end, we had 51 feaures rich tuples (37 +16 = 53, 53 - 2 = 51; we removed the 'timestamp' and 'barometer' features)


# Models
Since it was an educational work, we applied 3 models to these three datasets:
- Decision Tree
- Näive Bayesian
- Multi-Layer Feed Forward Neural Network.

  Regarding the Multi-Layer Feed Forward Neural Network, since time didn't let us do the proper fine tuning, we tried chaging the activation function from ReLU to Sigmoid, but it didn't output any considerable results


# Additional work
For every model, we applied two different types of normalization (z-score and Min-Max) and also performed PCA (Principal Component Analysis).


# Dataset 1 Analysis


![image](https://github.com/user-attachments/assets/1ca48f34-1bb9-4899-a40b-c5a490970234)


**Why accuracy values using Min-Max or Z-score normalization are the same for Decision Tree and Naive Bayesia nmodels?** Because Decision tree doesn't weights the features, so normalization doesn't change anything. Also Naive Bayesian already uses mean and standard deviation for calculating the probabilities, so measures are already normalized.

**Why instead for MMFFNN the accuracy increases  using z-score normalization?** Here in this model weights are applied to feature so evidently the use of z-score normalization works better because some outliers perturb the Min-Max method.

ALso, Naive Bayesian perfroms poorly because of the assumption it makes about the features: THEY ARE ALL INDEPENDENT. Evidently, this is not the case as we can see from the heatmap below, there are many featuresa that are coorelated.

![image](https://github.com/user-attachments/assets/8029d883-bccd-4c67-8c7f-a5be46cb4a8c)



Also, Naive Bayes model suffers from the **curse fo diensionality** because it calculates the propability for each feature independently, and here we have 51 features.

# PCA Analysis

![image](https://github.com/user-attachments/assets/9e61587c-3999-438b-acd7-363779f0096a)


Regarding the Naive Bayesian model, the PCA reduces the features from 49 to 10, and so the model is not anymore affected by the curse of dimensionality and improves a bit.

The accuracy decreases for the Decision Tree model instead because of how PCA works, creating fictituous features, in this way the decision tree doesn't work well. 

# Dataset 2

![image](https://github.com/user-attachments/assets/8505706f-a774-46fe-b895-de8ee20c7f74)

Decision tree looses accuracy with PCA becasue there are less features and also applying PCA, as we already sad, create fictitious features that compromise the overall model.

Regardin Non-PCA operations, Naive bayesian Model instead improves a lot, since features are reduced a lot and also becasue the fetures are less correlated with respect to the dataset 1, as we can see from the correlation matrix below


![image](https://github.com/user-attachments/assets/43d0c61f-444b-4ac4-9457-8d70f0ba1c06)


# Feature Importance

One impotant thing to get from these graphs below is that among the most important features, we have physical features. They help a lot the model in classifying classes correctly.

DATASET 1

![image](https://github.com/user-attachments/assets/c5c90fef-f4f5-49f8-91c5-728d8e8ed695)



DATASET 2
![image](https://github.com/user-attachments/assets/b032bfbd-0605-42b6-aa08-8d1fc3f8d66a)


# Conclusion
We can consider building a Real-Time Intrusion Detection System placing sensors on the drone and  using edge AI hardware like NVIDIA Jetson Nano or Intel Movidius to use our model on board while flying.




# References
[Original Paper](https://ieeexplore.ieee.org/abstract/document/10368002)
