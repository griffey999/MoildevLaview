# MoildevLaview


## MoilDev and Laview description
---------------------------------------------------------------------------------------------------------------------------

MoilDev is a collection of functions support c++ developments for fisheye image applications, tested both on ubuntu 18.04 and Raspberry Pi( Raspbian Buster ), gcc/g++ and OpenCV are required in the development.Laview is a system-design platform and development environment for a visual programming language from National Instruments.LabVIEW is commonly used for data acquisition, instrument control, and industrial automation on a variety of operating systems (OSs), including Microsoft Windows, various versions of Unix, Linux, and macOS.

How to implemnt MoilDev's on Laview system is a big problem. This description will introduce some tricks to integrate these two systems together to apply MoilDev's solution with Laview on windows 10 .(Note: MoilDev apply some opencv functions , we also need to take care opencv version for 32-bit)

![](https://github.com/griffey999/MoildevLaview/blob/master/image/final.png)

-----------------------------------------------------------------------------------------------------------------------------
## 1. Development environment

1.1 Laview : For general compatible for future developing , we choose 32-bit version because most device drivers are supported for 32-bit, 64-bit is few. 

1.2 Visual studio and opencv: In order ro develop with Laview 32-bit version, we need to install visual studio c++ package and opencv which could come out 32-bit (x86) DLL to link with Laview. After survey , the visual studio could be 2017 and opencv is ver2.4.13.6 . 

-----------------------------------------------------------------------------------------------------------------------------
## 2. Visual studio c++ and opencv installation.

Please refer to below link . 
https://jason-chen-1992.weebly.com/home/install-visual-studio-2017-with-opencv-on-win10

-----------------------------------------------------------------------------------------------------------------------------
## 3. Visual studio DLL developing: 

We want to use Laview to pass a fisheye image to MoilDev and get a panoramr image back. Providing MoilDev's DLL to Laview seems a simple way to achieve this goal. But Laview is a graphic software package and is not easy to operate each functions of MoilDev's lib (like c++ enviroment to calll the functions) to reach our goal. It means we need to develop a simple API (dll) which could achieve two conditions: 
1. The dll function need to be simple and meet Laview's library calling format.
2. The dll function need to complete all the panorama actions of MoilDev. 

----------------------------------------------------------------------------------------------------------------------------
The code in dll.h
```
#pragma once
#include<iostream>
#ifdef DLL_IMPLEMENT
#define DLL_API _declspec(dllimport)
#else
#define DLL_API _declspec(dllexport)
#endif

extern "C" DLL_API  void trnToPano(int32_t rows, int32_t cols, int32_t *data);

#include <stdio.h>
#include <string.h>
#include <opencv2/opencv.hpp>
#include <sstream>
#include <string>
#include <time.h>
#include <iostream>
#include <fstream>
#include "moildev.h"

using namespace std;
using namespace cv;

namespace Ui {
	class Panorama;
}

class Panorama
{
public:
	Panorama();
	unsigned char * Show(int rows, int cols, unsigned char *data);
	~Panorama();
private:
	Moildev *md;
	Mat image_input, temp;
	Mat mapX[1], mapY[1];
	double m_ratio;
	void freeMemory();
};

```
The code in dll.cpp
```
#include "dll.h"
#include <opencv2/core/core.hpp>
#include <opencv2/highgui/highgui.hpp>

using namespace std;
using namespace cv;

Panorama::Panorama()
{
	md = new Moildev();
}

_declspec(dllexport) extern void trnToPano(int32_t rows , int32_t cols, int32_t *data) {
	Panorama *p;
	p = new Panorama();
	p->Show(rows,cols,(unsigned char *)data);
}

unsigned char * Panorama::Show(int rows, int cols, unsigned char *data) {
	md->Config("Pano", 1.14, 1.69,
		950.0, 744.0, 1.48,
		1920, 1440, 3.00,
		0, 0, 0, -15.92, 31.34, 140.48
	);
	clock_t tStart = clock();
	image_input = cv::Mat(rows, cols, CV_8UC3, &data[0]);
	temp = cv::Mat(rows, cols, CV_8UC3, &data[0]);
	double w = image_input.cols = cols;
	double h = image_input.rows = rows;
	double calibrationWidth = md->getImageWidth();
	mapX[0] = Mat(h, w, CV_32F);
	mapY[0] = Mat(h, w, CV_32F);
	m_ratio = w / calibrationWidth;
	md->PanoramaM((float *)mapX[0].data, (float *)mapY[0].data, mapX[0].cols, mapX[0].rows, m_ratio, 90);   
	remap(temp , image_input , mapX[0], mapY[0], INTER_CUBIC, BORDER_CONSTANT, Scalar(0, 0, 0));
	double time_clock = (double)(clock() - tStart) / CLOCKS_PER_SEC;
	cout << "time: " << time_clock << endl;
}
```

---------------------------------------------------------------------------------------------------------------------------
## 4. Laview setup to call the dll : 

The Laview dll application is as below (Connectivity->Libraries & Executables->Call Library Function Node) :
![](https://github.com/griffey999/MoildevLaview/blob/master/image/laview1.png)
The function setting is as below :
![](https://github.com/griffey999/MoildevLaview/blob/master/image/laview2.png)
The parameter setting is as below : 
![](https://github.com/griffey999/MoildevLaview/blob/master/image/laview3.png)
The parameter setting is as below :(Please make sure below setting is correct) 
![](https://github.com/griffey999/MoildevLaview/blob/master/image/laview4.png)

---------------------------------------------------------------------------------------------------------------------------
## 5. For more information about MoilDev , please refer below link : 
https://github.com/griffey999/test0219
