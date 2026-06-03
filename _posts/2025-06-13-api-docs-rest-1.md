---
title: "[API Documentation] REST API ① 특성과 구성 요소, 그리고 리소스"
excerpt: "오늘날 가장 널리 쓰이는 웹 API인 REST API를 알아보자." 

categories:
  - Writing
tags:
  - [Technical Writing, API, REST]

permalink: /writing/api-docs-rest-1

toc: true
toc_sticky: true

date: 2025-06-13
last_modified_at: 2025-06-13
---

API는 서로 다른 소프트웨어가 소통하고 데이터를 주고받는 방법을 뜻한다. 이런 API를 어떻게 사용하고 요청해야 하는지 정리한 문서가 바로 **API 문서**다. 개발자가 API를 이해하고 활용하는 데 꼭 필요한 안내서 역할을 한다.  

API 문서를 읽고 활용하려면, 우선 **REST API**가 어떻게 작동하는지 이해해야 한다. 오늘날 REST API는 웹 API에서 가장 널리 쓰이는 접근방식이기 때문이다.  

이번 글을 시작으로 REST API의 기본 개념과 구성 요소를 살펴보고, 나아가 이를 어떻게 문서화할 수 있는지까지 알아보려 한다.

---

# REST API란?

**REST(REpresentational State Transfer) API**는 REST라는 아키텍처 스타일을 따르는 API다. REST는 **클라이언트-서버** 구조를 기반으로 하며, 이 둘이 **HTTP 프로토콜**을 사용해 데이터를 주고받는다.

REST에 관해 알아두면 좋을 특성이 몇 가지 있다.
    
- REST는 **리소스(resource)**를 중심으로 한다. 리소스란 클라이언트가 접근할 수 있는 데이터의 단위를 말한다. 예를 들어, 학습 앱에서는 '강사', '학생', '강의', '과제' 등이 모두 리소스가 될 수 있다.
    
    > REST에서는 **URL**을 사용해 어떤 리소스에 접근할지 지정하고, 무엇을 할지는 **HTTP 메서드**로 표현한다. 자세한 내용은 아래에서 살펴보겠다.
    > 
- REST는 **무상태(stateless)**하다는 특징이 있다. 이는 이전에 클라이언트에서 어떤 요청을 했는지 서버가 저장하지 않는다는 의미다. 따라서, 클라이언트는 요청마다 필요한 정보를 모두 담아서 보내야 한다.

