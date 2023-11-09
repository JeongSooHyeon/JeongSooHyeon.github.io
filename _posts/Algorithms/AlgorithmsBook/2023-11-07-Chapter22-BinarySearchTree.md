---
title:  "[알고리즘 문제 해결 전략] Chapter22 이진 검색 트리(Binary Search Tree)"

categories:
  - Algorithm
tags:
  - [Programming, Cpp, Algorithm]

toc: true
toc_sticky: false
 
date: 2023-11-07
last_modified_at: 2023-11-07
---
<br>

# 이진 검색 트리
## 22.1 도입

트리는 현실 세계의 `계층적 구조`를 표현하는 것 외에도 다양한 용도로 사용됨, 그중 대표적인 것이 `검색 트리(search tree)`이다.

**Search Tree**
- 연결 리스트나 큐처럼 자료들을 담는 컨테이너
- 자료들을 일정한 순서에 따라 정렬한 상태로 저장
- 원소의 *추가와 삭제, 특정 원소의 존재 여부 확인* 등의 다양한 연산을 빠르게 수행
- 사용 예시)
    - 이미 가입한 사용자들의 주민등록번호를 저장해 두고 특정 사용자가 이미 가입되었나 찾기
    - 모든 학생들의 시험 점수를 저장해 놓고, 나보다 1등 위인 사람과 1등 아래인 사람 찾기

검색 트리 중 가장 흔하게 사용되는 것이 `이진 검색 트리(binary search tree)`와 그 변종들

## 22.2 이진 검색 트리의 정의와 조작

### 정의

**이진 트리란?**

각 노드가 왼쪽과 오른쪽, 최대 두 개의 자식 노드만을 가질 수 있는 트리
- 자식 노드들의 배열 대신 두 개의 포인터 left, right를 담는 객체로 구현

이진 탐색(binary search)에서 아이디어를 가져와서 만든 트리
- n개의 원소 중에서 원하는 값을 찾는데, 매번 후보의 수를 절반씩 줄여갈 수 있다면 O(lgN) 시간에 찾을 수 있다.

1. 이진 검색 트리의 각 노드는 하나의 원소를 자고 있다. 
2. 각 노드의 왼쪽 서브트리에는 해당 노드의 원소보다 작은 원소를 가진 노드들이,
3. 오른쪽 서브트리에는 보다 큰 원소를 가진 노드들이 들어간다.

### 순회

이진 검색 트리를 *중위 순회*하면 크기 순서로 정렬된 원소의 목록을 얻을 수 있다.<br>
-> 집합에 포함된 최대 원소나 최소 원소를 쉽게 얻을 수 있다.

### 자료의 검색

이진 검색 트리에서는 아주 간단하게 특정 원소가 존재하는지 확인할 수 있다.

### 조작

이진 검색 트리가 진가를 드러내는 곳은 집합에 원소를 추가, 삭제하는 조작 연산을 할 때 !
- 추가 : 이진 검색 트리에는 선형적인 구조의 제약이 없기 때문에, 새 원소가 들어갈 위치를 찾고 거기에 노드를 추가하기만 하면 끝
- 삭제 : 가장 까다로운 연산, 간편한 방법으로는 `합치기` 연산을 구현하는 것
    - 노드 t를 지울 거라면, t의 두 서브트리를 합친 새로운 트리를 만든 뒤 이 트리를 t를 루트로 하는 서브트리와 바꿔치기

### X보다 작은 원소의 수 찾기 / K번째 원소 찾기

직접 이진 검색 트리를 구현해야 함

## 22.3 시간 복잡도 분석과 균형 잡힌 이진 검색 트리

이진 검색 트리에 대한 모든 연산은 모두 <u>루트에서부터 한 단계씩 트리를 내려가며 재귀 호출</u>을 통해 수행<br>
-> *<u>최대 재귀 호출의 횟수는 트리의 높이 h와 같다.</u>*

이진 검색 트리의 높이는 자료들이 어떤 순서로 추가되고 삭제되느냐에 따라 크게 달라짐
- 1부터 N까지의 숫자들을 순서대로 추가한다면, 트리의 높이는 N-1이 됨, 이런 트리를 한쪽으로 '기울어졌다'(skewed)고 함, 연결 리스트를 쓰는 것과 마찬가지

트리의 높이가 1 높아질 때마다 트리에 들어갈 수 있는 원소의 수가 대력 두 배 늘어난다는 점을 고려하면 <u>트리의 최소 높이는 O(lgN)</u>이 됨

