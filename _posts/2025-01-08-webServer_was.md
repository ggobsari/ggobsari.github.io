---
title: "웹 서버와 WAS"
name: 꼽사리구나
writer: 꼽사리구나
categories: [CS, WAS]
tags:
  - [CS, WAS]
toc: true
toc_sticky: true
date: 2025-01-08
last_modified_at: 2025-01-08
---

------------------------------------------------------------------------------------------------------------------------------------------------

# <span style="color:green">Web Server</span>
***웹 브라우저 클라이언트로부터 HTTP 요청을 받아들이고 HTML 문서와 같은 웹 페이지를 반환하는 컴퓨터 프로그램***  

웹 서버란 클라이언트가 웹 브라우저에서 어떠한 페이지 요청을 하면 웹 서버에서 그 요청을 받아 <u>정적 컨텐츠</u>를 제공하는 서버다.  
그렇다고 정적 컨텐츠만 제공하는 것이 아니라 웹 서버가 동적 컨텐츠를 요청 받으면 WAS에게 해당 요청을 넘겨주고, WAS에서 처리한 결과를
클라이언트에게 전달해주는 역할도 한다.  
대표적인 웹 서버로는 Apache, Nginx가 있다.  
```정적 컨텐츠 : 단순 HTML문서, CSS, javascript, 이미지, 파일 등 즉시 응답 가능한 컨텐츠```  
![webServer](https://github.com/user-attachments/assets/867b053a-7ead-43df-a8d5-2ba3a4687040)


# <span style="color:green">WAS</span>
***인터넷 상에서 HTTP 프로토콜을 통해 사용자 컴퓨터나 장치에 애플리케이션을 수행해주는 미들웨어로서, 주로 동적 서버 컨텐츠를 수행하는 것으로 웹 서버와 구별이 되며, 주로 데이터베이스 서버와 같이 수행***  

WAS는 웹 서버와 웹 컨테이너가 합쳐진 형태로써, 웹 서버 단독으로는 처리할 수 없는 <u>데이터베이스의 조회나 다양한 로직 처리가 필요한 동적 컨텐츠를 제공한다.</u>  
덕분에 사용자의 다양한 요구에 맞는 웹 서비스를 제공할 수 있다.  
WAS는 JSP, Servlet 구동환경을 제공해주기 때문에 웹 컨테이너 또는 서블릿 컨테이너라고도 불린다.  
대표적인 WAS 종류로는 Tomcat, Jetty, Undertow가 있다.  
```웹 컨테이너 : 웹 서버가 보낸 JSP, PHP 등의 파일을 수행한 결과를 다시 웹 서버로 보내주는 역할```  
```컨테이너 : JSP, Servlet을 실행시킬 수 있는 소프트웨어```  
![was](https://github.com/user-attachments/assets/838266f9-b40d-4088-a5cd-94c5e5ff45cb)  


# <span style="color:green">Web Service Architecture</span>
웹 어플리케이션은 요청 처리 방식에 따라 다양한 구조를 가질 수 있다.  

- 클라이언트 -> Web Server -> DB
- 클라이언트 -> WAS -> DB
- 클라이언트 -> Web Server -> WAS -> DB 
  
주로 사용하는 아키텍처는 3번째 아키텍처이다.  

![webServiceArchitecture](https://github.com/user-attachments/assets/816e3fad-43b8-44d5-bef1-4c110ba55064)  

<span style="color:red">클라이언트 -> Web Server -> WAS -> DB 구조의 동작 과정</span>  

1. Web Server는 웹 브라우저 클라이언트로부터 HTTP 요청을 받는다.
2. Web Server는 클라이언트 요청을 WAS에 보낸다.
3. WAS는 관련된 Servlet을 메모리에 올린다.
4. WAS는 web.xml을 참조하여 해당 Servlet에 대한 쓰레드를 생성한다. (Thread Pool 이용)
5. HttpServletRequest와 HttpServletResponse 객체를 생성하여 Servlet에 전달한다.
   1. 쓰레드는 Servlet의 service() 메소드를 호출한다.
   2. service() 메소드는 요청에 맞게 doGet() 또는 doPost() 메소드를 호출한다.
6. protected doGet(HttpServletRequest request, HttpServletResponse response)
7. doGet() 또는 doPost() 메소드는 인자에 맞게 생성된 적절한 동적 페이지를 Response 객체에 담아 WAS에 전달한다.
8. WAS는 Response 객체를 HttpResponse 형태로 바꾸어 Web Server에 전달한다.
9. 생성된 쓰레드를 종료하고, HttpServletRequest와 HttpServletResponse 객체를 제거한다.


<span style="color:red">그렇다면 WAS가 Web Server의 기능을 모두 수행하면 되지 않을까? No</span>  

- 기능을 분리하여 서버 부하 방지
  - WAS는 DB 조회나 다양한 로직을 처리하느라 바쁘기에 단순한 정적 컨텐츠는 Web Server에서 빠르게 클라이언트에 제공하는 것이 좋다.
  - WAS는 기본적으로 동적 컨텐츠를 제공하기 위해 존재
  - 정적 컨텐츠 request까지 처리한다면 정적 데이터 처리로 인해 부하가 커지게 되고, 동적 컨텐츠의 처리가 지연됨에 따라 수행 속도 저하
  - 즉, 페이지 노출 시간 지연
- 물리적으로 분리하여 보안 강화
  - SSL에 대한 암복호화 처리에 Web Server를 사용
- 여러 대의 WAS를 연결 가능
  - Load Balancing을 위해서 Web Server를 사용
  - 특히 대용량 웹 어플리케이션의 경우 Web Server와 WAS를 분리하고, 여러개의 서버를 사용함으로써 무중단 운영을 위한 장애 극복에 쉽게 대응 가능
- 여러 웹 어플리케이션 서비스 가능

<u>즉, 자원 이용의 효율성 및 장애 극복, 배포 및 유지보수의 편의성을 위해 Web Server와 WAS를 분리</u>  


# <span style="color:green">나만의 요약</span>
Web Server는 정적 리소스를 제공, WAS는 애플리케이션 로직까지 실행 가능.  
하지만 Web Server도 프로그램 실행하는 기능이 있고, WAS도 Web Server의 기능을 제공하기는 한다.  
차이점이라고 한다면 서블릿 컨테이너의 유무와 WAS는 애플리케이션 로직을 실행하는데 더 특화되어 있다고 우선 정리해본다.  


-----------------------------------------------------------------------------------------------------------------------------------------------


```참조```  
<https://gmlwjd9405.github.io/2018/10/27/webserver-vs-was.html>  
<https://velog.io/@leesomyoung/%EC%9B%B9-%EC%84%9C%EB%B2%84-vs-WAS>