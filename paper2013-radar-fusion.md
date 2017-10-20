|논문명 |Object Detection and Tracking with Side Cameras and RADAR in an Automotive Context |
| --- | --- |
| 저자\(소속\) | Peter Hofmann\(Hella Aglaia Mobile Vision\) |
| 학회/년도 | 석사학위 2013, [논문](http://www.mi.fu-berlin.de/inf/groups/ag-ki/Theses/Completed-theses/Master_Diploma-theses/2013/Hofmann/Master-Hofmann.pdf?1381479774) |
| 키워드 | |
| 데이터셋(센서)/모델 |RADAR + Side Cameras, |
| 참고 | |
| 코드 | |


### 1.2 Sensor Fusion

###### [ 정의 ]

Data fusion techniques combine data from multiple sensors and related information to achieve more specific inferences than could be achieved by using a single, independent sensor. [HL01]

###### [ 용어 ]

- A sensor : a device which provides information of the environment in any form of raw data. 

- Feature extraction : the process of extracting meaningful information from the raw data of a sensor
	-  e.g. points that represent corners in a camera image. 

- State estimation : the current state based on the given input, 
	- e.g. the position of a tracked vehicle

###### [ 분류 ]

- In [HL01] these are referenced as 
	- Data Level Fusion, 
	- Feature Level Fusion 
	- Declaration Level Fusion. 

#### A. Data Level Fusion

![](https://i.imgur.com/VISRuZz.png)

- Data Level Fusion or Low Level Sensor Fusion (Figure 1.1) describes 
- a method of combining raw data from different sensors with each other,
- e.g. having a calibrated camera and a calibrated Time-of-Flight (ToF) depth-sensor which creates a depth map of the environment, each camera pixel can be mapped to a distance measurement of the ToF sensor and vice versa.

#### B. Feature Level Fusion

![](https://i.imgur.com/NQv8AFk.png)

- In Feature Level Fusion (Figure 1.2) or Mid-Level Fusion, 
- the sensor data is presented via feature vectors which describe meaningful data extracted from the raw data. 
- These feature vectors build the base for fusion. 
- This method can be found in 3D-reconstruction for example. 
- In this approach image features from different cameras are extracted to identify corresponding points in each image of
different camera views.

#### A. Declaration Level Fusion

![](https://i.imgur.com/ezCh0VE.png)

- Declaration Level Fusion or High Level Fusion (Figure 1.3) is 
- the combination of independent state hypotheses from different sensors. 
- All sensors estimate the state individually. 
- The final state is a fusion of all state hypotheses from the different sensors. 
- The most famous implementation of this approach is probably the **Kalman Filter** [KB61]. 
	- Each state from the individual sensors with their respective error covariance is used for correcting the state estimate in the Kalman Filter. 
	- The error covariance represents the trust in the state estimation, e.g. a camera image is reliable for estimating the width of objects but distance or speed measurements are very inaccurate. 
	- In contrast a RADAR sensor provides very accurate distance and velocity measurements. 
	- Thus, in the final state estimate, velocity information and distance will be closer to the RADAR measurements, while the size would be closer to the measurements from the camera which, in theory, should result in a better final state estimate.

---

### 1.4 State of the Art

#### 1.4.1 RADAR-based Tracking Approaches
#### 1.4.2 Camera-based Approaches
#### 1.4.3 Sensor Fusion Approaches 

- 최근 연구는 3D 센서에 집중되어 있다. `Current research mostly focuses on sensors which provide 3-dimensional information of the environment. `

###### [LiDAR + Camera]

- The approach combines a **LIDAR sensor** with **camera data**. 
	- LIDAR sensor의 Distance Map에서 **RANSAC**와 **3D adjacency**를 이용하여 지면과 물체를 구분 할수 있다. `The distance map is used to classify parts of the image to ground plane or obstacles using RANSAC and 3D adjacency. `
	- image patches are classified to different classes by evaluating color histograms, texture descriptors from **Gaussian** and **Garbor filtered** images as well as local binary patterns. 

- Resulting in 107 features, each patch is classified by a Multi-Layer-Perceptron to different categories. 

- The size of the object determined by the LIDAR obstacle estimation as well as labels from the image analysis are passed into a fuzzy logic frame workwhich returns three labels – environment, middle high or obstacle. 

###### [RADAR + LiDAR]

- In [VBA08] a framework for **self-location** and **mapping** is presented. 

- This approach is based on an **occupancy grid** that is used to fuse information from **RADAR** and **LIDAR**. 
	- An occupancy grid discretizes the environment space into small two dimensional grid tiles. 
	- The **LIDAR sensor** is used to **estimate** a **probability** of a grid cell to be occupied. 
	- A **high probability** means there is an **obstacle** in the respective grid cell. 

- 이동하는 물체의 탐지는 해당 그리드값의 변화 감지를 통해 알수 있다. `Moving objects are detected through changes in the occupancy of grid cells. `
	- Basically, if a cell is occupied at a time point, and in the next timestep, the adjacent cell is detected to be occupied, this is assumed as a motion. 

###### [RADAR + Laser ]

- A similar method of fusing short range RADAR and laser measurements is proposed in [PVB+09]. 

- The system covers more performance optimizations than[VBA08]. 

- Instead of estimating a **probability representation**, the grid cells are either occupied or empty based on the number of measurements of the laser scanner for the respective cell. 

- Adjacent cells that are occupied are merged to an object. 

- A Kalman filter [KB61] is used to track the detected objects. 

###### [ RADAR+camera ]

- The approach proposed in [ABC07] shows a fusion of RADAR and camera 
	- in which the camera is used to verify and optimize RADAR object tracks. 

- The camera is oriented to the front. 

- The center points of RADAR objects are projected into the image based on the camera calibration. 

- The symmetry of image sections around the projected point is estimated. 

- Ideally, the symmetry is large around the RADAR hypothesis as front views and rear views of vehicles are symmetric.

- Searching for higher symmetry values in a predefined environment around the RADAR hypothesis is used to correct the position estimate. 

- However, this approach does only work for scenarios in which vehicles are viewed from the rear or front.

---

|논문명 |Multiple Sensor Fusion and Classification for Moving Object Detection and Tracking |
| --- | --- |
| 저자\(소속\) | Ricardo Omar Chavez-Garcia  |
| 학회/년도 | IEEE TRANSACTIONS 2016, [논문](http://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=7283636) |
| 키워드 | |
| 데이터셋(센서)/모델 | |
| 참고 | |
| 코드 | |

## 2. RELATED WORK
![](https://i.imgur.com/X0UK5vK.png)
Fig. 2 shows the different fusion levels inside a perceptionsystem. 

- **low level** fusion is performed within **SLAM component**, 
- **detection** and **track level** fusions are performed within **DATMO component**. 

- At detection level, 
	- fusion is performed between lists of moving object detections provided by individual sensors. 

- At track level, 
	- lists of tracks from individual sensor modules are fused to produce the final list of tracks.

### 2.1 

Promising SLAM results obtained in [1]–[3] motivated our focus on the DATMO component. 


###### [Vu [1] and Wang [3]]

- Whilst Vu [1] and Wang [3]use an almost deterministic approach to perform the association in tracking, we use an evidential approach based on mass distributions over the set of different class hypotheses. 


### 2.2 track level 

- Multi-sensor fusion at **track level** requires a list of updated tracks from each sensor to fuse them into a combined list of tracks. 

###### [2, 4, 5]

- The works in [2], [4], [5] solve this problem focusing on the association problem between lists of tracks, and implementing stochastic mechanisms to combine the related objects. 

- By using an effective fusion strategy at this level, false tracks can be reduced. 

- This level is characterized by including classification information as complementary to the final output.

### 2.3 detection level

- Fusion at **detection level** aims at gathering and combining early data from sensor detections. 

###### [6]

- Labayrade et al. propose to work at this level to reduce the number of mis-detections that can lead to false tracks [6]. 

###### [7, 8] 

- Other works focus on data redundancy from active and passive sensors, and follow physical or learning constrains to increase the certainty of object detection[7], [8]. 

- These works do not include all the available kinetic and appearance information. 

- Moreover, at this level, appearance information from sensor measurements is not considered as important as the kinetic data to discriminate moving and static objects.

- When classification is considered as an independent module inside the perception solution, this is often implemented as a single-class (e.g., only classifies pedestrians) or single-sensor based classification process [2], [5]. 

- This approach excludes discriminative data from multiple sensor views that can generate multi-class modules. 

- Research perspectives point-out the improvement of the data association and tracking tasks as a direct enhancement when classification information is managed at early levels of perception [2], [5], [9].

- 가장 일반적인 접근법은 probabilistic methods 이다. `The most common approaches for multi-sensor fusion are based on probabilistic methods [1], [2]. `

- However, methods based on the Evidential framework proposed an alternative not only to multi-sensor fusion but to many modules of vehicle perception [5], [6], [9]. 

- These methods highlight the importance of incomplete and imprecise information which is not usually present in the probabilistic approaches.

### 제안 논문의 장점 

- An advantage of our fusion approach at the **detection level** is that the description of the objects can be enhanced by adding knowledge from different sensor sources. 

- For example, lidar data can give a good estimation of the distance to the object and its visible size. 

- In addition, classification information, usually obtained from camera, allows to make assumptions about the detected objects. 

- An early enrichment of objects’ description could allow the reduction of the number of false detections and integrate classification as a key element of the perception output rather than only an add-on.
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTI2ODI0NDU3NV19
-->