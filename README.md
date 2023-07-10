# mapping

For more information information refer to 
https://github.com/introlab/rtabmap_ros/
OR
http://wiki.ros.org/rtabmap_ros

Note: rtabmap-ros must be installed!!
You can install it with:
```
sudo apt install ros-$ROS_DISTRO-rtabmap-ros
```

if you have the error error while loading shared libraries..., try ldconfig or add the next line at the end of your ~/.bashrc to fix it:
```
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/opt/ros/noetic/lib/x86_64-linux-gnu
```

And if that doesn't work you can directly clone the rtabmap_ros repo (recommended) in your workspace using:
```
git clone https://github.com/introlab/rtabmap_ros/ 
```

To get started with mapping and localization on zed2i camera, clone the repo with:
```
git clone https://github.com/assemblygod/mapping/
```

To run this use:
```
rosrun mapping combined.launch 
```

If it doesn't work lemme know I'll fix it and in the meantime:
open a new terminal and use this command:
```
rosrun mapping zed_no_tf.launch
```
and
in a different terminal run
```
rosrun mapping ll.launch 
```

Note: If you are changing some parameteres while using this then launch the files separately instead of launching it with combined.launch

Both rviz and rtabrviz will launch with this command and to turn it off rtabrviz use:
```
rosrun mapping ll.launch rtabrviz:=false
```
and to turn off rviz use:
```
rosrun mapping ll.launch rviz:=false
```

If you want to change parameter of the data publushed by zed, You can use:
```
roslaunch zed_wrapper zed_no_tf.launch
rosrun dynamic_reconfigure dynparam set zed_node depth_confidence 99
rosrun dynamic_reconfigure dynparam set zed_node depth_texture_conf 90
rosrun dynamic_reconfigure dynparam set zed_node depth_confidence 100
```

This is an example to change depth_confidence and depth_texture_conf, you can change other parameters also in a similar fashion.


In case that you are getting an error while launch ll.launch file separately saying no data received on such and such topics for more than 5 topics 
then make sure the zed_no_tf.launch file is launched, if the error is still there then you can change the topics in the launch file OR you can use the command:

```
roslaunch mapping ll.launch \
    rtabmap_args:="--delete_db_on_start" \
    rgb_topic:=/zed/rgb/image_rect_color \
    depth_topic:=/zed/depth/depth_registered \
    camera_info_topic:=/zed/rgb/camera_info \
    frame_id:=base_link \
    approx_sync:=false \
    stereo:=true \
    wait_imu_to_init:=true \
    imu_topic:=/zed_node/imu/data
    #if it doesn't work then try changing zed to zed_node or zed/zed_node everywhere in the comman
```
but with this command you might have to do this everytime you run it!!

remove the "--delete_db_on_start" and add localization:=true for running it in the localization mode, by this you will be using default map that is stored in ~/.ros/rtabmap.db

To use this in localization mode use:
```
   roslaunch mapping ll.launch \
   localization:=true \
   stereo_namespace:=/zed \
   rgb_topic:=/zed/rgb/image_rect_color \
   depth_topic:=/zed/depth/depth_registered \
   camera_info_topic:=/zed/rgb/camera_info 
   stereo:=true \
   frame_id:=base_link \
   imu_topic:=/zed/imu/data \
   wait_imu_to_init:=true
```

If you have any problem or query related to this, kindly contact me!!
