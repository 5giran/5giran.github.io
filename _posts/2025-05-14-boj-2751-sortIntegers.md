---
title: "백준 2751 | 수 정렬하기 2"
date: 2025-05-14
categories: ["Algorithm", "백준"]
tags: ["정렬"]
---

# 📝 문제 설명

1. N개의 수를 받아서
2. 이를 오름차순 정렬하라. (한 줄에 하나씩 출력)

[문제 링크](https://www.acmicpc.net/problem/2751)

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
import java.util.ArrayList;
import java.util.Collections;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int N = Integer.parseInt(br.readLine());

        ArrayList<Integer> numbers = new ArrayList<>();

        for (int i = 0; i < N; i++) {
            numbers.add(Integer.parseInt(br.readLine()));
        }

        Collections.sort(numbers);

        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < N; i++) {
            sb.append(numbers.get(i)).append("\n");
        }

        System.out.println(sb);
    }
}
```

---

# ⏱ 시간 복잡도

- **입력 처리**:  
  `N`개의 좌표를 입력받아 ArrayList에 저장한다. → `O(N)`

- **정렬**:  
  `Collections.sort()`의 최악/평균 시간 복잡도는 `O(N log N)`

- **출력**:  
  `StringBuilder`를 이용하여 `N`개의 결과를 출력 → `O(N)`

> 👉 **총 시간 복잡도**:  
> `O(N log N)`


---

# ✏️ 느낀 점

- 좌표 정렬하기와 유사한 유형이지만 더 간단한 문제
