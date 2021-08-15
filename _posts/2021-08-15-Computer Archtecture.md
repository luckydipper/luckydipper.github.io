---
layout: post
title: "컴퓨터구조"
categories:
  - Computer Archtecture
tags:
  - window system programing 
  - cpu
  - instruction
  - cache
last_modified_at: 2021-08-15
comments: true
---

## 윈도우 시스템 프로그래밍 책 review
#### 컴퓨터 구조 <br>
폰 노이만 구조 : 프로그램이 컴퓨터 내부에 저장되어 순차적으로 실행되는 구조 <br>
![archtecture](/assets/img/computer/archtecture/archtecture.jpg)
1. fetch$($Load$)$
2. Decode, Control unit이 어떤 명령어 인지 해석
3. execution. ALU에서 실제 실행

#### c언어 실행 방법 <br>
preprocessing -> compiler -> Assembly -> Linking<br>
preprocessing 과정에서 실행할 코드들을 가지고 옴. <br>
compile 과정에서 parcing, semantic analysis, Optimizing이 일어남 <br>
Assembly 부터는 cpu의 구조에 따라 나뉨 AT&T와 Intel의 assembly가 있음 <br>
linking 과정에서는 assembly를 가지고 instrucion이 됨.$($0과 1로 64bit, 32bit로$)$<br>
이런 명령어도 cpu종류에 따라 x86 명령어와 ARM명령어 등으로 나뉨. <br>
ex$)$ ADD r1 r2 7, r1에 r2 의 값과 7의 값을 더해라. //SUB DIV ocode <br>
ex$)$ LOAD r3 $[$0x11$]$ // 0x11에 저장된 주소로 가서 값을 읽어 r3 register에 실어라.<br>
ex$)$ STORE r2 0x08 // r2의 값을 0x08번지에 넣어라<br>
ex$)$ PUSH POP <br>

DIRECT vs INDIRECT<br>
$[$0x22 15 33$]$<br>
![assembly](/assets/img/computer/archtecture/assembly.jpg)
<br>

#### CPU눈 구조에 따라 RISC와 CISC으로 나뉨
RISC에는 ARM과 MIPS 등이 있고, CISC에는 INTEL과 AMD 등의 회사가 존재.<br>

#### 함수 호출 방법
<mark>IR, PC, SP, FP </mark><br>
<mark>호출과 반환 과정은 스택 관리 방법과 실행 위치 관리방법으로 구분됨.ㅌ</mark><br>
함수가 호출 됐을 때, 호출 정보를 저장하는 memory block이다. stack에 저장되 stack frame이라고 함.<br>
stack pointer register : stack에 쌓아올릴 다음 주소값이 저장되어있다. 함수 호출시에 점프되어 있다.<br>
frame pointer register : 함수가 호출 될 때, jump하기전 sp의 주소값을 복사한다. 이 값을 stack frame의 함수와 함수 사이 경계 부분에 저장한다.<br>
RAM : 함수가 호출 될 때, sp에 저장된 주소를 fp에 대입한후, sp는 jump. fp는 원래 sp에 저장된 주소값 ram에 한번더 저장한다.<br> 
return 할 때는 fp의 값을 sp에 넣는다.<br>
code 영역에는 코드를 compile한 명령어가 저장된다.<br>
Program Counter register <br>
이 명령어를 순차적으로 fetch 하기 위해, 다음에 실행될 명령어의 주소는 PC register에 저장된다.<br>
32bit computer에서는 명령어가 실행될 때마다 pc에 저장된 주소값이 4씩 증가된다.<br>
fetch$($명령어가 Load$)$ 될 때 자동적으로 pc의 값이 증가한다. <br>
PC의 값을 임시 백업하는 역할이 IR$($Instruction Register$)$이다. <br>
![memory_structure](/assets/img/computer/archtecture/memory_structure.jpg)
<br>

#### Calling convention을 통해 관리된다. 
stack frame을 반환하는 주체가 누구인지 같은 것을 설정 할 수 있다. <br>

#### Memory hierarchy
cache의 사용!<br>
temporal locality와 spatial locality<br>
속도가 빠른 곳일 수록 적은 메모리를 여러번 옮기고, 속도가 느린 곳일 수록, 많은 메모리를 한번에 옮긴다. <br>
multi thread를 이용하면 빠른이유이다. <br>
하나라도 느리면 bottle neck현상이 일어난다. <br>
<br>

### 가상메모리
mmu$($Memory management unit$)$은 cpu가 실제 물리 메모리가 아닌 가상 메모리를 인식하게 함.<br>
physical address를 Virtual address로 확장.<br>
swap File, auxilaray memory보조 기억장치를 쓴다.<br>
page table에 page별로 메모리를 관리함.<br>
page$(SW)$, page frame.$(HW)$<br>
즉, 보조 기억장치와 주기억장치의 관계를 cache와 ram 관계로 확장한다. <br>
context switching이 일어나서 다른 process를 사용할 때, 기존 것은 보조 기억장치에 저장한다<br> 
LRU$($least recently used$)$를 physical memeory에서 빼고, 보조기억 장치에 저장한다. <br>