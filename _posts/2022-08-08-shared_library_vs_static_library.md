---
layout: post
title: "동적 라이브러리 vs 정적 라이브러리"
categories:
  - etc computer
tags:
  - ELF
  - static library
  - shared library
  - dynamic link library
last_modified_at: 2022-08-20
comments: true
---
프로그램은 자신이 직접 짠 코드에 여러 라이브러리를 섞어서 만든다.<br>
섞는 방식은 두 가지가 있다.<br>
하나는 정적 라이브러리로 만드는 것이고, 다른 하나는 동적 라이브러리로 만드는 것이다.<br>
![shared_vs_static](/assets/img/computer/etc computer/shared_lib_vs_static_lib/static_shared_lib.png)
출처 : https://modoocode.com/321<br>
위의 그림이 두 라이브러리의 차이를 잘 보여준다. <br>
라이브러리가 다른 프로그램에서 많이 사용 된다면 동적 라이브러리로 만드는 것이 좋다.<br>
하지만 동적라이브러리를 사용하면, 처음 시작할 때 해당 라이브러리의 위치를 파악하는데 오래 걸린다.<br>
그래서 포토샵 같은 프로그램은 시작 할 때 오래 걸린다. <br> 

# 정적 라이브러리 $($static library$)$
링킹 할 때, 해당 라이브러리 전체를 실행 파일에 링킹한다.

### 만드는 방법
Makefile<br>
```
CC = clang++-9
flag = -c

add.o : add.cpp add.hpp
	$(CC) $(flag) add.cpp

mul.o : mul.cpp mul.hpp
	$(CC) $(flag) mul.cpp

sub.o : sub.cpp sub.hpp
	$(CC) $(flag) sub.cpp

div.o : div.cpp div.hpp
	$(CC) $(flag) div.cpp

arithmetic_operation.a : add.o mul.o sub.o div.o
	ar r arithmetic_operation.a add.o mul.o sub.o div.o
```
ar의 r flag는 replace existing or insert new files into the archive<br>

파일 구조
```
make_library/
├── Makefile
├── add.cpp
├── add.hpp
├── add.o
├── cube.o
├── div.cpp
├── div.hpp
├── div.o
├── mul.cpp
├── mul.hpp
├── mul.o
├── sub.cpp
├── sub.hpp
└── sub.o
결과 파일 arithmetic_operation.a
```

사칙 연산 파일 예시<br>
```
//add.hpp
int add(int a, int b);
```
```
//add.cpp
int add(int a, int b){
    return a + b;
}
```

라이브러리 살펴보는 방법 <br>
```
ar -tv make_library/arithmetic_operation.a

//결과
rw-r--r-- 0/0    960 Jan  1 00:00 1970 add.o
rw-r--r-- 0/0    960 Jan  1 00:00 1970 mul.o
rw-r--r-- 0/0    960 Jan  1 00:00 1970 sub.o
rw-r--r-- 0/0    960 Jan  1 00:00 1970 div.o
```

정적 라이브러리 링킹하여 사용하기<br>
```
make_library/
├── main.cpp
├── cube.cpp
├── cube.hpp
└── arithmetic_operation.a

// 실행 명령어
clang++-9 make_library/main.cpp make_library/cube.cpp make_library/arithmetic_operation.a -o main.o
// 혹은
clang++-9 make_library/cube.o make_library/main.o make_library/arithmetic_operation.a -o linking_main_cube_arithimetic.out
// 혹은
clang++ -o linking_main_cube_arithimetic.out main.o cube.o -Lpath/to/libarithmetic_operation.a -larithmetic_operation // 이때 확장자와 앞의 lib은 붙히지 않음.
```
<mark>library를 만들 때, 파일 이름은 lib{name}.a 또는 lib{name}.so로로 하면 좋다.<br>
왜냐면, clang과 gcc에서 -l flag로 library를 linking 할 때에는 lib가 앞에 적혀있어야 한다. lib가 앞에 붙은 so 파일은 링크 시간에 사용되는 library로 설계한다.<br>
만약 앞 부분에 lib가 없는 경우 runtime에 plug in 되는 library이다.</mark><br>
출처 : https://stackoverflow.com/questions/6561273/is-liblibrary-name-a-so-a-naming-convention-for-static-libraries-in-linux <br>

자세한 파일 내용<br>
```
// main.cpp
#include <cstdio>
int add(int a, int b);
int mul(int a, int b);
int sub(int a, int b);
int div(int a, int b);
int cube(int a);

int main()
{
    int example_num = 1;
    example_num = add(example_num,example_num); // 2 = 1 + 1
    example_num = mul(example_num,example_num); // 4 = 2 * 2
    example_num = sub(example_num, 1);          // 3 = 4 - 1
    example_num = div(example_num, 3);          // 1 = 3 / 3
    example_num = sub(example_num, 2);          // -1 = 1 -2 
    example_num = cube(example_num);            // -1 = -1 * -1 * -1
    printf("%i", example_num);
    //-1
    return example_num;
}
```
```
// cube.cpp
#include "cube.hpp"
int cube(int a)
{
    return a*a*a;
}
```

예제 코드 : https://github.com/luckydipper/c_cpp_compile_process/tree/main/make_library

# 동적 라이브러리 $($shared library$)$

### 만드는 방법
```
CC = clang++-9
flag = -c -fPIC -o  # postion independent code

add_.o : add.cpp add.hpp
	$(CC) $(flag) add_.o add.cpp 

mul_.o : mul.cpp mul.hpp
	$(CC) $(flag) mul_.o mul.cpp

sub_.o : sub.cpp sub.hpp
	$(CC) $(flag) sub_.o sub.cpp

div_.o : div.cpp div.hpp
	$(CC) $(flag) div_.o div.cpp

libarithmetic_operation.so : add_.o mul_.o sub_.o div_.o
	clang++-9 -shared -o libarithmetic_operation.so add_.o mul_.o sub_.o div_.o

# gcc -shared -W1,-soname,libmycalcso.so.1 -o libmycalcso.so.1.0.1 sum.o sub.o mul.o div.o
# ln -s libmycalcso.so.1.0.1 libmycalcso.so
# 이렇게 symbolic linking 해놓으면, 나중에 버전 관리 하기 쉽다.
```
실행
```
clang++-9 -o print_-1.out main.o cube.o -L./ -larithmetic_operatio
// print_-1.out이 만들어짐!

./print_-1.out
error while loading shared libraries: libarithmetic_operation.so: cannot open shared object file: No such file or directory
```
<mark> 
  호출할 라이브러리의 위치를 알아야 한다.<br>
  /etc/ld.so.conf 이 위치가 dll을 찾아보는 위치를 담고 있다. <br>
  혹은 환경변수 LD_LIBRARY_PATH를 활용하여 만들 수 있다. <br>
</mark>
```
export LD_LIBRARY_PATH=~/desktop/c_cpp_compile_process/make_library
```

# PE vs ELF

dll.

# Symbol

# 컴파일 과정 

# Link Description file

# Mapfile

# Disassembler
objdump<br>
llvm-objdump로 가능. <br>
file<br>
readelf<br>
file .o<br>

### elf를 linking 해보자.

### lib, dll을 만들어보자.

# 출처 
씹어 먹는 c++ <br>
embedded recipy <br>
https://www.joinc.co.kr/w/Site/C/Documents/CprogramingForLinuxEnv/Ch12_module<br>
