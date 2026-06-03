---
title: "[API Documentation] JSON의 개념과 구조, 그리고 문서화 방법"
excerpt: "JSON을 더 자세히 이해하고, JSON 데이터를 표로 정리해 보자." 

categories:
  - Writing
tags:
  - [Technical Writing, API, JSON]

permalink: /writing/api-docs-json

toc: true
toc_sticky: true

date: 2025-06-04
last_modified_at: 2025-06-04
---

[이전 글](https://seulahn.github.io/tw/api-docs-intro)에서는 API에서 데이터를 주고받는 방법과, 데이터 교환 형식인 JSON과 XML을 간략하게 설명했다. 

이번 글에서는 오늘날 가장 널리 쓰이는 데이터 형식인 JSON을 더 자세히 살펴보자. JSON의 개요와 데이터 타입, 그리고 API 문서에서 JSON 데이터를 표로 정리하는 방법까지 알아보겠다.

---

# JSON이란?

**JSON(JavaScript Object Notation)**은 텍스트 기반의 데이터 교환 형식으로, 주로 클라이언트와 서버 간에 데이터를 주고받을 때 사용된다. 이름에서 알 수 있듯 JavaScript에서 유래했지만, 특정 프로그래밍 언어에 종속되어 있지 않고 대부분의 언어에서 사용할 수 있다.

JSON은 기본적으로 정보를 **키-값의 쌍**과 **배열**로 표현한다. 자세한 구조는 차차 살펴보겠다. 

그렇다면 JSON이 다른 형식에 비해 인기가 많은 이유는 무엇일까?

- **간결하고 읽기 쉽다.**   
    키-값 쌍으로 이루어져 있어 이해하기 쉽다. 개발자 입장에서 비교적 빠르게 읽고 수정할 수 있다.
- **호환성이 높다.**   
    JavaScript, Python, Ruby 등 대부분의 프로그래밍 언어가 JSON을 기본적으로 지원한다.
- **가볍고 효율적이다.**   
    XML과 달리, 태그를 사용할 필요가 없어 데이터 크기도 작다. 그만큼 전송 속도도 빠르다.

---

# JSON 데이터 타입과 구조

## 🔡 데이터 타입

JSON의 데이터 타입은 여섯 가지로 나눌 수 있다. 특히 **배열**과 **객체** 개념에 주목하자.

- **문자열(string)**   
    여러 문자의 집합이다. **큰 따옴표(`"`)**로 감싼다.
    
    예: `"John"`, `"apple"`

- **숫자(number)**   
    정수 또는 실수
    
    예: `123`, `-45.78`

- **불리언(Boolean)**   
    참(`true`) 또는 거짓(`false`)
    
    예: `true`, `false`

- **null**   
    값이 없음을 나타낸다.
    
    예: `null`

- **<mark>배열(array)</mark>**   
    여러 값을 순서대로 나열한 데이터다. **대괄호(`[]`)**로 감싼다.   
    **서로 다른 자료형도 혼합**할 수 있으며, 각 항목은 쉼표로 구분한다.
       
    예:

    ```json
    ["apple", 42, true, null]
    ```

- **<mark>객체(object)</mark>**   
    키와 값의 쌍으로 이루어진 데이터(딕셔너리와 같은 개념)다. **중괄호(`{}`)**로 감싼다.   
    **`“키”:값`** 형태의 쌍으로 구성되며, 각 쌍은 쉼표로 구분한다.
    
    예:
    
    ```json
    {
    	"name": "Alice",
    	"age": 25,
    	"isStudent": false
    }
    ```
    
<br>

## 🔁 중첩 구조

배열과 객체는 자유롭게 중첩(nest)할 수 있다. 예를 들어, 배열 안에 객체가 들어갈 수도 있고, 객체 안에 배열이 포함될 수도 있다. **대부분의 JSON 데이터는 하나의 큰 객체 안에 여러 객체와 배열이 중첩된 구조다.**

예:

```json
{
	"user": {
		"name": "Alice",
		"hobbies": ["reading", "coding"]
	}
}
```

---

# JSON 문서 작성 방법

API에서 JSON 형식을 사용해 데이터를 주고받을 경우, 이 데이터를 어떻게 해석하고 활용할 수 있는지 명확하게 문서화할 필요가 있다. 일반적으로는 **필드(키)**를 기준으로 JSON 데이터를 표 형식으로 정리한다.

참고로 표를 만드는 방식에 정답이 있는 건 아니다. 이건 하나의 예시일 뿐, 문서화의 목적과 팀의 작업 방식에 맞게 정리하면 된다.

<br>

## 📥 JSON 응답

먼저, API를 통해 서버에서 클라이언트로 전달되는 JSON 형식의 응답 데이터를 표 형식으로 정리해 보자.
  
| **필드** | **설명** | **타입** | **비고** |
| --- | --- | --- | --- |
| <img width=60/> | <img width=60/> | <img width=50/> | <img width=100/> |
| <img width=60/> | <img width=60/> | <img width=50/> | <img width=100/> |
  
- **필드**: 키-값 쌍에서의 키 이름
- **설명**: 해당 필드에 관한 설명
- **타입**: 해당 필드의 데이터 타입
    
    (예: `number`, `string`, `boolean`, `array`, `object`, `null` 등)
     
- **비고**: 추가 설명이나 유의 사항
    
    (예: 값의 범위, 기본값, 조건 등)

<br>

예를 들어, 회원가입에 성공한 후 아래와 같은 JSON 응답을 받는다고 가정해 보자.

```json
{
    "status": "success",
    "message": "회원가입이 완료되었습니다.",
    "user_id": 1024,
    "username": "helloworld123",
    "email": "hello@example.com",
    "created_at": "2025-06-02T10:15:30Z"
}
```

각 필드를 표로 정리하면 다음과 같다. 비고에 들어갈 정보는 임의로 작성했다.

| **필드** | **설명** | **타입** | **비고** |
| --- | --- | --- | --- |
| status | 처리 결과 | string | `"success"` 또는 `"error"` |
| message | 처리 결과 메시지 | string | 사용자에게 보여줄 수 있는 한글 메시지 |
| user_id | 사용자 고유 ID | number | 시스템에서 자동 생성 |
| username | 사용자명 | string | 요청 시 입력한 값과 동일 |
| email | 이메일 주소 | string | 요청 시 입력한 값과 동일 |
| created_at | 가입 일시 | string | ISO 8601 포맷 (`YYYY-MM-DDTHH:mm:ssZ`) |
  
<br>

## 📤 JSON 요청

이번에는 클라이언트가 서버에 데이터를 보내는 JSON 형식의 요청 데이터를 정리해 보자.  
앞서 본 표와 거의 동일하지만, **필수 여부**라는 열이 하나 추가된다.
  
| **필드** | **설명** | **타입** | **필수 여부** | **비고** |
| --- | --- | --- | --- | --- |
| <img width=60/> | <img width=60/> | <img width=50/> | <img width=60/> | <img width=100/> |
| <img width=60/> | <img width=60/> | <img width=50/> | <img width=60/> | <img width=100/> |
  
필수 여부는 해당 필드가 꼭 포함되어야 하는 값인지를 나타낸다. “필수(Required)” 또는 “선택(Optional)”, “true” 또는 “false”, “O” 또는 “X” 등으로 구분한다.

<br>

아래는 회원가입을 요청할 때의 JSON 요청 예시다.

(여기서 `username`, `email`, 그리고 `password`는 필수 항목이고 나머지는 선택 항목이라고 가정한다. 또 `username`은 4~20자의 영문과 숫자 조합이며, `password`는 최소 8자 이상, `age`의 기본값은 0, `newsletter`의 기본값은 false라고 가정한다.)

```json
{
	"username": "helloworld123",
	"email": "hello@example.com",
	"password": "Pa$$w0rd!",
	"age": 28,
	"newsletter": true
}
```

이 데이터를 문서화하면 다음과 같이 정리할 수 있다.

| **필드** | **설명** | **타입** | **필수 여부** | **비고** |
| --- | --- | --- | --- | --- |
| username | 사용자 아이디 | string | 필수 | 4~20자, 영문+숫자 조합 |
| email | 이메일 주소 | string | 필수 |  |
| password | 비밀번호 | string | 필수 | 최소 8자 이상 |
| age | 나이 | number | 선택 | 기본값: 0 |
| newsletter | 뉴스레터 수신 여부 | boolean | 선택 | 기본값: `false` |

<br>

## 💡 중첩 구조 문서화 방식

지금까지 살펴본 예시는 모두 중첩이 없는 단일 계층(flat) 데이터였다.
하지만 앞에서 살펴봤듯이, 실제로는 객체 안에 또 다른 객체나 배열이 들어 있는 **중첩 구조**가 더 흔하다.

예를 들어, 아래의 사용자 정보 응답은 `user` 안에 여러 객체가 포함된 중첩 구조를 이루고 있다.

```json
{
	"user": {
		"id": 1,
		"name": "Jane",
		"profile": {
			"age": 28,
			"location": "Seoul"
		}
	}
}
```

이를 문서화하는 방법은 크게 두 가지가 있다.


- **들여쓰기 방식**  
    필드를 들여쓰기해서 계층을 표현하는 방법이다. 필드가 반복되지 않는, 비교적 단순한 구조일 때 유용하다.

    | **필드** | **설명** | **타입** | **비고** |
    | --- | --- | --- | --- |
    | `user` | 사용자 정보 | object |  |
    | └─ `id` | 사용자 ID | number |  |
    | └─ `name` | 사용자 이름 | string |  |
    | └─ `profile` | 프로필 정보 | object |  |
    | └─── `age` | 나이 | number | 단위: 세 |
    | └───`location` | 위치 | string | 예: Seoul |

<br>

- **필드별로 표 분리**  
    구조가 복잡하거나 동일한 필드가 여러 번 반복될 경우, 필드 단위로 표를 나누는 것이 효과적이다.  

    **전체 구조**
    
    | **필드** | **설명** | **타입** | **비고** |
    | --- | --- | --- |  --- |
    | `user` | 사용자 정보 | object |  |

    **user 상세 필드**
    
    | **필드** | **설명** | **타입** | **비고** |
    | --- | --- | --- | --- |
    | `id` | 사용자 ID | number |  |
    | `name` | 사용자 이름 | string |  |
    | `profile` | 프로필 정보 | object |  |

    **profile 상세 필드**
    
    | **필드** | **설명** | **타입** | **비고** |
    | --- | --- | --- | --- |
    | `age` | 나이 | number | 단위: 세 |
    | `location` | 위치 | string | 예: Seoul |

---
  
**참고자료**

- *Udemy - [Learn API Technical Writing: JSON and XML for Writers](https://www.udemy.com/course/api-documentation-1-json-and-xml/)*

- *JSON Lint - [Mastering JSON Format](https://jsonlint.com/mastering-json-format)*

- *MongoDB - [What is JSON?](https://www.mongodb.com/resources/languages/what-is-json)