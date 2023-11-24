---
title:  "[Unity 최적화]"

categories:
  - UnityOptimization
tags:
  - [Programming, C#, Unity]

toc: true
toc_sticky: true
 
date: 2023-11-24
last_modified_at: 2023-11-24
---
<br>

## 1. GetComponent(), Find() 메서드 사용 줄이기

객체 참조가 필요할 때는 Awake, Start 메서드에서 객체들을 캐싱하여 사용<br>

Inspector창에 직접 연결하는 건 ?<br>
-> 코드가 유연하지 못하고 유지보수 측면에 제한이 있음

결론 : 작은 게임이라면 `GetComponent()`사용해도 오버헤드가 덜함. 대규모 게임에서는 변수를 직접 연결하는 게 더 효율적일 수 있음