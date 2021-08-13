---
layout: post
title: "윈도우즈 시스템 프로그래밍"
categories:
  - Book Review
tags:
  - 컴퓨터 구조
  - 운영체제
last_modified_at: 2021-08-13
comments: true
---

## 컴퓨터 구조와 운영체제의 포괄적인 이해를 위한 책.
윤성우님의 책을 기반으로 작성하였습니다!<br>

### table of content
- 컴퓨터 구조는 Computer Archtectrue 카테고리에서 확인 가능해요!

|chapter  |content                          |
|chapter1 |Unicode vs ASCII code            |
|chapter2 |64bit vs 32bit                   |
|chapter3 |Process                          |
|chapter4 |Create delete Process            |
|chapter5 |Kernel Object                    |
|chapter6 |Mailslot IPC                     |
|chapter7 |Pipe IPC                         |
|chapter8 |Scheduling                       |
|chapter9 |Thread                           |
|chapter10|Thread create delete             |
|chapter11|Thread memory access synchronize |
|chapter12|Thread Execution oder synchronize|
|chapter13|Thread Pooling                   |
|chapter14|Exception Handling               |
|chapter15|I/O control                      |
|chapter16|synchronous vs asynchronous      |
|chapter17|virtual memory, heap, mmf        |
|chapter18|Dynamic Link Library             |

<br>
<br>

# chapter 1.  Unicode vs ASCII code
이 부분은 책의 내용이 살짝 부족했던 것 같다.ㅠㅜ<br>
유니코드, 인코딩, cp949의 차이를 설명해주고 있지 않네요.<br>
특히 WBCS를 유니코드로 설명해주고 있는데 너무 헷갈렸습니다.<br>
<mark>
결론1 : unicode encoding 방식은 에디터가 읽고 저장하는 방식에 따라, OS마따라, 컴파일러가 읽는 방식에 따라, 다를 수 있습니다. <br>
결론2 : wide character 를 사용하면, 문자들의 size를 일정하게 만들 수 있습니다. wchar_t => window visual studio에서는 2byte utf-16으로, linux gpp에선 4byte utf-32<br>
결론3 : window kernel에서는 UTF-16으로 2byte 환경을 사용한다. 하지만 byte order를 정해 놓지 않아, little endian은 bom을 붙힌다. 하지만 많은 system에서 bom을 문자열로 읽어들여서 에러가 날 수 있다. 그러므로 정보 교환용으로 utf-16은 좋지 않다.<br>
결론4 : window visual studio에서는 cpp source file을 cp949로 저장하고, wide char를 쓰면 컴파일러가 utf-16으로 읽음. 만약 안 쓰면 kernel쪽에서 utf-16으로 바꿔서 읽음. linux에서는 자동으로 utf-8로 읽는다.
<br>
</mark>


<br>


정리해 보겠습니다.<br>
ASCI    : 1byte로 모든 영어를 mapping.<br>
Unicode : 1~3byte 기반으로 모든 문자를 mapping.<br>
EUC-KR  : 옛날에 한국어 기록방법 <br>
cp949   : window에서 EUC-KR을 확장해서 사용.<br>
ANSI    : 컴퓨터 내부 system의 default값. 우리나라에서는 cp949<br>

유니코드 인코딩 방법<br>
UTF-8, UTF-16$($window에서 사용하는 2byte wide character$)$, UTF-32$($linux에서 wide character$)$<br>
UCS-2도 모든 code를 2byte로 읽는다. 하지만 읽지 못하는 것이 있다.<br>
한글의 UTF-8 encoding 값은 3byte이다.<br>
영어의 UTF-8 encoding 값은 1byte이다.<br>

제가 이해한 대로 써보겠습니다! <br>
다 같이 생각해보면 좋겠네요.  :)<br>
<br>

### 1. WBCS에 Unicode가 포함 될 수 있는가?
책에서는 wide character set이 모든 문자를 2byte로 처리하는 문자셋 이라고 표현 되어 있습니다. <br>
찾아보니, 2byte로 문자열의 size를 고정 하는 것은 맞습니다.<br>
하지만 wide character set에 Unicode가 포함 되는 것은 아닌 것 같습니다. <br>
유니코드는 숫자와 문자를 연결한 사전 같은 느낌입니다.<br>
그래서 1byte 부터 3byte까지 다양한 숫자들과 실제 문자가 mapping 됩니다!<br>
즉, 모든 문자가 0x 00 ~ 0x 10 FF FF 까지의 숫자 중 하나에 mapping 됩니다.<br>
이 유니코드들은 파일을 ​저장하거나 컴파일 할 때 encoding되고,<br>
파일을 열 때 decoding 됩니다.<br>
이 방식에 따라서 파일의 크기가 달라집니다.<br>
<br>

