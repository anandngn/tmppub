
# Sensor Fusion - 3D Object Detection

## A deep-learning approach is used to detect vehicles in LiDAR data based on a birds-eye view perspective of the 3D point-cloud. Also, a series of performance measures is used to evaluate the performance of the detection approach. The tasks in this part make up the mid-term project.


## Section 1 : Compute Lidar Point-Cloud from Range Image


### Visualize range image channels (ID_S1_EX1)

In the Waymo Open dataset, lidar data is stored as a range image. Therefore, this task is about extracting two of the data channels within the range image, which are "range" and "intensity", and convert the floating-point data to an 8-bit integer value range. Once you have done so, please use the OpenCV library to stack the range and intensity image vertically and visualize it.

#### Range Image: Frame 0 
![s1_ex1_range_image_1](./media/S1/s1_ex1_range_image_1.png)

#### Range Image: Frame 1 

![s1_ex1_range_image_2](./media/S1/s1_ex1_range_image_2.png)






### Visualize lidar point-cloud (ID_S1_EX2)

The goal of this task is to use the Open3D library to display the lidar point-cloud in a 3d viewer in order to develop a feel for the nature of lidar point-clouds. A detailed description of all required steps can be found in the code.

When the code is functional, you are supposed to use the viewer to locate and closely inspect point-clouds on vehicles and write a short report that includes the following items:

    Find and display 10 examples of vehicles with varying degrees of visibility in the point-cloud
    Identify vehicle features that appear as a stable feature on most vehicles (e.g. rear-bumper, tail-lights) and describe them briefly. Also, use the range image viewer from the last example to underpin your findings using the lidar intensity channel.


#### Lidar Movie : Zoom in and rotate the open3d display window on VM to capture the vehicle images
![s1_ex2_pointcloud_zoom_movie](./media/S1/s1_ex2_pointcloud_zoom_movie.mp4)



#### Lidar Point Cloud 1
![s1_ex2_lidar_pc_1](./media/S1/s1_ex2_lidar_pc_1.png)

#### Lidar Point Cloud 1
![s1_ex2_lidar_pc_2](./media/S1/s1_ex2_lidar_pc_2.png)

#### Lidar Point Cloud 1
![s1_ex2_lidar_pc_3](./media/S1/s1_ex2_lidar_pc_3.png)

#### Lidar Point Cloud 1
![s1_ex2_lidar_pc_4](./media/S1/s1_ex2_lidar_pc_4.png)

#### Lidar Point Cloud 1
![s1_ex2_lidar_pc_5](./media/S1/s1_ex2_lidar_pc_5.png)

#### Lidar Point Cloud 1
![s1_ex2_lidar_pc_6](./media/S1/s1_ex2_lidar_pc_6.png)

#### Lidar Point Cloud 1
![s1_ex2_lidar_pc_7](./media/S1/s1_ex2_lidar_pc_7.png)

#### Lidar Point Cloud 1
![s1_ex2_lidar_pc_8](./media/S1/s1_ex2_lidar_pc_8.png)

#### Lidar Point Cloud 1
![s1_ex2_lidar_pc_9](./media/S1/s1_ex2_lidar_pc_9.png)

#### Lidar Point Cloud 1
![s1_ex2_lidar_pc_10](./media/S1/s1_ex2_lidar_pc_10.png)


From the above images it can be clearly observed that the dominant parts that appear in the LIDAR point cloud are tail lamps, bumper, front light. Sometimes when the cars are at an angle side mirrors also appear clearly. 




## Section 2 : Create Birds-Eye View from Lidar PCL


In first step is to create a birds-eye view (BEV) perspective of the lidar point-cloud. Based on the (x,y)-coordinates in sensor space, respective coordinates within the BEV coordinate space are computed the the actual BEV map can be filled with lidar data from the point-cloud. once this task is done the respestice height and intensity map of the same is computed as well, which are shown below

### Convert sensor coordinates to BEV-map coordinates (ID_S2_EX1)
The goal of this task is to perform the first step in creating a birds-eye view (BEV) perspective of the lidar point-cloud. Based on the (x,y)-coordinates in sensor space, you must compute the respective coordinates within the BEV coordinate space so that in subsequent tasks, the actual BEV map can be filled with lidar data from the point-cloud.

