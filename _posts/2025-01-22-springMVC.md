---
title: "Spring MVC"
name: 꼽사리구나
writer: 꼽사리구나
categories: [Spring, MVC]
tags:
  - [Spring, MVC]
toc: true
toc_sticky: true
date: 2025-01-22
last_modified_at: 2025-01-22
---

------------------------------------------------------------------------------------------------------------------------------------------------

# <span style="color:green">Spring MVC</span>
MVC2 모델이 기반인 웹 모듈  
프론트 컨트롤러(Front Controller)가 우선적으로 클라이언트로부터 모든 요청을 받게 되며, 실제 요청의 처리는 개별 컨트롤러 클래스로 위임을 한다.  
개별 컨트롤러 클래스는 핸들러(Handler)라고도 하며, DI를 통해 생성해둔 Bean을 통해 비즈니스 로직 처리 결과를 Model에 담아 다시 프론트 컨트롤러로 보낸다.  
프론트 컨트롤러는 받은 Model을 알맞은 View 템플릿으로 전달하여 반영시키고, 최종적으로 클라이언트로 보낼 화면을 응답 결과로 전송하는 것이다.  

# <span style="color:green">Spring MVC 구조</span>
- DispatcherServlet
  - Front Controller 역할을 수행하며 Request를 각각의 Controller에게 위임한다.
  - 가장 앞 단에서 클라이언트의 요청을 처리하는 Controller로써 요청부터 응답까지 전반적인 처리 과정을 통제한다.
- HandlerMapping
  - 요청을 직접 처리할 컨트롤러를 탐색한다.
  - 구체적인 맵핑은 xml파일이나 java config 관련 어노테이션 등을 통해 처리할 수 있다.
- HandlerAdapter
  - 매핑된 컨트롤러의 실행을 요청한다.
- Controller
  - DispatcherServlet이 전달해준 HTTP요청을 처리하고 결과를 Model에 저장한다.
  - 직접 요청을 처리하며, 처리 결과를 반환한다.
  - 결과가 반환되면 HandlerAdapter가 ModelAndView 객체로 변환되며, 여기에는 View Name과 같이 응답을 보여줄 View에 대한 정보와 관련된 데이터가 포함되어 있다.
- ModelAndView
  - Controller에 의해 반환된 Model과 View가 Wrapping된 객체이다.
- View Resolver
  - View Name을 확인한 후, 실제 Controller로부터 받은 로직 처리 결과를 반영할 View 파일을 탐색한다.
- View
  - 로직 처리 결과를 반영한 최종 화면을 생성한다.

# <span style="color:green">Spring MVC 동작 구조</span>
![Image](https://github.com/user-attachments/assets/d0c77ae8-b8a3-4176-9775-d208ac9989eb)  
1. 클라이언트가 서버에 요청을 하면, DispatcherServlet 클래스가 요청을 받는다.
2. DispatcherServlet은 프로젝트 파일 내의 servlet-context.xml 파일의 @Controller 인자를 통해 등록한 요청 위임 컨트롤러를 찾아 맵핑된 컨트롤러가 존재하면 @RequestMapping을 통해 요청을 처리할 메소드로 이동한다.
3. 컨트롤러는 해당 요청을 처리한 Service를 받아 비즈니스 로직을 서비스에게 위임한다.
4. 서비스는 요청에 필요한 작업을 수행하고, 요청에 대해 DB에 접근해야 한다면 DAO에 요청하여 처리를 위임한다.
5. DAO는 DB정보를 DTO를 통해 받아 서비스에게 전달한다.
6. 서비스는 전달받은 데이터를 컨트롤러에게 전달한다.
7. 컨트롤러는 Model 객체에게 요청에 맞는 View 정보를 담아 DispatcherServlet에게 전송한다.
8. DispatcherServlet은 View Resolver에게 전달받은 View 정보를 전달한다.
9. View Resolver는 응답할 View에 대한 파일을 찾아 DispatcherServlet에게 전달한다.
10. DispatcherServlet은 응답할 뷰에게 렌더링을 지시하고 뷰는 로직을 처리한다.
11. DispatcherServlet은 클라이언트에게 렌더링된 뷰를 응답하며 요청을 마친다.

------------------------------------------------------------------------------------------------------------------------------------------------
`출처`  
<https://velog.io/@do_dam/Spring-MVC%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80-%EC%8A%A4%ED%94%84%EB%A7%81-MVC-%EA%B5%AC%EC%A1%B0-%EC%9D%B4%ED%95%B4>