이와 같은 단점을 개선한 변종을 `균형 잡힌 이진 검색 트리(balanced binary search tree)`라고 한다.
- 트리의 구조에 추가적인 제약을 정하고, 이를 만족되도록 노드들을 옮겨서 트리의 높이가 항상 O(lgN)이 되도록 유지
- 대표적인 예로 `레드-블랙 트리(red-black tree)`가 있다.

## 22.4 문제 : 너드인가, 너드가 아닌가? 2 (문제 ID : NERD2, 난이도:중)

진정한 프로그래밍 너드를 뽑기, 너드 지수가 높은 사람 결정

너드 지수 
1. 알고스팟 온라인 채점 시스템에서 푼 문제의 수 p
2. 밤 새면서 지금까지 끓여먹은 라면 그릇 수 q

대회에 참가할 수 있는 사람의 수는 새로운 사람이 참가 신청을 할 때마다 계속 바뀜

```
시간 및 메모리 제한 : 2초 안에 실행, 64MB 이하의 메모리를 사용

입력 : 입력의 첫 줄에는 테스트 케이스의 수 C(1<=C<=50), 각 테스트 케이스의 첫 줄에는 참가 신청한 사람들의 수 N(1<=N<=50000)이 주어짐.
 그 후 N줄에 각 2개의 정수로 각 사람의 문제 수 Pi와 라면 그릇 수 Qi가 참가 신청한 순서대로 주어짐(0<=Pi, Qi<=10000).
  두 사람의 문제 수나 라면 그릇 수가 같은 경우는 없다고 가정해도 좋다.
입력의 ㅇㅇ이 많으므로 가능한 한 빠른 입력 함수를 사용하는 것이 좋다.

출력 : 각 사람이 참가 신청을 할 때마다 대회 참가 자격이 되는 사람의 수를 계산한 뒤,
 각 테스트 케이스마다 그 합을 출력
```

## 22.5 풀이 : 너드인가, 너드가 아닌가? 2

준비
1. 입력이 두 개라는 점을 이용해 2차원 평면에 입력을 그려 보는 전략 사용
2. 한 점 a가 다른 점 b보다 오른쪽 위에 있을 때, a가 b를 지배한다
3. **결론 : 이 문제는 새 점이 하나 추가될 때마다 다른 점에 지배당하지 않는 점들의 수를 계산**

문제 풀이
1. 새 점이 하나 찍힐 때마다 기존 점들 중 이 점을 지배하는 점이 있는지 우선 확인
    - 있다면 : 새 점 무시
    - 없다면 : 기존 점들 중 새 점이 지배하는 점들 지우기
2. **핵심 : 지배당하지 않는 점들만 모아서 x좌표가 증가하는 순서대로 정렬해 보면, y좌표는 항상 감소**
3. 새 점이 주어졌을 때, 바로 오른쪽에 있는 점만을 확인하면 지배당하는지 판단 가능

자료구조 정하기 : 바로 오른쪽에 있는 점을 찾는 연산, 점의 추가와 삭제 등을 빠르게 할 수 있는 자료구조<br>
-> 이진 검색 트리(모든 연산 O(lgN) 시간)

**점들의 목록을 갖고 있을 때 이들 중 하나가 새로운 점을 지배하는지를 확인하는 알고리즘 구현**

STL의 균형 잡힌 이진 검색 트리 구현인 `map<int,int>`를 이용, `map<int,int>::lower_bound(x)`는 트리에 포함된 x 이상의 키 중 가장 작은 값을 돌려줌

```c++
// 현재 다른 점에 지배당하지 않는 점들의 목록 저장
// coords[x] = y
map<int,int> coords;
// 새로운 점 (x,y)가 기존의 다른 점들에 지배당하는지 확인
bool isDominated(int x, int y){
    // x보다 오른쪽에 있는 점 중 가장 왼쪽에 있는 점 찾기
    map<int,int>::iterator it = coords.lower_bound(x);

    // 그런 점이 없으면 (x,y)는 지배당하지 않음
    if(it == coords.end())
        return false;

    // 이 점은 x보다 오른쪽에 있는 점 중 가장 위에 있는 점이므로,
    // (x,y)가 어느 점에 지배되려면 이 점에도 지배되어야 함
    return y < it->second;
}
```

**새로운 점에 지배당하는 점들 모두 삭제, 문제 해결 구현**

