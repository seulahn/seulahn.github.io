---
title: "[API Documentation] REST API ④ 헤더"
excerpt: "자주 쓰이는 요청 헤더와 응답 헤더 알아보기, 그리고 헤더를 표로 정리하기"

categories:
  - Writing
tags:
  - [Technical Writing, API, REST]

permalink: /writing/api-docs-rest-4

toc: true
toc_sticky: true

date: 2025-06-27
last_modified_at: 2025-06-27
---

이전 글에서 본 [HTTP 메서드](https://seulahn.github.io/tw/api-docs-rest-2)와 [쿼리 매개변수](https://seulahn.github.io/tw/api-docs-rest-3)는 모두 요청 메시지의 가장 첫 줄, 즉 요청 라인(request line)을 구성한다.

<img src="/assets/images/posts_img/2025-06-27-api-docs-rest-4/01-post-request-example.png" alt="REST 요청 예시" width="500">  

이제 다음 줄인 **HTTP 헤더(HTTP header)**로 넘어가 보자.

HTTP 헤더는 서버와 클라이언트가 주고받는 메시지 안에서 **부가 정보**를 담는 영역이다. 

헤더는 **요청과 응답 메시지 모두에** 포함될 수 있다. 따라서 API 통신을 정확히 이해하려면, 요청뿐만 아니라 응답에 쓰이는 헤더까지 함께 살펴볼 필요가 있다.

---

# HTTP 헤더란?

**HTTP 헤더**는 요청 또는 응답 메시지에 포함되어, 본문이 어떤 형식인지, 메시지를 누가 보내는지와 같은 **메타데이터**를 전달한다. 헤더는 `키:값` 형식으로 이루어진다.

예를 들어,
- 클라이언트는 전송할 데이터의 형식이나 인증 정보를 **요청 헤더**에 담아 서버에 전달하고,
- 서버는 **응답 헤더**를 통해 응답 데이터의 형식, 상태 등을 클라이언트에 알릴 수 있다.

헤더의 종류는 다양하지만, 이 글에서는 요청과 응답에 자주 쓰이는 헤더를 중심으로 살펴볼 예정이다.

---

# 주요 요청 헤더

**HTTP 요청에서 자주 사용하는 헤더**는 다음과 같다.  
이 외에도 API별 필요에 따라 다른 헤더를 사용하거나 커스텀 헤더를 정의해서 사용할 수 있다. 

| **헤더** | **설명** | **값 예시** |
| --- | --- | --- |
| `Host` | 요청 대상 서버의 호스트명과 포트 번호 | `www.example.com`, `localhost:8080` |
| `Authorization` | 사용자 인증 정보(예: 접근 토큰, API 키) | `Bearer eyJhbGciOiJIUzI1NiIsInR...` |
| `Content-Type` | 요청 본문의 데이터 형식 | `application/json` |
| `Accept` | 응답받고 싶은 데이터 형식 | `application/json`, `text/html` |
| `User-Agent` | 클라이언트의 소프트웨어 및 버전 정보 | `PostmanRuntime/7.31.3` |

*** 

## 🏠 Host

클라이언트의 **요청을 받을 서버의 호스트명과 (필요시) 포트 번호**를 지정하는 헤더다. HTTP/1.1부터는 해당 헤더를 **필수로 포함**해야 한다.
- 포트 번호를 생략할 경우, 요청한 서비스의 기본 포트가 자동으로 사용된다. 예를 들어 HTTPS 요청은 443번 포트, HTTP 요청은 80번 포트가 기본이다.

<mark><strong>형식</strong></mark>
- `Host: <호스트명>`
- 또는 `Host: <호스트명>:<포트 번호>`

<mark><strong>사용 예시</strong></mark>

  ```
  GET /index.html HTTP/1.1
  Host: www.example.com
  ```

***

## 🔐 Authorization

**사용자 인증 정보**를 담는 헤더다. 인증된 사용자임을 서버에 증명해야 하는 경우 사용한다.

<mark><strong>형식</strong></mark>
- `Authorization: <인증 방식> <인증 정보>`

<mark><strong>사용 예시</strong></mark>

- OAuth 2.0 인증 프로토콜의 **토큰 인증 방식**을 사용할 경우 
  
  `Bearer <접근 토큰>`
    
    ```
    Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6...
    ```
    
- **기본 인증 방식**을 사용할 경우 

  `Basic <인코딩된 사용자ID:비밀번호 문자열>` 
    
    ```
    Authorization: Basic dXNlcm5hbWU6cGFzc3dvcmQ=
    ```

***

## 📦 Content-Type

**요청 본문이 어떤 데이터 형식으로 이루어졌는지** 명시하는 헤더다.

- 요청 본문이 있는 `POST` 또는 `PUT` 요청에서 사용된다.
- `GET`이나 `DELETE`처럼 본문이 없는 요청에는 사용되지 않는다.

<mark><strong>형식</strong></mark>

- `Content-Type: <MIME 타입>`

> **Note:** **MIME 타입(Multipurpose Internet Mail Extensions type)**은 인터넷에서 데이터의 형식을 나타내는 문자열이다. `타입/서브타입` 구조로 이루어진다.
>
> - `application/json`(JSON 형식의 데이터)
> - `text/html`(HTML 문서)
> - `image/png`(PNG 이미지)

<mark><strong>사용 예시</strong></mark>

```
# 요청 본문이 JSON 형식의 데이터로 이루어짐

POST /api/user HTTP/1.1
Content-Type: application/json

{"name": "Seul"}
```

***

## 🎯 Accept

클라이언트가 **서버로부터 어떤 형식의 응답 데이터를 받고 싶은지 지정**하는 헤더다. 

- **주로** `GET` **요청에 사용**하지만, 응답 본문이 있는 모든 요청에서 사용할 수 있다.

<mark><strong>형식</strong></mark>

- `Accept: <MIME 타입>`
- 또는 `Accept: <MIME 타입1>[; q=<선호도>], <MIME 타입2>[; q=<선호도>], ...` (여러 형식을 지정하는 경우)

예시로 살펴보자.

<mark><strong>사용 예시</strong></mark>

- **단일 MIME 타입 지정**
    
    특정 형식의 응답만 받고 싶을 때
    
    ```
    # JSON 형식의 응답만 허용
    Accept: application/json 
    ```
    
- **여러 MIME 타입 지정**
    
    여러 형식 중 어느 형식이든 허용할 때(우선순위 지정 X)
    
    ```
    # JSON 또는 HTML 응답이면 허용
    Accept: application/json, text/html
    ```
    
- **MIME 타입 +** `q` **매개변수 사용**
    
    여러 형식 중 우선순위를 명시하고 싶을 때
    
    ```
    # JSON을 가장 선호하고, HTML은 그다음으로 선호
    Accept: application/json;q=1.0, text/html;q=0.8 
    ```

    > **Note:** 여기서 `q`는 품질 값(quality value)으로, 각 형식에 대한 선호도를 나타낸다. 값은 `0.0`(가장 낮음)부터 `1.0`(가장 높음)까지 설정 가능하며, 생략 시 기본값은 `1.0`이다.
    > 

- **모든 형식 허용**
    
    ```
    # 어떤 형식이든 응답 가능
    Accept: */*
    ```
    
- **특정 형식의 모든 하위 형식 허용**
    
    ```
    # 이미지라면 어떤 서브타입이든 허용(PNG, JPG 등)
    Accept: image/*  
    
    # 텍스트라면 어떤 서브타입이든 허용(HTML, plain text 등)
    Accept: text/*
    ```
    

***

## 👤 User-Agent

**클라이언트의 소프트웨어 및 환경 정보를 서버에 전달**하는 헤더다.

<mark><strong>형식</strong></mark>

일반적으로 아래 형식을 따른다.
- `User-Agent: <클라이언트 이름>/<버전> (<OS 정보>) [기타 정보]`

<mark><strong>사용 예시</strong></mark>

```
# 브라우저 정보
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7)

# API 클라이언트 정보
User-Agent: PostmanRuntime/7.31.3
```

---

# 주요 응답 헤더

반면, 서버가 클라이언트에게 응답할 때 자주 사용하는 **응답 헤더**는 다음과 같다.

| **헤더** | **설명** | **값 예시** |
| --- | --- | --- |
| `Content-Type` | 응답 본문의 데이터 형식 | `application/json` |
| `Cache-Control` | 캐싱 정책 제어 | `no-cache`, `max-age=3600` |
| `Set-Cookie` | 클라이언트에 쿠키 전송 | `sessionId=abc123; Path=/; HttpOnly` |
| `Server` | 서버의 소프트웨어 정보 | `nginx/1.18.0`, `Apache/2.4.54` |

*** 

## 📦 Content-Type

**응답 본문이 어떤 데이터 형식으로 이루어졌는지** 명시한다.

<mark><strong>형식</strong></mark>

- `Content-Type: <MIME 타입>`

<mark><strong>사용 예시</strong></mark>

```
# 응답 본문이 JSON 형식의 데이터로 이루어짐

HTTP/1.1 200 OK
Content-Type: application/json

{
  "id": 101,
  "name": "Seul",
  "email": "seul@example.com"
}
```

***

## 🗂️ Cache-Control

응답 데이터에 대한 **캐싱 전략을 제어**하는 헤더다. 클라이언트 또는 중간 캐시 서버가 응답을 저장할 수 있는지, 그리고 저장 기간은 얼마인지 등을 지시한다.

<mark><strong>형식</strong></mark>

- `Cache-Control: <지시어1>, <지시어2>, ...`

<mark><strong>주요 지시어(directive)</strong></mark>

- `max-age=<숫자>`: 캐시 유효 시간(초 단위)
- `no-cache`: 캐시를 생성할 수 있지만, 재사용 전 반드시 서버에 검증해야 함
- `no-store`: 캐시 금지(민감한 정보일 때 사용)
- `public`: 응답을 모든 캐시(공용 포함)에 저장할 수 있음
- `private`: 응답을 개별 사용자 전용 캐시에만 저장할 수 있음

<mark><strong>사용 예시</strong></mark>

```
# 응답을 1시간 동안 캐시 가능하며, 공용 캐시 서버에도 응답을 저장할 수 있음
Cache-Control: max-age=3600, public
```

***

## 🍪 Set-Cookie

서버가 **클라이언트에게 쿠키를 어떻게 저장할지 지시**하는 헤더다. 

<mark><strong>형식</strong></mark>

- `Set-Cookie: <쿠키 이름>=<쿠키 값>; <속성1>; <속성2>; ...`

<mark><strong>주요 속성(attribute)</strong></mark>

- `expires=<날짜>`: 쿠키 만료 시점
- `max-age=<숫자>`: 쿠키 유효 시간(초 단위)
- `domain=<도메인 값>`: 쿠키가 전송될 호스트
- `HttpOnly`: Javascript로 쿠키에 접근 불가
- `secure`: HTTPS 프로토콜을 사용할 때만 쿠키 전송 허용
- `path=<경로 값>`: 쿠키가 유효한 경로 지정(경로 값은 `/`로 시작)

<mark><strong>사용 예시</strong></mark>

```
# 사이트 전체 경로에서 유효한 세션 쿠키를 설정하며, HTTPS에서만 전송되고 JavaScript에선 접근 불가
Set-Cookie: sessionId=abc123xyz; Path=/; HttpOnly; Secure
```

***

## 🖥️ Server

**서버의 소프트웨어 정보**를 나타내는 헤더다.

```
Server: nginx/1.18.0
```

---

# HTTP 헤더 문서화 방법

API 문서에서 HTTP 헤더를 정리하는 방법은 이전에 다룬 쿼리 매개변수와 비슷하다.

각 헤더가 어떤 목적을 두고 있는지, 허용되는 값은 무엇인지, 기본값이 있는지 등의 정보를 명확하게 설명하는 것이 좋다.

| **헤더** | **설명** | **필수 여부** | **값** |
| --- | --- | --- | --- |
| <img width=60/> | <img width=130/> | <img width=60/> | <img width=100/> |
| <img width=60/> | <img width=130/> | <img width=60/> | <img width=100/> |

<br>

예를 들어, 사용자가 자신의 계정 정보를 수정할 수 있는 기능을 제공하는 API가 있다고 가정하자.  
그리고 아래 정보를 고려해, 계정 정보 수정을 요청할 때 사용할 수 있는 헤더를 정리해 보자.

- 서버 주소는 `https://api.example.com/v1/`
- 메서드는 `PATCH`이며, 리소스 경로는 `/users/{userId}`
- 인증된 사용자만 자신의 정보를 수정할 수 있으므로, 반드시 인증이 필요하다.
    - `Authorization` 헤더에 Bearer 토큰을 명시해야 한다.
- `Content-Type` 헤더를 사용해 데이터 형식을 명시해야 한다.
    - 요청 본문은 JSON 형식으로 전송된다.
- 응답 형식을 지정하고 싶다면, `Accept` 헤더를 사용할 수 있다.
    - `application/json`이나 `application/xml` 중에서 선택할 수 있다. 지정하지 않을 경우 기본값은 `application/json`이다.

<br>

**정리한 결과는 다음과 같다.**

| **헤더** | **설명** | **필수 여부** | **값** |
| --- | --- | --- | --- |
| `Authorization` | 사용자 인증 정보(Bearer 토큰 값) | 필수 | 예시: `Bearer <access_token>` |
| `Content-Type` | 요청 본문의 데이터 형식 | 필수 | `application/json` |
| `Accept` | 응답 본문의 데이터 형식 지정 | 선택 | 예시: `application/json`, `application/xml` <br> 기본값: `application/json` |

<br>

실제 API 레퍼런스 문서를 살펴보면, 이 외에도 어떤 다양한 방식으로 HTTP 헤더를 정리하고 설명하고 있는지 확인할 수 있다.

- 카카오 T 예시([출처](https://logistics-developers.kakaomobility.com/document/tutorial))
  <img src="/assets/images/posts_img/2025-06-27-api-docs-rest-4/02-kakao-t-headers-example.png" alt="카카오 T의 헤더 문서화 예시" width="500">  

- 신한은행 예시([출처](https://api.shinhan.com/shbaas/guide/deveGuide))
  <img src="/assets/images/posts_img/2025-06-27-api-docs-rest-4/03-shinhan-bank-headers-example.png" alt="신한은행의 헤더 문서화 예시" width="500">

---
 
**참고자료**

- *Udemy - [Learn API Technical Writing 2: REST for Writers](https://www.udemy.com/course/learn-api-technical-writing-2-rest-for-writers/)*
- *Geeks for Geeks - [HTTP headers  Accept](https://www.geeksforgeeks.org/computer-networks/http-headers-accept/)*
- *Geeks for Geeks - [HTTP headers  Content-Type](https://www.geeksforgeeks.org/html/http-headers-content-type/)*
- *Lonti - [Using query parameters and headers in REST API design](https://www.lonti.com/blog/using-query-parameters-and-headers-in-rest-api-design)*
- *Postman - [What are HTTP headers?](https://blog.postman.com/what-are-http-headers/)*
- *Requestly - [What are HTTP Headers & Understand different types of HTTP headers](https://requestly.com/blog/what-are-http-headers-understand-different-types-of-http-headers/)*