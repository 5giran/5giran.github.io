---
title: "백준 2178 | 미로 탐색"
date: 2025-05-21
categories: ["Algorithm", "백준"]
tags: ["그래프 탐색", "너비 우선 탐색", "BFS", "격자 그래프"]
---

# 📝 문제 설명

- N x M 크기의 배열로 표현되는 **미로**가 있다.
- **미로**에서 1은 이동할 수 있는 칸을 나타내고, 0은 이동할 수 없는 칸을 나타낸다.
- (1, 1)에서 출발하여 (N, M)의 위치로 이동 할 때, 지나야 하는 최소 칸 수를 구해라.
- 서로 인접한 칸 끼리만 이동할 수 있다.
   
1. ***입력***
   - 첫째 줄에는 두 정수 `N`과 `M`을 입력 받는다. (2 ≤ `N`, `M` ≤ 100)
   - 다음 `N`개의 줄에는 각각 `M`개의 정수를 입력받아 미로를 만든다.
   - 각각의 수들은 붙어서 입력 받는다.
2. ***출력***
   - 첫째 줄에 지나야 하는 최소 칸 수를 출력한다.  


[문제 링크](https://www.acmicpc.net/problem/2178)

---

# 💡 아이디어

### ✊ BFS 코어
1. 시작 점에서 출발
2. 주변 4칸을 살펴보고
3. 방문 할 수 있는 칸은 '다음에 방문할 곳' 리스트인 큐에 추가
4. 큐에서 다음 칸으로 이동
5. 목표에 도달할 때까지 반복

> BFS는 모든 방향으로 동시에, 한칸 씩 퍼지듯 나아가기에  
> 가장 먼저 도달한 경로가 가장 짧은 거리이다.  

---

# 🛠 코드

```java
import java.io.*;
import java.util.*;

public class Main {
    static int[][] grid;
    static boolean[][] visited;
    static int N, M;
    static int[][] directions = {
            {-1, 0},
            {1, 0},
            {0, -1},
            {0, 1}
    };

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());

        N = Integer.parseInt(st.nextToken());
        M = Integer.parseInt(st.nextToken());

        grid = new int[N][M];
        visited = new boolean[N][M];

        for (int row = 0; row < N; row++) {
            String line = br.readLine();
            for (int col = 0; col < M; col++) {
                grid[row][col] = line.charAt(col) - '0';
            }
        }

        // 시작 좌표에서 호출
        bfs(0, 0);
        // 0에서 시작했기에 -1
        System.out.println(grid[N - 1][M - 1]);

    }

    static void bfs (int row, int col) {
        Queue<int []> queue = new LinkedList<>();
        visited[row][col] = true; // 시작 지점 방문 처리
        queue.offer(new int[]{row, col}); // 시작 좌표 큐에 넣기

        while (!queue.isEmpty()) {
            int[] current = queue.poll(); // 현재 위치 꺼내기
            int curRow = current[0];
            int curCol = current[1];

            // 4방향 모두 탐색
            for (int[] dir : directions) {
                int nextRow = curRow + dir[0];
                int nextCol = curCol + dir[1];

                // 격자 범위 밖이면 건너뛰기
                if (nextRow < 0 || nextRow >= N || nextCol < 0 || nextCol >= M) continue;

                // 이동 가능하고 방문 안한 칸이면
                if (grid[nextRow][nextCol] == 1 && !visited[nextRow][nextCol]) {
                    visited[nextRow][nextCol] = true; // 방문 체크
                    grid[nextRow][nextCol] = grid[curRow][curCol] + 1;
                    queue.offer(new int[]{nextRow, nextCol}); // 다음 칸 큐에 추가
                }
            }
        }
    }

}

```

---

# 🧬 BFS 로직 설명

```java
static void bfs(int row, int col) {
    Queue<int[]> queue = new LinkedList<>();
    visited[row][col] = true;
    queue.offer(new int[]{row, col});
```
- BFS (row, col) 위치에서 시작한다.
- 방문 처리 후, 해당 위치를 큐에 넣는다.
```java
while (!queue.isEmpty()) {
    int[] current = queue.poll();
    int curRow = current[0];
    int curCol = current[1];
```
- 큐에서 하나씩 꺼내서 현재 위치로 설정한다.

---

# ✏️ 느낀 점

- 필터링 로직 없이도 탐색으로 필터링 되는 구조구나
