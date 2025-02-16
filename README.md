# UAV-Attack-Detection

IN this project, me and my colleague wanted to apply some knowledge of Artificial Intelligence to Cybersecurity, and ended up building an a model that recognize when UAV (Unmanned Aerial Vehicle) are under attack. This README will explain the results we obtained using various models and the knowledge we gained from this work. IPYNB script are provided in order to replicate the work.


# Dataset

The hardest thing to do was working on the dataset. We took it from (here)[https://github.com/uamughal/UAVs-Dataset-Under-Normal-and-Cyberattacks].
As you can see, the dataset comprises 5 classes (Bening, DoS, Replay Attack, Evil Twin, FDI) and for each one of them there are 2 type of features, Cyber(e.g. 'frame.number', 'wlan.duration') and Physical ones (e.g. 'pitch','roll','yaw'). After doing some study on the data, we decided to remove the FDI (False Data Injection) class because the physical and Cyber features where completely different from the ones of other class, and so making it impossible for us to build a coherent model.

In total, for every class, we had 37 Cyber-Features and 16 Physical Features. Later in the study, we discovered that not all the classes have the same features. As it is the case for the data belonging to the Evil Twin attack, so we decided to create two different dataset:

- 1st Dataset --> Only Classes with common features (Bening, DoS, Replay) , 37 + 16 features in total
- 2nd Dataset --> All the classes, but just the common features. So now we work on Bening, DoS, Replay, Evil twin with just 17 features in total

   
# Models
Since it was an educational work, we applied 3 models to these three datasets:
- Decision Tree
- Näive Bayesian
- Multi-Layer Feed Forward Neural Network
