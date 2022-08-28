---
layout: post
title: "CMake로 Opencv project 만들기!"
categories:
  - Computer Vision
tags:
  - Opencv
  - Cmake
  - C++
last_modified_at: 2022-08-28
comments: true
---

개발 환경 : ubuntu20.04 <br>
소스 코드 : https://github.com/luckydipper/c_cpp_compile_process/tree/main/opencv_cmake_project <br>

# 목차

[1.어떻게 다운 받는가?](#어떻게-다운-받는가?) <br>
  * [1.1 필수적으로 다운 받아야 하는 파일](#11-필수적으로-다운-받아야-하는-파일)<br>
  * [1.2 선택적으로 받아야 하는 파일](#12-선택적으로-받아야-하는-파일)<br>
  * [1.3 프로젝트 directory 만들기](#13-프로젝트-directory-만들기)<br>
  * [1.4 Build 하기](#14-Build-하기)<br>
  * [1.5 다운로드 완료 확인](#15-다운로드-완료-확인)<br>

[2. 2. 예제](#2-예제) <br>
  * [2.1 파일 구조](#21-파일-구조)<br>
  * [2.2 코드](#22-코드)<br>
  * [2.3 CMakeLists](#23-CMakeLists)<br>
  * [2.4 Build](#24-Build)<br>
  * [2.5 Execute](#25-Execute)<br>

# 1. 어떻게 다운 받는가?

### 1.1 필수적으로 다운 받아야 하는 파일
```
sudo apt-get install build-essential
sudo apt-get install cmake git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev
```

### 1.2 선택적으로 받아야 하는 파일
```
sudo apt-get install python-dev python-numpy libtbb2 libtbb-dev libjpeg-dev libpng-dev libtiff-dev libjasper-dev libdc1394-22-dev
```

### 1.3 프로젝트 directory 만들기
```
git clone https://github.com/opencv/opencv.git

cd ~/opencv
mkdir build
cd build

cmake -D CMAKE_BUILD_TYPE=Release -D CMAKE_INSTALL_PREFIX=/usr/local .. 
```
cmake command 뒤에 ..도 빠트리면 안 된다.<br>

실행 후 파일 구조
```
$tree opencv -L 1
opencv
├── 3rdparty
├── CMakeLists.txt
├── CONTRIBUTING.md
├── COPYRIGHT
├── LICENSE
├── README.md
├── SECURITY.md
├── apps
├── build
├── cmake
├── data
├── doc
├── include
├── modules
├── platforms
└── samples
```

### 1.4 Build 하기
오래 걸리기 때문에 -j를 줘서 뒤의 숫자 만큼 병렬처리 한다. <br>
```
cd build
make -j7
```
build 한다. <br>
```
sudo make install
```

### 1.5 다운로드 완료 확인
/usr/local/include/opencv4/opencv2 <br>
에는 opencv들의 header들이 정의되어 있을 것이다. <br>

파일 구조는 아래와 같아 진다.
```
$tree opencv/build -L 1
opencv/build
├── 3rdparty
├── CMakeCache.txt
├── CMakeDownloadLog.txt
├── CMakeFiles
├── CMakeVars.txt
├── CPackConfig.cmake
├── CPackSourceConfig.cmake
├── CTestTestfile.cmake
├── Makefile
├── OpenCVConfig-version.cmake
├── OpenCVConfig.cmake
├── OpenCVModules.cmake
├── apps
├── bin
├── cmake_install.cmake
├── cmake_uninstall.cmake
├── configured
├── custom_hal.hpp
├── cv_cpu_config.h
├── cvconfig.h
├── data
├── doc
├── include
├── install_manifest.txt
├── lib
├── modules
├── opencv2
├── opencv_data_config.hpp
├── opencv_python_config.cmake
├── opencv_python_tests.cfg
├── opencv_tests_config.hpp
├── python_loader
├── setup_vars.sh
├── test-reports
├── tmp
├── unix-install
└── version_string.tmp
```

# 2. 예제
고양이 사진을 흑백으로 만들어 보자. <br>

|Before$($cat.jpg$)$|After$($gray_cat.jpg$)$|
|![image](/assets/img/computer/vision/opencv_cmake_tutorial/cat.jpg)|![image](/assets/img/computer/vision/opencv_cmake_tutorial/gray_cat.jpg)|

### 2.1 파일 구조
```
convert_cat/
├── CMakeLists.txt
├── cat.jpg
└── convert_color.cpp
```


### 2.2 코드
```
// convert_color.cpp
#include <opencv2/opencv.hpp>
#include <cstdio>

using namespace cv;

int main( int argc, char** argv)
{
    char* imageName = argv[1];
    Mat image = imread( imageName, IMREAD_COLOR);
    
    if( argc != 2 || !image.data)
    {
        printf("No image data\n");
        return -1;
    }

    Mat gray_image;
    cvtColor( image, gray_image, COLOR_BGR2GRAY);

    imwrite("./gray_cat.jpg", gray_image);

    return 0;
}
```


### 2.3 CMakeLists
```
cmake_minimum_required(VERSION 2.8)
project( convert_color )
find_package( OpenCV REQUIRED ) # 환경 변수 들을 찾아서등록
include_directories( ${OpenCV_INCLUDE_DIRS} )
add_executable( convert_color convert_color.cpp )
target_link_libraries( convert_color ${OpenCV_LIBS} )
```

### 2.4 Build
```
cd convert_cat
cmake .
```

이후 파일 구조
```
convert_cat/
├── CMakeCache.txt
├── CMakeFiles
│   ├── 3.16.3
│   │   ├── CMakeCCompiler.cmake
│   │   ├── CMakeCXXCompiler.cmake
│   │   ├── CMakeDetermineCompilerABI_C.bin
│   │   ├── CMakeDetermineCompilerABI_CXX.bin
│   │   ├── CMakeSystem.cmake
│   │   ├── CompilerIdC
│   │   │   ├── CMakeCCompilerId.c
│   │   │   ├── a.out
│   │   │   └── tmp
│   │   └── CompilerIdCXX
│   │       ├── CMakeCXXCompilerId.cpp
│   │       ├── a.out
│   │       └── tmp
│   ├── CMakeDirectoryInformation.cmake
│   ├── CMakeOutput.log
│   ├── CMakeTmp
│   ├── Makefile.cmake
│   ├── Makefile2
│   ├── TargetDirectories.txt
│   ├── cmake.check_cache
│   ├── convert_color.dir
│   │   ├── DependInfo.cmake
│   │   ├── build.make
│   │   ├── cmake_clean.cmake
│   │   ├── depend.make
│   │   ├── flags.make
│   │   ├── link.txt
│   │   └── progress.make
│   └── progress.marks
├── CMakeLists.txt
├── Makefile
├── cat.jpg
├── cmake_install.cmake
├── convert_color.cpp
└── gray_cat.jpg
```
make로 빌드. convert_cat directory에서
```
make
```
-> convert_color 실행 파일이 나옴.<br>

### 2.5 Execute
```
./convert_color cat.jpg
```
-> gray_cat.jpg 생성

# 출처
https://docs.opencv.org/4.0.0/ <br>
https://codingcat.codes/about/