- REST는 프로토콜이 아닌 **디자인 패턴**이다. 프로토콜이 엄격한 규약이라면, 디자인 패턴은 가이드라인에 가깝다.
    
    > 과거에는 **SOAP(Simple Object Access Protocol)** 이라는 프로토콜이 주로 사용됐다. SOAP는 [XML 형식](https://seulahn.github.io/tw/api-docs-xml-intro)만을 사용하며, 구조도 복잡한 편이다. 오늘날에는 보다 단순하고 유연한 REST 방식이 웹 API에서 더 많이 쓰이고 있다.

- REST API는 다양한 형태의 데이터를 주고받을 수 있다. **주로 [JSON 형식](https://seulahn.github.io/tw/api-docs-json)을 사용**하지만, 필요에 따라 XML, 이미지, 영상 및 그 외 다양한 파일도 주고받을 수 있다.

---

# REST API 요청 예시와 구성 요소

앞서 봤듯이 REST API는 클라이언트와 서버 간 통신으로 이루어지며, 그 시작은 클라이언트가 서버에 **요청**을 보내는 것이다. 이러한 요청을 구성하는 요소를 설명하기 전에, 실제 요청이 어떻게 생겼는지 먼저 확인해 보자.

챗GPT에 **여행 예약 시스템의 API**를 예시로 두 가지 시나리오를 만들어 달라고 해보았다.

<mark><strong>시나리오 1</strong>: 사용자가 다음 조건으로 항공권을 검색한다.</mark>
- 출발 도시: 서울
- 도착 도시: 도쿄
- 출발일: 2025년 7월 1일
- 결과: 최대 5개 제한  
- API의 베이스 URL은 `https://api.travelmate.com` 이라고 가정한다.

이때 서버로 전송되는 HTTP 요청은 다음과 같다.

<img src="/assets/images/posts_img/2025-06-13-api-docs-rest-1/01-get-request-example.png" alt="GET 요청 예시" width="600">


복잡해 보이지만, 크게 보면 **HTTP 메서드**와 **요청 URL**로 구성되어 있다. 또 요청 URL은 베이스 URL, 엔드포인트 경로 및 쿼리 스트링으로 나뉘어 있다.
  
- **HTTP 메서드(HTTP method)**: <mark>리소스로 뭘 할지</mark> 지시하는 명령어다.  
    여기에 쓰인 `GET`은 이러한 메서드 중 하나로, 서버에 특정 리소스의 데이터를 조회해 달라고 요청한다.
- **베이스 URL**: 서버의 기본 주소로, 요청 URL의 출발점 역할을 한다.
- **엔드포인트 경로(endpoint path)**: 서버에서 <mark>어떤 리소스에 접근할지</mark> 지정한다.  
    이 예에서는 `/flights`로 '항공편 목록'이라는 리소스를 지정했다.
- **쿼리 스트링(query string)**: 리소스에서 <mark>어떤 조건에 맞는 데이터를 받을지</mark> 지정한다.  

    이 예시에는 다음 네 가지 조건, 즉 **쿼리 매개변수(query parameter)**가 포함됐다.
    - `from=Seoul`: 출발 도시
    - `to=Tokyo`: 도착 도시
    - `date=2025-07-01`: 출발 날짜
    - `limit=5`: 최대 결과 개수

**정리하면**, 이 요청은 `/flights`라는 항공편 리소스를 `GET` 메서드로 요청하면서, 그 조건으로 출발지는 `Seoul`, 도착지는 `Tokyo`, 출발일은 `2025-07-01`, 결과는 최대 `5`개로 제한해달라는 의미다.

<br>

다른 예시도 살펴보자.  

<mark><strong>시나리오 2</strong>: 사용자가 다음 조건으로 항공권을 예약한다.</mark>
- 사용자 ID: 42
- 예약 항공편: TK123
- 탑승객 이름: 홍길동
- 여권 번호: M12345678
- 예약 좌석: 12A

이때 서버로 전송되는 HTTP 요청은 다음과 같다.

<img src="/assets/images/posts_img/2025-06-13-api-docs-rest-1/02-post-request-example.png" alt="POST 요청 예시" width="600">

HTTP 메서드와 URL로 시작하는 부분은 이전 예시와 비슷하다. 다만 이번에는 `POST` 메서드가 사용됐는데, 이는 서버에 새로운 데이터를 생성해 달라는 요청을 의미한다.

하지만 이 예시에서 특히 눈여겨 볼 부분은 **헤더**와 **본문**이다.

- **헤더(header)**는 HTTP 요청이나 응답에 포함되는 부가 정보다. 데이터 형식, 인증 정보, 클라이언트 환경 등의 내용을 담는다.  

    여기서는 `Content-Type`라는 헤더가 사용됐는데, 클라이언트가 서버에 보내는 데이터의 형식을 알려주는 역할을 한다. 그리고 `application/json`이라는 값은 본문이 JSON 형식임을 의미한다.
  
    > 이 외에도 자주 쓰이는 헤더로는 `Authorization`, `Accept`, `Bearer` 등이 있다. 각각의 역할은 차차 알아보자.

- **본문(body)**은 요청에 필요한 실제 데이터를 담는 부분이다. 많은 양의 정보를 전송할 때 사용되며, **메시지**라고 부르기도 한다.

    > 본문에는 보통 **JSON**이나 **XML** 형식을 사용하지만, 이미지나 동영상 같은 미디어 파일도 담을 수 있다. 본문은 `POST`나 `PUT` 요청처럼 서버에 데이터를 생성하거나 수정할 때 주로 사용된다.

---

#  리소스
 
REST API의 핵심 개념인 리소스를 조금 더 자세히 알아보자. 

앞서 언급했듯이, 리소스란 서버가 관리하는 데이터 단위를 말한다. 사용자, 게시물, 상품 등 우리가 웹에서 다루는 거의 모든 것이 리소스가 될 수 있다. REST에서는 이 리소스를 URL 안에 표시한다.  

리소스의 두 가지 특징을 살펴보자. 

- 각 리소스는 **고유한 ID**를 가지고 있다. 특정 리소스를 지정하려면 이 ID를 쓰면 된다.
 
    > 예를 들어, 음식 배달 앱의 API를 생각해 보자.  
    > **모든 음식점 목록**을 조회하려면 다음과 같이 요청하면 된다.
    >    
    > `GET http://api.example.com/restaurants`
    >  
    > 하지만, **ID가 42인 음식점 하나만** 조회하려면 다음처럼 ID를 명시해야 한다.
    >   
    > `GET http://api.example.com/restaurants/42`

- 한 리소스는 **다른 리소스를 포함**할 수 있다. 이 경우, 포함하는(상위) 리소스를 먼저 경로에 명시한다.

    > 예를 들어, 음식점 42번의 메뉴를 조회하려면 다음과 같이 요청하면 된다.
    >    
    > `GET http://api.example.com/restaurants/42/menus`
    >   
    > 만약 **그중 ID가 18인 메뉴만** 조회하려면, 다음과 같이 메뉴의 ID를 포함해야 한다.
    >   
    > `GET http://api.example.com/restaurants/42/menus/18`

<br>

다음 글부터는 HTTP 메서드, 쿼리 매개변수, 헤더 등 REST API 요청의 주요 구성 요소를 더 자세히 다뤄보겠다.

---
 
**참고자료**

- *Udemy - [Learn API Technical Writing 2: REST for Writers](https://www.udemy.com/course/learn-api-technical-writing-2-rest-for-writers/)*
- *GeeksforGeeks - [REST API Introduction](https://www.geeksforgeeks.org/node-js/rest-api-introduction/)*
- *Lonti - [The key ingredients of RESTful APIs: resources, representations, and statelessness](https://www.lonti.com/blog/the-key-ingredients-of-restful-apis-resources-representations-and-statelessness)*
- *SmartBear - [SOAP vs. REST: What API Testers and Developers Need to Know](https://smartbear.com/learn/api-design/soap-vs-rest-apis/)*