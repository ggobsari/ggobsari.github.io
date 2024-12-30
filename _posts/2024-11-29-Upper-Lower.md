---
title: "프로그래머스 Lv0 대소문자 바꿔서 출력하기"
name: 꼽사리구나
writer: 꼽사리구나
categories: [코딩테스트, 프로그래머스]
tags:
  - [코딩테스트, 프로그래머스, Lv0]
toc: true
toc_sticky: true
date: 2024-11-29
last_modified_at: 2024-11-29
---

소문자 --> 대문자 : toUpperCase()
대문자 --> 소문자 : toLowerCase()

toCharArray()란?
String 문자열을 char형 배열로 바꿔서 반환해주는 메소드
"test"라는 문자열이 있으면
arr[0] = 't'
arr[1] = 'e'
arr[2] = 's'
arr[3] = 't'

charAt 함수
String 타입의 데이터(문자열)에서 특정 문자를 char 타입으로 변환할 때 사용하는 함수


```java
import java.util.Scanner;

public class Solution {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String a = sc.next();
        
        String answer = "";
        
        char[] arr = a.toCharArray();
        
        for(int i = 0; i < arr.length; i++){
            if(Character.isUpperCase(arr[i])){
                answer += Character.toLowerCase(arr[i]);
            } else{
                answer += Character.toUpperCase(arr[i]);
            }
        }
        System.out.println(answer);
    }
}
```


아스키코드로도 풀이가 가능할 듯 하다.