#### Lidar Point Cloud 1
![s2_ex1_bev_from_pcl_1](./media/S2/s2_ex1_bev_from_pcl_1.png)


### Compute intensity layer of the BEV map (ID_S2_EX2)

The goal of this task is to fill the "intensity" channel of the BEV map with data from the point-cloud. In order to do so, you will need to identify all points with the same (x,y)-coordinates within the BEV map and then assign the intensity value of the top-most lidar point to the respective BEV pixel. Please name the resulting list of points lidar_pcl_top as it will be re-used in later tasks. Also, you will need to normalize the resulting intensity image using percentiles, in order to make sure that the influence of outlier values (very bright and very dark regions) is sufficiently mitigated and objects of interest (e.g. vehicles) are clearly separated from the background.

####  Intensity layer of the BEV map 
![s2_ex2_bev_intensity_1](./media/S2/s2_ex2_bev_intensity_height_1.png)


### Compute height layer of the BEV map (ID_S2_EX3)
The goal of this task is to fill the "height" channel of the BEV map with data from the point-cloud. In order to do so, please make use of the sorted and pruned point-cloud lidar_pcl_top from the previous task and normalize the height in each BEV map pixel by the difference between max. and min. height which is defined in the configs structure.

####  Height layer of the BEV map 
![s2_ex3_bev_height_1](./media/S2/s2_ex2_bev_intensity_height_1.png)






## Section 3 : Model-based Object Detection in BEV Image

### Add a second model from a GitHub repo (ID_S3_EX1)
The model-based detection of objects in lidar point-clouds using deep-learning is a heavily researched area with new approaches appearing in the literature and on GitHub every few weeks. On the website Papers With Code(opens in a new tab) and on GitHub, several repositories with code for object detection can be found, such as Complex-YOLO: Real-time 3D Object Detection on Point Clouds(opens in a new tab) and Super Fast and Accurate 3D Object Detection based on 3D LiDAR Point Clouds(opens in a new tab).

The goal of this task is to illustrate how a new model can be integrated into an existing framework. 


### Extract 3D bounding boxes from model response (ID_S3_EX2)
As the model input is a three-channel BEV map, the detected objects will be returned with coordinates and properties in the BEV coordinate space. Thus, before the detections can move along in the processing pipeline, they need to be converted into metric coordinates in vehicle space. This task is about performing this conversion such that all detections have the format [1, x, y, z, h, w, l, yaw], where 1 denotes the class id for the object type vehicle.

#### 3D bounding boxes added to the images
![s3_ex2_detection_bnd_box_1](./media/S3/s3_ex2_detection_bnd_box_1.png)





## Section 4 : Performance Evaluation for Object Detection


### Compute intersection-over-union between labels and detections (ID_S4_EX1)
The goal of this task is to find pairings between ground-truth labels and detections, so that we can determine wether an object has been (a) missed (false negative), (b) successfully detected (true positive) or (c) has been falsely reported (false positive). Based on the labels within the Waymo Open Dataset, your task is to compute the geometrical overlap between the bounding boxes of labels and detected objects and determine the percentage of this overlap in relation to the area of the bounding boxes. A default method in the literature to arrive at this value is called intersection over union, which is what you will need to implement in this task.


### Compute false-negatives and false-positives (ID_S4_EX2)
Based on the pairings between ground-truth labels and detected objects, the goal of this task is to determine the number of false positives and false negatives for the current frame. After all frames have been processed, an overall performance measure will be computed based on the results produced in this task.

### Compute precision and recall (ID_S4_EX3)
After processing all the frames of a sequence, the performance of the object detection algorithm shall now be evaluated. To do so in a meaningful way, the two standard measures "precision" and "recall" will be used, which are based on the accumulated number of positives and negatives from all frames.


#### Graphing performance metrics
![s4_ex3_default_perf_1](./media/S4/s4_ex3_default_perf_1.png)


#### Using the labels, metrics should get perfect scores
![s4_ex3_perfect_perf_1](./media/S4/s4_ex3_perfect_perf_1.png)


