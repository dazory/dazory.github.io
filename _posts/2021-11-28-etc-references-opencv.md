---
toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
toc_sticky: true

layout: post-with-comments
title: '[References-001] opencv with C++ 프로젝트'
excerpt: "openCV로 C++ 프로그래밍할 때 유용한 문법들 모음"
date: 2021-11-28
last_modified_at: 2021-11-28
categories:
  - etc-references
tags: 
   - [references, opencv, C++]

use_math: false
comments: true
share : false
---

<br>

# 기본

## 경로

* `"filename.type"` : 코드가 위치한 곳
* `"/filename.type"` : 코드가 위치한 프로젝트 기준

<br>

<br>

# OpenCV

## Mat

### Generate

```c++
Mat()
Mat(int rows, int cols, int type)
Mat(Size size, int type)
Mat (int rows, int cols, int type, const Scalar &s)
Mat (Size size, int type, const Scalar &s)
Mat (const Mat &m)
```

* Type

  * **CV_8U** (=0) : 8-bit unsigned integer: uchar ( 0..255 )
  
    ※ 일반적인 .JPG 파일의 type은 CV_8UC3이다. ※<br>
    ※ Vec3b(uchar) type으로 pixel단위 접근을 할 수 있다. ※

    <img src="/files/2021-11-28-etc-references-opencv/image-20210927234133991.png" alt="image-20210927234133991" style="width:200px;" /><img src="/files/2021-11-28-etc-references-opencv/image-20210927234549686.png" alt="image-20210927234549686" style="width:100px;" />

  * **CV_8S** (=1) : 8-bit signed integer: schar ( -128..127 )

    <img src="/files/2021-11-28-etc-references-opencv/image-20210927234159040.png" alt="image-20210927234159040" style="width:200px;" /><img src="/files/2021-11-28-etc-references-opencv/image-20210927234611358.png" alt="image-20210927234611358" style="width:100px;" />

  * **CV_16U** (=2) : 16-bit unsigned integer: ushort ( 0..65535 )

    ※ Vec3w(ushort) type으로 pixel단위 접근을 할 수 있다. ※

    <img src="/files/2021-11-28-etc-references-opencv/image-20210927234213848.png" alt="image-20210927234213848" style="width:200px;" /><img src="/files/2021-11-28-etc-references-opencv/image-20210927234630359.png" alt="image-20210927234630359" style="width:100px;" />

  * **CV_16S** (=3) : 16-bit signed integer: short ( -32768..32767 )

    ※ Vec3s(short) type으로 pixel단위 접근을 할 수 있다. ※

  * **CV_32S** (=4) : 32-bit signed integer: int ( -2147483648..2147483647 )

    ※ Vec3i(int) type으로 pixel단위 접근을 할 수 있다. ※

  * **CV_32F** (=5) : 32-bit floating-point number: float ( -FLT_MAX..FLT_MAX, INF, NAN )

    ※ Vec3f(float) type으로 pixel단위 접근을 할 수 있다. ※

    <img src="/files/2021-11-28-etc-references-opencv/image-20210927234231565.png" alt="image-20210927234231565" style="width:300px;" /><img src="/files/2021-11-28-etc-references-opencv/image-20210927234651138.png" alt="image-20210927234651138" style="width:100px;" />

  * **CV_64F** (=6) : 64-bit floating-point number: double ( -DBL_MAX..DBL_MAX, INF, NAN )

    ※ Vec3d(double) type으로 pixel단위 접근을 할 수 있다. ※

    <img src="/files/2021-11-28-etc-references-opencv/image-20210927234249304.png" alt="image-20210927234249304" style="width:300px;" /><img src="/files/2021-11-28-etc-references-opencv/image-20210927234708659.png" alt="image-20210927234708659" style="width:100px;" />

  ※ 이러한 type 뒤에 channel의 개수를 붙여서 사용하기도 함 (e.g., CV_8UC1) ※<br>
  ※ channel이 하나 증가할 때 마다 8씩 증가된다. ※
  e.g., CV_8UC1=0,  CV_8UC2=8,   CV_8UC3=16;
          CV_8SC1=1,  CV_8SC2=9,  CV_8SC3=17;

<br>

<br>

### Read

```c++
Mat img = imread("img.jpg");
```

<br>

<br>

### Write

```c++
int i=0;
string title = "img";
imwrite(title+to_string(i), img);
```

※ 이때, 저장할 경로의 directory가 이미 존재해야한다. 없을 경우 저장되지 않음. ※

<br>

<br>

### Show

```c++
imshow("title", Mat img);
waitKey(0);
destroyAllWindows();
```

<br>

<br>

### Approach

#### each pixel

```c++
Mat::at(int row, int col)
Mat::at(int i0, int i1, int i2) // in: Index along the dimension n
Mat::at(const int *idx) // idx: Array of Mat::dims indices
Mat::at(const Vec<int,n> &idx)
Mat::at(Point pt) // pt: Element position specified as Point(j,i).
```

```c++
img.at<uchar>(y,x) = 255;
img.at<Vec3b>(y,x)[c] = 255;
```

