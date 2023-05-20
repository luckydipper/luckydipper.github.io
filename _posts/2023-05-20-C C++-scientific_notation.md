---
layout: post
title: "scientific notation"
categories:
  - Algorithm 
tags:
  - scientific notation
  - exponential
last_modified_at: 2023-05- 20
comments: true
---

# scientific notation
과학적 기수법.  
유효숫자와 지수부분을 따로 때서 표기.  

```C++
#include <iostream>
using namespace std;
cout.precision(5);
cout << setprecision(3) << f;
cout << scientific;
// 위와 같이 썼을 시 
// 계수e지수 표현
// ex) 52.4 * 10^32
//     == 5.24e32
// !! caution 2e-32는 10-32으로 적은 수임. 
```