---
title: "[HTML_CSS 미니 프로젝트] 행사 랜딩 페이지 만들기"
excerpt: "HTML과 CSS를 활용해 간단한 행사 랜딩 페이지를 설계하고 구현해 보자."

categories:
  - HTML_CSS
tags:
  - [HTML, CSS]

permalink: /html-css/mini-project-event-landing-page

toc: true
toc_sticky: true

date: 2025-04-08
last_modified_at: 2025-04-08
---

HTML과 CSS를 활용한 첫 번째 미니 프로젝트로 **행사 랜딩 페이지**를 제작해 보았다.  

CSS의 모든 개념을 알지는 못하지만, 실제로 웹페이지를 만들어 보면서 익히고 싶었다.  

이번 프로젝트에서는
- 계획한 레이아웃과 마진을 정확하게 구현하고,
- 잘 구조화된 HTML과 CSS 파일을 만들겠다는 목표를 두었다.

---

## 전체 페이지 구상하기

전체 페이지를 다섯 개의 주요 섹션으로 구성했다.

1. 상단 소개(Hero Section)
2. 행사 설명(About Section)
3. 연사 소개(Speakers Section)
4. 일정(Schedule Section)
5. 이메일 구독(Sign-Up Section)

<br>

ChatGPT로 섹션별 더미 텍스트를 생성했고, Figma를 사용해 전체 구도와 요소 간 간격을 스케치했다.

<p align="center">
  <img src="/assets/images/posts_img/2025-04-08-html-css-01/01-figma-overall-sketch.png" alt="전체 Figma 스케치" width="400">
  <br>
  <em>Figma 스케치</em>
</p>

<br>

세부 사항은 아래에서 더 다루겠지만, 기본 규칙은 다음과 같다.

- 모든 섹션의 패딩은 사방 80px
- (Hero 섹션 제외) 각 섹션의 타이틀과 그 아래 콘텐츠 간 마진은 40px
- 버튼은 Hero 섹션과 Sign-Up 섹션에 각각 하나씩, 총 두 개 사용
- 두 버튼은 스타일은 동일하고, 배경색과 글자색만 다르게 설정

---

## HTML 구조 잡기

먼저 페이지의 기본 뼈대가 될 HTML을 작성했다. 

템플릿을 복붙하고 `<head>`의 `<title>`에 행사명을 넣어주었다.  
CSS 파일은 `style.css`로 분리했다.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Creative Minds Conference 2025</title>
  <link rel="stylesheet" href="style.css" />
</head>
<body>
...
</body>
</html>
```

`<body>` 에는 `<section>` 태그를 사용해 각 섹션을 나눴는데, 어려운 부분은 없었기 때문에 자세한 과정은 생략하겠다. HTML 파일만 작성한 후의 모습은 대략 이렇다.  

<p align="center">
  <img src="/assets/images/posts_img/2025-04-08-html-css-01/02-html-only-preview.png" alt="HTML만 적용한 초기 화면" width="500">
  <br>
</p>

더미 이미지는 [https://placehold.co](https://placehold.co/) 서비스를 활용해 불러왔다.  

이어서 CSS 작업을 하며 각 요소에 class명을 추가했다.

---

## CSS 스타일링하기

### ✅ 초기화

간단한 리셋으로 브라우저 기본 스타일을 제거했다. 이미지에는 `max-width: 100%`를 설정해 부모 요소를 넘지 않도록 했고, 비율 유지를 위해 `height: auto`를 추가했다.

```css
/* ========== Reset ========== */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

button {
  background: none;
  border: none;
  cursor: pointer;
}

