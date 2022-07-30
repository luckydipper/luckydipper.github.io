---
layout: post
title: "c c++ 컴파일러 정리"
categories:
  - etc computer
tags:
  - TCC 
  - GCC
  - Clang
  - msvc
  - llvm
  - C++
  - Make
  - Cmake
  - ninja
last_modified_at: 2022-07-30
comments: true
---
# 대표적인 c 컴파일러 종류

| 컴파일러 장단점 | pro | cons|
| GCC           | GNU linux kernel 만들 때 씀. 크로스 컴파일. 다른 코딩테스트에서도 code server 빼고는 모두 gcc씀 | tcc보단 큼. clang보단 뭉쳐 있는 구조 |
| TCC           | 컴파일 속도가 빠르다. 컴파일러 자체의 크기가 작다. | 실행 프로그램이 최적화가 덜된다. 결과 파일 크기가 크고 느림|
| msvc          | visual studio로 컴파일 해서, 디버깅까지 가능!| window만 지원.|
| Clang         | opencl opengl front end compiler, 모듈화. 에러 메세지 간결. mac에 기본 컴파일러이다.| x86컴파일에선 더 느리게 컴파일 된다. |
| llvm          | clang의 backend. 모든 언어 ISA가 같이 지원 되도록!||

Low-Level Virtual Machine

### llvm & clang
compiler는 frontend middleend backend로 나뉜다. <br>
front end compiler : frontend 언어를 IR$($intermediate representation$)$로 바꾼다. ex$)$ clang, rustc 러스트, ghc 하스켈<br>
llvm : IR을 프로세서에 맞는 Instruction Set Archtecture $($ cisc, rics, mips, x86, arm $)$로 만든다. <br>
![llvc_architecture](/assets/img/computer/etc computer/llvc_architecture.png )
```
// clang-9 download 방법
sudo apt install clang-9 --install-suggests
clang++9 file.cpp -o name
-E : preprocess한다. -C는 comment를 포함시켜서 preprocessing, 한다.
-c : linkable한 elf obj까지 만들어라.
-S : assembly까지 process한다.
-I -J: path ,관련
-target : cross compile option 가능하다. target arm이면 arm processor로 crosscompile한다.
-shared : shared lib만들기
ar r library.a lib_source.o : ar를 이용하여 static lib 만들기

```


# build system
Make CMAKE Ninja MSBuild. 등등 <br>
Ninja는 google이 빠르게 컴파일 하기 위해 만든 build system이다.<br>

# Link Description file


# Disassembler
objdump<br>
llvm-objdump로 가능. 

### elf를 linking 해보자.


### lib, dll을 만들어보자.


# 출처 
[gcc vs clang&llvm](https://www.incredibuild.com/blog/gcc-vs-clang-battle-of-the-behemoths) <br>


# 결론
1. 직접 써 봐야지 장단을 알 수 있겠다. 다음부터 컴파일 할 때에는 gcc와 clang 위주로 컴파일 해봐야 겠다.
2. 왜 컴파일러에 관한 공부를 하고 있는 것인가? 알고리즘, network, processor에 의한  overhead가 휠신 클 것이다.



