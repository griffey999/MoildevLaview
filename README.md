# MoildevLaview
============================================================================================================================

MoilDev and Laview description

    MoilDev is a collection of functions support c++ developments for fisheye image applications, tested both on ubuntu 18.04 and Raspberry Pi( Raspbian Buster ), gcc/g++ and OpenCV are required in the development. 
    Laview is a system-design platform and development environment for a visual programming language from National Instruments.LabVIEW is commonly used for data acquisition, instrument control, and industrial automation on a variety of operating systems (OSs), including Microsoft Windows, various versions of Unix, Linux, and macOS.
    How to implemnt MoilDev's on Laview system is a big problem. This description will introduce some tricks to integrate these two systems together to apply MoilDev's solution with Laview on windows 10 .(Note: MoilDev apply some opencv functions , we also need to take care opencv version for 32-bit)

![](https://user-images.githubusercontent.com/3524867/73999970-65850480-49a1-11ea-9e0b-6b88d1d49fb7.jpg)
-----------------------------------------------------------------------------------------------------------------------------
1. Development environment
```
Laview : 
   For Laview , For general complicant for future developing , we choose 32-bit version because most device drivers are supported for 32-bit driver, 64-bit is few. 
```
```
Visual studio and opencv:
    In order ro develop with Laview 32-bit version, we need to install visual studio c++ package and opencv which could come out 32-bit (x86) DLL to link with Laview. After survey , we find that the visual studio could be 2015 and opencv is 2.6.14
```