img {
  max-width: 100%;
  height: auto;
}
```

<br>

### ✅ 변수 설정

여러 색상을 사용하지는 않았지만, 추후 쉽게 관리할 수 있도록 자주 쓰는 색을 변수로 선언했다.

```css
/* ========== Variables ========== */
:root {
  --primary-bg: #161179;
  --primary-text: #000;
  --secondary-text: #fff;
  --border-color: #ccc;
}
```

<br>

### ✅ 타이포그래피

기본 폰트는 내가 좋아하는 Roboto로 지정하고, 줄 간격, 글자 색, 배경색 등을 설정했다.  브라우저 크기가 줄어들어도 `<body>`의 너비가 최소 360px은 유지하도록 `min-width: 360px`를 설정했다.

```css
/* ========== Typography ========== */
body {
  font-family: Roboto, sans-serif;
  line-height: 1.6;
  color: var(--primary-text);
  background: var(--secondary-text);
  min-width: 360px;
}
```

모든 섹션의 최대 넓이는 1440px로, 패딩은 80px로 적용했다. `margin: 0 auto`로 섹션을 가운데 정렬해 주었다.

```css
section {
  width: 100%;
  max-width: 1440px;
  padding: 80px;
  margin: 0 auto;   
}
```

제목 및 본문 텍스트의 크기도 지정했다.  
예외로 Hero 섹션의 `h1`은 하단 마진이 10px이어야 하므로, 이따 별도 설정할 예정이다.

```css
section h1 {
  font-size: 2.8rem;
  margin-bottom: 40px;
}

section h2 {
  font-size: 2.4rem;
  margin-bottom: 40px;
}

section h3 {
  font-size: 2rem;
}

