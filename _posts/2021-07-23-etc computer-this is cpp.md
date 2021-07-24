---
layout: post
title: "This is C++"
categories:
  - Computer Network
tags:
  - 
  - 
last_modified_at: 2021-07-12
comments: true
---
# 이것이 C++이다.
제 2장 namespace 두번째와 세번째 강의<br>

### template <br>

### C++에서 overloading이 가능한 것은 name magling기능 덕분이다.<br>
이름은 같지만 다른 type의 function들은 각각 다른 이름도 부여되어 있다<br>



### r value reference <br>

### format 방법 3가지. <br>

### return pointer <br>
> 함수 안에서 malloc으로 입력 받거나 <br>

C로 자료구조를 만들면서 불편했던 점.<br>
structure에는 상속 기능 + constructor 기능이 없다. <br>
즉 어떤 instance를 만들 때, declare하고 format(define)을 따로 해야한다. <br>
가령<br>
```
typedef struct Node
{
    int data;
    struct Node* p_next;
} Node;

void NodeInit(Node* node)
{
    node->data = 0;
    node->p_next = NULL;
}

int main()
{
    Node s
    return 0;
}
```
```
// 만약 data의 type이 int가 아닌 다른 2차원 평면에서의 좌표 struct 라고 하자. 
// constructor(생성자)가 없기 때문에 malloc을 통해서 좌표와 Node를 따로 따로 할당해줘야한다.
// C++에서는 initializer list에서 base class(abstract class)의 private 변수도 같이 return

#include <stdio.h>
#include <Windows.h>
#include <assert.h>

// 2D point type
typedef struct Coordinate2D {
    int x_;
    int y_;
}Coordinate2D;

// 
typedef struct node {
    Coordinate2D* p_coordinate;
    struct node* p_next_node;
}Node;

// 
typedef struct LinkedList {
    Node *p_current_node;
    Node *p_header_node;
}LinkedList;

// pointer를 return 하려면, const를 써야지 return 할 수 있지 않나?
// 이곳의 const의 유무가 어떤 역할인지..
// malloc에서 주는 변수는 heep memory에 있으므로, return해도 해제되지 않는군.
// literal도 함수 stack에 저장되지 않으므로, char* name = "blabla" return 해도 되는 군!
// literal과 dynamic allocated pointer는 return해도 됨.
Node* NodeConstructor(const int x, const int y)
{
    Node*         const p_new_node   = (Node*)malloc(sizeof(Node));
    Coordinate2D* const p_2d         = (Coordinate2D*)malloc(sizeof(Coordinate2D));
    
    assert(p_new_node != NULL);
    assert(p_2d       != NULL);

    p_new_node->p_coordinate = p_2d;
    p_new_node->p_coordinate->x_ = x;
    p_new_node->p_coordinate->y_ = y;
    p_new_node->p_next_node = NULL;

    return p_new_node;
}

// 
void ListInit(LinkedList* list)
{
    Node* p_dummy_node = NodeConstructor(/*x = */0, /*y = */ 0);
    list->p_current_node = NULL;
    list->p_header_node = p_dummy_node;
}

// TODO
// x축 오름차순 
// y축 내림차순


void PrintList(LinkedList* list)
{
    list->p_current_node = list->p_header_node;
    while (list->p_current_node != NULL)
    {
        printf("x coordinate : %d \n", list->p_current_node->p_coordinate->x_);
        printf("y coordinate : %d \n", list->p_current_node->p_coordinate->y_);
        printf("               | \n");
        printf("               V \n");
        list->p_current_node = list->p_current_node->p_next_node;
    };
    printf("             NULL\n\n");
}

// dumy가 깨지는 거 아님? dumpy 다음에 붙히면 되네.
void AppendFront(LinkedList* list, Node* node)
{
    node->p_next_node = list->p_header_node->p_next_node;
    list->p_header_node->p_next_node = node;
}

void Insert();


void DeleteList(LinkedList* list)
{
    list->p_current_node = list->p_header_node;
    while (list->p_current_node != NULL)
    {
        Node* p_delete_node  = list->p_current_node;
        list->p_current_node = list->p_current_node->p_next_node;
        free(p_delete_node->p_coordinate);
        free(p_delete_node);
    };
}


int main()
{
    LinkedList coordinate_list; // 궂이 이렇게 format을 해야 하는지는 의문,
    ListInit(&coordinate_list); // constructor로 생성할 때, format해서 return 해주면 안 됨? Time_T 처럼.
    PrintList(&coordinate_list);
    Node* new_nodes[5];
    Node* p_new_node = NodeConstructor(1, 6);
    AppendFront(&coordinate_list, p_new_node);
    PrintList(&coordinate_list);
    DeleteList(&coordinate_list);
    return 0;
}

```


> initializer list로 abstract class(virtual method)<br>

return const value <br>
return reference <br>

### SBCS, MBCS, WBCS

