---
title: "[API Documentation] REST API ③ 쿼리 매개변수"
excerpt: "페이지 매김, 정렬, 필터링을 위해 쿼리 매개변수 사용하기, 그리고 쿼리 매개변수를 표로 정리하는 방법"

categories:
  - Writing
tags:
  - [Technical Writing, API, REST]

permalink: /writing/api-docs-rest-3

toc: true
toc_sticky: true

date: 2025-06-21
last_modified_at: 2025-06-21
---

지금까지는 REST API의 기본 데이터 단위인 **[리소스](https://seulahn.github.io/tw/api-docs-rest-1)**, 그 리소스로 어떤 작업을 할지 명령하는 **[HTTP 메서드](https://seulahn.github.io/tw/api-docs-rest-2)**를 살펴봤다. 

하지만, 두 요소만으로는 원하는 데이터를 정확하게 가져오기 어렵다.  

예를 들어 이런 경우를 생각해 보자.

- 데이터가 너무 많아 **나눠서 조회하고 싶을 때**
- **특정 조건에 맞는 데이터만** 받고 싶을 때
- 데이터를 **특정 기준으로 정렬**하고 싶을 때

이러한 세부 조건을 요청에 담으려면 리소스 경로로는 부족하다. 그래서 필요한 것이 바로 **쿼리 매개변수(query parameter)**다.

---

# 쿼리 매개변수란?

일반적으로 **매개변수**란 어떠한 동작의 조건이나 범위를 지정해 주는 값을 말한다. 그중 **쿼리 매개변수**는 API 요청에 조건을 부여함으로써, 응답 데이터의 내용이나 형태를 조정할 수 있게 해준다.

쿼리 매개변수가 포함된 요청의 기본 구조는 다음과 같다.

```
GET /resources?키1=값1&키2=값2&키3=값3 ...
```

- 쿼리 매개변수는 URL에서 **리소스 경로 다음에** `?` **기호**를 **붙여 시작**한다.
- 각 매개변수는 `키=값` **형식**으로 작성한다.
- 여러 개의 매개변수가 있으면 **서로** `&` **로 구분**한다.

---

# 쿼리 매개변수 사용 예시

이제 REST API에서 쿼리 매개변수를 사용하는 대표적인 사례를 살펴보자.  

**페이지 매김(pagination)**, **정렬(sorting)**, **필터링(filtering)**과 같은 주요 사례부터, 비교적 잘 쓰이지 않는 사례도 간단히 살펴보겠다. 

각 항목을 아주 깊이 파고들기보다는, **각 매개변수가 어떤 역할을 하는지 이해할 수 있는 수준**으로 정리할 예정이다.

***

## 📄 페이지 매김

데이터가 수천 건, 혹은 그 이상으로 많을 때 이를 한꺼번에 모두 받아오는 건 비효율적이다. 응답 속도가 느려지고, 서버와 클라이언트에 부담을 줄 수 있기 때문이다.

이때 사용하는 것이 **페이지 매김**, 혹은 페이지네이션이다. 페이지 매김이란 데이터를 여러 개의 작은 단위로 나눠서 가져오는 기법이다. 마치 구글에서 검색 결과가 한 번에 모두 나오지 않고 페이지별로 나뉘어 보이는 것과 같다.

페이지 매김 방식은 크게 세 가지로 나뉜다.

- 가장 간단한 **페이지(page) 기반** 방식,
- 그와 유사한 **오프셋(offset) 기반** 방식,
- 그리고 보다 복잡하지만 중요한 **커서(cursor) 기반** 방식

<br>

### **페이지 기반**

각 페이지에 들어갈 항목 수(`limit`)를 정하고, **페이지 번호(**`page`**)를 기준으로** 데이터를 요청하는 방식이다.

- **장점:** 구현하기 쉽고, 전체 페이지 수와 현재 위치를 쉽게 파악할 수 있다. 페이지 번호를 사용해 원하는 페이지로 바로 이동할 수도 있다.
- **단점:** 요청 중에 새 항목이 추가되거나 삭제되면, 페이지 내용이 밀려서 중복 또는 누락되는 데이터가 생길 수 있다. 또, 데이터가 많아질수록 성능이 저하될 수 있다. 따라서 **대규모 실시간 데이터에는 부적합**하다.

<mark><strong>매개변수</strong></mark>

| **매개변수** | **설명** |
| --- | --- |
| `page` <img width=40/> | 요청할 페이지 번호 <img width=100/> |
| `limit` <img width=40/> | 페이지당 항목 개수 <img width=100/> |

> **Note:** API마다 사용하는 매개변수 이름은 다를 수 있다. 예를 들어, '페이지당 항목 개수'를 나타내는 매개변수로 `limit` 대신 `page_size`, `size`, `per_page`를 사용하기도 한다. 중요한 건 하나의 API에서 일관된 매개변수 이름을 사용하는 것이다.
> 

<mark><strong>요청 예시</strong></mark>

```
# 기본 구조
GET /resources?page=<페이지 번호>&limit=<페이지당 항목 개수>

# 요청 예시
GET /posts?page=1&limit=10    # 1페이지에 해당하는 10개의 항목 요청
GET /posts?page=2&limit=10    # 2페이지에 해당하는 10개의 항목 요청
```

<br>

### 오프셋 기반

오프셋 기반 방식은 페이지 기반 방식과 비슷하지만, 페이지 번호 대신 **몇 번째 항목부터**(`offset`) 가져올지를 기준으로 한다. 

장단점 역시 페이지 기반 방식과 비슷하다. 구현하기는 쉽지만, 데이터가 중간에 변경되는 경우나 대규모 데이터에는 적합하지 않다.

<mark><strong>매개변수</strong></mark>

| 매개변수 | 설명 |
| --- | --- |
| `offset` <img width=40/> | 몇 번째 항목부터 요청할지 지정(0부터 시작) |
| `limit` <img width=40/> | 한 번에 가져올 항목 개수 |

<mark><strong>요청 예시</strong></mark>

```
# 기본 구조
GET /resources?offset=<몇 번째 항목부터 요청할지 지정>&limit=<가져올 항목 개수>

# 요청 예시
GET /posts?offset=0&limit=10     # 첫 10개 항목 요청
GET /posts?offset=10&limit=10    # 다음 10개 항목 요청
```

<br>

### 커서 기반

**특정 항목의 고유 식별자(=커서)를 기준으로, 그 이후**(`after`) **데이터를 요청하는 방식이다.** 

페이지나 오프셋 기반 방식이 ‘몇 번째 페이지인지’와 ‘몇 번째 항목부터 시작할지’를 기준으로 삼았다면, 커서 기반 방식은 ‘**이 전에 어떤 항목까지 봤는지**’를 기준으로 한다.

- **장점:** 대규모 데이터에서도 성능이 뛰어나며, 데이터가 변경되어도 누락이나 중복이 발생할 가능성이 적다. 인스타그램, 트위터 등의 무한 스크롤 기능을 구현할 때 적합하다.
- **단점:** 구현하기가 상대적으로 복잡하다. 특정 페이지 번호로 바로 이동할 수 없다.

<mark><strong>커서</strong></mark>

**커서**는 데이터 목록에서 특정 항목의 위치를 나타내는 **고유한 식별자**를 말한다. 커서 값은 고유하며(unique), 순서대로 정렬할 수 있어야(sequential) 한다. 주로 다음과 같은  값을 식별자로 사용한다.

- ID(예: `12345`)
- 타임스탬프(예: `2025-06-20T12:00:00Z`)
- 인코딩된 문자열(예: `eyJpZCI6MTIzNDV9`)

<mark><strong>매개변수</strong></mark>

| **매개변수** | **설명** |
| --- | --- |
| `after` <img width=40/> | 기준이 되는 마지막 항목의 식별자 |
| `limit` <img width=40/> | 한 번에 가져올 항목 개수 |
| `before` <img width=40/> | `after`와 반대로 특정 항목 이전 데이터를 가져올 때 사용되나, 잘 쓰이지는 않는다. |

<mark><strong>요청 예시</strong></mark>

```
# 기본 구조
GET /resources?after=<커서 값>&limit=<가져올 항목 개수>

# 요청 예시
GET /posts?limit=5               # 가장 최신 5개 항목 요청
GET /posts?after=101&limit=5     # 이전 응답에서 반환된 커서 값(id=101라고 가정) 이후의 5개 항목 요청
GET /posts?after=106&limit=5     # 이전 응답에서 반환된 커서 값(id=106라고 가정) 이후의 5개 항목 요청
```

***

## 🎛️ 필터링

클라이언트가 항상 어떠한 리소스 경로의 **모든** 데이터를 불러오는 건 아니다. 예를 들면 `/restaurants` 경로에서 음식점 전체 목록이 아닌, 태국 음식점만 조회하고 싶을 수 있다.

이런 경우, **특정 조건에 맞는 데이터만 조회**할 수 있도록 **필터**를 적용해야 한다.

<br>

### 기본 필터링

가장 기본적인 방법은 **특정 필드의 값이 일정한 조건과 일치하는** 데이터를 조회하는 방법이다.

- 하나의 조건만 지정할 수도 있고,
- `&` 기호를 사용해 여러 조건을 모두 만족하도록(AND) 지정하거나, 
- 쉼표(`,`)를 사용해 여러 조건 중 하나라도 만족하도록(OR) 지정할 수 있다.

<mark><strong>요청 예시</strong></mark>

```
# 기본 구조
GET /resources?<필드>=<값>

# 요청 예시
GET /restaurants?type=thai             # type이 'thai'인 항목
GET /restaurants?type=thai&city=seoul  # type이 'thai'이고 city가 'seoul'인 항목
GET /restaurants?type=thai,mexican     # type이 'thai' 또는 'mexican'인 항목
```

<br>

### 연산자 선언

그러나 기본 필터링으로는 충분하지 않을 때가 많다. 예를 들어, 평점이 **4.0점 이상**인 음식점을 모두 조회하려면 어떻게 해야 할까?

이처럼 값의 범위를 지정하거나 값을 비교할 경우 **연산자(operator)**를 사용한다.

<mark><strong>자주 쓰이는 연산자</strong></mark>

- `lt`: 미만(less than)
- `gt`: 초과(greater than)
- `lte`: 이하(less than or equal)
- `gte`: 이상(greater than or equal)
- `ne`: 같지 않음 (not equal)

<mark><strong>요청 예시</strong></mark>

보통 연산자는
- **필드명에 접미사로 붙여 사용**하거나(`<필드_연산자>=<값>`),
- 구분자와 함께 사용한다(예: `:`를 사용해 `<필드>=<연산자>:<값>` 형식으로 표현).  

아래 예시에서는 **접미사 방식을 사용**했다.

```
# 기본 구조
GET /resource?<필드_연산자>=<값>

# 요청 예시
GET /restaurants?rating_gte=4.0        # 평점이 4.0 이상인 음식점 조회
GET /restaurants?rating_lte=2.0        # 평점이 2.0 이하인 음식점 조회
```

<br>

### 표준 스타일 사용

필터 조건이 복잡해질수록, 매번 임의의 규칙으로 매개변수를 만드는 대신 **표준 스타일([JSON:API](https://jsonapi.org/format/#query-parameters-families), [OData](https://www.odata.org/getting-started/basic-tutorial/#queryData) 등)**을 따르는 것도 좋은 선택이 될 수 있다.

<mark><strong>요청 예시</strong></mark>

예시로 든 두 스타일이 어떻게 생겼는지만 간단히 살펴보겠다.  
`type=thai`, `city=seoul`인 음식점을 조회한다고 가정하자.

```
# 기본 REST 스타일
GET /restaurants?type=thai&city=seoul

# JSON:API
GET /restaurants?filter[type]=thai&filter[city]=seoul

# OData
GET /restaurants?$filter=type eq 'thai' and city eq 'seoul'
```

<br>

> **Note:** 지금까지 본 것처럼, REST API에서 데이터를 필터링할 때는 주로 **쿼리 매개변수**를 활용한다. 하지만 경우에 따라 **경로 매개변수**나 **요청 본문**을 활용할 수도 있다.
> 
> - **경로 매개변수 활용**
>     
>     예: `GET /restaurants/type/thai`
>     
> - **요청 본문 활용**
>     
>     (주로 `POST` 요청에서 복잡한 조건을 전송할 때 사용)
>     
>     예: 
>     
>     ```
>     POST /restaurants/search
>     {
>     "type": "thai",
>     "rating": { "gte": 4.0 }
>     }
>     ```
>     

***

## ↕️ 정렬

정렬은 **응답 데이터의 순서를 지정**할 때 사용된다. 필터링과 마찬가지로, 대량의 데이터를 반환하는 API에서 꼭 필요한 기능이다.

일반적으로는 서버에서 지정한 기본 정렬 순서가 있지만, 필요한 경우 쿼리 매개변수를 사용해 이를 **덮어쓰고 원하는 순서로 데이터**를 정렬할 수 있다.

<mark><strong>매개변수</strong></mark>

| **매개변수** | **설명** |
| --- | --- |
| `sort` 또는 `order_by` | 정렬 기준 필드를 지정 |
| `order` 또는 `direction` | 정렬 방향을 지정 <br> 예: `asc`(오름차순), `desc`(내림차순) |

<mark><strong>요청 예시</strong></mark>

```
# 단일 필드 정렬
GET /products?sort=price&order=desc        # price를 기준으로 내림차순 정렬
GET /products?sort=price:desc              # 위와 동일
GET /products?sort=-price                  # 위와 동일

# 복수 필드 정렬
GET /products?sort=price:desc,name:asc     # price 내림차순 → name 오름차순 순으로 정렬
GET /products?sort=-price,name             # 위와 동일
```

- 기본적으로는 필드와 방향을 각각 지정하지만, 이 둘을 콜론(`:`)으로 연결해 `sort`에 함께 지정할 수도 있다.
    
    `sort=<필드>&order=<방향>` → `sort=<필드>:<방향>`
    
- 방향(`asc`, `desc`)을 축약해 다음과 같이 표현하기도 한다.
    - 오름차순은 별도 표기 없이 필드명만 쓴다.
        
        `sort=<필드>&order=asc` → `sort=<필드>` 
        
    - 내림차순은 필드명 앞에 `-` 기호를 붙인다.
        
        `sort=<필드>&order=desc` → `sort=-<필드>`
        
이 외에도 다양한 정렬 방식이 있을 수 있으니 API 문서를 확인하는 것이 좋다.

***

## 🧾 응답 형식 지정(*잘 사용하지 않음)

API 응답의 형식을 쿼리 매개변수로 지정하기도 한다.

```
GET http://api.example.com/restaurants?format=xml
```

그러나 최근에는 보통 `Accept`**라는 헤더(header)를 사용**해 형식을 지정한다. 

```
Accept: application/xml
```

***

## 🔐 인증(*잘 사용하지 않음)

인증 정보를 쿼리 매개변수에 포함하는 경우도 있다. 하지만 **보안상의 이유로 권장되지는 않는다.**

```
GET http://api.example.com/restaurants?token=abcd1234
```

대신 `Authorization`**라는 헤더를 사용**하는 것이 일반적이다. 

```
Authorization: Bearer abcd1234
```

**헤더**와 **인증**에 관해서는 이후 글에서 더 자세히 알아보자.

---

# 쿼리 매개변수 문서화 방법

지금까지 본 것처럼, **쿼리 매개변수**는 페이지 매김, 필터링, 정렬 등 다양한 조건을 활용해서 보다 구체적인 요청을 구성할 수 있게 도와주는 중요한 수단이다.

따라서 **API 문서**를 작성할 때는 어떤 쿼리 매개변수를 사용할 수 있는지, 어떤 역할을 하는지 등의 정보를 개발자가 파악할 수 있도록 명확하게 설명해야 한다.

API 문서에서 쿼리 매개변수는 주로 **표 형태**로 정리하며, 각 매개변수의 이름, 설명, 타입, 필수 여부, 예시나 제약 조건 등을 포함한다.

예를 들어 음식점을 조회하는 API가 있다면, 아래와 같이 쿼리 매개변수를 정리할 수 있다.

| 매개변수         | 설명                | 타입      | 필수 여부 | 비고                                           |
| ------------ | -------------------- | ------- | ------- | --------------------------------------------- |
| `type`       | 음식 종류              | string  | 선택     | 예: `thai`, `mexican`             |
| `city`       | 도시명                 | string  | 선택    | 예: `seoul`, `busan`               |
| `rating_gte` | 최소 평점               | float   | 선택    | 1.0~5.0 사이의 값, 0.5 단위          |
| `sort`       | 정렬 기준 필드 지정       | string  | 선택    | 예: `rating`, `price`              |
| `order`      | 정렬 방향 지정           | string  | 선택    | `asc`(오름차순), `desc`(내림차순) <br> 기본값: `asc`         |
| `limit`      | 응답으로 받을 최대 결과 수  | integer | 선택    | 최대: 30, 기본값: 10                 |
| `offset`     | 결과 시작 지점           | integer | 선택    | 기본값: 0     |

그럼 위의 표를 참고해 아래와 같은 요청을 구성할 수 있게 된다.

```
GET /restaurants?type=thai&rating_gte=3.5&sort=rating&order=desc&limit=5&offset=10
```

<br>

문서화 방식은 서비스마다 다르니, 여러 API 문서를 참고해 보면 좋다.

- 네이버 클라우드 예시([출처](https://api.ncloud-docs.com/docs/blockchainservice-viewnetworklist))
  <img src="/assets/images/posts_img/2025-06-21-api-docs-rest-3/01-naver-cloud-query-params-example.png" alt="네이버 클라우드의 쿼리 매개변수 문서화 예시" width="550">  

- 카카오맵 예시([출처](https://developers.kakao.com/docs/latest/ko/talkcalendar/rest-api))
  <img src="/assets/images/posts_img/2025-06-21-api-docs-rest-3/02-kakaomap-query-params-example.png" alt="카카오맵의 쿼리 매개변수 문서화 에시" width="500">

---
 
**참고자료**

- *Udemy - [Learn API Technical Writing 2: REST for Writers](https://www.udemy.com/course/learn-api-technical-writing-2-rest-for-writers/)*
- *Apidog - [How to Implement Pagination in REST APIs (Step by Step Guide)](https://apidog.com/blog/pagination-in-rest-apis/)*
- *Atatus - [REST API Design: Filtering, Sorting, and Pagination](https://www.atatus.com/blog/rest-api-design-filtering-sorting-and-pagination/#filtering-methods)*
- *Dev.to (Pragati Verma) - [Unlocking the Power of API Pagination](https://dev.to/pragativerma18/unlocking-the-power-of-api-pagination-best-practices-and-strategies-4b49)*
- *Oracle - [REST API query parameters](https://docs.oracle.com/en/cloud/saas/cx-commerce/22b/ccdev/rest-api-query-parameters.html)*
- *Speakeasy - [API Design Guide](https://www.speakeasy.com/api-design)*