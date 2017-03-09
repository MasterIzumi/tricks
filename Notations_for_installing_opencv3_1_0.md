#Notations for installing OpenCV 3.1.0

##Installing Dependencies

```
[compiler] sudo apt-get install build-essential  
[required] sudo apt-get install cmake git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev  
[optional] sudo apt-get install python-dev python-numpy libtbb2 libtbb-dev libjpeg-dev libpng-dev libtiff-dev libjasper-dev libdc1394-22-dev

```

##Getting Source Code

```
cd ~/<my_working_directory>  
git clone https://github.com/Itseez/opencv.git  
git clone https://github.com/Itseez/opencv_contrib.git  

```

Then, checkout to version 3.1.0.

```
git checkout 3.1.0
```

Both opencv and opencv_contrib must **be synchronized**.

##Compile

Using the following command to build with modules from opencv_contrib and avoid some bugs.
```
cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local -D INSTALL_C_EXAMPLES=ON -D INSTALL_PYTHON_EXAMPLES=ON -D OPENCV_EXTRA_MODULES_PATH=~/Toolsets/opencv_contrib/modules -D WITH_LIBV4L=ON -D WITH_V4L=OFF -D BUILD_EXAMPLES=ON ..
```

##Epilogue
There are many errors when I was building OpenCV 3.1.0. It seems that all the errors are from opencv_contrib. You can search these errors in google, and nearly all of them are issued in github. Thus, you can lookup the issues and find the changes of the code in corresponding commits.



