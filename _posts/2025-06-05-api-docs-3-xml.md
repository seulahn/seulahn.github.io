---
title: "[API Documentation] XML의 기본 구조와 문법"
excerpt: "XML의 요소를 이루는 태그와 속성을 알아보고, 선언, 네임스페이스를 포함한 기초 문법까지 살펴본다." 

categories:
  - Writing
tags:
  - [Technical Writing, API, XML]

permalink: /writing/api-docs-xml-intro

toc: true
toc_sticky: true

date: 2025-06-05
last_modified_at: 2025-06-05
---

[이전 글](https://seulahn.github.io/tw/api-docs-json)에서는 JSON의 개념과 데이터 타입, 구조, 그리고 표로 문서화하는 방법을 정리했다.

이번 글부터는 API 전송에 쓰이는 또 다른 데이터 형식인 XML을 알아보겠다. XML의 개념은 상당히 방대하지만, 여기서는 데이터를 문서화하는 데 필요한 기본 개념을 위주로 살펴보려고 한다.

---

# XML이란?

XML(eXtensible Markup Language)은 데이터를 구조화하여 저장하고 전달하기 위한 마크업 언어다. 태그를 사용해 속성과 데이터를 구분한다.

JSON의 장점이 간결함과 속도라면, XML의 장점은 복잡한 데이터 구조를 다루고 엄격하게 검증할 수 있다는 점이다. 이러한 특성 덕분에 금융과 의료 업계, 공공 분야에서 주로 쓰이고 있다.

XML은 태그(`<>`)를 기반으로 하며, 언뜻 보면 HTML과 비슷하다. 하지만 둘 사이에는 몇 가지 중요한 차이점이 있다. 

- HTML은 데이터를 **화면에 표시**하는 데 초점이 맞춰져 있다. 반면, XML은 **데이터의 구조와 의미를 정의하고, 저장, 전송**하는 데 초점을 둔다.
- HTML은 **미리 정해진 태그**(예: `<head>`, `<body>`)를 사용하지만, XML에서는 **직접 태그를 정의**할 수 있다.
- 이외 XML은 대소문자를 구분하지만, HTML은 구분하지 않는 등 문법적인 차이가 있다.

---

# XML 기본 구조와 문법

## 🧱 기본 구조

XML 문서를 구성하는 기본 단위는 **요소(element)**다.  
요소는 기본적으로 **태그**와 그 사이의 **콘텐츠**로 이루어진다.

```xml
<element_name>content</element_name>
```

여기에 **속성**을 추가하면 다음과 같은 구조가 된다.

```xml
<element_name attribute="value">content</element_name>
```

그럼 태그, 콘텐츠, 그리고 속성을 하나씩 살펴보자.

<br>

### 🏷️ 태그

**태그(tag)**는 요소의 시작과 끝을 표시하는 이름이다.

- 모든 요소에서는 시작 태그(`<element_name>`)와 종료 태그(`</element_name>`)가 쌍을 이루어야 한다.
- 태그명은 문자 혹은 밑줄(`_`)로 시작해야 한다.
- 만약 태그 사이 콘텐츠가 비어 있다면, 시작 태그와 종료 태그를 단축해서 아래와 같이 쓸 수 있다.
    
    `<element_name></element_name>` → `<element_name />`

<br>

### ✏️ 콘텐츠

**콘텐츠(content)**는 태그 사이에 들어가는 데이터다.  
단순한 문자열일 수도 있고, 다른 요소를 중첩할 수도 있다.

예: 

```xml
<greeting>Hello, world!</greeting>
```

```xml
<book>
	<title>XML Guide</title>
	<author>me</author>
</book>
```

<br>

### 🧩 속성

**속성(attribute)**은 요소에 관한 부가 정보를 담는다.

- 시작 태그 안에 `key="value"` 형태로 작성한다.
- 속성값은 반드시 큰따옴표(`"`) 혹은 작은따옴표(`'`)로 감싸야 한다.
- 한 요소에 여러 개의 속성을 넣을 수 있다. 단, 같은 이름의 속성을 중복해서는 안 된다.
    
    ```xml
    <user id="1234" role="admin" active="true">John Doe</user>
    ```

> **속성 vs. 요소**  
  속성과 요소 모두 데이터를 표현할 수 있다. 하지만 일반적으로 **속성에는 부가 정보(식별자, 상태 등 메타데이터)만** 넣고, **실제 데이터는 요소로** 표현하기를 권장한다.
>
>   ```xml
>   <!-- ✅ 좋은 예: 식별자 외 데이터는 요소로 표현 -->
>   <person id="n341">
>       <gender>female</gender>
>       <firstname>Mary</firstname>
>       <lastname>Coopers</lastname>
>   </person>
> 
>   <!-- 🚨 나쁜 예: 모든 데이터를 속성에 몰아넣음 -->
>   <person id="n341" gender="female" firstname="Mary" lastname="Coopers">
>   </person>
>   ```

<br>

## 🧱 기타 문법 요소

### 📜 선언

XML 문서의 첫 줄에는 보통 다음과 같은 선언을 넣는다. 이 문서가 XML이라는 사실을 알리기 위해서다.

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
```

- `version`: XML의 버전(보통 `"1.0"`)
- `encoding`: 문자 인코딩 방식(기본값: `"UTF-8"`)
- `standalone`: 외부 리소스(DTD 등) 참조 여부
    - `no`(기본값): 외부 리소스를 참조할 수 있음
    - `yes`: 외부 참조 없이 독립적으로 동작

<br>

### 💬 주석

XML 주석은 HTML과 비슷하다. 앞에만 느낌표(`!`)가 붙는다.

```xml
<!-- 주석 내용 -->
```

<br>

### 🌐 네임스페이스
XML에서는 **이름이 같은 태그라도 서로 다른 의미로** 쓰일 수 있다. 이럴 때 **네임스페이스(namespace)**를 사용해 각 태그를 구분한다.

예를 들어, 도서관에서 책 또는 음반을 대출하는 상황을 생각해 보자.

```xml
<library>
	<title>XML Guide</title>
	<title>Classical Hits</title>
</library>
```

이렇게만 보면 두 개의 `title` 중 어떤 것이 책이고, 어떤 것이 음반인지 알기 어렵다.  

**네임스페이스**를 사용하면, 각 태그가 **어디에서 온 것인지**, 즉 어떤 종류의 데이터인지 명확히 알 수 있다. 쉽게 말해, 네임스페이스는 ‘이건 책 태그’, ‘저건 음반 태그’라고 출처를 표시해 주는 꼬리표 역할을 한다.

<br>

<mark>네임스페이스 사용 방법</mark>

1. **네임스페이스 선언하기**
    
    먼저 네임스페이스를 선언한다. 보통 **루트(최상위) 요소**에 다음과 같은 형식으로 작성한다.
    
    ```xml
    <루트태그 xmlns:접두어="URI">
    ```
    
    - `xmlns`: XML 네임스페이스를 선언한다는 의미
    - `접두어(prefix)`:  태그가 어떤 네임스페이스에 속하는지 알려주는 문자열
        - 예: `book`, `music`
    - `URI`: 네임스페이스의 고유한 식별자
        - 예: `http://www.w3.org/XML/1998/namespace`
        - **URL 형식**을 주로 사용하지만, **URN**을 사용하기도 한다. 실제 존재하는 웹페이지일 필요는 없다. 단지 **고유한 문자열**을 부여하는 게 목적이다.

    > 한 요소에 여러 네임스페이스를 선언할 수도 있다.
    > 
    > ```xml
    > <루트태그 xmlns:접두어1="URI1" xmlns:접두어2="URI2">
    > ```

    > 처음에는 **접두어와 URI 개념**이 어렵게 느껴졌는데, **URI가 너무 길기 때문에 대신 접두어를 사용**하는 것으로 생각하니 도움이 됐다. 즉, 접두어는 태그에 붙는 짧은 별명, URI는 그 별명이 가리키는 진짜 출처인 셈이다.

2. **선언한 네임스페이스 사용하기**
    
   네임스페이스를 선언했으면, 자식 요소의 **태그명 앞에** 접두어를 붙여 사용하면 된다.
    
    ```xml
    <접두어:태그명>값</접두어:태그명>
    ```

    이제 해당 태그가 여러 번 나오더라도 서로 다른 네임스페이스로 구분할 수 있다.

<br>

그럼 네임스페이스를 사용해 앞서 본 예시를 수정해 보자.

```xml
<library xmlns:book="http://example.com/book" xmlns:music="http://example.com/music">
	<book:title>XML Guide</book:title>
	<music:title>Classical Hits</music:title>
</library>
```

네임스페이스를 사용한 덕분에, `book:title`은 책 제목이고 `music:title`은 음반 제목임을 구분할 수 있게 됐다.

---
 
**참고자료**

- *Udemy - [Learn API Technical Writing: JSON and XML for Writers](https://www.udemy.com/course/api-documentation-1-json-and-xml/)*

- *AWS — [What is XML?](https://aws.amazon.com/what-is/xml/?nc1=h_ls)*  
- *Directual — [JSON and XML: Which one is better for no-coders?](https://www.directual.com/blog/json-and-xml-which-one-is-better-for-no-coders)*  
- *SmartParse — [The Role of JSON and XML in API Data Transfers](https://smartparse.io/posts/json-xml-api-data-transfer/)*  
- *W3Schools — [XML Tutorial](https://www.w3schools.com/xml/default.asp)*  