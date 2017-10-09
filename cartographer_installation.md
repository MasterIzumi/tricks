#Cartographer Installation

## Dependencies
sudo apt-get install -y google-mock libboost-all-dev  libeigen3-dev libgflags-dev libgoogle-glog-dev liblua5.2-dev libprotobuf-dev  libsuitesparse-dev libwebp-dev ninja-build protobuf-compiler python-sphinx  ros-indigo-tf2-eigen libatlas-base-dev libsuitesparse-dev liblapack-dev

## Ceres solver
git clone https://github.com/hitcm/ceres-solver-1.11.0.git  
cd ceres-solver-1.11.0/build  
cmake ..  
make –j  
sudo make install  


## Cartographer
git clone https://github.com/hitcm/cartographer.git  
cd cartographer/build  
cmake .. -G Ninja  
ninja  
ninja test  
sudo ninja install  

## cartographer_ros
cd ~/catkin_ws/src  
git clone https://github.com/hitcm/cartographer_ros.git  
cd ..  
catkin_make  

## Run!
roslaunch cartographer_ros demo_backpack_2d.launch bag_filename:=${HOME}/Downloads/cartographer_paper_deutsches_museum.bag  
roslaunch cartographer_ros demo_backpack_3d.launch bag_filename:=${HOME}/Downloads/cartographer_3d_deutsches_museum.bag  


### Reference
http://www.cnblogs.com/hitcm/p/5939507.html
