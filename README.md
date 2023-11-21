# LVI-SAM-Easyused

This repository contains the modified code of [LVI-SAM](https://github.com/TixiaoShan/LVI-SAM) for easier using, which mainly solves the problem of ambiguous extrinsic configuration of the original LVI-SAM. Using this code, you only need to configure the extrinsic between LiDAR and IMU (**T_imu_lidar**), the extrinsic between Camera and IMU (**T_imu_camera**), and the properties of the IMU itself (**which axis the IMU rotates around counterclockwise to get a positive Euler angle output**), and then you can run LVI-SAM on different devices.


### Update

- The "**new**" branch is avaliable. We **recommend you to use the "new" branch**, because the LiDAR-Inertial system in the original LVI-SAM code repo uses an old version of [LIO-SAM](https://github.com/TixiaoShan/LIO-SAM) with some bugs, which have been fixed in the latest LIO-SAM code repo. At present, we have updated the latest version of LIO-SAM into LVI-SAM, so the system is more robust. You can use the following commands to download and compile the "**new**" branch.


## Compile

You can use the following commands to download and compile the package.

```shell
mkdir -p ~/catkin_ws/src
cd ~/catkin_ws/src
git clone https://github.com/asimfathekhani/LVI-Easyused
cd ..
catkin_make
```
---



## Run the package on different datasets

1. [Official dataset](https://drive.google.com/drive/folders/1q2NZnsgNmezFemoxhHnrDnp1JV_bqrgV)

   - Run the launch file:

     ```
     roslaunch lvi_sam run.launch
     ```

     **Note**: If you want to test the origin official LVI-SAM code (e.g. set `add_definitions(-DIF_OFFICIAL=1)` in CMakeLists.txt to compile), you should run launch file as following.

     ```
     roslaunch lvi_sam run_official.launch
     ```

   - Play existing bag files, e.g. handheld.bag:

     ```
     rosbag play handheld.bag 
     ```

   - Results of origin official code (up fig) and our modified code (down fig) on handheld.bag:

     <p align='center'>
         <img src="./doc/fig/handheld-official.png" alt="drawing" width="600"/>
     </p>
      
     
     <p align='center'>
         <img src="./doc/fig/handheld.png" alt="drawing" width="600"/>
     </p>

2. [M2DGR dataset](https://github.com/SJTU-ViSYS/M2DGR)

   - Run the launch file:

     ```
     roslaunch lvi_sam M2DGR.launch
     ```

   - Play existing bag files, e.g. gate_01.bag:

     ```
     rosbag play gate_01.bag 
     ```

   - Results of our modified code on gate_01.bag:

     <p align='center'>
         <img src="./doc/fig/gate_01.png" alt="drawing" width="600"/>
     </p>

3. [UrbanNavDataset](https://github.com/weisongwen/UrbanNavDataset)

   - Run the launch file:

     ```
     roslaunch lvi_sam UrbanNavDataset.launch
     ```

   - Play existing bag files, the params we provided is for [UrbanNav-HK-Data20200314](https://www.dropbox.com/s/3mtlncglrv7p39l/2020-03-14-16-45-35.bag.tar.gz?dl=0). If you use other bag files of UrbanNavDataset, please check if the params need to be changed.

     ```
     rosbag play 2020-03-14-16-45-35.bag 
     ```

   - Results on UrbanNav-HK-Data20200314:

     <p align='center'>    <img src="./doc/fig/urbannav.png" alt="drawing" width="600"/></p>

3. [KITTI raw dataset](https://www.cvlibs.net/datasets/kitti/raw_data.php)

   - Run the launch file:

     ```
     roslaunch lvi_sam KITTI.launch
     ```

   - Play existing bag files. Please note that you must use **KITTI raw dataset** rather than KITTI Odometry dataset, because the latter's IMU frequency is too low. If you want to use KITTI raw dataset for LVI-SAM, you need to get rosbag files firstly. You can get it refer to [LIO-SAM/config/doc/kitti2bag](https://github.com/TixiaoShan/LIO-SAM/tree/master/config/doc/kitti2bag). Here we use KITTI_2011_09_26_drive_0084_synced raw data to get rosbag file. The transformed rosbag file can get at [this link](https://1drv.ms/u/s!AqYajE_ft9lwg0tuhqyZqd4MUjqp?e=hnvkZo).

     ```
     rosbag play kitti_2011_09_26_drive_0084_synced.bag  
     ```

   - Results of our modified code on kitti_2011_09_26_drive_0084_synced.bag:

     <p align='center'>
         <img src="./doc/fig/kitti.png" alt="drawing" width="600"/>
     </p>

3. [My test dataset](https://1drv.ms/u/s!AqYajE_ft9lwg0paJQu_DRzU-GQ5?e=A95yfn)

   - Run the launch file:

     ```
     roslaunch lvi_sam backbag.launch
     ```

   - Play existing bag files, e.g. backbag.bag:

     ```
     rosbag play backbag.bag 
     ```

   - Results of our modified code on backbag.bag:

     <p align='center'>
         <img src="./doc/fig/backbag.png" alt="drawing" width="600"/>
     </p>

   - Results of our modified code on our own 0117-1525.bag (Device is different from backbag.bag, so it has another params. However, sorry for privacy issues, this data package can not open source):

     ```
     roslaunch lvi_sam ljj.launch
     rosbag play 0117-1525.bag 
     ```

     <p align='center'>    <img src="./doc/fig/ljj.gif" alt="drawing" width="600"/></p>
   
6. [KAIST Complex Urban Dataset](https://sites.google.com/view/complex-urban-dataset) 

   See TODO.

---



## TODO

  - [x] ~~More test on different dataset, e.g. [KAIST Complex Urban Dataset](https://sites.google.com/view/complex-urban-dataset). **However**, these datasets' lidar data have no **ring** information. So LVI-SAM can't run directly. If you want to run on these datasets, you need to modifidy the code to add this information refer to [LeGO-LOAM](https://github.com/RobustFieldAutonomyLab/LeGO-LOAM).~~

- We have test  [KAIST Complex Urban Dataset](https://sites.google.com/view/complex-urban-dataset) on "**new**" branch. We mainly made two changes:

  - We updated the latest version of LIO-SAM repo code into LVI-SAM, so the system is more robust and can run on KAIST Complex Urban Dataset successfully.
  - We generate rosbag from the origin KAIST Complex Urban Dataset, and recover the `ring`and `time` field of LiDAR pointcloud. You can using the ros package in [doc/kaise-help](./doc/kaist-help) to generate rosbags.

- Test on  KAIST Complex Urban Dataset urban26 sequence.

  - Run the launch file:

    ```
    roslaunch lvi_sam KAIST.launch
    ```

  - Play the generated bag files, e.g. urban26.bag:

    ```
    rosbag play urban26.bag 
    ```

  - Results of our modified code on urban26.bag:

    <p align='center'>    <img src="./doc/fig/kaist1.png" alt="drawing" width="600"/></p>

    <p align='center'>    <img src="./doc/fig/kaist2.png" alt="drawing" width="600"/></p>

    We can see that the trajectory has a large drift, and the loop closure doesn't be detected successfully. This may be due to the reason that the LiDAR of KAIST dataset is installed obliquely, resulting in too few valid pointclouds for registration.

---







## Acknowledgement

- Origin official [LVI-SAM](https://github.com/TixiaoShan/LVI-SAM).
