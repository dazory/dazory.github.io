---
toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
toc_sticky: true

layout: post-with-comments
title: '[ALGO-02] Lect2. 기본적인 선형 자료구조의 종류, 구동 원리 및 활용법 이해'
excerpt: "[ALGO] 성균관대학교 허재필 교수님의 K-mooc 알고리즘 강의를 듣고 공부한 자료입니다."
date: 2021-08-05
last_modified_at: 2021-08-05
categories:
  - computerScience-algorithm
tags: 
   - [computer science, algorithm, ALGO]

use_math: false
comments: true
share : false
---

<div class="info-dialog">
    <div class="title">강의 정보</div>
    <div class="content">
        강의명: [집콕]인공지능을 위한 알고리즘과 자료구조: 이론, 코딩, 그리고 컴퓨팅 사고<br>
        교수: 성균관대학교 소프트웨어학과 허재필 교수님<br>
        사이트: <a href="http://www.kmooc.kr/courses/course-v1:SKKUk+SKKU_46+2021_T1/course/">http://www.kmooc.kr/courses/course-v1:SKKUk+SKKU_46+2021_T1/course/</a>
    </div>
</div>

<br>
<br>


# 선수지식

## 메모리 접근 오퍼레이터

**용어**

* **Pointer**<br>
어떤 variable은 크게 `Address`, `Value`, `Name`으로 표현된다고 할 때, pointer를 이용하여 value가 아닌 <hl>address로 값을 받아올 수 있다.</hl>

* **&(Ampersand) Operator**<br>
& 연산자는 reference 연산자라고도 불리며, <hl>변수의 주소값을 반환</hl>한다.

* ***(Asterisk) Operator**<br>
*연산자는 dereference 연산자라고도 불리며, 포인터 앞에서 <hl>포인터가 가르키는 값에 접근</hl>한다.

<br>

**예제1**

변수 n의 주소를 변수 pn에 초기화

```c++
int n=3;
int *pn = &n;
```

<img src="/files/2021-08-05-cs-algorithm-algo02/image-20210801152358545.png" alt="image-20210801152358545" style="width:600px;" />

<br>

**예제2**

```c++
char c = 'A';
char *pc = &c;
printf("c=%c, *pc=%c \n", c, *pc);

*pc = 'C';
printf("c=%c, *pc=%c \n", c, *pc);
```

출력결과:

```tex
c=A, *pc=A
c=C, *pc=C
```

<br><br>

## Function Call

함수를 호출할 때 <hl>argument를 넣어주는 방식</hl>에 따라 크게 두 가지로 나뉜다.

**1. Call by value**

* <hl>값 자체</hl>를 넣어준다.
* 원래 변수의 값을 수정할 수 없다.

**2. Call by reference**

* <hl>포인터/주소값</hl>으로 함수를 호출한다.
* 원래 변수의 값을 수정할 수 있다.

<br>

**예제**

```c++
void swap1(int x, int y);
void swap2(int *px, int *py);

int main(int argc, char **argv){
    int a=5, b=7;
    printf("a=%d, b=%d\n", a, b);

    swap1(a,b);
    printf("swap1: a=%d, b=%d\n", a, b);

    swap2(&a, &b);
    printf("swap2: a=%d, b=%d\n", a, b);    

    return 0;
}

void swap1(int x, int y){
    int tmp = x;
    x = y;
    y = tmp;
}
void swap2(int *px, int *py){
    int tmp = *px;
    *px = *py;
    *py = tmp;
}
```

출력결과:

```tex
a=5, b=7
swap1: a=5, b=7
swap2: a=7, b=5
```

<br><br><br>

# 배열과 리스트

<img src="/files/2021-08-05-cs-algorithm-algo02/image-20210801143659915.png" alt="image-20210801143659915" style="width:600px;" />

선형 자료 구조 중 대표적인 <hl>array와 list</hl>에 대해 알아본다.

## Array

array는 어떤 자료의 값들을 모아둔 자료 구조이다.


### 1. 기본개념


**구분자**

array <hl>index를 통해 구분</hl>되며, 프로그래밍 언어에 따라 인덱스는 0부터 시작할 수도, 1부터 시작할 수도 있다. C/C++, JAVA 프로그래밍 언어에서는 array index가 0부터 시작한다. 

가령 아래와 같이 size가 10인 int형 array score를 선언할 때 array index는 0~9이다.

<img src="/files/2021-08-05-cs-algorithm-algo02/image-20210801144312550.png" alt="image-20210801144312550" style="width:300px;" />

<br>


**저장**

프로그래밍 언어를 이용하여 array를 선언하면, <hl>physical memory인 RAM에 연속적인 공간을 할당받는다.</hl> 

<br>


**선언**

array를 선언하는 방법은 크게 두 가지가 있다.

1. <hl>정적 생성</hl>

   ```c++
   Type d[10];
   ```

2. <hl>동적 생성</hl>

   ```c++
   Type *d = new Type[size];
   ```

   동적으로 생성한 array를 다 사용한 경우, 할당을 삭제해야 한다: `delete [] d;`

<br>


**접근**

```c++
d[5] = 2;
```

인덱스를 이용하여 접근할 수 있다.

<br>

### 2. Array 생성 (in C++)

**1차원 array**

```c++
int arr[10];
// 혹은 
int *arr = new int[10];
```

<img src="/files/2021-08-05-cs-algorithm-algo02/image-20210801145529448.png" alt="image-20210801145529448" style="width:500px;" />

<br>

**2차원 array**

```c++
int a[3][4];
// 혹은 
int **a = new int *[3];
for(int i=0; i<3; i++)
    a[i] = new int[4];
```

