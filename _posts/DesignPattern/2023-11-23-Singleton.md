---
title:  "[디자인패턴] 싱글턴 패턴(Singleton Pattern)"

categories:
  - DesignPattern
tags:
  - [Programming, DesignPattern]

toc: true
toc_sticky: false
 
date: 2023-11-23
last_modified_at: 2023-11-23
---
<br>

# 싱글턴 패턴(Singleton Pattern)

Singleton Pattern : <u>오직 한 개의 클래스 인스턴스</u>만을 갖도록 보장하고, 이에 대한 <u>전역적인 접근점을 제공</u>

## Unity 안에서의 싱글턴 패턴 객체

1. 같은 씬 안에서의 데이터 공유
2. 서로 다른 씬들간의 데이터 공유
- Scene에 **빈 객체**를 생성한 후, 오직 하나의 객체만 생성되도록 만들고, **DontDestroyOnLoad 메서드**를 호출하여 Scene 변경 시에도 Destroy를 막아주는 형태로 구현

## 싱글턴 패턴의 객체는 일종의 전역 변수이다.

전역 변수가 가지는 모든 장단점을 갖고 있음.
<br>
단점

1. 모든 곳에서 접근 가능하니, 객체의 변경 시점과 변경 주체를 알기 어려움.(모든 코드들을 다 뒤져야 알 수 있다.)
2. 여러 클래스와 Coupling 됨. 하나의 코드를 수정했을 때 싱글턴과 연결된 다양한 곳에서 문제 발생할 수 있음
3. 멀티 쓰레드의 환경에서 문제 발생. 모든 곳에서 접근 가능하기 때문에 race condition 발생, 그걸 피하기 위해 필연적으로 mutex lock과 unlock을 반복해서 걸게 되어 코드 전체적으로 Performance가 떨어지는 문제 발생

**참고**
race condition : 
mutex lock, unlock : 
Instance() 메서드 : 

## DontDestroyOnLoad(GameObject obj)

```c++
void Awake(){
  if(_instance == null){
    _instance = this;
    DontDestroyOnLoad(this.gameObject);
  }
  else{
    if(this != _instance){
      Destroy(this.gameObject);
    }
  }
}
```

- `DontDestroyOnLoad(this.gameObject);` : 이 객체를 가진 Scene이 종료를 할 때 이 객체는 Destroy하지 말아라.
- Scene이 다시 넘어왔을 때 새로생긴 this가 기존 _instance랑 다르면 기존 거 남기고 새로 생긴 건 없애라

### DontDestroyOnLoad 주의

GameManager에 Scene 전환 스크립트를 넣으면 문제가 생김

## Additive Loading

기본적으로는 화면에는 1개의 Scene이 로드되고, 다른 Scene을 다시 로드하는 방식으로 Scene을 교체한다.<br>

Additive Scene은 기존 Scene에 Scene을 추가할 수도 있다.
- 예) 말풍선 모양으로 상상하는 Scene

## UnloadSceneAsync()

Scene 삭제
- 비동기식이므로 완료 시간 보장되지 않음
- 로드할 장면이 없으면 수행 불가. 예를 들어 단일 Scene 프로젝트에서는 이 정적 멤버를 사용할 수 없다.

## 싱글턴 패턴 예시

싱글턴 객체, DontDestroyOnLoad(), Additive Loading을 사용

- 기존 Scene에 새로운 Scene을 추가
- 새로 추가되는 Scene에는 GameManager(싱글턴 패턴)가 없지만 기존 Scene에 있는 DontDestroyOnLoad로 GameManager가 있기 때문에 사용 가능

