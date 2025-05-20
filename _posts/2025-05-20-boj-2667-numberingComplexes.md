---
title: "백준 2667 | 단지번호붙이기"
date: 2025-05-20
categories: ["Algorithm", "백준"]
tags: ["그래프 탐색", "격자 그래프", "깊이 우선 탐색", "DFS"]
---

# 📝 문제 설명

<img src="assets/images/250520_numbering.jpg" alt="단지번호붙이기 예시 좌표" width="350">

- 위 그림과 같은 정가각형 모양의 지도를 만든다. (가로 N, 세로 N)
- 집이 있는 곳은 1, 없는 곳은 0으로 입력 받는다.
- **단지**: 이 지도 내에서 연결된 집의 모임
  - 연결되었다는 건, 아딴 집의 좌우 혹은 아래위로 다른 집이 붙어 있는 것을 얘기한다.
  - 대각선상에 있는 집은 연결된게 아니다.
- 총 단지의 개수와, 각 단지 내에 속한 집의 개수를 출력하라.  
   
1. ***입력***
   - 첫째 줄에는 지도의 크기  `N`을 입력 받는다.
     - 정사각형이므로 가로와 세로의 크기는 같으며 `5≤N≤25`
   - 그 다음 `N`줄에는 각각 `N`개의 자료(0혹은 1)를 입력받는다.
2. ***출력***
   - 첫쩨 줄에는 총 단지 수를 출력한다.
   - 다음 줄 부터는 각 단지 내 집의 수를 오름차순으로 정렬하여 한 줄에 하나를 출력한다.


[문제 링크](https://www.acmicpc.net/problem/2606)

---

# 💡 아이디어

### 🪴 그래프 탐색 질문 템플릿
1. ***뭘 세라고 했지?***
   - 집 단지의 총 개수
   - 각 단지 내 집의 개수 (각 단지의 size)
2. ***이 문제에서 내가 세야 하는게 정점 수야, 그룹 수야, 거리야, 최댓값이야?***
   - 그룹 수(집 단지-그룹 수)와 정점 수(단지내 집-정점 개수)
3. ***이걸 세기 위해 탐색을 몇번이나 더 해야하지?***
   - 아직 방문하지 않은 집을 만날때마다 탐색을 해야한다.
   - 즉, 단지 개수만큼 탐색을 해야한다.
4. ***탐색이 진행되는 중에 카운트 변수를 더해줘야하나? 아니면 탐색이 끝나거나 시작될 때 더해줘야 하나?***
   - 탐색이 진행되는 중 카운트: 단지 내 집 개수
   - 탐색이 시작될 때 마다 카운트: 단지 개수

### 🪴 격자 이동 표현하기
1. `directions[0]` = {-1, 0} → 위로 한 칸
2. `directions[1]` = { 1, 0} → 아래로 한 칸
3. `directions[2]` = { 0,-1} → 왼쪽으로 한 칸
4. `directions[3]` = { 0, 1} → 오른쪽으로 한 칸

---

# 🛠 코드

```java
import java.io.*;
import java.util.*;

public class Main {
    static int N;  // 격자의 크기 (N x N)
    static int[][] grid;  // 지도 정보 (0: 집없음, 1: 집있음)
    static boolean[][] visited;  // 방문 여부 체크 배열

    // 상하좌우 4방향 이동 정의 배열
    static int[][] directions = {
            {-1, 0},  // 위로 한 칸
            {1, 0},  // 아래로 한 칸
            {0, -1},  // 왼쪽으로 한 칸
            {0, 1}  // 오른쪽으로 한 칸
    };

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        N = Integer.parseInt(br.readLine());

        // visited와 grid 배열을 N×N 크기로 초기화
        visited = new boolean[N][N];
        grid = new int[N][N];

        // N 줄에 걸쳐 격자-지도를 한 줄씩 읽어서 grid 배열 채우기
        // 입력된 문자 '0' 또는 '1'을 정수 0 또는 1로 변환해 grid[i][j]에 저장
        for (int i = 0; i < N; i++) {
            String line = br.readLine();
            for (int j = 0; j < N; j++) {
                grid[i][j] = line.charAt(j) - '0';
            }
        }

        // componentSizes: 발견된 각 단지(component)에 속한 집 수를 순서대로 저장
        List<Integer> componentSizes = new ArrayList<>();

        /*
         * 모든 칸을 순회하며,
         * 집(1)이면서 아직 방문하지 않은 칸을 찾으면
         * dfs로 새로운 단지에 속한 집 개수를 세어
         * houseCount에 받고,
         * componentSizes에 추가한다.
         */
        for (int row = 0; row < N; row++) {
            for (int col = 0; col < N; col++) {
                if (grid[row][col] == 1 && !visited[row][col]) {
                    int houseCount = dfs(row, col);
                    componentSizes.add(houseCount);
                }
            }

        }

        // 단지별 집 개수 오름차순 정렬
        Collections.sort(componentSizes);

        // 결과 출력
        System.out.println(componentSizes.size());
        for (int size : componentSizes) {
            System.out.println(size);
        }


    }

    /*
     * dfs: (row, col)에서 시작해
     *  상하좌우로 연결된 모든 집(값 1)을 깊이 우선 탐색으로 방문 처리하고,
     *  해당 component에 속한 집 개수(houseCount)를 반환
     */
    static int dfs(int row, int col) {
        visited[row][col] = true;  // 현재 칸 방문 처리
        int houseCount = 1;  // 현재 칸에 있는 집 1채 카운트

        // 4방향 하나씩 확인하며 재귀 호출
        for (int[] dir : directions) {
            int newRow = row + dir[0];
            int newCol = col + dir[1];

            if (newRow >= 0 && newRow < N && newCol >= 0 && newCol < N) {
                if (!visited[newRow][newCol] && grid[newRow][newCol] == 1) {
                    houseCount += dfs(newRow, newCol);
                }
            }

        }
        // 이 단지에 속한 집의 최종 개수를 반환
        return houseCount;

    }
}

```

---

# ✏️ 느낀 점

- 격자 그래프라고 떡하니 적혀있는데 함수 그래프를 생각해서 괜히 헤맸다.
- 반복문 변수를 무지성으로 i, j 쓰지 않고 문제에 맞는 변수명으로 쓰니까 풀때 편하다.
