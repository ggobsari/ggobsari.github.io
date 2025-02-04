---
title: "타임리프"
name: 꼽사리구나
writer: 꼽사리구나
categories: [스프링, 타임리프]
tags:
  - [스프링, 타임리프]
toc: true
toc_sticky: true
date: 2024-12-09
last_modified_at: 2025-01-08
---

------------------------------------------------------------------------------------------------------------------------------------------------

# 타임리프 특징
1. 서버 사이드 HTML 렌더링(SSR)
2. 네츄럴 템플릿
3. 스프링 통합 지원

## "서버 사이드 HTML 렌더링(SSR)"  
타임리프는 백엔드 서버에서 HTML을 동적으로 렌더링 하는 용도로 사용된다.

## "네츄럴 템플릿"  
타임리프는 순수 HTML을 최대한 유지하는 특징이 있다.  
타임리프로 작성한 파일은 HTML을 유지하기 때문에 웹 브라우저에서 파일을 직접 열어도 내용을 확인할 수 있고, 서버를 통해 뷰 템플릿을 거치면 동적으로 변경된 결과를 확인할 수 있다.  
JSP를 포함한 다른 뷰 템플릿들은 해당 파일을 열면 소스코드와 HTML이 섞여서 웹 브라우저에서 정상적인 HTML 결과를 확인할 수 없다. 오직 서버를 통해서 렌더링 되고 HTML 응답 결과를 받아야 화면을 확인할 수 있다.  
반면에 타임리프로 작성된 파일은 해당 파일을 그대로 웹 브라우저에서 열어도 정상적인 HTML 결과를 확인할 수 있다.  
이렇게 순수 HTML을 유지하면서 뷰 템플릿도 사용할 수 있는 타임리프의 특징을 '네츄럴 템플릿'이라고 한다.  

## "스프링 통합 지원"  
타임리프는 스프링과 자연스럽게 통합되고, 스프링의 다양한 기능을 편리하게 사용할 수 있게 지원한다.  

타임리프를 사용하려면 다음 선언을 하면 된다.
```html
<html xmlns:th="http://www.thymeleaf.org">
```

------------------------------------------------------------------------------------------------------------------------------------------------

# 텍스트 - text, utext
타임리프의 텍스트를 출력하는 기능

타임리프는 기본적으로 HTML 태그의 속성에 기능을 정의해서 동작한다.  
HTML의 콘텐츠에 데이터를 출력할 때 'th:text:'를 사용하면 된다.
```html
<span th:text="${data}">
```

HTML 태그의 속성이 아니라 HTML 콘텐츠 영역안에서 직접 데이터를 출력하고 싶으면 '[[...]]'를 사용하면 된다.
```html
컨텐츠 안에서 직접 출력하기 = [[${data}]]
```

### HTML 엔티티
웹 브라우저는 '<'를 HTML 태그의 시작으로 인식한다.  
따라서 '<'를 태그의 시작이 아니라 문자로 표현할 수 있는 방법이 필요한데, 이를 HTML 엔티티라고 한다.  
그리고 HTML에서 사용하는 특수 문자를 HTML 엔티티로 변경하는 것을 이스케이프라고 한다.  
타임리프가 제공하는 'th:text', '[[...]]'는 기본적으로 이스케이프를 제공한다.  

이스케이프 기능을 사용하지 않기 위해서 타임리프는 다음 두 기능을 제공한다.
```html
th:text -> th:utext
[[...]] -> [(...)]
```

------------------------------------------------------------------------------------------------------------------------------------------------

# 변수 - SpringEL
타임리프에서 변수를 사용할 때는 변수 표현식을 사용한다.  
```html
변수 표현식 : ${...}
```
그리고 이 변수 표현식에는 SpringEL이라는 스프링이 제공하는 표현식을 사용할 수 있다.

## SpringEL 다양한 표현식 사용
### Object
- user.username : user의 username을 프로퍼티 접근 -> user.getUsername()
- user['username'] : 위와 같음 -> user.getUsername()
- user.getUsername() : user의 getUsername()을 직접 호출

