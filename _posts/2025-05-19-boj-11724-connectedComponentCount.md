---
title: "백준 11724 | 연결 요소의 개수"
date: 2025-05-19
categories: ["Algorithm", "백준"]
tags: ["그래프 탐색", "깊이 우선 탐색", "DFS"]
---

# 📝 문제 설명

방향 없는 그래프가 주어졌을 때, 연결 요소(Connected Component)의 개수를 구하는 프로그램을 작성하시오.  
   
1. ***입력***
   - 첫째 줄에서 정점의 개수 `N`과 간선의 개수 `M`을 함께 입력 받는다.
   - 둘째 줄 부터 `M`개의 줄에 간선의 양 끝점 `u`와 `v`가 주어진다.
2. ***출력***
   - 첫째 줄에 연결 요소(Connected Component)의 개수를 춣력한다.

[문제 링크](https://www.acmicpc.net/problem/11724)

---

# 💡 아이디어

- 방향 없는 그래프란? 간선이 양방향으로 연결된 그래프라는 뜻이다.
  - 일반적인 DFS로 구현 가능
- 바이러스 문제의 `count`와 이 문제의 `componentCount`의 차이점

| 문제 이름 | 세는 대상    | 탐색 반복 여부                | 변수 선언 |
| :-----: | :--------: | :-----------------------: | :--------: |
| 연결 요소의 개수 | (연결) 그룹 개수    | DFS 탐색 시작이 여러 번<br>(그 시작 횟수를 셈)           | 지역 변수    |
| 바이러스  | 감염된 정점 개수 | DFS 탐색 자체는 한 번이지만<br>한번의 탐색 내부에서 계속 세야 함 | 전역 변수    |

- 바이러스 문제와 연결 요소의 개수 문제 전체 풀이 비교
 
|     항목    |      연결 요소의 개수      |              바이러스             |
| :-------: | :-----------------: | :---------------------------: |
| DFS 호출 횟수 | 최대 N번 (방문 안 한 정점마다) |       1번 (오직 정점 1에서만 시작)       |
|   탐색 범위   |        전체 그래프       |          1번과 연결된 컴퓨터만         |
|     목적    |     모든 연결 요소 세기     |       1번을 통해 감염된 컴퓨터 세기       |
|   시간 복잡도  |    `O(N + M)` 확정    | 평균 `O(K)` 최악 기준 `O(N + M)` |
| DFS 시작 위치 |       모든 정점 시도      |            1번 정점 고정           |


---

# 🛠 코드

```java
import java.io.*;
import java.util.*;

public class Main {
    // 정점(노드)의 수, 간선의 수
    static int N, M;
    // 그래프를 인접 리스트 형태로 표현
    static List<Integer>[] adj;
    // 정점 방문 여부를 저장하는 배열
    static boolean[] visited;

    public static void main(String[] args) throws IOException {
        // 1단계: 입력 받기
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());

        N = Integer.parseInt(st.nextToken()); // 정점의 수 입력
        M = Integer.parseInt(st.nextToken()); // 간선의 수 입력

        // 2단계: 인접 리스트 초기화
        adj = new ArrayList[N + 1]; // 정점 번호가 1부터 시작하므로 N+1 크기로 생성
        for (int i = 1; i <= N; i++) {
            adj[i] = new ArrayList<>(); // 각 정점에 대해 빈 리스트 생성
        }

        // 3단계: 간선 정보를 읽어 그래프를 구성
        for (int i = 0; i < M; i++) {
            st = new StringTokenizer(br.readLine());
            int u = Integer.parseInt(st.nextToken()); // 간선의 한쪽 정점
            int v = Integer.parseInt(st.nextToken()); // 간선의 다른쪽 정점

            adj[u].add(v); // u -> v 간선 추가
            adj[v].add(u); // v -> u 간선 추가 (무방향 그래프 = 간선 양방향 연결)
        }

        // 4단계: 연결 요소의 개수 세기
        visited = new boolean[N + 1]; // 방문 여부 배열 초기화
        int componentCount = 0; // 연결 요소 카운터 초기화

        for (int i = 1; i <= N; i++) { // 모든 정점을 순회하며
            if (!visited[i]) { // 아직 방문하지 않은 정점이라면
                dfs(i); // DFS로 연결된 모든 정점 방문
                componentCount++; // 새로운 연결 요소 발견 시 카운트 증가 (방문하지 않은 정점이 있다 = 인접 요소가 아닌 노드가 있다)
            }
        }

        // 5단계: 결과 출력
        System.out.println(componentCount);
    }

    // DFS로 연결요소 탐색 구현
    static void dfs(int n) {
        visited[n] = true; // 현재 정점 방문 표시

        // 인접한 모든 정점을 탐색
        for (int nxt : adj[n]) { // 현재 정점과 연결된 각 정점에 대해
            if (!visited[nxt]) { // 아직 방문하지 않았다면
                dfs(nxt); // 재귀적으로 방문
            }
        }
    }
}

```

---

# ⏱ 시간 복잡도

- **입력 처리**:  
  `M`개의 간선정보를 읽고 양방향 인접 리스트에 저장 → `O(M)`

- **그래프 탐색**:  
  DFS는 정점 수 `N`과 간선 수 `M`에 비례하여 모든 정점과 간선을 한번씩 탐색 → `O(N + M)`

- **출력**:  
  연결 요소 개수 1개 출력 → `O(1)`

> 👉 **총 시간 복잡도**:  
> `O(N + M)`


---

# ✏️ 느낀 점

- 그래프 탐색을 하면서, 단순 출력이 아닌 카운팅을 할 때- 뭘, 어떤 방식으로 카운팅 해야하는지 좀 깊이 고민하면 더 자연스럽게 풀 수 있을 것 같다. 다음 문제는 문제 읽으면서 표 먼저 입력해 봐야지.
