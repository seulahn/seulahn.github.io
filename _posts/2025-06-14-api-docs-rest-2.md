---
title: "[API Documentation] REST API ② HTTP 메서드(GET, POST, PUT, DELETE)"
excerpt: "REST API에서 주로 쓰이는 네 가지 HTTP 메서드의 역할과 요청 예시"

categories:
  - Writing
tags:
  - [Technical Writing, API, REST]

permalink: /writing/api-docs-rest-2

toc: true
toc_sticky: true

date: 2025-06-14
last_modified_at: 2025-06-14
---

먼저 살펴볼 REST API의 핵심 요소는 **HTTP 메서드다.**  

[이전 글](https://seulahn.github.io/tw/api-docs-rest-1)에서 본 것처럼, HTTP 메서드는 클라이언트가 서버에 **'리소스로 어떤 작업을 수행할지'** 명령하는 수단이다. HTTP 메서드의 종류는 매우 다양하지만, 그중에서도 `GET`, `POST`, `PUT`, `DELETE` 네 가지에 집중해 설명할 예정이다.

---

# CRUD란?

그렇다면 왜 `GET`, `POST`, `PUT`, `DELETE` 이 네 가지 메서드가 중요한 걸까?  
이들이 기본적인 데이터 처리 작업인 CRUD를 수행하기 위한 메서드고, 또 그만큼 자주 사용되기 때문이다.

CRUD는 본래 데이터베이스에서 **데이터를 다룰 때 가장 기본이 되는 네 가지 작업**, **C**reate, **R**etrieve, **U**pdate, **D**elete의 앞 글자를 딴 용어다.

- **C**reate: 새로운 데이터 생성
- **R**etrieve: 데이터 조회
- **U**pdate: 데이터 수정
- **D**elete: 데이터 삭제

REST API도 결국 클라이언트와 서버가 **데이터를 주고받기 위한 수단**이기 때문에, 자연스럽게 CRUD 작업을 포함한다. 그리고 이를 수행하는 데 사용되는 것이 바로 `GET`, `POST`, `PUT`, `DELETE` 메서드다.

각 CRUD 작업에 대응하는 HTTP 메서드는 다음과 같다.

| **CRUD 작업** | **HTTP 메서드** | **HTTP 메서드의 역할** |
| --- | --- | --- |
| Create | `POST` | 리소스 생성 |
| Retrieve | `GET` | 리소스 조회 |
| Update  | `PUT` (, `PATCH`) | 리소스 수정 |
| Delete | `DELETE` | 리소스 삭제 |

> HTTP 메서드와 CRUD 작업이 엄밀히 1:1로 대응하는 것은 아니다. 
> 예를 들어, `PATCH` 메서드는 `PUT` 처럼 리소스를 수정하는 데 사용된다. 
> 또한, CRUD 외의 작업을 요청하는 HTTP 메서드도 많다.

---

# 네 가지 HTTP 메서드

이제 각 메서드가 어떤 상황에서 어떻게 사용되는지, 예시와 함께 알아보자.

<br>

## 🔍 GET: 리소스 조회

`GET`은 서버에 저장된 **리소스를 읽어올 때** 사용하는 메서드다.

- **리소스 ID**를 사용해 특정한 리소스를 지정할 수 있다.
- 요청에 본문은 포함하지 않는다.

예: 도서(`book`) 정보를 관리하는 서버가 있다면 아래와 같이 요청할 수 있다.

- 전체 책 목록 조회
    
    ```json
    GET /book
    ```
    
- ID가 1234인 책 정보 조회
    
    ```json
    GET /book/1234
    ```

<br>

## ✍️ POST: 리소스 생성

`PUT`은 서버에 **새로운 리소스를 생성**할 때 사용한다.

- **필요한 정보를 요청 본문에 포함**한다.

예: 새로운 책 정보 생성 요청

```json
POST /book
Content-Type: application/json

{
"title": "API 문서 이해하기",
"author": "홍길동",
"published_year": 2025
}
```


<br>

## 🛠️ PUT: 리소스 수정

`PUT`은 **기존 리소스를 수정**할 때 사용한다.

- 일반적으로 **ID**를 사용해 특정한 리소스를 지정한다.
- **덮어쓸 데이터를 본문에 포함**한다.

예: ID가 1234인 책 정보 수정

```json
PUT /book/1234
Content-Type: application/json

{
"title": "API 문서 이해하기",
"author": "홍길동",
"published_year": 2025
}
```

> `PUT`은 전체 리소스를 덮어쓰는 방식으로 수정한다. 반면, 리소스를 일부만 변경하려면 `PATCH`라는 메서드를 사용해야 한다.

<br>

## 🗑️ DELETE: 리소스 삭제

`DELETE`는 서버에 특정 **리소스를 삭제**할 때 사용한다.

- 일반적으로 **ID**를 사용해 특정한 리소스를 지정한다.
- 요청에 본문은 포함하지 않는다.

예: ID가 1234인 책 정보 삭제

```json
DELETE /book/1234
```

---

# 요약

지금까지 살펴본 내용을 정리하면 다음과 같다. 


| **HTTP 메서드** | **CRUD 작업** | **설명** | **요청 URL 예시** | **요청 본문** |
| --- | --- | --- | --- | --- |
| `GET` | Retrieve | 리소스 조회 | `GET /book` <br> `GET /book/1234` | 없음 |
| `POST` | Create | 새로운 리소스 생성 | `POST /book` | 있음 |
| `PUT` | Update | 리소스 수정 | `PUT /book/1234` | 있음 |
| `DELETE` | Delete | 리소스 삭제 | `DELETE /book/1234` | 없음 |

- **본문 관련**  
    - `GET`과 `DELETE` 요청은 일반적으로 **본문을 포함하지 않는다.**  
    - 반면, `POST`와 `PUT` 요청은 일반적으로 리소스 데이터를 담은 **본문을 포함한다.**

        ```json
        {
        "title": "API 문서 이해하기",
        "author": "홍길동",
        "published_year": 2025
        }
        ```
- **리소스 ID 관련**  
    - `GET` 요청에서는 **경우에 따라 리소스 ID를 지정**해 특정 리소스를 조회할 수 있다.  
    - `PUT`과 `DELETE` 요청에서는 **일반적으로 리소스 ID를 지정**한다.
    - 반면 `POST` 요청에서는 일반적으로 리소스 ID를 사용하지 않는다.

---
 
**참고자료**

- *Udemy - [Learn API Technical Writing 2: REST for Writers](https://www.udemy.com/course/learn-api-technical-writing-2-rest-for-writers/)*
- *Apidog - [What are HTTP Methods (GET, POST, PUT, DELETE)](https://apidog.com/blog/http-methods/)*
- *Budibase - [CRUD vs REST What’s the Difference?](https://budibase.com/blog/data/crud-vs-rest/)*