바로 왼쪽에 있는 점에서부터 시작해 왼쪽으로 움직이면서 지배되는 점들 지워나가기

```c++
// 새로운 점(x,y)에 지배당하는 점들을 트리에서 지운다.
void removeDominated(int x, int y){
    map<int,int>::iterator it = coords.lower_bound(x);  

    // (x,y)보다 왼쪽에 있는는 점이 없다
    if(it == coords.begin())    
        return;
    --it;   // 내 왼쪽 점으로 이동

    // 반복문 불변식 : it는 (x,y)의 바로 왼쪽에 있는 점.
    while(true){
        // (x,y) 바로 왼쪽에 오는 점을 찾는다
        // 기저 사례 : it가 표시하는 점이 (x,y)에 지배되지 않는다면 곧장 종료
        if(it->second > y)
            break;
        
        // 기저 사례 : 이전 점이 더 없으므로 it만 지우고 종료
        if(it == coords.begin()){
            coords.erase(it);
            break;
        }

        // 이전 점으로 이터레이터를 하나 옮겨 놓고 it를 지운다
        else{
            map<int,int>::iterator jt = it;
            --jt;
            coords.erase(it);
            it = jt;
        }
    }
}
// 새 점 (x,y)가 추가되었을 때 coords 갱신
// 다른 점에 지배당하지 않는 점들의 개수 반환
int registered(int x, int y){
    // (x,y)가 이미 지배당하는 경우에는 그냥 (x,y)를 버린다.
    if(isDominated(x,y))
        return coords.size();

    // 기존에 있던 점 중 (x,y)에 지배당하는 점들을 지운다.
    removeDominated(x,y);
    coords[x] = y;  // 지배되지 않았기 때문에 점 갱신
    return coords.size();
}
```

## 22.6 균형 잡힌 이진 검색 트리 직접 구현하기 : 트립

표준 라이브러리의 이진 검색 트리는 대부부느 x보다 작은 원소의 수를 계산하거나 k번째 원소를 찾는 연산을 지원하지 않기 때문에, 이런 연산이 필요할 때는 직접 구현해야 함.<br>
균형 잡힌 이진 검색 트리를 구현해야 함, AVL 트리나 레드 블랙 트리 등은 구현이 매우 까다로움.<br>
구현이 간단한 이진 검색 트리인 트립(treap)을 구현한다.

### 트립의 정의

입력이 특정 순서로 주어질 때 그 성능이 떨어진다는 이진 검색 트리의 단점 해결.<b>
-> `랜덤화`된 이진 검색 트리<br>

트리의 형태가 원소들의 추가 순서에 따라 결정되지 않고 `난수`에 의해 임의대로 결정.<br>
-> 원소들의 추가, 삭제 순서에 상관 없이 트리 높이 기대치 항상 일정<br>

새 노드가 추가될 때마다 해당 노드에 우선순위 부여, 이는 난수를 통해 생성

**트립의 조건**
1. 이진 검색 트리의 조건 : 왼쪽 서브트리에 있는 노드들의 원소는 해당 노드의 원소보다 작고, 오른쪽 서브트리에 있는 노드들의 원소는 해당 노드의 원소보다 크다.
2. 힙의 조건 : 모든 노드의 우선순위는 자식 노드보다 크거나 같다.

### 트립의 구현

```c++
typedef int KeyType;

// 트립의 한 노드 저장
struct Node{
    // 노드에 저장된 원소
    KeyType key;
    // 이 노드의 우선순위
    // 이 노드를 루트로 하는 서브트리의 크기
    int priority, size;
    // 두 자식 노드의 포인터
    Node *left, *right;
    // 생성자에서 난수 우선순위를 생성하고, size와 left/right를 초기화
    Node(const KeyType& _key) : key(_key), priority(rand()), size(1), left(NULL), right(NULL){}
}

void setLeft(Node* newLeft) { left = newLeft; clacSize();}
void setRight(Node* newRight) { right = newRight; calcSize();}

// size 멤버를 갱신
void calcSize(){
    size = 1;
    if(left)    // NULL 판단
        size += left->size;
    if(right)
        size += right->size;
}
```

- size : k번째 원소 찾기 연산과 x보다 작은 원소를 세는 연산을 쉽게 구현할 수 있다.

### 노드의 추가와 '쪼개기' 연산

**노드 추가 알고리즘**