### 2. 유니코드가 저장 될 때에는 인코딩에 따라 다양한 크기로 저장된다.<br>
문서를 UTF-8로 인코딩$($저장$)$ 하는 경우, MBCS와 비슷한 방식을 취합니다.<br>
Variable-width encoding을 취합니다.<br>
영어가 저장될 때는 1byte, 한글이 저장 될 때에는 3byte로 저장됩니다!<br>
각 문자별로 1~4byte 로 저장됩니다. 
<br>

### 3. 그럼 왜 windows에서 strlen$($"한"$)$은 2byte를 나타내는가?<br>
window에서 제공하는 visual studio는 기본 저장 방식이 cp949으로 돼있습니다!<br>
cpp 파일을 notepad로 열어보면 window system에서 기본으로 제공하는 ANSI로 돼있습니다.<br>
한국에서는 cp949로 문자열을 저장합니다! 이 문자 set에서는 한글의 strlen은 2byte로 저장됩니다. <br>
cpp 파일을 unicode 형태로, UTF-8로 인코딩 해서 저장 한 경우에도, 한글의 strlen은 2byte로 계산 됐습니다.<br>
컴파일러가 읽어 드릴 때, cp949 문자열로 읽는 것 같습니다. <br>
<br>

이렇게 정리 해봤습니다.<br>
틀린 부분이 있을 수 있습니다.<br>
다른 분들의 생각이 궁금합니다! :) <br>

이분들 posting이 도움 됐습니다. <Br>
reference<br>
<a href = "https://norux.me/31">https://norux.me/31</a><br>
<a href = "https://umbum.dev/328">https://umbum.dev/328</a><br>
<a href = "https://docs.microsoft.com/en-us/cpp/c-language/multibyte-and-wide-characters?view=msvc-16">ms wide character  </a><br>
<a herf = "http://www.liangshunet.com/en/202003/567276536.htm">
tool custumize를 통해, advanced save option을 추가할 수 있습니다!</a><br>
<a herf = "https://en.wikipedia.org/wiki/Wide_character">wide character와 ucs-2의 관계</a><br>
<a herf = "https://brownbears.tistory.com/124">utf-8 BOM(Byte Oder Mark)</a>
<br>

```
// wide char를 쓰면 컴파일러얌, UTF-16으로 읽으(디코딩)하란 뜻
// 그냥 wide character 기반으로 사용하기 위해서 사용하는 것 같음.
wmain(int argc, wchar_t* argv[])
{
  // argc : 전달 parameter의 수
  // argv : 첫번째는 실행 경로, 나머지는 command에서 넘겨준 실행 parameter
    wchar_t some[] = "aa";
    // 한국어 출력시 setlocale 해야됨..
    _wsetlocale(LC_ALL, L"korean");
    wprintf(wide_character);
};


// window에서만 사용 가능
// wide character 활용시, 몇몇 함수들과 호환이 안 될 수 있음.
// 그러므로 _t를 매크로로 사용함.
#include tchar.h
_tmain(int argc, char* argv[])
{
  //UNICODE로 조건부 정의 돼있음
  TCHAR str2[] = _T("aa");
}
```
윈도우에서는 type 이름을 대문자로 씁니다.<br> 
또한 헝가리안 표기법 비슷하게 씁니다.<br>
UI를 만들 때는 꼭 알아야 할 것 같아요!<br>
encoding decoding 해서 저장하거나 읽는 것은 editor나 compiler에 따라 다르다.<br>
<mark>linux에서 vim으로 coding하면 default로 utf-8로 encoding된다. 컴파일러도 똑같이 utf-8로 읽는다. </mark><br>
<mark>linux에서 wide character를 쓰지 않는 이유는 문자 1개를 4byte로 쓰기 때문이다. window에서는 2byte utf-16이다.</mark><br>

```
// in linux gpp compiler
int main()
{
    char english_korean[] = "EN한글";
    wchar_t wide_char[] = "EN한글"
    std::cout << strlen(english_korean);
    // 8이 출력 된다. 
    // 1 + 1+ 3 +3 

    std::cout << sizeof(english_korean) <<std::endl; // 9byte!
    std::cout << sizeof(wide_char) <<std::endl;      // 20 byte!
}
```
<br>

# chapter 2.  64bit vs 32bit
64bit 와 32bit의 차이는 IO bus에서 주소가 한번에 이동하는 크기.<br>
CPU에서 한 쿨럭에 처리하는 크기.<br>
process에 가상메모리를 할당해주는 크기.<br>
windows 기반 자료형이 존재한다.<br>
GetLastError함수를 통해 에러코드를 얻을 수 있다.<br>
polimorphic 자료형을 제공한다. Linux에서는 호환성을 갖지 않는다.<br>
<br>

