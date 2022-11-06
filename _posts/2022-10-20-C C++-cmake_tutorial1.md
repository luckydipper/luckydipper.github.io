---
layout: post
title: "cmake_tutorial1"
categories:
  - C C++
tags:
  - Cmake
last_modified_at: 2022-11-06
comments: true
---
## 1. Step1 Basic Starting Point

#### 1$)$ CMakeLists의 꼭 필요한 구조
```CMake
#CMakelists.txt
cmake_minimun_required(VERISION 3.1)
project(Name
        VERSION 1.0.0)
add_executable(Name code)
```
```shell
mkdir build
cd build
cmake ..
cmake --build . # or make
```
#### 2$)$ C++ standard
```CMake
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)
```

#### 3$)$ Add version number
CMakeLists.txt에서 만든 변수의 값을 실제 코드에서 쓰고 싶을 때
```
#CMakeLists.txt
config_file(Tutorialconfig.h.in Tutorialconfig.h)
```
```c
//Tutorialconfig.h.in
#define Tutorial_VERSION_MAJOR @Tutorial_VERSION_MAJOR@
#define Tutorial_VERSION_MINOR @Tutorial_VERSION_MINOR@
#define i_want_to_use_this_variable_in_source_code "@i_want_to_use_this_variable_in_source_code@"
```
를 만들고 실행시킬시, Tutorialconfig.h에 알맞은 값이 들어가서 사용할 수 있다.  
Tutorial_VERSION_MINOR와 Tutorial_VERSION_MAJOR은 project를 설정할 때 넣은 version값으로 내부에서 만들어진다.  


## 2. Adding Library
#### 1) 파일 구조
```
.
├── CMakeLists.txt
├── MathFunctions
│   ├── CMakeLists.txt
│   ├── MathFunctions.h
│   └── mysqrt.cxx
├── Step2_build
│   ├── CMakeCache.txt
│   ├── CMakeFiles
│   ├── Makefile
│   ├── MathFunctions
│   ├── Tutorial
│   ├── TutorialConfig.h
│   └── cmake_install.cmake
├── TutorialConfig.h.in
└── tutorial.cxx
```
#### 2) 라이브러리 만들기
```Cmake
add_library(Name STATIC code.cxx code2.cxx) 
```
라이브러리는 STATIC, SHARED, MODULE 타입을 가진다. STATIC는 정적 라이브러리, SHARED는 동적 라이브러리, MODULE은 실행중 올릴 수 있는 동적 라이브러리를 만든다.  

내부 라이브러리까지 만든다.
```
add_subdirectory(Library_directory_name)
```

#### 3) 라이브러리 링킹하기.
Build Specification
```
target_include_directories(program PUBLIC ${CMAKE_SOURCE_DIR}/includes)
target_include_directories(MathFunctions 
                           INTERFACE "${CMAKE_CURRENT_SOURCE_DIR}"
                           )
target_link_libraries()
```

target vs property  

#### 4) option!
선택적으로 컴파일 할 때 사용.  
가령 google test나 doxygen 같은 것 추가해서 돌릴 것인가?  
```CMake
option(USE_MYMATH "Use tutorial provided math implementation" ON)
```
option뒤의 변수가 ON과 OFF의 state가 있음. 
```
if(USE_MYMATH)
    add_subdirectory(MathFunctions)
    list(APPEND EXTRA_LIBS MathFunctions)
    list(APPEND EXTRA_INCLUDES "${PROJECT_SOURCE_DIR}/MathFunctions")
endif()
target_link_libraries(Tutorial PUBLIC ${EXTRA_LIBS}
target_include_directories(Tutorial PUBLIC
                            "${PROJECT_BINARY_DIR}"
                            ${EXTRA_INCLUDES})
```
이와 같이 작성하면, cmake 컴파일시, -D flag를 줘서 해당 파일을 링크 할지 설정 할 수 있음.  
##### ps. target_compile_options$($program PUBLIC -Wall -Werror$)$
위와 같이 컴파일 옵션을 줄 수 있음.   

## 3. Adding Usage Requirements for a Library

#### 1$)$ property. PRIVATE vs PUBLIC vs INTERFACE
PRIVATE  
타겟 A의 속성을 PRIVATE로 지정하면, 타겟 A에게만 적용된다.(Link Dependency)  
INTERFACE  
타겟 A의 속성을 INTERFACE로 지정하면, 타겟 A를 사용하는 제3의 타겟에게만 적용된다. (Link Interface)  
PUBLIC  
타겟 A의 속성을 PUBLIC으로 지정하면, 타겟 A와 타겟 A를 사용하는 제3의 타겟에게 적용된다.(Link Dependency and Link Interface)  
## 출처
[cmake documentation][1]
[moducode][2]
[cmake_property][3]

[1]:[https://cmake.org/cmake/help/latest/index.html]
[2]:[https://modoocode.com/332]
[3]:[https://medium.com/@yjo/cmake-%EB%B9%8C%EB%93%9C-%EC%8B%9C%EC%8A%A4%ED%85%9C-%EB%A7%8C%EB%93%A4%EA%B8%B0-9ec3e2d66cf0]