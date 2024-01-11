---
title:  "프로그래머스/프로세스"

categories:
  - CoTe_StackQueue
tags:
  - [Programming, CodingTest]

toc: true
toc_sticky: false
 
date: 2024-01-10
last_modified_at: 2024-01-10
---
<br>

## 프로세스

프로그래머스>코딩테스트 연습>스택/큐>프로세스

### 내가 한 풀이
```c++
int solution(vector<int> priorities, int location) {
    int answer = 0;

    int first;   // 대기 중인 첫번째 프로세스
    int pri;   // 가장 높은 우선순위 

    while (true) {
        first = priorities.front();
        pri = priorities[0];

        // 가장 높은 우선순위 찾기
        for (int i = 1; i < priorities.size(); i++) {
            if (pri < priorities[i]) {
                pri = priorities[i];
            }
        }

        // 배열 옮기기
        for (int i = 0; i < priorities.size() - 1; i++) {
            priorities[i] = priorities[i + 1];
        }

        // 현재 프로세스보다 더 높은 프로세스가 있으면
        if (first < pri) {   
            
            priorities[priorities.size() - 1] = first;   // 다시 뒤로 넣어줌
        }
        else {  // 없으면 실행
            answer++;
            if (location == 0)
                break;
            priorities.pop_back();  
        }
      

        location--;
        if (location < 0) {
            location = priorities.size() - 1;
        }
    }
    return answer;
}
```

- 아쉬운 점 
    - 중첩 반복문
    - 비효율적인 for문
    - 라이브러리 사용 미숙

### 다른 사람 풀이

```c++
int solution(vector<int> priorities, int location) {
    queue<int> printer;  // queue를 새로 할당해서 사용
    vector<int> sorted;

    // index를 queue에 넣음
    for(int i=0; i<priorites.size(); i++){
        printer.push(i);
    }

    while(!printer.empty()){
        int now_index = printer.front();
        printer.pop();

        // 가장 높은 우선 순위 프로세스가 아니면
        if(priorities[now_index] != 
            *max_element(priorities.begin(), priorities.end())){
                printer.push(now_index);    // 뒤에 다시 넣어
            }
        // 맞으면
        else{
            sorted.push_back(now_index);
            priorites[now_index] = 0;   // 우선순위 0으로 
        }
    }

    for(int i=0; i<sorted.size(); i++){
        if(sorted[i] == location)
            return i+1;
    }
}
```

- 반복문 하나씩만 씀
- STL을 이용함, **queue, max_element()**
- index를 이용해 간단한 풀이

## 후기

vector가 아닌 queue로 바꿔서 풀려고 했는데 임의접근연산자 지원하지 않아서 vector로 하니 원소를 추가하고 삭제하는 비효율적인 코드를 넣어야 했다. 또 location을 확인해야 하는데 억지로 끼워맞추듯 location을 직접 움직였다.

## 개선할 점 

1. 주어진 컨테이너가 비효율적이라면 다른 컨테이너를 추가적으로 사용
2. index를 사용
3. STL 사용
d