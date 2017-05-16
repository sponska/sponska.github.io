---
layout: post
title: 다양한 예제로 학습하는 데이터 구조와 알고리즘 for Java - 1
category: Algorithm
tags: Algorithm
date: 2017-04-26
author: SiHun
---

* content
{:toc}

다양한 예제로 학습하는 데이터 구조와 알고리즘 for Java - 1
==========================================================

### 1.1 변수

변수란 프로그래머에 의해 이름을 할당받은 메모리 영역이다. 이 메모리영역은 프로그램에게 사용될 정보를 저장하기 위해 사용된다.

### 1.2 데이터형

* 시스템 정의 데이터형(원시 데이터형) - Primitive Data Type
* 사용자 정의 데이터형 - Reference Data Type

### 1.3 데이터 구조

1)선형 데이터 구조:항목들이 순차적 차례에 따라 접근되지만 순차적으로 저장되어야 하는 것은 아니다.

예:연결 리스트, 스택, 큐

2)비선형 데이터 구조:항목들이 비선형의 차례로 저장/접근 된다.

예:트리, 그래프

### 1.4 추상 데이터형(ADT - Abstract Data Type)
ADT 의 구성은 두가지로 되어있다.

1.데이터의 선언

2.연산의 선언

### 1.5 알고리즘이란 무엇인가?

알고리즘은 주어진 문제를 풀기 위한 단계별 지시사항들이다.

### 1.6 왜 알고리즘을 분석 하는가?

알고리즘 분석은 시간과 공간적으로 어느 것이 가장 효율적인지 알 수 있게 해준다.

### 1.7 알고리즘 정렬의 목적

알고리즘을 비교하기위해

### 1.8 수행 시간 분석이란 무엇인가?

문제의 크기(입력의 크기)가 증가함에 따라 처리 시간이 얼마나 증가하는지를 분석하는 것이다.

### 1.9 어떻게 알고리즘을 비교하는가?

주어진 알고리즘의 수행 시간을 입력 크기 n에 따른 함수 f(n) 으로 표현하고 수행시간에 따라 이 함수들을 비교한다.

### 1.10 증가율이란 무엇인가?

함수의 연산에서 입력하는 값에 따라 수행 시간이 증가하는데, 이 비율을 증가율이라고 한다.

### 1.11 많이 사용되는 증가율

시간복잡도 | 명칭 
----- | ----- 
1 | 상수형 
log n| 로그형
n| 선형
nlog n| 선형로그형
n²| 2차형
n³| 3차형
2ⁿ| 지수형

### 1.12 분석의 종류

* 최악의 경우

* 최선의 경우

* 평균의 경우

### 1.13 점근적 표기법

* 빅-오 표기법 (최악의 경우)

* 오메가 표기법 (최선의 경우)

* 세타 표기법 (평균의 경우)

### 1.14 빅-오 표기법
f(n) = O(g(n)) 

n의 값이 클 때 f(n)에서 최대의 증가율이 g(n)이라는것