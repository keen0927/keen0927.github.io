---
layout: post
title: Servlet, Tomcat WAS WAR
excerpt: "Java에서의 Servlet, WAS, WAR"
categories: [java]
comments: true
---

React.js를 스프링부트에 태워서 서버 구성을 하는 도중 생소한것들에 대해서 정리를 해보았다.

## 서블릿이란?

> 클라이언트 요청을 처리하고 그 결과를 다시 클라이언트에게 전송하는 <br> 서블릿 클래스의 규현 규칙을 지킨 자바 프로그래밍 기술

SPA에서 클라이언트의 요청시 기본 create-react-app이나 보일러플레이트로 구성하는 로컬 서버에서는 문제가 전혀 안되지만 리액트앱을 번들링하여 스프링부트쪽에서 요청시 라우터 맵핑이 필요하다. 안그러면 로컬호스트 3000에서 잘나오던 페이지가 톰캣서버를 띄워서 요청했을때 화이트 라벨 페이지가 나온다ㅠㅠ. 이부분은 JAVA를 모르면 서버개발자의 도움을 받는게 정신건강에 이롭다.

## 서블릿 동작 방식
1. 클라이언트가 URL을 요청하면 HTTP 리퀘스트를 Servlet Container로 전송한다.
2. 전송받은 Servlet Container는 HttpServletRequest, HttpServletResponse 두 객체를 생성한다.
3. web.xml은 요청한 URL을 분석후 어떤 서블릿에 요청을 한것인지 검색한다.
4. 해당되는 서블릿에서 service메소도를 호출하고 클라이언트의 GET, POST 요청에 따라 doGet(), doPost()를 호출한다.
5. 동적 페이지를 생성한 후 HttpServletResponse객체에 응답을 내보낸다.
6. 응답이 종료되면 HttpServletRequest, HttpServletResponse 두 객체를 destroy한다.

## W.A.S 란?

일단 web서버에 대해 알아보자. 웹서버는 HTTP프로토콜을 기반으로 클라이언트 요청을 서비스한다. 주 역할은 html, image, xml등에 대한 처리를 한다. <br>

Web Application서버도 알아보다. 여러 클라이언트의 요청을 web서버 혼자 감당하기는 힘들기 때문에 web서버 기능을 분리하기 위해 만들어진 것으로 Web Application Server(W.A.S)라고 한다. <br>

그럼 위 2개의 차이점은 무엇인가? <br>

크게 사용목적이 다르다. 웹서버는 html, image 요청을 처리하는것에 빠르고, was는 서블릿이나 jsp 비즈니스 로직을 수행하는데 적합하다. 다만 was는 web서버에 비해 html, image등의 요청에 느리다. 그래서 이 2가지의 강점을 조합해서 사용하기 위해 web서버와 was를 연동하여 서비스를 한다. 

## WAR란 ?
WAR 파일은 소프트웨어 공학에서 자바서버 페이지, 자바 서블릿, 자바 클래스, XML, 파일, 태그 라이브러리, 정적 웹 페이지 및 웹 애플리케이션을 함께 이루는 기타 자원을 한데 모아 배포하는데 사용되는 JAR 파일이다.
