---
title: "프로그래머스 Lv0 부분 문자열"
name: 꼽사리구나
writer: 꼽사리구나
categories: [코딩테스트, 프로그래머스]
tags:
  - [코딩테스트, 프로그래머스, Lv0]
toc: true
toc_sticky: true
date: 2024-12-04
last_modified_at: 2024-12-04
---

처음에는 for문을 돌려서 풀어볼까 하다가 조금 더 간단하게 풀이하는 방법이 있을 것 같아서 검색해봤다.

contains() : 대상 문자열의 특정 문자열이 포함되었는지 여부를 알고자 할 때 사용 (boolean타입 반환)
indexOf() : 대상 문자열의 특정 문자의 index값을 찾고자 할 때 사용 (해당 index값 반환)
matches() : 대상 문자열의 정규표현식이 포함되었는지 여부를 알고싶을 때 사용 (boolean타입 반환)

```java
class Solution {
    public int solution(String str1, String str2) {
        int answer = 0;
        
        if (str2.contains(str1)){
            answer = 1;
        }
        
        return answer;
    }
}
```
