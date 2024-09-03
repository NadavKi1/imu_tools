# imu_tool (kinetic version) upgrade



## Main goal

Migrate updates from **noetic** version of imu_tool to **kinetic** version 

### Progect location

The kinetic version is located locally at:

```bash
/home/nadavki/Desktop/imu_project/imutool_k_ws/src/imu_tools_kinetic
```

The updated noetic version is located at:

```bash
/home/nadavki/imutool_ws/src/imu_tools
```

/home/nadavki/Desktop/imu_project/imu_tools

### Main inital functionallity

The initial focus should be on the `imu_filter_madgwick` part of the code.

### Workaround

Note that initally compiling the kinetic version of the package with docker resulted in error regarding tf2_geometry_msgs. Therefor we made a new docker image that has this geometry package isntalled with apt install. This new docker image is called `magicvoxl:withGeometry2`, and the original image is called `magicvoxl:v2.11`. 

May want to talk to Amit about this when possible.

**<u>note:</u>** the repositories are local. Try moving to remote (for real backup).



### Compiling the kinetic version of imu_tool using docker:

Running the container from the image with "ros-kinetic-tf2-geometry-msgs" installed with apt-get:

```bash
cd /home/nadavki/Desktop/imu_project
run -it --rm -v /home/nadavki/Desktop/imu_project/imutool_k_ws:/home/root/imutool_k_ws magicvoxl:withGeometry2 bash
```

**<u>note:</u>** -v flag means shared volume. This means that any changes made locally at `/home/nadavki/Desktop/imu_project/imutool_k_ws` will also affect the internal volume inside the docker container.

**<u>note:</u>** the --rm flag means that when exeting the docker conatiner, the container will be removed.



Originial repo: ```https://github.com/CCNYRoboticsLab/imu_tools'```

To compile the original version we must add the upcoming CATKIN_IGNORE's: 

```bash	
cd home/root/imutool_k_ws
touch src/imu_tools_kinetic/imu_tools/CATKIN_IGNORE
touch src/imu_tools_kinetic/imu_complementary_filter/CATKIN_IGNORE
touch src/imu_tools_kinetic/rviz_imu_plugin/CATKIN_IGNORE
```

inside the container:

```bash
cd /home/root/imutool_k_ws
catkin_make
```



## Compiling the noetic verison (without docker):



```bash
cd /home/nadavki/imutool_ws
catkin_make
```





## Conclusion

After the file updates, follow and preform the upcoming commands:
Inside the docker, run these commands:

```
cd /home/root/imutool_k_ws/
chmod +x /home/root/imutool_k_ws/src/imu_tools_kinetic/imu_filter_madgwick/cfg/ImuFilterMadgwick.cfg
```

In the CMakeLists.txt, Add these lines after ```project(imu_filter_madgwick)```:

```bas
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)
```