1. root와 node의 우선순위 확인
2. root의 우선순위가 더 높다면 node는 root 아래로 들어가야 함
    1. 두 노드의 원소를 비교하여 왼쪽, 오른쪽 서브트리 결정
    2. 재귀 호출로 해당 서브트리에 node 삽입
3. node의 우선순위가 root보다 높으면
    1. node가 기존에 있던 루트를 밀어내고 이 트리의 루트가 되어야 함
    2. 트리를 node가 가진 원소를 기준으로 쪼개서 기준보다 작은 원소만을 갖는 서브트리, 큰 원소만을 갖는 서브트리 하나를 만든다.
    3. node의 양쪽 서브트리로 두기

**쪼개기(split) 알고리즘**
root의 원소가 key보다 작은 경우
1. 오른쪽 서브트를 재귀 호출 적용해 해당 트리를 key 기준으로 쪼개기
2. 쪼갠 결과 중 key보다 작은 원소를 갖는 트리를 root의 오른쪽 자손으로 연결

**추가, 쪼개기 연산 구현**

```c++
typedef pair<Node*, Node*> NodePair;
// root를 루트로 하는 트립을 key 미만의 값과 이상의 값을 갖는
// 두 개의 트립으로 분리
NodePair split(Node* root, KeyType key){
    if(root == NULL)
        return NodePair(NULL, NULL);
    // 루트가 key 미만이면 오른쪽 서브트리를 쪼갠다.
    if(root->key < key){
        NodePair rs = split(root->right, key);
        root->setRight(rs.first);      
        return NodePair(root, rs.second);
    }
    // 루트가 key 이상이면 왼쪽 서브트리를 쪼갠다
    NodePair ls = split(root->left, key);
    root->setLeft(ls.second);
    return NodePair(ls.first, root);
}
// root를 루트로 하는 트립에 새 노드 node를 삽입한 뒤 결과 트립의 루트를 반환
Node* insert(Node* root, Node* node){
    if(root == NULL)
        return node;
    // node가 루트를 대체해야 한다. 해당 서브트리를 반으로 잘라
    // 각각의 자손으로 한다.
    if(root->priority < node->priority){
        NodePair splitted = split(root, node->key);
        node->setLeft(splitted.first);
        node->setRight(splitted.second);
        return node;
    }
    else if(node->key < root->key)
        root->setLeft(insert(root->left, node));
    else
        root->setRight(insert(root->right, node));
    return root;
}
```

```c++
// 새 값 value 추가
root = insert(root, new Node(value));
```

- insert() : root를 루트로 하는 서브트리에 node를 삽입한 뒤 새 트리의 루트를 반환
    - 새 노드를 삽입한 결과 트리의 류트가 바꼈을 수 있기 때문

### 노드의 삭제와 '합치기' 연산

**합치기 연산**

```c++
// a와 b가 두 개의 트립이고, max(a) < min(b)일 때 이 둘을 합친다
Node* merge(Node* a, Node* b){
    if(a == NULL)
        return b;
    if(b == NULL)
        return a;
    if(a->priotiry < b->priority){
        b->setLeft(merge(a, b->left));
        return b;
    }
    a->setRight(merge(a->right, b));
    return a;
}
// root를 루트로 하는 트립에서 key를 지우고 결과 트립의 루트를 반환
Node* erase(Node* root, KeyType key){
    if(root == NULL)
        return root;
    // root를 지우고 양 서브트리를 합친 뒤 반환
    if(root->key == key){
        Node* ret = merge(root->left, root->right);
        delete root;
        return ret;
    }
    if(key < root->key)
        root->setLeft(erase(root->left, key));
    else
        root->setRight(erase(root->right, key));
    return root;
}
```

- erase() : 새로운 트리의 루트를 반환, root가 지워질 경우 새 노드를 반환해야 함
- merge() : 내부에서 두 노드 중 하나가 NULL인지 확인하기 때문에 root가 한쪽 자손만 갖고 있는 경우, root가 자손이 없는 경우 등을 모두 별도의 예외처리 없이 처리 가능
    - 이진 검색 트리에서 노드의 삭제와 거의 동일. 두 서브트리를 합칠 때 어느 쪽이 루트가 되어야 하는지 우선순위를 통해 판단.

### k번째 원소 찾기

왼쪽 서브트리의 크기 : l

1. k<=l : k번째 노드는 왼쪽 서브트리에 속해 있다.
2. k=l+1 : 루트가 k번째 노드
3. k>l+1 : k번째 노드는 오른쪽 서브트리에서 k-l-1번째 노드가 됨

