---
layout: post
title: AWS Elastic beanstalk에 대하여
excerpt: "블루,그린 배포등 AWS에서 제공하는 어플리케이션 배포에 관한내용"
categories: [AWS]
comments: true
---

## 왜 쓸까요?
- 코드의 배포, 프로비저닝, 관리가 복잡
- 서버, DB, 로드밸런서, 방화벽 그리고 복잡한 네트워크를 구성하고 관리하는데 전문성과 시간이 필요
- 어플리케이션의 스케일 아웃/인을 자동화 해야하는데 이걸 어떻게 하면 좋을지
- 팀내/팀간 갈등

**이런것들을 해결하려 나왔습니다**

## 인프라의 구성만 놓고 봤을때 
- on-premise에서 수동 구성 하나부터 열까지 다해야함. 
- 하지만 클라우드를 이용해서 한다면 AWS EC2등에서 사용한다면 OS설치단계 까지는 벤더에 맡기고 그위에 어떤 것을 설치할지 우리가 결정
- 그럼에도 불구하고 절반정도의 공수는 남아있다
- 이걸 더 줄이기 위해 나온것이 엘라스틱 빈스톡이다
- 어플리케이션 프로토타입이 나왔을때 어떤 인프라에 배포시 원-클릭 배포
- 프로덕션에서도 사용이 가능하다
- 시간 + 돈 절감
- 빠르고 간단한 시작
- 개발자의 생산성이 높아진다
- 완전한 자원 제어 (모든 인스턴스에 SSH에 접속가능해서 제어가능)
- 불필요한 자원 낭비 없음 사용한 만큼만 비용 지불
- 사용에 따른 추가 요금이 없음 (사용되는 AWS 리소스 (EC2, S3등)에 대해서만 비욜을 지불

## 살펴보자
- 웹 어플리케이션/ 웹 서비스를 배포하고, 확장하고, 관리하는데 있어 쉽고 빠르게 할 수 있도록 돕는 완전 관리형 서비스이다.
- AWS에서 관리형서비스를 제공하기 때문에 개발자는 코드레벨에서만 집중하면된다

## 사용사례
- 웹사이트 - 프론트엔드 단
- 백엔드 - 다양하게
- API

## 인프라스트럭처 스택 구성 과정
- 1. 특정한 플랫폼을 선택하고 로드 밸런서(스케일 아웃, 스케일 인)를 더하고 오토 스케일링 그룹도 설정, 인스턴스 배치(EC2) 이것들을 전부 연결, End User가 붙는 DNS서비스를 사용함(예 router53)  로그와 앱 버전들에 대한 설정은 모두 S3에 둠
- 2. 사용자는 애플리케이션 작성(코드레벨)에만 집중 
- 3. 다양한 플랫폼 지원

바구니에 패키지를 담는다는 느낌으로 꽂아(애플리케이션을) 사용할 수 있다.

## 어플리케이션 배포에 필요한 정보
- 1. 코드
- 2. 리전 (이디서 운영할지)
- 3. 스택타입 (플랫폼 타임 node.js java 등)
- 4. 단일 인스턴스(1개에 올릴지 - 로그인 테스트를 해보고 싶다 등) or 오토스케일링, 로드 밸런싱(여러곳에 올릴지 - 로드를 테스트해보고싶다, 스트레스 제너레이션, 운영 등)
- 5. 데이터베이스 (반드시 필요한것 아님)

## 배포 옵션
- all at once : 모든 인스턴스에 동시에 새 버전 배포
- Rolling: 배치 단위로 새 버전배포 
- Rolling with additional batch: 배치 단위로 새 버전 배포, +1 추가 배치 (capacity 100%유지하고 싶을 때)
- Immutable: 새로운 인스턴스 그룹에 배포 (블루 그린 배포와 개념이 다른게 있음. 오토스케일링 그룹을 하나 만들어서 v2를 하나 붙여서 정상 작동하면 나머지를 띄우고 기존 v1은 보냄. )

## 블루/그린 배포
- clone Enviremonet 메뉴를 클릭하면 복제된 새로운 환경이 구성됨. dns이름만 하나 추가 해줌
- 기존에 있던 환경은 내버려두고 복제된 Environment에서 v2테스트 및 나머지 진행후 이상없으면
- swap Environment URLS 를 클릭하면 v1, v2의 URL이 서로 변경됨
- 브라우저 캐쉬에 대한 문제가 있지만 클라이언트에서 처리해주면 됨

#### 장점
- 이전 환경이 여전히 실행 중, 언제든 빠른 롤백 가능 (스왑 버튼만 누르면 됨)
- 다운타임 없이 배포 가능
- 새로운 환경 배포가 실패하더라고 기존 인스턴스에 영향 X

#### 단점
- 새로운 환경 생성으로 인해 느린 배치 (5분)
- RDS까지 구축되어 있는 경우 RDS는 별도 복제 필요 (자동으로 clone안됨 - 수동으로 스냅샷 떠서 연결을 해줘야 함)

> [위 모든 내용은 링크 동영상에 존재 합니다](https://www.youtube.com/watch?v=AfRnvsRxZ_0)