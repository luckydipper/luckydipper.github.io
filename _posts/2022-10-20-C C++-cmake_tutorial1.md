---
layout: post
title: "cmake_tutorial"
categories:
  - C C++
tags:
  - Cmake
last_modified_at: 2023-02-05
comments: true
---
## Step1. Basic Starting Point

#### 1$)$ CMakeLists의 꼭 필요한 구조
```CMake
#CMakelists.txt
cmake_minimun_required(VERISION 3.1)
project(Name
        VERSION 1.0.0)
add_executable(Name code)
```
#### 1.1$)$ build 하는 법
```shell
mkdir build
cd build
cmake ..
cmake --build . # or make
```
make로 빌드 할 수 있지만, --build flag를 주는 것이 더 일반적으로 사용된다. nija같은 컴파일러를 사용할 수도 있기 때문이다.  
#### 2$)$ C++ standard
```CMake
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)
```

#### 3$)$ Add version number
CMakeLists.txt에서 만든 변수의 값을 실제 코드에서 쓰고 싶을 때 가령 소스코드에서 프로젝트의 버전을 출력 하려고 한다. 
```
#CMakeLists.txt
#Tutorialconfig.h.in 파일을 이용해 Tutorialconfig.h를 만들려고 한다.
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


## Step2. Adding Library
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
```
# MathFunctions/CMakeLists.txt 라이브러리 안의 cmake에 넣는다.
add_library(Name STATIC code.cxx code2.cxx) 
```
라이브러리는 STATIC, SHARED, MODULE 타입을 가진다. STATIC는 정적 라이브러리, SHARED는 동적 라이브러리, MODULE은 실행중 올릴 수 있는 동적 라이브러리를 만든다.  

```
# CMakeLists.txt linking하는 라이브러리에 등록한다.
add_subdirectory(MathFunctions)
add_executable(Tutorial tutorial.cxx)
```

#### 3) 라이브러리 링킹하기.

``` 

add_executable(Tutorial tutorial.cxx)
# add_executable 뒤에서 라이브러리들의 executable한 target에 정적으로 링킹한다.
target_link_libraries(Tutorial PUBLIC MathFunctions)
# library의 헤더 파일이 어디 있는지 알려준다.
# ex) target_include_directories(program PUBLIC ${CMAKE_SOURCE_DIR}/includes)
 
target_include_directories(Tutorial PUBLIC
                          "${PROJECT_BINARY_DIR}"
                          "${PROJECT_SOURCE_DIR}/MathFunctions"
                          )

```

#### 4) option!
선택적으로 컴파일 할 때 사용.  
가령 google test나 doxygen 같은 것 추가해서 돌릴 것인가?  
```
#add_subdirectory 앞에 해준다.
option(USE_MYMATH "Use tutorial provided math implementation" ON)
add_subdirectory(MathFunctions)
```

option뒤의 변수가 ON과 OFF의 state가 있음.  
```
option(USE_MYMATH "Use tutorial provided math 
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
위와 list로 target을 만들면,```target_link_libraries(Tutorial PUBLIC ${EXTRA_LIBS})```으로 라이브러리를 링킹 할 수 있다. 헤더도  
```
target_include_directories(Tutorial PUBLIC
                           "${PROJECT_BINARY_DIR}"
                           ${EXTRA_INCLUDES}
                           )
```  
로 가능하다.  
cmake 컴파일시, -D flag를 줘서 해당 파일을 링크 할지 설정 할 수 있음. 

## Step3. Adding Usage Requirements for a Library
step2보다 최신 방법으로 library를 링킹하는 법을 소개한다.  
interface이다.  
MathFunctions에 링킹되는 것들은 mathfunction의 현재 주소에서 헤더를 읽어야한다.  
```
#MathFunctions/CMakeLists.txt
add_library(MathFunctions mysqrt.cxx)

target_include_directories(MathFunctions
    INTERFACE ${CMAKE_CURRENT_SOURCE_DIR})
```
interface는 사용자는 필요하지만 제공자는 필요하지 않는 것.  
타겟 A의 속성을 INTERFACE로 지정하면, 타겟 A를 사용하는 제3의 타겟에게만 적용된다.  

## Step 4: Adding Generator Expressions
generator expression일반화 표현식은 빌드과정에서 미세한 조정을 할 때 쓰인다.  
target의 property로 많이 쓰인다. configuration이 아닌 generating step에서 사용된다.  
```$<...>``` 구문으로 사용한다.   
c++표준이나 컴파일 플래그를 줘서, 조건적으로 링킹할 때 사용.
ex$)$ c++ 버전을 고정 시킬 때,
```
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)
```
대신 
```
#CMakeLists.txt
add_library(tutorial_compiler_flags INTERFACE)
target_compile_features(tutorial_compiler_flags INTERFACE cxx_std_11)
target_link_libraries(Tutorial PUBLIC ${EXTRA_LIBS} tutorial_compiler_flags)
```
```
#MathFunctions/CMakeLists.txt
target_link_libraries(MathFunctions tutorial_compiler_flags)
```
를 사용한다.  
ex$)$ warning flag를 줄 때  
 cmake 3.15 이후 버전에서 적용된다.  

## Step 4: Installing and Testing
install은 빌드와 다르게 컴퓨터의 환경변수에 모두 등록된다.  
아래 명령어로 install 할 수 있다.  
```
//sudo가 필요하다. 
cmake --build . --target install --config Debug
cmake --install .
cmake --install . --prefix "/home/myuser/installdir"
```
```
# MathFunctions/CMakeLists.txt
set(installable_libs MathFunctions tutorial_compiler_flags)
install(TARGETS ${installable_libs} DESTINATION lib)
install(FILES MathFunctions.h DESTINATION include)
```
```
install(TARGETS Tutorial DESTINATION bin)
install(FILES "${PROJECT_BINARY_DIR}/TutorialConfig.h"
  DESTINATION include
  )
```
```
@instance-1:~/Desktop/cmake-3.26.0-rc1-tutorial-source/Step5/build$ sudo cmake --install .
-- Install configuration: ""
-- Installing: /usr/local/lib/libMathFunctions.a
-- Installing: /usr/local/include/MathFunctions.h
-- Installing: /usr/local/bin/Tutorial
-- Installing: /usr/local/include/TutorialConfig.h
```

gcc의 기본 include 경로를 살펴보면, /usr/local/include가 있다.
```
echo | gcc -v -x c++ -E -
```
이제 터미널에
```
Tutorial이라고 치면 컴파일된 Tutorial
```

#### TODO : 씹어먹는 c++  참조
target vs property  

##### ps. target_compile_options$($program PUBLIC -Wall -Werror$)$
위와 같이 컴파일 옵션을 줄 수 있음.   

링크 하는 것 뿐 아니라, 자신이 만든 라이브러리를 쓸지 말지 파일들도 고쳐야한다.
```
#tutorial.cxx
#ifdef USE_MYMATH
#  include "MathFunctions.h"
#endif
#ifdef USE_MYMATH
  const double outputValue = mysqrt(inputValue);
#else
  const double outputValue = sqrt(inputValue);
#endif
```
TutorialConfig.h.in는 option 뒤에 와야 한다.  
왜냐면 TutorialConfig.h.in에서 USE_MYMATH값을 쓰기 때문이다.  
```
#TutorialConfig.h.in
#cmakedefine USE_MYMATH
```

## 3. Adding Usage Requirements for a Library

#### 1$)$ property. PRIVATE vs PUBLIC vs INTERFACE
PIRVATE  
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