# chapter 3, 4. 9. Process Scheduling
메인 메모리와 register chache에 올라가서 돌아가는 프로그램을 process라고한다. <br>
process의 구조는 data, instruction, heap, stack 4개의 section으로 구성된다. 각 프로세스들은 독립된 우선순위와 메모리 공간을 갖는다.<br>
process는 크게 3가지로 나뉜다. run$($cpu에서 연산 중$)$, Block$($IO 연산 중$)$, Ready$($OS에 의해 context switching이 언제든 가능하다.$)$<br>
이런 process들이 context switching 하며 돌아가는 프로그램을 multi processing이라고 한다.<br>
OS는 scheduler를 통해 context switching을 한다. 한 process가 다른 process의 cpu 점유 시간까지 먹고 있는 상태를 starvation 이라고 한다.<br>
scheduling algorithm은 선점형$($preemption$)$ 비선점형$($non-preemption$)$ 방식이 있다.<br>

non-preemption 방식에는 FIFO, Sortest Job First, HRN $($대기 시간과 실행 시간을 혼합하여 결정$)$<br>
preemption 방식에는 라운드 로빈$($Round Robin$)$, MultiLevel Queue, MultiLevel Feedback Queue<br>
두 개를 합친 priority scheduling도 있다.<br>
$($Round Robin$)$에서는 우선순위가 같은 process들은 형성있게, priority가 낮은 process는 높은 priority가 끝날 때 까지 기다린다.<br>
이때 scheduler가 확인하는 시간 단위를 quentum 또는 time slice라고 한다. <br> 
1. time slice마다<br>
2. process의 생성 및 소멸마다<br>
3. 실행중인 process가 io 상태로 blocking 될 때 마다 실행된다.<br>
Prioriy inversion을 통해서 우선순위가 높은 프로세스가 낮은 프로세스에게 기회를 줄 수 있다.<br>
Soft RTOS는 time slice가 작아서 반응성이 좋다. context switching이 자주 일어난다, <br>
HARD RTOS는 Dead line이 존재해서 그 시간안에 무조건 끝낸다.<br>
<br>

<mark>CreateProcess</mark> 함수로 자식 process를 만든다.<Br>
STARTUPINFO, PROCESSINFOMATION 등으로 console을 어디다가 어떻게 띄울지 결정한다.<br>
SetProcessInfomation..<br>
GetCurrentDirectory, GetSystemDirectory, GetWindowsDirectory, GetEnvorn.. <br>
CreateProcess의 두번째 인자로 실행할 exe와 parameter를 넘길 수 있다. <br>
current dir -> system dir -> windows dir -> environment path variable 순으로 찾는다. <br>
PCB$($Process Control Block$)$를 windows 에서는 kernel object를 생성한다. <br>
<br>


# chapter 5. Kernel Object
OS는 process들을 관리하기 위해, PCB라는 process control block을 만든다.<br>
Process control block 안에는 program count, priority, status, process id, 메모리 관리 등등이 저장된다.<br>
windows의 PCB는 kernel object이다.<br>
kernel은 OS의 핵심 부분이다. scheduling, processing, 보안, deive driver등의 역할을 한다.<br>
또한 process가 kernel에 접촉할 수 있게 빼놓은 포인터가 handle이다.<br>
각 process는 독립적인 handle table을 갖고 있어, process에서 kenelobject의 제어가 가능하다.<br>
handle table에는 자신의 kernel object를 가르키는 handle은 없다<br>
private이기 때문에 kenelobject 안의 값을 직접적으로 바꾸지는 못한다. <br>
Pipe, MailSlot, File 모두 kernel object로 관리된다.$($각각의 형태는 다르다.$)$<br>
CloseHandle을 통해서 program counter의 값을 낮출 수 있다. program count가 0이 될 때, kernel object는 사라진다. <br>
```
#include <Windows.h>
int main()
{
    STARTUPINFO start_info = { 0 };
    PROCESS_INFO process_info = {0};
    process_info.hProcess;
    process_info.hThread;
    HANDLE a = GetCurrentProcess();
    TCHAR command[] = _T("AdderProcess.exe 1 2");
    STARTUPINFO start_info{ 0 };
    start_info.cb = sizeof(start_info);

    PROCESS_INFORMATION process_info{ 0 };

    bool is_run =CreateProcess(
        NULL, // 생성할 프로세스의 실행 파일 이름, 경로 추가지정 가능
        command, // 원래 command를 여기다 치고, 위에는 process 이름
        NULL, // p보안
        NULL, // t보안
        TRUE, // 상속여부
        NULL, //  CREATE_NEW_CONSOLE, process특성 우선순위나 새로운 process만들 때.
        NULL, // 환경 변수 블럭 
        NULL, // 생성하는 프로세스의 현재 디렉터리를 설정 
        &start_info,
        &process_info
    );

    //block 상태에서 child process가 끝날 때 까지 기다림
    WaitForSingleObject(process_info.hProcess, INFINITE)


    //정상적으로 종료됐는지 부모가 child kernel object의 handle을 가지고 확인가능!
    GetExitCodeProcess(process_info.hProcess, &state);


    //자식 process와 연결을 끊음. program counter를 1 낮춤
    CloseHandle(process_info.hProcess);
    CloseHandle(process_info.hThread);

    //TerminateProcess로 프로세스 종료 가능
}
```



