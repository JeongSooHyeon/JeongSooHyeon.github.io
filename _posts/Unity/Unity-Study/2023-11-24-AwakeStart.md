---
title:  "[Unity 공부] Awake()와 Start(), OnEnable()"

categories:
  - UnityStudy
tags:
  - [Programming, C#, Unity]

toc: true
toc_sticky: true
 
date: 2023-11-24
last_modified_at: 2023-11-24
---
<br>

# Unity 이벤트 함수 호출 순서

# 'Start()'와 'Awake()'

## 공통점
둘 다 MonoBehaviour 클래스가 초기화 될 때 호출되는 이벤트 함수.<br>
## 차이점 
1. Awake가 먼저 호출 되고 Start가 호출
2. Awake는 Start와 달리 스크립트가 비활성화 상태일 때도 호출 

## Awake() 사용 방법
Awake는 스크립트와 연결된 GameObejct가 인스턴스화 되거나, 스크립트가 처음 로드될 때 호출됨.<br>

각 스크립트에서 딱 한 번만 호출되고 다른 개체가 초기화된 후에만 호출.<br>
-> 다른 컴포넌트에 대한 참조를 만들어야 한다면 Awake 안에서 하는 것이 안전

모든 개체의 Awake 함수는 무작위로 호출됨(순서 알 수 없음)

## Start() 사용 방법
Start는 컴포넌트가 활성화될 때 Awake와 Update 함수 호출 사이에 한 번 호출됨.<br>

스크립트가 비활성화 된 상태에서 개체가 생성되면 Awake는 호출되지만 Start는 호출되지 않는다.

## Start를 지연 실행하는 방법
1. IEnumerator Start() ...로 사용
    - 다른 개체들의 초기화가 늦어지거나 빨라지면 시간 낭비
2. 필요할 때까지 스크립트를 비활성화 상태로 유지하여 Start 실행 수동으로 지연
    - 안전하고 낭비가 없는 방법

## Start와 Awake를 같이 사용하면 초기화 작업을 두 단계로 분리할 수 있다.
스크립트 자체의 초기화(ex : 컴포넌트 생성 시 참조와 변수를 초기화하는 것들)를 Awake에서 완료하고 다른 스크립트의 Start에서 해당 데이터에 접근하여 사용하도록 하여 초기화가 되지 않은 데이터에 접근하는 오류 방지

# OnEnable()

Start보다 먼저 호출.

Start는 컴포넌트가 생성되고 최초 활성화될 때 한 번만 호출, OnEnable은 컴포넌트 또는 연결된 객체가 활성화될 때마다 호출됨.

일반적으로 한 번만 발생해야 하는 초기화 작업에 사용하기에는 부적합 XXX<br>
-> 객체 풀링에서 객체가 비활성화되었다가 활성화될 때마다 초기화를 진행할 때 사용하면 좋다.