* type

  * **Mat1b** (=Mat<uchar\>)<br>
    **Mat2b** (=Mat<Vec2b\>)<br>
    **Mat3b** (=Mat<Vec3b\>)<br>
    **Mat4b** (=Mat<Vec4b\>)<br>
    **Vec2b** (=Vec<uchar, 2>)<br>
    **Vec3b** (=Vec<uchar, 3>)<br>
    **Vec4b** (=Vec<uchar, 4>)<br>
  * **Mat1w** (=Mat<ushort\>)<br>
    **Mat2w** (=Mat<Vec2w\>)<br>
    **Mat3w** (=Mat<Vec3w\>)<br>
    **Mat4w** (=Mat<Vec4w\>)<br>
    **Vec2w** (=Vec<ushort, 2>)<br>
    **Vec3w** (=Vec<ushort, 3>)<br>
    **Vec4w** (=Vec<ushort, 4>)<br>
  * **Mat1s** (=Mat<short\>)<br>
    **Mat2s** (=Mat<Vec2s\>)<br>
    **Mat3s** (=Mat<Vec3s\>)<br>
    **Mat4s** (=Mat<Vec4s\>)<br>
    **Vec2s** (=Vec<short, 2>)<br>
    **Vec3s** (=Vec<short, 3>)<br>
    **Vec4s** (=Vec<short, 4>)<br>
  * **Mat1i** (=Mat<int\>)<br>
    **Mat2i** (=Mat<Vec2i\>)<br>
    **Mat3i** (=Mat<Vec3i\>)<br>
    **Mat4i** (=Mat<Vec4i\>)<br>
    **Vec2i** (=Vec<int, 2>)<br>
    **Vec3i** (=Vec<int, 3>)<br>
    **Vec4i** (=Vec<int, 4>)<br>
    **Vec6i** (=Vec<int, 6>)<br>
    **Vec8i** (=Vec<int, 8>)<br>
  * **Mat1f** (=Mat<float\>)<br>
    **Mat2f** (=Mat<Vec2f\>)<br>
    **Mat3f** (=Mat<Vec3f\>)<br>
    **Mat4f** (=Mat<Vec4f\>)<br>
    **Vec2f** (=Vec<float, 2>)<br>
    **Vec3f** (=Vec<float, 3>)<br>
    **Vec4f** (=Vec<float, 4>)<br>
    **Vec6f** (=Vec<float, 6>)<br>

  * **Mat1d** (=Mat<double\>)<br>
    **Mat2d** (=Mat<Vec2d\><br>
    **Mat3d** (=Mat<Vec3d\><br>
    **Mat4d** (=Mat<Vec4d\><br>
    **Vec2d** (=Vec<double, 2>)<br>
    **Vec3d** (=Vec<double, 3>)<br>
    **Vec4d** (=Vec<double, 4>)<br>
    **Vec6d** (=Vec<double, 6>)<br>

<br>

#### Range

$$
\text{Mat M }  = \begin{bmatrix} 
	1&2&3\\ 4&5&6\\ 7&8&9
\end{bmatrix}
$$

일 때,

```c++
Mat M_(행,열) = M(행의 범위, 열의 범위)
```

Example : 

```c++
Mat M_0x = M(Range(0,1), Range(0,3));	// [1,2,3]
Mat M_1x = M(Range(1,2), Range(0,3));	// [4,5,6]
Mat M_2x = M(Range(2,3), Range(0,3));	// [7,8,9]

Mat M_x0 = M(Range(0,3), Range(0,1));	// [1;4;7;]
Mat M_x1 = M(Range(0,3), Range(1,2));	// [2;5;8;]
Mat M_x2 = M(Range(0,3), Range(2,3));	// [3;6;9;]

Mat M__ = M(Range(0,2), Range(1,3));	// [2,3; 5,6;]
```

<br>

#### Circle

```c++
circle(img, Point2f, 3, Scalar(0, 255, 0), 3);
```

<br>

<br>

### Resize

```c++
Mat::resize(size_t sz)	// sz: New number of rows
Mat::resize(size_t sz, const Scalar &s) // s: Value assigned to the newly added elements
```

```c++
resize(src, dst, Size(width, height));
```

<br>

<br>

### Convert Type

```c++
Mat::convertTo(OutputArray m, int rtype, double alpha=1, double beta=0)
```

```c++
src.convertTo(dst, CV_8UC3);
```

<br>

<br>

# C++

## min/max

```c++
int minVal = min({1,2,3,4}); // 1
int maxVal = max({1,2,3,4}); // 4
```

이런식으로 중괄호를 통해 여러 값에 대해 최소/최대값을 구할 수 있다.

<br>

<br>

## Vector

array와 달리 동적으로 원소를 추가할 수 있으며 크기가 자동으로 늘어난다.

### 생성

```
vector<int> v;
```

<br>

### 추가 & 삭제

```c++
v.push_back(1);	// vector의 back에 원소를 추가
v.pop_back();	// vector의 back에 원소를 삭제
```

<br>

### 접근

```c++
int v1 = v[i];	// i번째 원소를 반환
int v1 = v[i];	// i번째 원소를 반환
int v_front = v.front();	// front([0]) 원소를 반환
int v_back = v.back();		// back([size-1]) 원소를 반환
```

<br>

### 기타

```c++
bool isEmpty = v.empty();	// vector가 비었으면 true를 반환
int size = v.size();		// vector의 원소 개수를 반환
```

<br>

<br>

## Queue

### 생성

```c++
queue<int> q;
```

<br>

### 추가 & 삭제

```c++
q.push(1);	// queue의 back에 원소 추가
q.pop();	// queue의 front쪽 원소를 삭제
```

<br>

### 탐색

```c++
int front = q.front();	// queue의 front쪽 원소를 반환
int back = q.back();	// queue의 back쪽 원소를 반환
```

<br>

### 기타

```c++
bool isEmpty = q.empty();	// queue가 비었으면 true를 반환
int size = q.size();		// queue의 size를 반환
```

<br>

<br>

