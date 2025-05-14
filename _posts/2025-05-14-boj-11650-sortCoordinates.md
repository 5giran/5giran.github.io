---
title: "백준 11650 | 좌표 정렬하기"
date: 2025-05-14
categories: ["Algorithm", "백준"]
tags: ["정렬"]
---

# 📝 문제 설명

1. 2차원 평면 위의 점 N개를 입력 받는다.
2. 입력받은 좌표를 정렬한다.
   - x좌표가 같지 않다면 x좌표가 증가하는 순으로 (x좌표 기준 오름차순으로)
   - x좌표가 같다면 y좌표가 증하는 순으로 (y좌표 기준 오름차순으로)

[문제 링크](https://www.acmicpc.net/problem/11650)

---

# 💡 아이디어

- `int[][]` 형태의 2차원 배열을 선언하여 x, y 좌표를 저장한다.
- `Arrays.sort()` 와 `Integer.compare()` 을 활용하여 오름차순으로 정렬한다.
- 출력 시 `StringBuilder` 를 사용하여 I/O 성능을 향상시킨다.

---

# 🛠 코드

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.Arrays;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int N = Integer.parseInt(br.readLine());

        // x, y좌표 저장 배열
        int [][] points = new int[N][2];
        for (int i = 0; i < N; i++) {
            String[] input = br.readLine().split(" ");
            points[i][0] = Integer.parseInt(input[0]); // x 좌표
            points[i][1] = Integer.parseInt(input[1]); // y 좌표
        }

        // 정렬
        Arrays.sort(points, (a, b) -> {
            // x좌표 값이 같을 경우
            if (a[0] == b[0]) {
                // y좌표 값 기준 오름차순으로 정렬
                return Integer.compare(a[1], b[1]);
            }
            // x좌표 값이 다를 경우 x좌표 값 기준 오름차순으로 정렬
            return Integer.compare(a[0], b[0]);
        });

        // 빠른 출력 - 모아서
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < N; i++) {
            sb.append(points[i][0]).append(" ").append(points[i][1]).append("\n");
        }
        System.out.println(sb);
    }
}
```

---

# ⏱ 시간 복잡도

- **입력 처리**:  
  `N`개의 좌표를 입력받아 2차원 배열에 저장 → `O(N)`

- **정렬**:  
  `Arrays.sort()`의 최악/평균 시간 복잡도는 `O(N log N)`

- **출력**:  
  `StringBuilder`를 이용하여 `N`개의 결과를 출력 → `O(N)`

> 👉 **총 시간 복잡도**:  
> `O(N log N)`


---

# ✏️ 느낀 점

- 시간 복잡도도 입력 처리 / 정렬 / 출력 / 총 시간 복잡도로 나눠서 정리해야겠다.
- 알고리즘에서 좌표를 입력받는 법
