---
layout: post
title: "C++로 순열 조합 분할 구현하기"
categories:
  - Algorithm 
tags:
  - power set
  - Permutation with repeatation 
  - Permutation
  - Combination 
  - Homogenious
  - partition 
last_modified_at: 2023-05-20
comments: true
---
해당 포스팅에는 배열과 숫자를 나눠서 탐색하는 방법을 적겠습니다.

## 1. power set, 멱집합 
멱집합은 해당 집합의 부분집합들의 집합입니다.  
```c++
int arr[5] = {0,1,2,3,4} // 다른 숫자로 써도 됩니다. arr에 index로 접근하면 되기 때문입니다.  
```
### 1.1 positional notation, 위치 기수법
- 시간 복잡도 $O(N 2^N)$  
- 공간 복잡도 $O(1)$  
example  

```
void superSetByPlaceNumber(int arr[], int n){
    int power_set_size = 1;
    for(int i = 0; i <n; i++) power_set_size *= 2;  
    // becuase double has 15 significant number. if we use pow, there whould be error.
    
    for(int counter = 0; counter < power_set_size; counter++){
        for(int place_number = 0; place_number < n; place_number++){
            if(counter&(1 << place_number))
                cout << arr[place_number];
        }
        cout << "\n";
    }
}

```
### 1.2 Back tracking 
장점 : 단순 arr의 선택이 아닌 복잡한 2D 좌표간의 이동도 나타낼 수 있습니다.  
1~N개의 combination을 돌리면 된다.  
- 시간 복잡도 $O(N 2^N)$
- 공간 복잡도 $O(N)$
example  

```
vector<int> picked;
void superSetByBackTracking(int m, int n){
    if(picked.size() == n){
        for(int i : picked)
            cout << i << " ";
        cout << "\n";
        return;
    }
    int smallest = picked.empty()? 0 : picked.back()+1;
    for(int i = smallest; i < m; i++){
        picked.push_back(i);
        superSetByBackTracking(m,n);
        picked.pop_back();
    }

int main(){
    for(int i = 0; i <= 5; i++)
        superSetByBackTracking(sizeof(arr)/sizeof(int), i);
}
```
### 1.3 lexicographical order 사전순 구현 
n개의 선택하는 원소와 next_permutation을 통해 나타낸다.  
- 시간 복잡도 $O(N logN+ N 2^N)$  
- 공간 복잡도 $O(N)$  
example
```
//step1 sorting : 오름차순 정렬 됩니다.
//step2 next permutation : 다음 내림차순으로 원소를 바꿉니다.  
```

## 2. Permutation with repeatation
서로 같은 것을 서로 다른 것에 분할 하는 방입니다.  
### 2.1 back tracking 구현 
- 시간 복잡도 $O(N^M)$  
- 공간 복잡도 $O(N)$  
example
```
void permutationWithRepeatation(int arr[], int to_pick){ 
  // 인수로 total을 넘기면서 search도 가능. 하지만 무엇을 뽑았는지는 저장 안 됨.
    if(picked.size() == to_pick){
        for(int i : picked)
            cout << i << " ";

        cout << "\n";
        return;
    }
    for(int i = 0; i < 5; i++){
        picked.push_back(i);
        permutationWithRepeatation(arr,to_pick);
        picked.pop_back();
    }
}
```
### 2.2 positional notation
n진법을 쓰면 0~n-1 숫자로 이루어진 집함을 만들 수 있습니다.    
- 시간 복잡도 $O(N^M)$  
- 공간 복잡도 $O(1)$
example  

```
void permutationWithRepeatation2(int n, int m ){ // 인수로 total을 넘기면서 search도 가능. 
    long long count = 1;
    for(int i = 0; i < m; i++) count *= n;
    while(count--){
        long long tmp = count;
        for(int place_num = 0; place_num < m; place_num++){
            cout << tmp % n;
            tmp /= n;
        }
        cout << "\n";
    }
}

```  

## 3.Permutation

- 시간 복잡도 $O(N * N!) $  
- 공간 복잡도 $O(N)$  
### 3.1 lexicographical 사전순으로 구현
사전순으로 구현할 시, nPn만 구할 수 있다.  
example  

```
void Permutation(int n, int m){
    vector<int> increase(m);
    for(int i = 0; i<m; i++) increase[i]= i;
    do{
        for(int i : increase)
            cout << i << " ";
        cout << "\n";
    }while(next_permutation(increase.begin(), increase.end()));
}

int main(){
    Permutation(5,3); // 5개 중에서 순서를 고려해서 3개 뽑을 수 없음. 
    //5개중에 5개 모두 뽑거나, 3개중에 3개 모두 뽑아야 함.
}
```
### 3.2 back tracking
만약 nPm을 구하고 싶다면 back tracking으로 직접 구현해야 한다.  
example  

```
bool is_used[5];
void backPermut(int n, int m){
    if(picked.size() == m){
        for(int i: picked)
            cout << i << " ";
        cout << "\n";
    }
    for(int i = 0; i < n; i++){
        if(is_used[i]) continue;
        is_used[i] = true;
        picked.push_back(i);
        backPermut(n,m);
        is_used[i] = false;
        picked.pop_back();
    }
}
int main(){
    backPermut(5,3); 
}
```

## 4. Combination 
순서를 강제해서 뽑는다고 생각하면 됩니다.  
- 시간 복잡도 $O(min(n^k, n^(n-k)))$  
- 공간 복잡도 $O(N)$  

### 4.1 lexicographical  
```
//next_permutation에 같은 원소를 넣어서 구현한다.
// 가령 0 0 0 0 1 1  은 5C2입니다.   
```
### 4.2 choose smallest in the array. using vector STL
example  
```
void print_combination(int n, int m){
    if(picked.size() == m){
        for(int elem: picked)
            cout << elem << " ";
        cout << "\n";
        return;
    }
    
    // 내림 차순 정렬 후, 안 쓰는 smallest indx 반환하는 것이 중요.
    int smallest = picked.empty() ? 1 : picked.back()+1;
    
    //smalllest부터 돌음
    for(int i = smallest; i <= n; i++){
        picked.push_back(i); 
        print_combination(n, m); 
        picked.pop_back();
    }
}
```

## Partition of natural number
서로 같은 것을 서로 같은 것에 분할 하는 방법.
-> 아마 DP로 할 듯. 

## Partition of set
서로 다른 것을 서로 같은 것으로 분할 하는 방법.  
자연수의 분할을 시행한 후, combination으로 중복 처리를 해준다.  