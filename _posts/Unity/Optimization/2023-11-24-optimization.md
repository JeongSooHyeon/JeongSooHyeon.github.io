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

# Unity 최적화

## GetComponent(), Find() 메서드 사용 줄이기

객체 참조가 필요할 때는 Awake, Start 메서드에서 객체들을 캐싱하여 사용
- Inspector창에 직접 연결하는 건 ?
- 나는 작은 프로젝트니까 그냥 함수 쓰는 게 낫겠다