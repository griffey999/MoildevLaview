MoildevLaview
============================================================================================================================

MoilDev and Laview description
---------------------------------------------------------------------------------------------------------------------------

MoilDev is a collection of functions support c++ developments for fisheye image applications, tested both on ubuntu 18.04 and Raspberry Pi( Raspbian Buster ), gcc/g++ and OpenCV are required in the development.Laview is a system-design platform and development environment for a visual programming language from National Instruments.LabVIEW is commonly used for data acquisition, instrument control, and industrial automation on a variety of operating systems (OSs), including Microsoft Windows, various versions of Unix, Linux, and macOS.

How to implemnt MoilDev's on Laview system is a big problem. This description will introduce some tricks to integrate these two systems together to apply MoilDev's solution with Laview on windows 10 .(Note: MoilDev apply some opencv functions , we also need to take care opencv version for 32-bit)

![](https://github.com/griffey999/MoildevLaview/blob/master/image/final.png)

-----------------------------------------------------------------------------------------------------------------------------
1. Development environment

1.1 For Laview , For general complicant for future developing , we choose 32-bit version because most device drivers are supported for 32-bit driver, 64-bit is few. 

1.2 Visual studio and opencv: In order ro develop with Laview 32-bit version, we need to install visual studio c++ package and opencv which could come out 32-bit (x86) DLL to link with Laview. After survey , we find that the visual studio could be 2017 and opencv is ver2.4.13.6 . 

-----------------------------------------------------------------------------------------------------------------------------
2. Visual studio c++ and opencv installation.

Please refer to below link . 
https://jason-chen-1992.weebly.com/home/install-visual-studio-2017-with-opencv-on-win10

-----------------------------------------------------------------------------------------------------------------------------
3. Visual studio DLL developing: 

To communicate between MoilDev and Laview is our target. Providing MoilDev's DLL to Laview a simple way to achieve this goal. But Laview is a graphic software package and is not easy to operate MoilDev's dll to implement our target. It means we need to develop a simple API (dll)which could achieve two conditions: 
1. The dll function need to meet Laview's library calling format.
2. The dll function need to complete all the panorama actions. 
So let's go . 
-----------------------------------------------------------------------------------------------------------------------------
dll.hpp
```
```
dll.cpp
```
```

-----------------------------------------------------------------------------------------------------------------------------
4. Laview setup to call the dll : 

The Laview dll application is as below (Connectivity->Libraries & Executables->Call Library Function Node) :
![](https://github.com/griffey999/MoildevLaview/blob/master/image/laview1.png)
The function etting is as below :
![](https://github.com/griffey999/MoildevLaview/blob/master/image/laview2.png)
The parameter setting is as below : 
![](https://github.com/griffey999/MoildevLaview/blob/master/image/laview3.png)
The parameter setting is as below :(Please make sure below setting is correct) 
![](https://github.com/griffey999/MoildevLaview/blob/master/image/laview4.png)
