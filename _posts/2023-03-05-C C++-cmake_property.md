---
layout: post
title: "cmake Interface vs Private vs Public"
categories:
  - C C++
tags:
  - Cmake
last_modified_at: 2023-03-05
comments: true
---
본 포스팅에서는 
target 에 줄 수 있는 property에 대해 정리한다.

default는 public이다.  

### target_link_library
PUBLIC : 헤더파일과 내부 구현에서 모두 사용
PRIVATE : 내부 구현에만 사용. header file은 사용하지 않는다.  
INTERFACE : 헤더만 사용 내부 구현에선 사용하지 않는다.  
내부 구현에만 사용, header file에는 사용하지 않음.  
예를 들어  
```
// shape.cc
#include <iostream>
#include <thread>

#include "shape.h"

Rectangle::Rectangle(int width, int height) : width_(width), height_(height) {}

int Rectangle::GetSize() const {
  std::thread t([this]() { std::cout << "Calulate .." << std::endl; });
  t.join();

  // 직사각형의 넓이를 리턴한다.
  return width_ * height_;
}

```

```
add_library(shape STATIC shape.cc)
target_link_libraries(shape PRIVATE pthread)
```
라면 shape.h에는 사용하지 않고 shape.cc에서만 해당 library를 사용하기 때문에 private으로 하는 것이 좋음.

### target_include_directory
PUBLIC : include 되는 경로가 상속된다.
PRIVATE : 현재 타깃만 include 경로를 사용한다.  
INTERFACE : 다른 타깃만 해당 include 경로를 사용한다.

### target_compile_option
Public : target과 해당 target을 link하는 target에게 모두 적용된다.  
PRIVATE : 현재 target에게만 적용된다.  
INTERFACE : 아래와 같이 target을 link하는 target에게 컴파일 옵션을 줄 수 있다.  
```
target_compile_features(tutorial_compiler_flags INTERFACE cxx_std_11)
target_link_libraries(Tutorial PUBLIC ${EXTRA_LIBS} tutorial_compiler_flags)
```


### Reference
[blog](https://leimao.github.io/blog/CMake-Public-Private-Interface/)
[씹어먹는c++](https://modoocode.com/332)