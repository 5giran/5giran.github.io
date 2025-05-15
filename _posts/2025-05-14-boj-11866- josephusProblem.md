---
title: "백준 11866 | 요세푸스 문제 0"
date: 2025-05-14
categories: ["Algorithm", "백준"]
tags: ["자료구조", "큐"]
---

# 📝 문제 설명

1. 첫째 줄에서 정수 N과 K를 빈칸 사이에 두고 입력받는다.
   - N: 1번부터 N번까지 N번의 사람이 원을 이루며 앉아있다.
   - K: K번째 사람을 제거한다.
2. 한 사람이 제거되면 남은 사람들로 이루어진 원을 따라 이 과정을 계속 해 나간다.
3. 이 과정은 N명의 사람들이 모두 제거 될 때까지 계속된다.

[문제 링크](https://www.acmicpc.net/problem/11866)

---

# 💡 아이디어

- 한 줄에 여러개 입력 받을때는 `StringTokener`를 사용한다.
- 요세푸스 로직은 덱에서 시행하고, 요세푸스 결과 저장은 리스트에 진행한다.
- 덱에서 K번째 수(사람)을 바로 삭제할 수 없기 때문에, `pollFirst()`, `pollLast()`를 이용하여 K번째 수를 삭제할 수 있도록 K-1번까지 회전시킨다.

---

# 🛠 코드

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.*;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        // 한 줄에 여러개 입력 받을 때 사용
        StringTokenizer st = new StringTokenizer(br.readLine());
        int N = Integer.parseInt(st.nextToken());
        int K = Integer.parseInt(st.nextToken());

        // 덱 초기화 - 덱 입력 (1 - N)
        Deque<Integer> deque = new ArrayDeque<>();
        for (int i = 1; i <= N; i++) {
            deque.add(i);
        }

        // 요세푸스 결과 저장용 리스트
        List<Integer> josephus = new ArrayList<>();

        // 요세푸스 로직
        while (!deque.isEmpty()) {
            // K번째를 제거하기 위해 K-1까지 회전 (뒤로 넘김)
            for (int i = 1; i < K; i++) {
                deque.addLast(deque.pollFirst());  // 앞에서 꺼내서 뒤로 추가함
            }
            josephus.add(deque.pollFirst());  // K번째는 제거 후 요세푸스 리스트에 추가
        }

        // 결과 출력
        StringBuilder sb = new StringBuilder();
        sb.append("<");
        for (int i = 0; i < josephus.size(); i++) {
            sb.append(josephus.get(i));
            // 마지막 요소 빼고 쉼표 추가
            if (i != josephus.size() - 1) sb.append(", ");
        }
        sb.append(">");
        System.out.println(sb);
    }
}
```

---

# ⏱ 시간 복잡도

- **입력 처리**:  
  N개의 사람 번호를 덱에 추가 → `O(N)`

- **요세푸스 순열 계산**:  
  매 단계마다 `K-1`명 회전 + 1명 제거 → `O(N × K)`

- **출력 처리**:  
  결과 리스트 순회 → `O(N)`

> 👉 **총 시간 복잡도**:  
> `O(N × K)` (이 문제에서는 통과 가능)


