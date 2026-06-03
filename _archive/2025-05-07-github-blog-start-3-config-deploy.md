---
title: "[깃알못의 GitHub 블로그 시작하기] 3. 블로그 기본 설정과 GitHub Pages 배포"
excerpt: "_config.yml 및 navigation.yml 파일을 수정하고 블로그를 세상에 공개하자." 

categories:
  - GitHub Blog
tags:
  - [GitHub, Blog]

permalink: /github-blog/github-blog-start-3-config-deploy

toc: true
toc_sticky: true

date: 2025-05-07
last_modified_at: 2025-05-07
---

[이전 글](https://seulahn.github.io/github-blog/github-blog-start-2-jekyll-setup)에서는 Jekyll을 설치하고 로컬 환경에서 블로그를 실행하는 과정을 살펴보았다.

이번 글에서는 블로그 정보를 설정하고 GitHub Pages를 통해 배포해 보겠다. 이전 단계에 비하면 매우 쉽다!

블로그를 얼마나 완성한 뒤에 배포할지는 개인의 선택이다. 나는 favicon을 추가하고 첫 게시물까지 작성한 뒤 배포했지만, 꼭 그럴 필요는 없다. 이 글에서는 기본 정보만 설정하고 배포하는 흐름으로 설명하려고 한다.

---

## 블로그 정보 설정하기

> 사용하는 테마에 따라 파일명이나 경로가 다를 수 있으므로, 선택한 테마의 `README.md` 파일을 먼저 참고하길 추천한다.
> 

<br>

### ✅ 기본 정보 변경

블로그의 기본 정보는 `_config.yml` 파일에서 관리할 수 있다. 이 파일에는 여러 항목이 있는데, 기본 설정을 위해 `# Site Settings`, `# Site Author`, `# Footer` 항목만 수정했다.

<br>

**`# Site Settings` 항목**

아래의 예시처럼 블로그 언어, 이름, URL 등을 설정한다.

```yaml
# Site Settings
locale: "ko-KR" # 블로그 언어(영문이라면 "en-US")
title: "Seul.Log()" # 상단 헤더와 브라우저 탭에 표시될 블로그 이름
title_separator: "&#124;"
subtitle: # 서브타이틀, 사용하지 않으면 비워둬도 됨
name: "Seul" # 작성자 이름 또는 닉네임
description: "Seul's blog" # 블로그 설명
url: "https://seulahn.github.io" # 블로그 URL
baseurl: # 웹사이트의 서브 경로, 사용하지 않으면 비워둬도 됨
repository: "seulahn/seulahn.github.io" # GitHub 저장소 경로
teaser: # 게시물 썸네일로 사용할 기본 이미지의 경로, 사용하지 않으면 비워둬도 됨
```

> **`baseurl`**은 서브 디렉토리에서 웹사이트를 호스팅할 때만 사용한다. 예를 들어 저장소 내 특정 폴더(`https://<사용자 이름>.github.io/myblog/`)에 웹사이트를 배포할 경우, `baseurl: "/myblog"`와 같이 서브 경로를 지정해야 한다. 하지만 이번 글처럼 루트 경로(`https://<사용자 이름>.github.io/`)에 배포하는 경우에는 비워두면 된다  
… 고 이해했다.
> 

<br>

**`# Site Author` 항목**

작성자 정보를 설정한다.

```yaml
# Site Author
author:
  name: "Seul" # 블로그 닉네임
  avatar: "/assets/images/meee.png" # 블로그 프로필 사진
  # bio              : "hi all!"
  # location         : "Seoul, Korea"
  # email            : "abc@email.com"
```

나는 프로필 이미지를 `/assets/images` 디렉토리에 업로드한 뒤 경로를 지정했다. `bio`, `location`, `email` 등은 필요에 따라 추가하거나 주석 처리할 수 있다.

<br>

**`# Footer` 항목**

블로그 하단에 표시할 외부 링크를 추가한다.

```yaml
# Site Footer
footer:
  links:
    - label: "Twitter"
      icon: "fab fa-fw fa-twitter-square"
      # url:
    - label: "Facebook"
      icon: "fab fa-fw fa-facebook-square"
      # url:
    - label: "GitHub"
      icon: "fab fa-fw fa-github"
      url: "https://github.com/seulahn"
		...
```

나는 GitHub 프로필 URL만 추가하고, 사용하지 않는 나머지 URL은 주석 처리했다.

<br>

### ✅ 상단 네비게이션과 카테고리 설정

블로그의 상단 네비게이션과 카테고리는 [`_data/navigation.yml`] 파일에서 관리할 수 있다.

`# main links`와 `# sidebar navigation` 항목으로 나뉘어 있다.

<br>

**`# main links` 항목**

상단 네비게이션을 설정한다.

```yaml
# main links
main:
  - title: "Home"
    url: https://seulahn.github.io/

  - title: "About"
    url: /about/

  - title: "GitHub"
    url: https://github.com/seulahn
```

<br>

**`# sidebar navigation` 항목**

사이드바에 표시되는 카테고리를 설정한다. 나는 두 개의 카테고리를 설정했다. 

```yaml
# sidebar navigation (categories)
categories:
  - title: "HTML/CSS"
    url: /categories/html-css/
  - title: "GitHub Blog"
    url: /categories/github-blog/
```

> 카테고리 이름에 단어가 두 개 이상일 경우, URL에서는 **하이픈(-)**으로 단어를 이어주는 게 일반적이다.
> 

---

## 불필요한 파일 정리하기

블로그를 이대로 배포해도 되지만, 사용하지 않는 파일을 삭제해 두면 추후 디렉토리를 관리하기가 더 편해진다.

Minimal Mistakes 테마의 [공식 가이드](https://mmistakes.github.io/minimal-mistakes/docs/quick-start-guide/#remove-the-unnecessary)에 따르면 아래 항목은 삭제해도 무방하다.

- `.editorconfig`
- `.gitattributes`
- `.github`
- `/docs`
- `/test`
- `CHANGELOG.md`
- `minimal-mistakes-jekyll.gemspec`
- `README.md`
- `screenshot-layouts.png`
- `screenshot.png`

내가 포크한 저장소는 커스텀 버전이라 일부 항목은 아예 존재하지 않았다. 이 경우, 찾을 수 있는 항목만 삭제하면 된다.  
참고로 `README.md` 파일은 삭제한 뒤, 나중에 블로그에 맞게 새로 작성했다.

---

## GitHub Pages로 배포하기

이제 수정한 파일을 GitHub 저장소에 반영하고, GitHub Pages를 통해 배포한다.

<br>

### ✅ **변경 사항 적용**

터미널에서 아래 명령어를 입력해 변경 사항을 커밋(commit)하고, GitHub로 푸시(push)한다.

```bash
git add .
git commit -m "<커밋 메시지 입력>"
git push origin master
```

> 사용하는 테마에 따라 기본 브랜치의 이름이 `master`가 아닌 `main`일 수도 있다. `git branch` 명령어를 입력하면 현재 브랜치 이름을 확인할 수 있다.
> 

<br>

### ✅ **GitHub Pages로 배포**

마지막으로, GitHub 저장소로 이동한다.

1. 상단 메뉴에서 **Settings** > **Pages**로 이동한다.
2. Source 항목에서 **Deploy from a branch**를 선택한다.
3. Branch 항목에서 `master`(또는 `main`)를 선택하고, **[Save]**를 누른다.

<img src="/assets/images/posts_img/2025-05-07-github-blog-start-3/01-github-pages-settings.png" alt="GitHub Pages 설정 화면" width="600">

그리고 브라우저에서 블로그 URL로 접속하면 배포 결과를 확인할 수 있다. 🥳 GitHub Pages 변경 사항이 적용되기까지 10분 정도 걸릴 수 있다고 하는데, 나는 거의 바로 접속됐다.

---

### 마무리

블로그 테마를 선택하고, 로컬에서 실행하고, 배포하기까지. 시작할 때는 필요한 프로그램도 많고 용어도 복잡해 보였지만, 하나씩 검색하면서 문제를 해결하다 보니 흐름을 이해할 수 있었다.

지금도 게시물을 발행하면서 새로운 문제를 계속 마주한다. 분명 로컬에서는 글이 보이는데 웹사이트에서는 안 보인다든지, 이미지가 엑박으로 뜬다든지… 이런 장애물을 하나씩 넘는 게 GitHub 블로그를 운영하는 재미인 것 같다. 

앞으로는 favicon 설정, 게시물 발행, 댓글 기능 추가 등, 블로그를 가꿔나가는 과정을 천천히 다뤄볼 예정이다.