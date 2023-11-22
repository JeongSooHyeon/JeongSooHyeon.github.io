---
title:  "[디자인패턴] 컴포넌트 패턴(Component Pattern)"

categories:
  - DesignPattern
tags:
  - [Programming, DesignPattern]

toc: true
toc_sticky: false
 
date: 2023-11-22
last_modified_at: 2023-11-22
---
<br>

# 컴포넌트 패턴(Component Pattern)

Component Pattern : 컴포넌트를 만들어 한 개체가 여러 분야로 서로 <u>커플링 없이</u> 다룰 수 있게 한다.

- 로직을 **기능별**로 컴포넌트화 하는 것
- **클래스를 분리**하고 **가독성**을 올리고 **Decoupling** 시키는 것

- Unity의 GameObject는 Component Pattern이 잘 반영되어 설계된 클래스
    - GetComponent 메서드로 각각의 Decoupling된 하위 컴포넌트들을 불러 사용
- Unity는 새로운 행동을 추가하는 composition(합성) 구조를 채용.
    - 상속을 통해 확장하는 오브젝트 지향 클래스 계층과 다름

## 컴포넌트 패턴 예시

회전하며 이동하는 GameObject 만들기
- 0.3f마다 회전하는 스크립트 Rotate.cs와 0.5f마다 이동하는 스크립트 Translate.cs를 각각 따로 생성하고 GameObject에 추가한다.
