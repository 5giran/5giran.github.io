---
title: "백준 4963 | 섬의 개수"
date: 2025-05-21
categories: ["Algorithm", "백준"]
tags: ["그래프 탐색", "격자 그래프", "깊이 우선 탐색", "DFS", "격자 그래프", "플러드 필"]
---

# 📝 문제 설명

<img src="assets/images/250521_island.jpeg" alt="섬의 개수 예시 좌표" width="350">

>괜히 직접 그렸다가 정사각형이 아니게 되었네요  
>방금 예시 그림이 정사각형으로 보이는 마법을 걸었습니다 뿅 🪄
- 정사각형으로 이루어져 있는 섬과 바다 지도가 주어진다. 섬의 개수를 세는 프로그램을 작성하시오.
- **섬이란?**
  - 한 정사각형과 가로, 세로 또는 대각선으로 연결되어 있는 사각형은 걸어갈 수 있는 사각형이다.
  - 두 정사각형이 같은 섬에 있으려면, 한 정사각형에서 다른 정사각형으로 걸어서 갈 수 있는 경로가 있어야 한다.
   
1. ***입력***
   - 여러개의 테스트 케이스를 입력받는다.
   - 각 테스트 케이스의 첫째 줄에는 지도의 너비 `w`와 높이 `h`를 입력 받는다.
   - `w`와 `h`는 50보다 작거나 같은 양의 정수이다.
   - 둘째 줄부터 `h`개 줄에는 각각 `w`개의 1, 0을 입력받는다. (지도)
   - 입력의 마지막 줄에는 0이 두개 주어진다.
2. ***출력***
   - 각 테스트 케이스에 대하여, 섬의 개수를 출력한다.


[문제 링크](https://www.acmicpc.net/problem/4963)

---

# 💡 아이디어

### 🪴 그래프 탐색 질문 템플릿
1. ***뭘 세라고 했지?***
   - 섬(인접한 땅이 모인 덩어리)의 개수
2. ***이 문제에서 내가 세야 하는게 정점 수야, 그룹 수야, 거리야, 최댓값이야?***
   - 그룹 수
   - 1로 이루어진 연결 컴포넌트를 세는 문제
   - 각 DFS 탐색 한번에 섬 하나의 전체를 방문하므로, DFS를 시행한 횟수 = 섬의 개수이다.
3. ***이걸 세기 위해 탐색을 몇번이나 더 해야하지?***
   - 방문하지 않은 땅(1)이 존재할 때마다 탐색 시작
   - 따라서 DFS 탐색 횟수는 섬의 개수와 같다.
4. ***탐색이 진행되는 중에 카운트 변수를 더해줘야하나? 아니면 탐색이 끝나거나 시작될 때 더해줘야 하나?***
   - 탐색을 시작 할 떄 - DFS 로직 호출 직전에 카운팅을 해야한다.
   - 새로운 섬을 발견하면 바로 카운트

### 🪴 격자 이동 표현하기 (8방향으로 확장)

1. `directions[0]` = {-1,  0} → **위로 한 칸**
2. `directions[1]` = { 1,  0} → **아래로 한 칸**
3. `directions[2]` = { 0, -1} → **왼쪽으로 한 칸**
4. `directions[3]` = { 0,  1} → **오른쪽으로 한 칸**
5. `directions[4]` = {-1, -1} → **왼쪽 위(↖) 대각선으로 한 칸**
6. `directions[5]` = { 1, -1} → **왼쪽 아래(↙) 대각선으로 한 칸**
7. `directions[6]` = {-1,  1} → **오른쪽 위(↗) 대각선으로 한 칸**
8. `directions[7]` = { 1,  1} → **오른쪽 아래(↘) 대각선으로 한 칸**


---

# 🛠 코드

```java
import java.io.*;
import java.util.*;

public class Main {
    // 지도 row (w), column (h)
    static int w, h;
    // 지도 형태를 저장할 배열
    static int[][] grid;
    // 해당 땅 방문 여부 저장 배열
    static boolean[][] visited;

    // 8방향 탐색 배열
    static int[][] directions = {
            {-1, 0},
            {1, 0},
            {0, -1},
            {0, 1},
            {-1, -1},
            {+1, -1},
            {-1, +1},
            {+1, +1}
    };

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        // 여러개의 테스트 케이스를 반복해서 입력
        while (true) {
            StringTokenizer st = new StringTokenizer(br.readLine());
            w = Integer.parseInt(st.nextToken());
            h = Integer.parseInt(st.nextToken());

            // 입력이 0 0 이면 종료 (반복 종료 조건)
            if (w == 0 && h == 0) break;

            // 방문 여부, 지도 배열 초기화
            visited = new boolean[h][w];
            grid = new int[h][w];

            // 지도 정보 입력 받기 (h행동안 반복)
            for (int row = 0; row < h; row++) {
                st = new StringTokenizer(br.readLine());
                for (int col = 0; col < w; col++) {
                    // 공백을 사이에 두고 0, 1 입력 받음
                    grid[row][col] = Integer.parseInt(st.nextToken());
                }
            }

            // 섬의 개수를 세는 변수
            int islandCount = 0;

            // 전체 지도를 순회하며 방문하지 않은 땅(1)을 발견하면 DFS 수행
            for (int row = 0; row < h; row++) {
                for (int col = 0; col < w; col++) {
                    // 땅이고 아직 방문하지 않았다면
                    if (grid[row][col] == 1 && !visited[row][col]) {
                        dfs(row, col); // 연결된 모든 땅 방문처리
                        islandCount++; // 새로운 섬 방문이므로 카운팅
                    }
                }
            }

            System.out.println(islandCount);
        }
    }

    // DFS 로직을 이용해 연결된 땅들을 모두 방문 처리
    static void dfs(int row, int col) {
        // 현재 위치 방문 표시
        visited[row][col] = true;

        // 인접 8방향 확인
        for (int[] dir : directions) {
            int newRow = row + dir[0];
            int newCol = col + dir[1];

            // 새로 관측된 좌표가 지도 범위 내인지 확인
            if (newRow >= 0 && newRow < h && newCol >= 0 && newCol < w) {
                if (!visited[newRow][newCol] && grid[newRow][newCol] == 1) {
                    dfs(newRow, newCol); // 재귀 호출로 연결된 땅 탐색
                }
            }
        }
    }

}
```

---

# ✏️ 느낀 점

- 단지번호붙이기와 유사한 문제였다.
- 하지만 반복 조건을 구현하면서 뺴먹어서 한번 틀렸다. 문제가 제시하는 구현 조건을 좀 더 잘 메모 정리를 해놓고 시작하는 습관을 들여야겠다.