section p, ul {
  font-size: 1.6rem;
}
```

<br>

### ✅ 버튼

두 버튼의 공통 스타일을 적용했다.

```css
/* ========== Button ========== */
button {
  width: 100%;
  max-width: 250px;
  height: 70px;
  padding: 12px 24px;
  font-size: 1.4rem;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  text-align: center;
}
```

<br>

### ✅ 섹션별

#### **1. Hero 섹션**
  
  Hero 섹션에서는 먼저 배경색과 글자 색을 설정하고,
  
  ```css
  /* ========== Hero Section ========== */
  .hero-section {
    background-color: var(--primary-bg);
    color: var(--secondary-text);
  }
  ```
  
  `h1`(Creative Minds Conference 2025)의 하단 마진은 10px로,  
  `h3`(Unlock Your Imagination)의 하단 마진은 60px로 설정했다.
  
  ```css
  .hero-section h1 {
    margin-bottom: 10px;
  }
  
  .hero-section h3, {
    margin-bottom: 60px;
  }
  ```
  
  <p align="center">
    <img src="/assets/images/posts_img/2025-04-08-html-css-01/03-firma-hero-margin.png" alt="Hero 섹션 마진" width="500">
    <br>
    <em>Figma 스케치와 비교</em>
  </p>



  끝으로 REGISTER 버튼 배경색, 글자 색 및 상단 마진을 설정했다.
  
  ```css
  .hero-section .button {
    background: var(--secondary-text);
    color: var(--primary-text);
    margin-top: 60px;
  }
  ```

<br>

#### **2. About 섹션**

  딱히 설정할 게 없어서 생략!

<br>

#### **3. Speakers 섹션**
    
  <p align="center">
    <img src="/assets/images/posts_img/2025-04-08-html-css-01/05-figma-speakers-margin.png" alt="Speakers 섹션 마진" width="500">
    <br>
  </p>

  `display: flex`로 각 연사 박스를 가로로 정렬하고, `gap`으로 박스 사이 간격을 60px로 설정했다. 또, 화면이 작을 때 연사 박스가 자동으로 줄 바꿈 되도록 `flex-wrap: wrap`을 적용했다.  
  사진 크기와 하단 마진도 설정해 줬다.
  
  ```css
  /* ========== Speakers Section ========== */
  
  .speaker-box {
    display: flex;
    flex-wrap: wrap;
    gap: 60px;
    justify-content:left;
    }

  .speaker-box img {
    width: 387px;
    height: 380px;
    margin-bottom: 40px;
    }
  ```
  
  💡 `flex`는 바로 아래에 있는 자식 요소만 정렬하기 때문에, HTML 파일에서 각 연사 정보를 <div>로 묶어줬다.
   
   
  ```html
  <div class="speaker-box">
  	<div>
  		<img src="https://placehold.co/387x380">
      <h3>Speaker One</h3>
      <p>Position</p>
      <p>Company Name</p>
    </div>
    ...
  </div>
  ```
 
  ❗ 그런데 이런 식으로 사진 크기와 `gap`을 고정하니, 아무리 큰 화면으로 봐도 **마지막 사진 하나가 줄 바꿈 되어 아래로 내려가는** 문제가 발생했다. 검색하다가 `grid-template-columns` 속성에 `auto-fit`과 `minmax()`를 사용해 해결하는 방법이 있다는 걸 알게 되었다.

  아래와 같이 수정해, 각 그리드의 아이템이 최소 300px의 너비를 유지하되, 남는 공간이 있으면 화면 너비에 맞게 늘어나도록 했다.

  ```css
  .speaker-box {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    gap: 60px;
    }
  ```

  이미지는 그리드 셀의 너비에 맞춰 꽉 채우고, 비율은 유지되도록 다음과 같이 설정했다.

  ```css
  .speaker-box img {
    width: 100%;
    height: auto;
    display: block;
    margin-bottom: 40px;
    }
  ```

<br>

#### **4. Schedule 섹션**
    
  목록 부분이 왼쪽으로 삐져나오는 게 보기 싫어서 들여쓰기를 살짝 적용했다.
  
  ```css
  /* ========== Schedule Section ========== */
  .schedule-section li {
    margin-left: 16px;
  }
  ```

  <p align="center">
    <img src="/assets/images/posts_img/2025-04-08-html-css-01/06-figma-schedule-margin.png" alt="	Schedule 섹션 마진" width="500">
    <br>
  </p>

<br>

#### **5. Sign-Up 섹션**
  
  각 입력창의 스타일을 설정했다. `border-radius`는  버튼과 동일하게 4px로 설정했다.
  
  ```css
  /* ========== Sign-Up Section ========== */
  .sign-up-section input {
    width: 100%;
    max-width: 900px;
    padding: 10px;
    font-size: 1rem;
    border: 1px solid var(--border-color);
    border-radius: 4px;
    margin-top: 18px;
    margin-bottom: 20px;
  }
  ```
  
  마지막으로, SIGN UP 버튼의 배경색과 글자 색을 설정했다.
  
  ```css
  .sign-up-section .button {
    background: var(--primary-text);
    color: var(--secondary-text);
    margin-top: 40px;
  }
  ```
  
  입력창의 `margin-bottom`이 20px이므로, 버튼의 `margin-top`은 40px으로 설정해 입력창과 버튼 사이 마진을 60px로 맞췄다.
  
  <p align="center">
    <img src="/assets/images/posts_img/2025-04-08-html-css-01/07-figma-sign-up-margin.png" alt="Sign-up 섹션 마진" width="500">
    <br>
  </p>
  
  > **💭 Note to Self**: 앞의 입력창 부분에서 `:not(:last-child)` 선택자를 사용해, 마지막 입력창에 하단 마진이 적용되지 않도록 설정했으면 더 깔끔했을 것 같다. 그렇게 하면 버튼의 상단 마진을 계산할 필요 없이 60px로 설정하면 된다.

---

## 최종 결과물

결과물은 다음과 같다. 스케치했던 구조와 거의 비슷하게 완성되었다.

<p align="center">
  <img src="/assets/images/posts_img/2025-04-08-html-css-01/08-initial-browser-screenshot.png" alt="전체 페이지 브라우저 화면" width="400">
  <br>
</p>

다만, 실제로 브라우저에서 확인해 보니 글자, 버튼, 입력창 등이 조금 부담스러울 정도로 커 보였다. 따라서 전체 디자인이 더 조화롭게 보이도록 크기를 조금씩 조정했다. 연사 사진도 줄여서, 기존에는 한 줄에 세 명이 배치됐던 것을 네 명까지 들어가도록 수정했다.

<p align="center">
  <img src="/assets/images/posts_img/2025-04-08-html-css-01/09-final-browser-screenshot.png" alt="수정한 전체 페이지 브라우저 화면" width="400">
  <br>
</p>

이렇게 최종 완성했다.

---

## 마무리

완성한 HTML 및 CSS 파일은 미니 프로젝트용으로 만든 GitHub 저장소에 올렸다.  
Codepen으로 데모도 만들어 `README.md`에 링크를 추가했다.

- GitHub 저장소: [https://github.com/seulahn/html-css-projects](https://github.com/seulahn/html-css-projects)
- Codepen 데모: [https://codepen.io/seulahn/pen/ByaeWMy](https://codepen.io/seulahn/pen/ByaeWMy)  

<br>

아직 CSS와 작업 방식에서 부족한 점이 많다. 더 깔끔하고 빈틈없는 CSS를 짜는 날까지 🐣