### List
- users[0].username : List에서 첫 번째 회원을 찾고 username 프로퍼티 접근 -> list.get(0).getUsername()
- users[0]['username'] : 위와 같음
- users[0].getUsername() : List에서 첫 번째 회원을 찾고 메소드 직접 호출

### Map
- userMap['userA'].username : Map에서 userA를 찾고, username 프로퍼티 접근 -> map.get("userA").getUsername()
- userMap['userA']['username'] : 위와 같음
- userMap['userA].getUsername() : Map에서 userA를 찾고 메소드 직접 호출

```html
<h1>SpringEL 표현식</h1>
<ul>Object
    <li>${user.username} = <span th:text="${user.username}"></span></li>
    <li>${user['username']} = <span th:text="${user['username']}"></span></li>
    <li>${user.getUsername()} = <span th:text="${user.getUsername()}"></span></li>
</ul>
<ul>List
    <li>${users[0].username} = <span th:text="${users[0].username}"></span></li>
    <li>${users[0]['username']} = <span th:text="${users[0]['username']}"></span>
    </li>
    <li>${users[0].getUsername()} = <span th:text="${users[0].getUsername()}"></span>
    </li>
</ul>
<ul>Map
    <li>${userMap['userA'].username} = <span th:text="${userMap['userA'].username}"></span></li>
    <li>${userMap['userA']['username']} = <span th:text="${userMap['userA'] ['username']}"></span></li>
    <li>${userMap['userA'].getUsername()} = <span th:text="${userMap['userA'].getUsername()}"></span></li>
</ul>
```

## 지역 변수 선언
th:with을 사용하면 지역 변수를 선언해서 사용할 수 있다.  
지역 변수는 선언한 태그 안에서만 사용할 수 있다.
```html
<h1>지역 변수 - (th:with)</h1>
 <div th:with="first=${users[0]}">
    <p>처음 사람의 이름은 <span th:text="${first.username}"></span></p>
 </div>
```

------------------------------------------------------------------------------------------------------------------------------------------------

# 기본 객체
타임리프는 기본 객체를 제공한다
- ${#request}
- ${#response}
- ${#session}
- ${#servletContext}
- ${#locale}

#request는 HttpServletRequest객체가 그대로 제공되기 때문에 데이터를 조회하려면 request.getParameter("data")처럼 불편하게 접근해야한다.  

이를 해결하기 위해 편의 객체도 제공한다.
- HTTP 요청 파라미터 접근 : param
  * ${param.paramData}
- HTTP 세션 접근 : session
  * ${session.sessionData}
- 스프링 빈 접근 : @
  * ${@helloBean.hello('Spring!')}

------------------------------------------------------------------------------------------------------------------------------------------------

# 유틸리티 객체와 날짜
타임리프는 문자, 숫자, 날짜, URI등을 편리하게 다루는 유틸리티 객체를 제공한다
- #message : 메세지, 국제화 처리
- #uris : URI 이스케이프 지원
- #dates : java.util.Date 서식 지원
- #calendars : java.util.Calendar 서식 지원
- #temporals : 자바 8 날짜 서식 지원
- #numbers : 숫자 서식 지원
- #strings : 문자 관련 편의 기능
- #objects : 객체 관련 기능 제공
- #bools : boolean 관련 기능 제공
- #arrays : 배열 관련 기능 제공
- #lists, #sets, #maps : 컬렉션 관련 기능 제공
- #ids : 아이디 처리 관련 기능 제공

------------------------------------------------------------------------------------------------------------------------------------------------

# URL 링크
타임리프에서 URL을 생성할 때는 @{...} 문법을 사용하면 된다.  

### 단순한 URL
```html
@{/hello} -> /hello
```

### 쿼리 파라미터
```html
@{/hello(param1=${param1}, param2=${param2})}
  -> /hello?param1=data1&param2=data2
  ()에 있는 부분은 쿼리 파라미터로 처리된다.
```

### 경로 변수
```html
@{/hello/{param1}/{param2}(param1=${param1}, param2=${param2})}
  -> /hello/data1/data2
  URL 경로상에 변수가 있으면 ()부분은 경로 변수로 처리된다.
```

### 경로 변수 + 쿼리 파라미터
```html
@{/hello/{param1}(param1=${param1}, param2=${param2})}
  -> /hello/data1?param2=data2
  경로 변수와 쿼리 파라미터를 함께 사용할 수 있다.
```

상대경로, 절대경로, 프로토콜 기준을 표현할 수도 있다.
- /hello : 절대경로
- hello : 상대경로

------------------------------------------------------------------------------------------------------------------------------------------------

# 리터럴
리터럴은 소스 코드상에 고정된 값을 말하는 용어다.
```java
String a = "Hello";
int a = 10 * 20;
```
다음 코드에서 "Hello"는 문자 리터럴, 10, 20은 숫자 리터럴이다.  

타임리프에는 다음과 같은 리터럴이 있다.
- 문자 : 'hello'
- 숫자 : 10
- 불린 : true, false
- null : null  

타임리프에서 문자 리터럴은 항상 ''로 감싸야 한다.
```html
<span th:text="'hello'">
```  

하지만 공백 없이 쭉 이어진다면 하나의 의미있는 토큰으로 인지해서 작은 따옴표를 생략할 수 있다.
```html
<span th:text="hello">
```  

##### 오류
```html
<span th:text="hello world!"></span>
```  
##### 수정
```html
<span th:text="'hello world!'"></span>
```  

### 리터럴 대체
```html
<span th:text="|hello ${data}|">
```  
마지막 리터럴 대체 문법을 사용하면 마치 템플릿을 사용하는 것 처럼 편리하다.

------------------------------------------------------------------------------------------------------------------------------------------------

# 연산
타임리프 연산은 자바와 크게 다르지 않으며, HTML 안에서 사용하기 때문에 HTML 엔티티를 주의해야 한다.
- 산술연산

```html
<li>10 + 2 = <span th:text="10 + 2"></span></li>
<li>10 % 2 == 0 = <span th:text="10 % 2 == 0"></span></li>
```
- 비교연산 : HTML 엔티티를 사용해야 하는 부분을 주의
  * '>(gt), <(lt), >=(ge), <=(le), !(not), ==(eq), !=(neq, ne)'

```html
<li>1 > 10 = <span th:text="1 &gt; 10"></span></li>
<li>1 gt 10 = <span th:text="1 gt 10"></span></li>
<li>1 >= 10 = <span th:text="1 >= 10"></span></li>
<li>1 ge 10 = <span th:text="1 ge 10"></span></li>
<li>1 == 10 = <span th:text="1 == 10"></span></li>
<li>1 != 10 = <span th:text="1 != 10"></span></li>
```
- 조건식 : 자바의 조건식과 유사

```html
<li>(10 % 2 == 0)? '짝수':'홀수' = <span th:text="(10 % 2 == 0)? '짝수':'홀수'"></span></li>
```
- Elvis 연산자 : 조건식의 편의 버전

```html
<li>${data}?: '데이터가 없습니다.' = <span th:text="${data}?: '데이터가 없습니다.'"></span></li>
<li>${nullData}?: '데이터가 없습니다.' = <span th:text="${nullData}?: '데이터가 없습니다.'"></span></li>
```
- No_Operation : _인 경우 마치 타임리프가 실행되지 않는 것 처럼 동작한다.

```html
<li>${data}?: _ = <span th:text="${data}?: _">데이터가 없습니다.</span></li>
<li>${nullData}?: _ = <span th:text="${nullData}?: _">데이터가 없습니다.</span></li>
```

------------------------------------------------------------------------------------------------------------------------------------------------

# 속성 값 설정
## 타임리프 태그 속성
타임리프는 주로 HTML 태그에 th:* 속성을 지정하는 방식으로 동작한다.  
th:*로 속성을 적용하면 기존 속성을 대체한다.  
기존 속성이 없으면 새로 만든다.

### 속성 설정
th:* 속성을 지정하면 타임리프는 기존 속성을 th:*로 지정한 속성으로 대체한다.  
기존 속성이 없다면 새로 만든다.
```html
<input type="text" name="mock" th:name="userA"/>
타임리프 렌더링 후 -> <input type = "text" name = "userA"/>
```

### 속성 추가
th:attrappend : 속성 값의 뒤에 값을 추가한다.  
th:attrprepend : 속성 값의 앞에 값을 추가한다.  
th:classappend : class 속성에 자연스럽게 추가한다.  
```html
- th:attrappend = <input type="text" class="text" th:attrappend="class='large'"/><br/>
- th:attrprepend = <input type="text" class="text" th:attrprepend="class='large'"/><br/>
- th:classappend = <input type="text" class="text" th:classappend="large"/><br/>
```

### checked 처리
HTML에서는 input type="checkbox" name="active" checked="false" -> 경우에도 checked 속성이 있기에 checked 처리가 되어버린다.  
타임리프의 th:checked는 값이 false인 경우 checked 속성 자체를 제거한다.
```html
- checked o <input type="checkbox" name="active" th:checked="true"/><br/>
- checked x <input type="checkbox" name="active" th:checked="false"/><br/>
- checked=false <input type="checkbox" name="active" checked="false"/><br/>
```

------------------------------------------------------------------------------------------------------------------------------------------------

# 반복
타임리프에서 반복은 th:each를 사용한다

## 반복 기능
tr th:each="user : ${users}"
- 반복할 때 오른쪽 컬렉션 (${users})의 값을 하나씩 꺼내서 왼쪽 변수(user)에 담아서 태그를 반복 실행한다.
- th:each는 List뿐만 아니라 배열, java.util.Iterable, java.util.Enumeration을 구현한 모든 객체를 반복에 사용할 수 있다.
- Map도 사용할 수 있는데 이 경우 변수에 담기는 값은 Map.Entry이다.

## 반복 상태 유지
tr th:each="user, userStat : ${users}"
반복의 두번째 파라미터를 설정해서 반복의 상태를 확인할 수 있다.  
두번째 파라미터는 생략 가능한데, 생략하면 지정한 변수명(user) + Stat가 된다.
위에서는 user + Stat = userStat이므로 생략 가능하다.

## 반복 상태 유지 기능
- index : 0부터 시작하는 값
- count : 1부터 시작하는 값
- size : 전체 사이즈
- even, odd : 홀수, 짝수 여부(boolean)
- first, last : 처음, 마지막 여부(boolean)
- current : 현재 객체

------------------------------------------------------------------------------------------------------------------------------------------------

# 조건부 평가
타임리프의 조건식

## if, unless(if의 반대) 
타임리프는 해당 조건이 맞지 않으면 태그 자체를 렌더링하지 않는다.  
만약 다음 조건이 false인 경우 ```<span>...<span>```부분 자체가 렌더링되지 않고 사라진다.  
```<span th:text="'미성년자'" th:if="${user.age lt 20}"></span>```

## switch
*은 만족하는 조건이 없을 때 사용하는 디폴트다.

------------------------------------------------------------------------------------------------------------------------------------------------

# 주석

## 표준 HTML 주석
자바스크립트의 표준 HTML 주석은 타임리프가 렌더링하지 않고, 그대로 남겨둔다.

## 타임리프 파서 주석
  타임리프 파서 주석은 타임리프의 진짜 주석. 렌더링에서 주석부분을 제거하며, 타임리프에서는 대부분 이것을 사용한다.

## 타임리프 프로토타입 주석
  타임리프 프로토타입은 HTML 주석에 약간의 구문을 추가했다.  
  HTML 파일을 웹 브라우저에서 그대로 열어보면 HTML 주석이기 때문에 이 부분을 웹 브라우저가 렌더링하지 않는다.  
  타임리프 렌더링을 거치면 이 부분이 정상 렌더링 된다.  
  즉, HTML 파일을 그대로 열어보면 주석처리가 되지만, 타임리프를 렌더링 한 경우에만 보이는 기능이다.

------------------------------------------------------------------------------------------------------------------------------------------------

# 블록
```<th:block>```은 HTML 태그가 아닌 타임리프의 유일한 자체 태그이다.  

타임리프의 특성상 HTML 태그안에 속성으로 기능을 정의해서 사용하는데, 다음과 같은 예시처럼 사용하기 애매한 경우에 ```<th:block>```을 사용한다.  
렌더링할 때 제거된다.  

```html
<th:block th:each="user : ${users}">
  <div>
  사용자 이름1 <span th:text="${user.username}"></span>
  사용자 나이1 <span th:text="${user.age}"></span>
   </div>
   <div>
  요약 <span th:text="${user.username} + ' / ' + ${user.age}"></span>
   </div>
</th:block>
```

------------------------------------------------------------------------------------------------------------------------------------------------

# 자바스크립트 인라인
타임리프는 자바스크립트에서 타임리프를 편리하게 사용할 수 있는 자바스크립트 인라인 기능을 제공한다.  
다음과 같이 적용하면 된다.  
```<script th:inline="javascript">```  

## 텍스트 렌더링
```var username = [[${user.username}]];```  
인라인 사용 전 -> ```var username = userA;```  
인라인 사용 후 -> ```var username = "userA";```  

인라인 사용 전 렌더링 결과를 보면 userA라는 변수 이름이 남아있다.  
타임리프 입장에서는 정확하게 렌더링 한 것이지만 개발자 입장에서는 "userA"라는 문자를 기대했을 것이다.  
결과적으로는 userA가 변수명으로 사용되어서 자바스크립트 오류가 발생한다.  
숫자의 경우에는 ```"```가 필요 없기 때문에 정상 렌더링 된다.  

인라인 사용 후 렌더링 결과를 보면 문자 타입인 경우 ```"```를 포함시켜준다.  
자바스크립트에서 문제가 될 수 있는 문자가 포함되어 있으면 이스케이프 처리도 해준다.  
```" -> \"```  

## 자바스크립트 내츄럴 템플릿
타임리프는 HTML 파일을 직접 열어도 동작하는 내츄럴 템플릿 기능을 제공한다.  
자바스크립트 인라인 기능을 사용하면 주석을 활용해서 이 기능을 사용할 수 있다.  

```var username2 = /*[[${user.username}]]*/ "test username";```  
인라인 사용 전 -> ```var username2 = /*userA*/ "test username";```  
인라인 사용 후 -> ```var username2 = "userA";```  

인라인 사용 전 결과를 보면 순수하게 해석을 하기에 내츄럴 템플릿 기능이 동작하지 않고 렌더링 내용이 주석처리 된다.  
인라인 사용 후 결과를 보면 주석 부분이 제거되고, "userA"가 정확하게 적용된다.  

## 객체
타임리프의 자바스크립트 인라인 기능을 사용하면 객체를 JSON으로 자동 변환해준다.  

```var user = [[${user}]];```  
인라인 사용 전 -> ```var user = BasicController.User(username=userA, age=10);```  
인라인 사용 후 -> ```var user = {"username":"userA","age":10};```  

인라인 사용 전은 객체의 toString()이 호출된 값이다.  
인라인 사용 후는 객체를 JSON으로 변환해준다.

## 자바스크립트 인라인 each
자바스크립트 인라인은 each를 지원하며, 다음과 같이 사용한다.  
```html
<!-- 자바스크립트 인라인 each -->
 <script th:inline="javascript">

    [# th:each="user, stat : ${users}"]
    var user[[${stat.count}]] = [[${user}]];
    [/]

 </script>
```

------------------------------------------------------------------------------------------------------------------------------------------------

# 템플릿 조각
웹 페이지를 개발할 때 공통 영역이 많이 존재한다.  
이런 부분을 코드를 복사해서 사용한다면 여러 페이지를 다 수정해야 하므로 상당히 비효율적이다.  
타임리프는 이런 문제를 해결하기 위해 템플릿 조각과 레이아웃 기능을 지원한다.

# 템플릿 레이아웃
템플릿 조각은 일부 코드 조각을 가지고 와서 사용했다면, 템플릿 레이아웃은 코드 조각을 레이아웃에 넘겨서 사용하는 방법이다.  
예를 들어서 ```<head>```에 공통적으로 사용하는 ```css, javascript```같은 정보들이 있는데 이러한 공통 정보들을 한 곳에 모아두고, 
공통으로 사용하지만 각 페이지마다 필요한 정보를 더 추가해서 사용하고 싶은 경우 사용한다.