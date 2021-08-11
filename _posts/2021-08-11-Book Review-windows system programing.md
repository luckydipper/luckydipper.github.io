---
layout: post
title: "윈도우즈 시스템 프로그래밍"
categories:
  - Book Review
tags:
  - 컴퓨터 구조
  - 운영체제
last_modified_at: 2021-08-11
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
이 부분은 책의 내용이 살짝 부족했던 것 같습니다.ㅠㅜ<br>
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
<br>

```
// wide char를 쓰면 컴파일러얌, UTF-16으로 읽으(디코딩)하란 뜻
// 그냥 wide character 기반으로 사용하기 위해서 사용하는 것 같음.
wmain(int argc, char* argv[])
{
    wchar_t some[] = "aa";
    // 한국어 출력시 setlocale 해야됨..
    _wsetlocale(LC_ALL, L"korean");
    wprintf(wide_character);
};


// window에서만 사용 가능
// wide character 활용시, 몇몇 함수들과 호환이 안 될 수 있음.
// 그러므로 _t를 매크로로 사용함.
_tmain(int argc, char* argv[])
{
    TCHAR
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

# chapter 2.  64bit vs 32bit


