---
title: "[API Documentation] API와 정형 데이터"
excerpt: "API는 어떻게 작동하고, JSON과 XML은 무엇일까?" 

categories:
  - Writing
tags:
  - [Technical Writing, API]

permalink: /writing/api-docs-intro

toc: true
toc_sticky: true

date: 2025-06-02
last_modified_at: 2025-06-02
---

# API와 정형 데이터

## 🔌 API란?

**API(Application Programming Interface)**는 두 개의 서로 다른 소프트웨어가 소통할 수 있도록 돕는 규칙과 프로토콜을 말한다. 

개발자는 API를 활용해 여러 소스에서 데이터를 가져오고, 이를 조합해 새로운 애플리케이션을 만들 수 있다. 개발 시간과 비용을 절감할 수 있는 방법이기도 하다.

오늘날 많은 기업이 자사의 데이터, 기능 또는 서비스를 외부 개발자가 활용할 수 있도록 API를 공개하고 있다. 다른 애플리케이션이나 서비스에 통합되면, 자사 서비스의 노출과 수익 채널을 확대할 수 있기 때문이다.

<p align="center">
    <img src="/assets/images/posts_img/2025-06-02-api-docs-1/01-spotify-web-api-doc.png" alt="스포티파이 웹 API 문서 화면" width="600">
    <br>
    <em>스포티파이는 곡 정보, 아티스트 정보, 플레이리스트 등을 외부에서 활용할 수 있도록 오픈 API를 제공하고 있다.</em>
</p>

<br>
API에는 여러 종류가 있지만, 앞으로의 글에서는 웹 API에 집중해서 설명할 예정이다.

<br>

API 통신은 보통 두 주체 간의 상호작용으로 이뤄진다. **클라이언트(client)**와 **서버(server)**다.

- **클라이언트**는 데이터를 요청(request)하는 쪽이다. 우리가 일상에서 사용하는 모바일 앱이나 웹 브라우저가 이에 해당한다.
- **서버**는 이 요청을 받고, 필요한 데이터를 찾아 응답(response)하는 쪽이다.   

<br>

이 구조를 식당에 비유하자면 다음과 같다.

- **클라이언트**: 음식을 주문하는 손님
- **서버**: 음식을 만드는 주방
- **API**: 주문을 전달하고 음식을 가져다주는 직원

<br>

## 📦 정형 데이터의 대표 형식: JSON과 XML

대부분의 경우, 서버는 클라이언트에게 **정형 데이터(structured data)** 로 응답을 보낸다. 

정형 데이터는 미리 정해진 구조를 따라 저장된 데이터로, 보통 표 형태로 정리된다. 반대로, 정해진 형식을 따르지 않는 데이터를 비정형 데이터라고 부른다.

<br>

정형 데이터를 표현할 때 주로 쓰이는 형식은 다음 두 가지다.

- **JSON(JavaScript Object Notation)**
- **XML(eXtensible Markup Language)**

XML이 오랫동안 표준으로 사용되었으나, 최근에는 더 간결하고 전송이 빠른 JSON이 대세로 자리 잡았다.

<br>

실제 응답에서 JSON과 XML의 구조는 대략 아래와 같다.

**JSON**

```json
{
"name": "Alice",
"email": "alice@example.com"
}
```

**XML**

```xml
<person>
  <name>Alice</name>
  <email>alice@example.com</email>
</person>
```

---

# 데이터 타입

## 🔤 기본 데이터

API에서 주고받는 데이터에는 숫자, 문자, 참/거짓 등 다양한 타입이 있다.

<br>

**JSON**은 다음의 기본 데이터 타입을 지원한다.

- **숫자**
  
    예: `42`, `3.11`
    
- **문자열(string)**: 여러 문자의 집합
  
    (숫자, 글자, 문장부호 등이 포함될 수 있으며, 전체 문자열은 따옴표로 감싼다.)
    
    예: `"Hello"`, `"user@example.com"`, `“124-1353-1234”`
    
- **불리언(Boolean)**: 참(`true`) 또는 거짓(`false`)
  
    예: `true`
    
<br>

한편, **XML**은 구조상 모든 데이터를 **문자열**로 간주한다.

```xml
<age>42</age>
<name>Alice</name>
<active>true</active>
```

<br>

## 🧺 컬렉션 데이터

기본 데이터 외에도, 여러 데이터가 모인 구조 **컬렉션(collection)** 데이터가 있다.

<br>

대표적인 컬렉션 데이터 타입은 다음과 같다.

- **배열(Array):** 여러 값을 순서대로 나열한 구조
  
    예: `6,1,42,3.2,0`, `“Alice”, “Jess”, “Charlie”, Brenda”`
    
- **딕셔너리(Dictionary):** 키(key)와 값(value)의 쌍으로 이루어진 구조

    예: `“name”: Alice, “age”: 30, “location”: seoul`
    
<br>

컬렉션을 서로 조합해서 사용할 수도 있다. 

- 딕셔너리를 나열한 배열(array of dictionaries)
- 배열을 포함한 딕셔너리(dictionary of arrays)
- 딕셔너리를 포함하는 딕셔너리(dictionary of dictionaries)

<br>

JSON과 XML에 대한 보다 자세한 내용과 문서화 방법은 다음 글에서 알아보겠다.

---

**참고자료**

- *Learn API Technical Writing: JSON and XML for Writers*
  
    [https://www.udemy.com/course/api-documentation-1-json-and-xml/learn/lecture/2122892#content](https://www.udemy.com/course/api-documentation-1-json-and-xml/)
    
- *What is an API?*
  
    [https://aws.amazon.com/what-is/api/](https://aws.amazon.com/what-is/api/)