---
layout: post
title: React & Springboot VSCODE에서 환경셋팅
excerpt: "Springboot 환경에서 React를 올려서 사용하는 초기 셋팅에 대한 설명."
categories: [react, java]
comments: true
---

### 개요
Springboot 환경에서 React를 올려서 사용하는 초기 셋팅에 대한 설명.

### Springboot에 React를 태운 이유
회사 프로젝트에서 보안을 강조하여 그런 위험성을 보안하고자 client -> server -> api server 의 3way구조 방식으로 설계.

- client side와 server side security
- 화면을 처리하는 행위와 서버와 통신하는 행위에 대한 분리
- Gateway API의 주소가 노출방지.
- 요청 시 access token 과 refresh token 의 웹기반 노출 방지.
- 프론트 서버간 통신의 보안 이슈 강화

### 설치순서

1. VSCODE에 다음의 EXTENSION을 설치한다. (기본적으로 VSCODE, Node.js, JDK 설치된 상태여야 함)

2. **Java Extension Pack** / **Spring Boot Extension Pack** (Pivotal 제작)

3. VSCODE의 설정창에서 **java.home**을 검색하여 사용자설정에 **java.home** 추가 및 자신의 자바 경로로 변경한다.

3-1. java home 위치는 다음 명령어로 찾는다.
```js
$ cd /Library/Java/JavaVirtualMachines/ 
$ ls // 나오는 디렉토리로 이동후
$ pwd // 나오는 루트 복사
```

3-2. setting.json에 추가하기
```html
"java.home" : "위의 경로를 붙여 넣기",
"code-runner.runInTerminal": true,
"java.errors.incompleteClasspath.severity": "ignore"
```

4. F1을 눌러 Spring Initalizr: Generate a Gradle Project 선택

5. Group ID / Artifact ID 를 입력.

6. Spring Boot 버전선택 (최신버전으로)

7. 의존성 선택은 필요한것으로 선택하면 된다. **(필수 선택 DevTools / Web - Tomcat)**

8. 설치할 디렉토리를 지정하거나 새로만들어서 지정.

9. React-create-APP front-end 명령어로 front-end폴더에 react 설치 (front-end는 폴더명으로 입맛대로 변경)

10. 설치후 front-end 디렉토리로 이동 후 yarn eject 를 실행하여 webpack설정을 꺼내옴.

11. front-end/config/paths.js 에서 appBuild의 경로를 수정 

{% highlight javascrot %}
appBuild: resolveApp('../src/main/resources/static'),
{% endhighlight %}

12. **yarn build** 명령어를 실행하여 빌드 결과물을 11번의 경로로 output.

13. 스프링부트를 실행 후 로컬호스트의 8080포트로 들어가서 렌더링 된 화면을 확인

14. 끝

### 정리
로컬 개발 시 front-end 디렉토리 에서 **yarn start**를 통해 로컬 개발을 한다.<br>
빌드 배포는 배포 스크립트에 yarn build를 포함하여 작성을 하여 처리.

### 추가 GIT 대응
springboot의 루트 디렉토리에서 git init을 했을 때 front-end 디렉토리는 create-react-app으로 설치를 하여서 해당 디렉토리또한 git이 적용된 상태이므로 git을 삭제해야 함.

{% highlight bash %}
rm -rf .git
{% endhighlight %}

front-end 디렉토리에서 위의 명령어로 git을 삭제 후 springboot 루트 디렉토리의 gitignore에서 /front-end/node_modules 을 추가 해준다.