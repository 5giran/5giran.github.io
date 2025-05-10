---
title: "백준 9012 | 괄호"
date: 2025-05-03
categories: ["Algorithm", "백준"]
tags: ["스택", "문자열"]
---

# 📝 문제 설명  

1. 올바른 괄호 문자열(Valid Parenthesis String, VPS) 인지 판별하는 문제
2. 올바른 괄호 문자열(VPS) 이면 'YES' 출력, 아니라면 'NO'를 출력한다. (한 줄에 하나씩) <br><br>
   
3. 괄호 문자열(Parenthesis String, PS)이란?
   - 두개의 괄호 기호 `'('`와 `')'`로만 이루어진 문자열이면 모두 OK <br>
  
4. **올바른 괄호 문자열** (Valid Parenthesis String, VPS)이란?
   - 두개의 괄호 기호 `'('`와 `')'`으로 이루어졌을 뿐만 아니라, 괄호의 모양이 바르게 구성된- 괄호의 짝이 정확히 맞는 문자열을 올바른 괄호 문자열(VPS)이라고 한다.
   - 한쌍의 괄호 기호로 된 `"( )"` 문자열은 기본 VPS 이라고 한다.
   - 만일 x가 VPS라면, x를 하나의 괄호 쌍(기본 VPS)에 넣은 문자열 `"(x)"`도 VPS가 된다.
   - 만일 x, y가 VPS라면 `"xy"`도 VPS가 된다. <br><br>
  
[문제 링크](https://www.acmicpc.net/problem/9012)  

---

# 💡 아이디어  

- 스택을 구현하기 위해 더 진보된 자료구조인 Deque를 사용한다.
- `'('`가 나오면 스택에 넣는다. (쌓는다)
- `')'`가 나오면 스택에서 '('를 하나 꺼내고, 삭제한다.
  - 꺼낼 게 있으면 VPS
  - 꺼낼 게 없으면 단순 PS
- 여는 괄호는 단순 push로 스택 내부에 삽입하고, 스택 내부에 삽입되고 삽입 될 문자열이 여는 괄호 밖에 없으므로 닫는 괄호 또한 스택 최 상단의 문자열을 단순 pop한다.  

---

# 🛠 코드

```java
import java.io.*;
import java.util.*;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int T = Integer.parseInt(br.readLine());

        while (T-- > 0) {
            String input = br.readLine();
            Deque<Character> stack = new ArrayDeque<>();
            boolean isVPS = true;

            for (int i = 0; i < input.length(); i++) {
                char c = input.charAt(i);

                if (c == '(') {
                    stack.push(c);
                } else {
                    if (stack.isEmpty()) {
                        isVPS = false;
                        break;
                    }
                    stack.pop();
                }
            }

            System.out.println((isVPS && stack.isEmpty()) ? "YES" : "NO");
        }
    }
}
```  

---

# ⏱ 시간복잡도
O(T): 하지만 문자열 길이가 최대 50이므로 사실상 상수 시간 처리가 가능하다.  

---

# ✏️ 느낀 점

- 처음 풀때는 괄호 짝맞추기에만 집중해서 다른 방식으로 풀어서 틀렸다.
- 닫는 괄호 문자열의 여는 괄호 문자열 선별 조건 같은것도 고민했다.
- 스택 구조가 적합하다는걸 금방 깨달았고, 문제를 더 풀면 또 다른 문제는 금방 풀 수 있겠지.
