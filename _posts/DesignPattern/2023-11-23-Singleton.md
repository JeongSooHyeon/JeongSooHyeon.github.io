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

Manager류의 클래스, 오디오, Event 등 디바이스 I/O를 다루는 곳에서 자주 쓰임.

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
race condition : 다중 프로그래밍 시스템이나 다중 처리기 시스템에서 두 명령어가 동시에 같은 기억 장소를 엑세스할 대 그들 사이의 경쟁에 의해 수행 결과를 예측할 수 없게 되는 것.
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


## 싱글턴 패턴 예시

1\. Audio를 다루기 위한 Manager 게임 오브젝트 생성

공이 떨어지면서 바닥에 부딪히면 소리 재생 후 파괴

공 프리팹에 MyAudioPlay.cs 추가
```c#
public class MyAudioPlay : MonoBehaviour {
  public AudioClip clip;

  void OnCollisionEnter(Collision collision){
    AudioManager.Instance().Play(clip);
    Destroy(this.gameObject);
  }
}
```
- 싱글톤 객체의 함수 호출 후 Destroy하면 됨

2\. 싱글톤 패턴의 객체를 만들어 전역 변수로 사용

DontDestroyOnLoad 사용

3\. 남아있는 싱글톤 객체를 다른 Scene에서 사용하기

**AddScene()** : 새로운 씬을 현재 씬에 추가적으로 로드
```c#
SceneManager.LoadScene("씬 이름", LoadSceneMode.Addtive); // Single은 기존 씬 닫음
```
- 예를 들어 말풍선 모양으로 상상하는 씬 같은 느낌

**RemoveScene()** : 현재 활성화된 씬을 unload
```c#
SceneManager.UnloadSceneAsync("씬 이름");
SceneManager.LoadScene(SceneManager.GetActiveScene().buildIndex);
```

추가된 씬에서 싱글톤 객체 사용 가능, 기존 씬의 다른 오브젝트에 할당돼 있는 스크립트도 사용 가능