```c++
// root를 루트로 하는 트리 중에서 k번째 원소를 반환
Node* kth(Node* root, int k){
    // 왼쪽 서브트리의 크기를 우선 계산
    int leftSize = 0;
    if(root->left != NULL)
        leftSize = root->left->size;
    if(k <= leftSize)
        return kth(root->left, k);
    if(k == leftSize+1)
        return root;
    return kth(root->right, k-leftSize-1);
}
```
- 트립이 아니라 다른 종류의 이진 검색 트리를 구현하더라도 각 `서브트리의 크기`만 알 수 있으면 함수 사용 가능

### x보다 작은 원소 세기

특정 범위 [a,b)가 주어질 때 ,이 범위 안에 들어가는 원소들의 숫자 계산<br>
- 주어진 원소 x보다 작은 원소의 수를 반환하는 countLessThan(x)가 있으면 간단하게 구현 가능
- countLessThan(b) - countLessThan(a)로 표현 가능
- root >= x : 왼쪽 서브트리에 있는 원소들만 재귀적으로 세서 반환
- root < x : 왼쪽 서브트리는 전부 답에 들어감, 오른쪽 서브트리에서 재귀적으로 찾고 왼쪽 서브트리와 루트를 더한다.

```c++
// key보다 작은 키값의 수를 반환
int countLessThan(Node* root, KeyType key){
    if(root == NULL)
        return 0;
    if(root->key >= key)
        return countLessThan(root->left, key);
    int ls = (root->left ? root->left->size : 0);
    return ls + 1 + countLessThan(root->right, key);
}
```
- 이진 검색 트리이기만 하면 잘 동작


### 22.7 문제 : 삽입 정렬 뒤집기(문제 ID : INSERTION)

**삽입 정렬** : 정렬된 부분 배열을 유지하며, 이 배열에 새 원소를 삽입
```c++
void insertionSort(vector<int>& A){
    for(int i=0; i< A.size(); i++){
        // A[0..i-1]에 A[i]를 끼워넣는다.
        int j = i;
        while(j > 0 && A[j-1] > A[j]){
            // 불변식 a : A[j+1..i]의 모든 원소는 A[j]보다 크다.
            // 불변식 b : A[0..i] 구간은 A[j]를 제외하면 정렬되어 있다.
            swap(A[j-1], A[j]);
            --j;
        }
    }
}
```

삽입 정렬은 A[0...i-1]이 정렬된 배열일 때, A[i]를 적절한 위치를 만날 때까지 왼쪽으로 한 칸씩 움직임.<br>
각 위치에 있는 값들이 움직인 칸수가 주어질 때 원래 수열 찾아내라.

입력 : 테스트 케이스 수 C, 각 테스트 케이스의 첫 줄에는 원 배열의 길이 N(1<=N<=50000)이 주어짐, N개의 정수로 움직인 칸수 주어짐. 

출력 : 재구성한 A[]를 한 줄에 출력

### 풀이 : 삽입 정렬 뒤집기

1. 뒤에서부터 문제를 풀어 나간다
2. 마지막 숫자 A[N-1]이 왼쪽으로 몇 칸 움직였는지를 보면 A[N-1]에 어떤 숫자가 들어가야 할지 알 수 있다.
3. A[i]에 들어갈 수 있는 숫자들의 집합을 어떻게 저장할 것인가 ?
<br>
이 알고리즘을 구현하기 위해서는 k번째 원소가 무엇인지 찾고, 삭제하는 작업을 빠르게 수행할 수 있어야 함
-> 이진 검색 트리 !

```c++
// shifted[i]=A[i]가 왼쪽으로 몇 칸 움직이는가 ?
int n, shifted[50000];
int A[50000];
// n, shifted[]를 보고 A[]에 값을 저장
void solve(){
    // 1~N까지의 숫자를 모두 저장하는 트립을 만든다
    Node* candidates = NULL;
    for(int i=0; i<n; i++){
        candidates = insert(candidates, new Node(i+1));
    }
    // 뒤에서부터 A[]를 채워나간다.
    for(int i=n-1; i>=0; i--){
        // 후보 중 이 수보다 큰 수가 larger개 있다.
        int larger = shifted[i];
        Node* k = kth(candidates, i+1-larger);
        A[i] = k->key;
        candidates = erase(candidates, k->key);
    }
}
```