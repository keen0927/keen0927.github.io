---
layout: post
title: CORS, CORB 해결 (feat. S3 & CLOUD FRONT)
excerpt: "AWS의 클라우드프론트에서의 CORS 문제 해결에 대한 설명"
categories: [aws]
comments: true
---

## CORS란 무엇인가?
>간단하게 설명하자면 자신의 서버에 있는 자원이 아닌 다른 서버, 즉 도메인이 다른곳에서 자원을 가져올때 에러가 나는것을 말한다. [CORS 설명](https://developer.mozilla.org/ko/docs/Web/HTTP/Access_control_CORS)

주로 프론트엔드 단에서는 로컬 개발 포트와(localhost:8080 등) 백엔드 통신을 할때 자주 발생한다. 물론 로컬개발에서 에러가 발생하면 프로덕션에서도 같이 발생한다

## 해결하기 전
해결방법은 서버에서 접근제한을 허용하거나 사전 리퀘스트를 날려 확인을 하는방법등 몇가지가 있지만 그부분은 정리가 잘된 블로그 글들이 많기 때문에 클라우드 프론트에서 발생했을때의 해결방법을 설명하겠다.

## 에러 예시
자신이 프론트프레임워크(React등)를 사용하여 로컬서버를 띄우고 가져와야할 데이터(JSON)가 클라우드프론트에 있을때.
물론 데이터를 백엔드서버(PHP, JAVA)에서 가져올땐 프론트단에서 프록시 설정을 하거나 백엔드 쪽에서 접근제한을 협의해서 해결 할 수 있다. 하지만 AWS CLOUDFRONT에서는 콘솔에 접근하여 몇가지만 추가하면 끝이 난다.

## 해결
1. 콘솔 접속 S3 > 권한 > CORS구성 > 다음 코드 추가
```XML
<CORSConfiguration>
    <CORSRule>
        <AllowedOrigin>*</AllowedOrigin>
        <AllowedMethod>GET</AllowedMethod>
        <MaxAgeSeconds>3000</MaxAgeSeconds>
        <AllowedHeader>*</AllowedHeader>
    </CORSRule>
</CORSConfiguration>
```

2. 클라우드프론트 > Origin Access Identity 에서 Amazon S3 mazon S3 Canonical User ID를 복사
3. S3 > 권한 > 버킷 정책에 다음 코드 추가
```JS
{
    "Version": "2008-10-17",
    "Id": "PolicyForCloudFrontPrivateContent",
    "Statement": [
        {
            "Sid": "1",
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::cloudfront:user/CloudFront Origin Access Identity ??????",
                "CanonicalUser": "여기에 복붙"
            },
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::dev.service.promotion/*"
        }
    ]
}
```
4. CloudFront > Origin Setting(선택한 CF의 origins and origin groups 탭에서 도메인 선택후 Edit으로 이동 후 Origin Access Identify를 'Use an Existing Identity'로 선택 한 뒤 이미 생성해 두었던 Origin Access Identity를 선택하여 설정한다.
5. CloudFront > Behavior
이번엔 CloudFront의 Behavior 탭을 선택한 후 CORS 설정을 한 S3 Origin을 선택한 후 Edit 버튼을 눌러 CloudFront의 Caching 처리 방식을 변경해야 한다.<br/>
Cache Based on Selected Request Headers를 "None"에서 "Whitelist"로 변경하고 아래와 같이 3개의 헤더를 추가한다. 
```HTML
Access-Control-Request-Headers
Access-Control-Request-Method
Origin
```
6. 끝!

글을 마치며 S3에서 서비스할 경우 위 설명의 1번만 실행하면 해결이 되지만 클라우드프론트에서 서비스가 될때에는 번거롭지만 몇개의 수고를 해야 한다는 점을 유의하자.