# Abuse Detection System
---
#### Project creators
- Amit Hayun [amitos684@gmail.com](amitos684@gmail.com)
- Bar Loupo [barloupo@gmail.com](barloupo@gmail.com)
                    



Provide a Deep Learning-Based real-time solution for nursing homes and hospitals for detecting cases of abuse in the elderly population by analyzing security camera frames and performing real-time forecasting using three machine learning models YOLO, DeepSort, ADS

## Table of contents
---
* [Project highlights](#Project-highlights)
* [Introduction ](#Introduction)
* [Data collection](#Data-collection)
* [Model architecture](#Model-architecture)
* [Model training && Evaluation](#Model-training-&&-Evaluation)
* [System overview](#System-overview)
* [ADS PIPELINE Demo](#ADS-PIPELINE-Demo)
* [Input-Output examples](#Input-Output-examples)
* [Setup](#setup)
* [Reference](#Reference)


## Project highlights
---
-	Creating a new novel dataset ADS-dataset that contain worldwide videos
clips of abuse capture by surveillance cameras in the real-world scenes 

-	Design and implemented ML pipeline for video raw data to generate features, data augmentation techniques, and resampled datasets for model training.

-	Build and train a machine learning model[ADS] based on MobileNet-SSD architecture with 3D-CNN and two-stream method [RGB, OPT].
Training and evaluation of the model using AWS-SageMaker and TensorFlow frameworks. Achieved 87% in F1-score on DCSASS Abuse Dataset and 84%  on ADS Dataset.

-	Combine all models to an overall system and deploying the system in the Streamlit web app that enables the user to get real-time notification alerts
when an abuse event capture by the security camera.

## Introduction 
---
This project is defined as research(60%)\development(40%).

- Research 
Build and train deep learning models(according to a standard ML approach) to automatically identify abuse event capture by security camera

- Development
Build prototype system ADS(Abuse detection system) for deploying models and test them in a real-time environment

### Our Main goal - provide an automated solution for detecting cases of abuse of the elderly.

## Data collection
---
Data collection
In order to train deep learning models, the first step is data collection
We build data collection pipe and gather abuse video from the web
we collect 842 video clips after the filtering process


We work according to a machine learning methodology
1. search abuse video links online
2. download the links
3. convert the video to AVI format with FBS=30sec
4. cut the video into 5-sec clips
5. manual extracting from each video 5sec clips [3,4 clips for each video]
6. create more videos by using 5 data argumentation techniques
7. split the data to Train, Val, Test as shown in table2

- Method and DB expleind -[method and DB expleind.pdf](https://github.com/1amitos1/AbuseDetectionSystem_demo/files/6423235/method.and.db.expleind.pdf)

 <img src="https://user-images.githubusercontent.com/34807427/117050368-f15d1c00-ad1d-11eb-85eb-d21343f74e55.png" width="300" height="300">


## Model architecture
---
The model architecture is based on mobileNet SSD.
And the highlight of this model is utilizing
a branch of the optical flow channel to 
help build a pooling mechanism.

- Conv3D split into two channels -  RGB frame and Optical flows as shown in the figure below.
- Relu activation is adopted at the end of the RGB channel. 
- Sigmoid activation is adopted at the end of the Optical flow channel.
- RGB and Optical Flow channels outputs are multiplied together and processed by a temporal max-pooling.
- Merging Block is composed of basic 3D CNNs, used to process information after self learned temporal pooling. 
- Fully-connected layers generate output.

 <img src="https://user-images.githubusercontent.com/34807427/117047169-3c753000-ad1a-11eb-93a5-7825120596ca.png" width="550" height="400">




## Model training && Evaluation 
---
### Model training
We conduct tow experiment with different optimization algorithm
Stochastic gradient descent (SGD) and Adam   

- Experiment-1 - [Experiment 1 SGD.pdf](https://github.com/1amitos1/AbuseDetectionSystem_demo/files/6423205/Experiment.1.SGD.pdf)

- Experiment-2 - [Experiment 2 Adam.pdf](https://github.com/1amitos1/AbuseDetectionSystem_demo/files/6423210/Experiment.2.Adam.pdf)

### Evaluation
- Full Evaluation report [Model evaluation report.pdf](https://github.com/1amitos1/AbuseDetectionSystem_demo/files/6423215/Model.evaluation.report.pdf)

- We evaluate our proposed model on five Dataset.

  1-ADS data set

  2-DCSASS Abuse Dataset
 
  3-Automatic violence detection data set
 
  4- DCSASS Abuse Dataset + Automatic violence detection data set 

  5-RWF real world fight dataset

- F1 score result 
 
![res_f1_summary](https://user-images.githubusercontent.com/34807427/117053146-4a7a7f00-ad21-11eb-8c9d-74858d67efea.png)


## ADS PIPELINE Demo
---

### First step:
- We sample from the IP camera 2 frame set each set containing
149 frame set, and then we saves the sampling video in a folder

![First step](https://user-images.githubusercontent.com/34807427/117056502-0b4e2d00-ad25-11eb-9f83-a9bc9148d680.gif)

### Second step:
- Open sampling video folder and run yolo and DeepSort models to predict  tracking    bounding boxes in the video
- Bluer each frame set by bounding boxes then save those video clips in a folder

![Second step](https://user-images.githubusercontent.com/34807427/117056508-0d17f080-ad25-11eb-8119-d921dd36d9bd.gif)


### Third step:
- We open the video folder implement ads model preprocessing [resize shape, extract optical flow , uniform sampling to  64 frame for predication]


![Third step](https://user-images.githubusercontent.com/34807427/117056490-05f0e280-ad25-11eb-86b3-8706517114c2.gif)

- if the ads model identified the frame set as violence, we save the video clip and send it to the user email address     

![email](https://user-images.githubusercontent.com/34807427/117056968-93cccd80-ad25-11eb-9c8d-bd402e0fe378.png)


## System overview
---
![ads overview2](https://user-images.githubusercontent.com/34807427/117057636-5ddc1900-ad26-11eb-9b71-d6344bd0dc78.png)



## Input-Output examples
---

 #### EX-1     
 <img src="https://user-images.githubusercontent.com/34807427/117035434-fcf41700-ad0c-11eb-9e6f-c0c6d542f3ef.gif" width="350" height="350"> <img src="https://user-images.githubusercontent.com/34807427/117035426-fa91bd00-ad0c-11eb-93ff-6504835bee3e.gif" width="350" height="350">


 #### EX-2     
 <img src="https://user-images.githubusercontent.com/34807427/118256765-85fe1180-b4b6-11eb-8849-b7f1c792c1af.gif" width="350" height="350"> <img src="https://user-images.githubusercontent.com/34807427/118256758-84344e00-b4b6-11eb-8ae4-add1dca63d35.gif" width="350" height="350">



 #### EX-3     
 <img src="https://user-images.githubusercontent.com/34807427/117101927-e9ce5f00-ad7f-11eb-9226-a19eee618fdc.gif" width="350" height="350"> <img src="https://user-images.githubusercontent.com/34807427/117101936-eb982280-ad7f-11eb-8bdb-6967b9af0967.gif" width="350" height="350">



 #### EX-4     
 <img src="https://user-images.githubusercontent.com/34807427/118256967-c2317200-b4b6-11eb-8a7a-bbd98c815491.gif" width="350" height="350"> <img src="https://user-images.githubusercontent.com/34807427/118256964-c067ae80-b4b6-11eb-983d-3a61ebc169ed.gif" width="350" height="350">


 #### EX-5     
 <img src="https://user-images.githubusercontent.com/34807427/118257351-3409bb80-b4b7-11eb-9314-e7cbeaa6bc1c.gif" width="350" height="350"> <img src="https://user-images.githubusercontent.com/34807427/118257679-95318f00-b4b7-11eb-997e-bcf8265584ca.gif" width="350" height="350">


 #### EX-6     
 <img src="https://user-images.githubusercontent.com/34807427/118257495-5ef40f80-b4b7-11eb-8360-72f975abd006.gif" width="350" height="350"> <img src="https://user-images.githubusercontent.com/34807427/118257489-5dc2e280-b4b7-11eb-81fa-b0ee8a2745de.gif" width="350" height="350">







## Setup  
---
- First step downlaod yolo_v3 model
  [Yolo_v3 model](https://drive.google.com/file/d/1IbR2LtlqQxOr5w9u8yIeFYWtLJHksguF/view?usp=sharing)
- Add the Yolo_v3.h5 to model data folder
- Add the Yolo_v3.h5 path to [Yolo_v3.py](https://github.com/1amitos1/AbuseDetectionSystem_demo/blob/main/ADS_DEMO/yolo_v3.py) in __init__ function
```
self.model_path = r'./model_data/yolov3_model.h5'
```


### In ADS_pipeLine.py change to following 
- provide main_folder_output path
- provide src video input path
```
main_folder_output = r""
src_video_input = r""
user_email = None
ads_wights_path = r".\Model_to_test\model_json_format\ADS_weights.h5"
ads_model_path = r".\Model_to_test\model_json_format\ADS_model.json"
deep_sort_model_path = ".\deep_sort/mars-small128.pb"
```
### If you want to send an email alert to the user
 - fill in the user_email to the address you want to send
 ```
 user_email = None
 ```
 - go to ADS_pipeline.py in the send_email_alert function
 
```
 fromaddr = "<your email account>"
 EMAIL_PASSWORD ="<your email password>"
```


## Reference
---
- Yolo_v3 (https://github.com/qqwweee/keras-yolo3)
- DeepSort (https://github.com/nwojke/deep_sort)
- RWF (https://github.com/mchengny/RWF2000-Video-Database-for-Violence-Detection/blob/master/README.md)


```
@article{RWF-2000,
  author = {Ming Cheng, Kunjing Cai, and Ming Li},
  title={RWF-2000: An Open Large Scale Video Database for Violence Detection},
  year={2019}
}
```


```
@article{DeepSort,
  author = { B. A. &. P. D. Wojke},
  title={Simple online and realtime tracking with a deep association metric},
  year={2017}
}
```

```
@article{YOLOv3,
  author = {Joseph Redmon },
  title={YOLOv3: An Incremental Improvement},
  year={2018}
}
```

