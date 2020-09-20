# Livox ROS Driver For Windows
This package allow you to connect to the livox-mid-40 with ROS Melodic on Windows like you would with the [ubuntu version of this package](https://github.com/Livox-SDK/livox_ros_driver).

**Disclaimer this package have only been tested on ros melodic isntalled on windows 10 and the livox-mid-40. The livox hub have not been tested.**

There are a lot more steps to install everything for Windows than ubuntu. But if you want to make the livox laser scanner work on windows this is a package that could help you.

Also inorder for me to make this package to work on Windows, I had to remove the GPS sync from the package since the USB com ports libaries are very different from windows to linux.
So if anybody wants to contiue on making that work on windows, be welcome to fix that problem in this windows version of the Livox_ros_driver. I will not contiue working on this package since i got the ubuntu version to work on my windows computer for my porpose i need the driver for.

Sorry for the bad english aswell as the bad formatted readme.

## 1. Install dependencies

Before running livox_ros_driver, ROS, Livox-SDK and PCL must be installed.

### 1.1 ROS installation

For ROS installation, please refer to the ROS installation for Windows guide :

[ROS installation guide](http://wiki.ros.org/Installation/Windows)

&ensp;&ensp;&ensp;&ensp;***Note :***

&ensp;&ensp;&ensp;&ensp;(1) Be sure to install the full version of ROS (ros-melodic-desktop-full);

&ensp;&ensp;&ensp;&ensp;(2) If the ROS installtion is giving a lot of errors, try updating your powershell to a newer version;

&ensp;&ensp;&ensp;&ensp;(3) There are 7 to 8 steps in ROS installation, please read the installation guide in detail;

From now on **Use only the ROS terminal** like shown in the ROS Windows isntallation. This is important since we need to install the next comple of things so ROS can find the packages. 

### 1.1 PCL and VTK installation
Usually VTK is install with ROS on ubuntu. but thats not the case on Windows so we need to install it.

I found the best way of installing the VTK with the vcpkg package manager. which can be install from [here](https://github.com/microsoft/vcpkg).

The VTK package will be install alongside PCL by follow the guide on [pcl website](https://pointclouds.org/downloads/) on how to install it for windows.

**Remember to isntall the PCL with 64 bit support:** 

　
　```.\vcpkg install pcl --triplet x64-windows```


This part might take a while since VTK is a quit a big package. Once it is isntall we can contiue with isntall the Livox SDK


### 1.2 Livox-SDK Installation
Install the Livox-SDK into your src folder in your catkin workspace is the best, like shown in the figure below:

```
src
    └── livox_ros_driver
    └── Livox-SDK
```

**Make sure that you install the Livox-SDK with 64-bit support**

Download or clone [Livox-SDK](https://github.com/Livox-SDK/Livox-SDK) from Github to local;

Building the Livox-SDK on windows can be a bit tricky so here is a short guide to how i did it:

```
cd Livox-SDK
.\third_party\apr\apr_build.bat amd64
cd build
cmake .. -G "Visual Studio 16 2019" -A x64
```

If you are using an older version of Visual Studio use instead:

```
cmake .. -G "Visual Studio 15 2017 Win64"
```

Once this step is done there should be a visual studio solution which you can open inside the **Livox-SDK/build folder**.
Inside the visual studio solution put the build to Release and build ALL.

Once this step is done you should have built the Livox SDK.
Now we just have to link the SDK to the ros package.

## 2. Get and build livox_ros_driver


Get livox_ros_driver from GitHub :

```
cd to_your_catkin_worksapce_folder/src
git clone https://github.com/runeg96/livox_ros_driver.git
cd to_your_catkin_worksapce_folder/
catkin_make or catkin build 
```
If the Livox-SDK is not install the correct place in the src folder some of the paths might be wrong and needs to be changed to link the SDK libaries with the ros package.

In this case the CMakeLists.txt inside the livox_ros_driver folder needs to be chaned to where you isntall the SDK. like shown below:

```
gedit src/livox_ros_driver/CMakeLists.txt

# find and change it to the path of where you install the Livox-SDK:

# set(APR_INCLUDE_DIRS ${CMAKE_CURRENT_SOURCE_DIR}/../Livox-SDK/third_party/apr/include)
# set(APR_LIBRARIES ${CMAKE_CURRENT_SOURCE_DIR}/../Livox-SDK/third_party/apr/lib/libapr-1.lib)
# set(APR_FOUND "YES")

# also find and change it to the path of where you install the Livox-SDK:

# find_library(LIVOX_SDK_LIBRARY livox_sdk_static.lib ${CMAKE_CURRENT_SOURCE_DIR}/../Livox-SDK/build/sdk_core/Release)
```
Once everything is build with catkin we need to source the workspace with. Use the following command to update the current ROS package environment :

`source /devel/setup.sh`


## 3. Run livox_ros_driver

Now that every thing is built you can now use the livox_ros_driver like it was built on ubuntu or other linux distributuion. 

I dont want to show you how to use the package.
If you need help with using the package consult the real livox_ros_driver [page](https://github.com/Livox-SDK/livox_ros_driver).