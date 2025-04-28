---
title: "BOJ 1316 그룹 단어 체커 문제 풀이"
date: 2025-04-28
categories: ["Algorithm", "BOJ"]
tags: ["문자열", "구현"]
---

# 📝 문제 설명

> (이 글은 테스트용 포스트입니다.)

그룹 단어란, 각 문자가 연속해서 등장하거나 아예 등장하지 않는 단어를 말한다.  
주어진 단어들이 그룹 단어인지 판별하여 그룹 단어의 개수를 세는 문제.

[문제 링크](https://www.acmicpc.net/problem/1316)

---

# 💡 아이디어

- 각 단어마다 알파벳 출현 여부를 체크하면서 진행한다.
- 현재 문자가 이전 문자와 다르다면, 이미 등장했던 문자인지 검사한다.
- 중복이 발생하면 그룹 단어가 아니므로 바로 실패 처리.

---

# 🛠 코드

```java
import java.io.*;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int n = Integer.parseInt(br.readLine());
        int count = 0;

        for (int i = 0; i < n; i++) {
            String word = br.readLine();
            if (isGroupWord(word)) {
                count++;
            }
        }
        System.out.println(count);
    }

    static boolean isGroupWord(String word) {
        boolean[] visited = new boolean[26];
        char prev = ' ';

        for (int i = 0; i < word.length(); i++) {
            char curr = word.charAt(i);
            if (curr != prev) {
                if (visited[curr - 'a']) {
                    return false;
                }
                visited[curr - 'a'] = true;
            }
            prev = curr;
        }
        return true;
    }
}
```

---

# ⏱ 시간복잡도
단어 길이를 L, 단어 수를 N이라 할 때 각각의 단어를 한 번씩 훑기 때문에 전체 시간복잡도는 O(N × L)이다.

---

# ✏️ 느낀 점

- 문제를 읽을 때 "연속" 조건을 제대로 이해하는 것이 중요했다.
- boolean 배열로 등장 여부를 체크하는 방식은 다른 유사 문제에서도 많이 쓸 수 있을 것 같다.