<img src="/files/2021-08-05-cs-algorithm-algo02/image-20210801145906817.png" alt="image-20210801145906817" style="width:300px;" />

<br>

### 3. Array in Memory

```c++
int score[3] = {52, 17, 61};
```

위와 같이 score array를 생성했을 때, physical memory에는 <hl>아래 이미지<hl>와 같이 값이 쓰여진다.

<img src="/files/2021-08-05-cs-algorithm-algo02/image-20210801150840947.png" alt="image-20210801150840947" style="width:600px;" />

<br>

### 4. Insertion and Deletion


**Insertion**

array 요소의 중간에 어떤 값을 삽입하고싶을 때, <hl>이후 값을 모두 1칸씩 뒤로 밀고 생긴 빈자리에 값을 넣어</hl>주어야 한다.

```c++
int main(int argc, char **argv){
    int arr[5] = {3, 10, 7, 6,};
    printIntArray(arr, 5);
    
    // 3(arr[0])과 10(arr[1]) 사이에 5를 삽입
    for(int i=5; i>1; i--)
        arr[i] = arr[i-1];
    arr[1] = 5;

    // array 결과를 출력
    printIntArray(arr, 5);

    return 0;
}
```

출력결과:

```tex
3 10 7 6 0 
3 5 10 7 6
```

<br>

**Deletion**

마찬가지로 어떤 요소값을 삭제할 때도 <hl>이후 값을 모두 1칸씩 앞으로 밀어주어야</hl> 한다.

```c++
int main(int argc, char **argv){
    printf("Hello World!\n");

    int arr[5] = {3, 5, 10, 7, 6};
    printIntArray(arr, 5);
    
    // 3(arr[0])를 삭제
    for(int i=0; i<5; i++)
        arr[i] = arr[i+1];

    // array 결과를 출력
    printIntArray(arr, 5);

    return 0;
}
```

출력결과:

```tex
3 5 10 7 6 
5 10 7 6 0
```

<br>

## List

insertion 혹은 deletion을 수행할 때 해당 인덱스를 기점으로 뒤의 값을 모두 밀어주어야하는 <hl>array의 단점을 해결</hl>하기위해 제안된 자료구조가 바로 linked list이다. physical memory상에서 순서대로 작성되지 않으므로 <hl>삽입과 삭제가 한 번의 동작으로 가능</hl>하다.

### 1. 기본개념

```c++
typedef int Data;
typedef struct _Node
{
    Data item;
    struct _Node * next;    
} Node;

typedef struct
{
    Node *head;
    int len;
} LinkedList;

```

<img src="/files/2021-08-05-cs-algorithm-algo02/image-20210801160842809.png" alt="image-20210801160842809" style="width:600px;" />

<br><br><br>

# 스택과 큐

<hl>stack과 queue</hl>는 빈번히 사용되는 자료구조이다. C++에서는 STL(standard template library) 소프트웨어 패키지를 이용하여 `pair`, `vector`, `list`, `queue`, `stack`과 같은 다양한 종류의 container를 제공한다. 

## Stack

<img src="/files/2021-08-05-cs-algorithm-algo02/image-20210801161404482.png" alt="image-20210801161404482" style="width:100px;" />

삽입과 삭제가 <hl>LIFO(last-in first-out)</hl> 순서로 진행된다. 

**용어**

* `Top`: stack의 최상단 지점
* `Push`: top 위치에 아이템을 삽입
* `Pop`: top에 위치한 아이템을 제거

<br>

**응용**

<hl>괄호매칭 문제</hl>에서 stack이 사용된다. 열린 괄호가 나타날 때 stack에 열린괄호를 push하고, 닫힌 괄호가 나타날 때 열린 괄호를 pop한다. 최종적으로 stack이 비어있다면 괄호매칭이 잘 된 것이라 판단한다.

<br><br>

## Queue

<img src="/files/2021-08-05-cs-algorithm-algo02/image-20210801161957500.png" alt="image-20210801161957500" style="width:120px;" />

삽입과 삭제가 <hl>FIFO(first-in first-out)</hl> 순서대로 진행된다.

**용어**

* `Front (head)`: queue의 앞 부분 (deletion이 발생)
* `Rear (back, tail)`: queue의 뒷 부분 (insertion이 발생)
* `Enqueue`: rear에서 아이템이 삽입됨
* `Dequeue`: front에서 아이템이 삭제됨

<br>

### Linear Queue vs. Circular Queue

<img src="/files/2021-08-05-cs-algorithm-algo02/image-20210801163040665.png" alt="image-20210801163040665" style="width:400px;" />

linear queue는 enqueue/dequeue를 반복하다보면 rear가 주어진 array의 마지막을 가리키게 된다. 따라서 front의 앞 부분에 분명 공간이 남아있음에도 사용할 수 없는 <hl>공간 낭비 문제</hl>가 발생한다.

이를 해결하기 위해 linear queue에서 할당된 array의 양끝을 이어붙인 <hl>circular queue</hl> 구조가 제안되었다. circular queue는 rear가 array의 마지막을 간 경우, 다시 0 index로 돌아오는 형태로 되어있다. 


<br><br>


# References

1. kmooc-[집콕]인공지능을 위한 알고리즘과 자료구조: 이론, 코딩, 그리고 컴퓨팅 사고: [http://www.kmooc.kr/courses/course-v1:SKKUk+SKKU_46+2021_T1/course/](http://www.kmooc.kr/courses/course-v1:SKKUk+SKKU_46+2021_T1/course/)


<br>