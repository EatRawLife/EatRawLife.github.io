---
title: 그래프, DFS , BFS
date: 2025-07-15
permalink: /posts/2025/07/15/3
tags:
---

"코딩 테스트 준비는 DP, 그리디, DFS,BFS 골드정도만 풀고 유기해!"

라는 말은 어디선가 들어본 기억이난다.. 그만큼 이것이 중요한 것 아닐까 싶다.

과거 배웠던 내용을 참고하여 DFS, BFS, 그리고 그래프에 대해서 공부해 볼까한다.

이는 2-2에 배웠던 알고리즘의 내용을 참고한다.

## 그래프

버텍스 셋 + 엣지 셋으로 구성된 구조.

아래 DFS에서 self loof, multi edge가 없는 것을 가정한다. 이를 simple graph라고 한다. (이런 케이스의 문제는?)

엣지하나로 이어진 버텍스들을 adjecent(인접), (directed의 경우는 연결된한쪽은 인접하지 않음)

방향성이 있는 경우 ordered, 없는 경우 unordered, (여기는 unordered)

버텍스로 들어오는 엣지 수를 in-degree, 나가는 엣지 수를 out-degree

그래프의 엣지, 버텍스의 부분집합으로된 그래프 > 서브그래프.

어떤 두 버텍스간의 walk > 두버텍스 간 경로에 만나는 버텍스를 순서대로 써둔 것.
(심플그래프 얘기할거니 두 버텍스 사이 간선은 하나, 버텍스만 차례로 쓰면 되긋지). 그때 같은 버텍스가 두번이상 반복 안되는 walk를 **path**

시작, 마지막 버텍스가 같은 path를 cycle이라함.

두 버텍스가 reachable하다 두 버텍스 사이에 path가 존재하면 된다.

## DFS

**explore 프로시저**

인풋의 그래프의 버텍스에서 rechable 모든 버텍스를 찾는 프로시저.

**과정**

1. 현재 방문중인 버텍스 true
2. previsit(다음 버텍스 방문전 할일하기)
3. 현재 방문한 버텍스와 adjacent한 방문하지 않은(visited배열등을 이용해 확인) 모든 버텍스를 explore(재귀)
4. postvisit(방문 후해야할 일 하기.)

만약 explore를 실행한다면 트리같은 형태가 나올 것이다. (DFS 트리)

그래프는 서로 연결된 버텍스가 존재하지 않을 수 있다. 따라서 그래프상 모든 버텍스를 한번 씩 방문할때까지 explore를 하는 것을 DFS라고 한다.

**time complexity**

G(n,m)

비지티드 배열 초기화 O(n)

모든 버텍스 explore O(m) (버텍스마다 방문했는지 확인 > 2m번 확인)

총 O(n+m)

**사용**

경로탐색

모든 노드 방문

하지만 최단거리 보장 x

## BFS

참고한 영상 : https://www.youtube.com/watch?v=xlVX7dXLS64

가중치가 없는 그래프의 모든 버텍스로의 최단 거리를 찾는 문제에 답할 수 있음

특정 버텍스로 부터의 다른 버텍스의 최단 거리는 시작하는 버텍스를 루트로 한상태로 트리처럼 그린다면 명확하다. 목적지의 버텍스의 깊이가 최단거리가 될 것이다.

BFS는 이렇게 트리로 그려진 상태에서 n 깊이 에 있는 것을 차례로 탐색한다.

따라서 BFS로 구한 최단 거리는, 목적지 버텍스를 방문하자 말자, 그때 탐색하던 깊이에 해당할 거다.

최단거리로 가는 방법은 여러개가 있을 수 있을 것이다. 따라서 bfs로의 오더링도 여러개 일 수 있다.

**implement**

0. 모두 false인 boolean list를 제작.(방문한 버텍스를 체크하기 위함)
1. 탐색해야할 버텍스들을 저장할 큐 만들기
2. 큐가 비지 않았다면 하나 큐에서 pop
3. 마크 되지 않았다면 방문 후 마크.
4. 방문하지 않은 자식을 queue에 삽입.

**사용**

가중치 없는 그래프의 최단거리 구하기

경로탐색

모든 노드 방문

하지만 큰 메모리 공간 필요.

![](/postPic/25-07-15/2.png)

## 구현 및 문제 풀기

['백준 1260번: dfs와 bfs'](https://www.acmicpc.net/problem/1260)

일단 리처블 하지 않은 버텍스가 있다면 탐색하지 않는 것으로 생각하고 구현해보았다.

```c++
#include <iostream>
#include <sstream>
#include <deque>
int n;
int m;
int start;
int v1[1001];
int v2[1001];
int mat[1001][1001];
int bfsvisit[1001];
int dfsvisit[1001];
std::ostringstream outputstring;

void dfs(int vertex){
  dfsvisit[vertex] = 1;
  outputstring << vertex << ' ';

  for(int i = 1; i <= n; i++){
    if(mat[vertex][i] == 1 && dfsvisit[i] == 0)
    dfs(i); 
  }
}

void bfs(int start) {
  std::deque<int> q;
  q.push_front(start);
  bfsvisit[start] = 1;

  while(!q.empty()){
    int vertex = q.back();
    q.pop_back();
  
    outputstring << vertex << ' ';

    for(int i = 1; i<=n; i++) {
      if(mat[vertex][i] == 1 && bfsvisit[i]==0){
        q.push_front(i);
        bfsvisit[i] = 1;//탐색전에, 미리 큐에 같은 버텍스 삽입 방지
      }
    }
  }
}

int main(){
  std::cin >> n;
  std::cin >> m;
  std::cin >> start;

  for(int i = 0; i< m; i++){
    int st;
    int en;

    std::cin >> st;
    std::cin >> en;

    mat[st][en] = 1;
    mat[en][st] = 1;
  }
  // 표준입력 받아서 그래프 만들기

  dfs(start);
  outputstring << '\n';
  bfs(start);

  std::cout << outputstring.str() << std::flush;
  //출력
}
//문제 생김. 깊이 n에 4로 가는 경로가 두개라면, 4가 다 추가 될 것. 왜냐. 이는 이전
//자식을 큐에 추가할때는 이미 넣은거 인지 확인 안할테니.
```

실수한 부분:

zero based numbering인가 one based numbering인가 조심. 위 문제의 경우에는 one based라서 배열을 인덱스 0부터 받으면 잘못된 결과가 나왔따.

재귀함수로 dfs를 구현. 따라서 특정 기능이 두번 구현 되지 않도록 조심.
