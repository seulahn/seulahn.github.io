---
title: "[깃알못의 GitHub 블로그 시작하기] 1. 테마 선택, 포크 및 클론"
excerpt: "마음에 드는 테마를 선택하고 내 컴퓨터로 가져오기."

categories:
  - GitHub Blog
tags:
  - [GitHub, Blog]

permalink: /github-blog/github-blog-start-1-theme-setup

toc: true
toc_sticky: true

date: 2025-04-05
last_modified_at: 2025-04-05
---

이제 GitHub 블로그를 어떻게 시작했는지, 그 과정을 본격적으로 소개해 보려 한다.

블로그를 0부터 직접 만들 수도 있겠지만, 나는 기존의 테마를 활용해 빠르게 시작하는 방식을 택했다. 초기 개발에 드는 시간을 줄이고 블로그 운영과 커스터마이징에 더 집중하고 싶었기 때문이다.

---

## GitHub 블로그 구축 순서 개요

테마를 활용한다고 해도 GitHub 블로그를 처음 만드는 과정은 아주 복잡하게 느껴졌다.  
하지만 (지금 와서) 전체 흐름만 보면 꽤 단순하다.

- 원하는 테마를 GitHub에서 복사하고,
- 내 컴퓨터, 즉 로컬 환경으로 가져와 수정한 뒤,
- 다시 GitHub에 올려 웹에 게시한다!  

<br>

각 단계를 자세히 살펴보면 다음과 같다.

> 1. 사용할 테마를 고른다.
> 2. 선택한 테마의 GitHub 저장소를 **내 계정으로 포크(fork)**하고, 로컬로 **클론(clone)**한다.
> 3. 로컬에서 블로그를 실행하기 위한 환경(**Ruby, Bundler, Jekyll 등)**을 구축한다.
> 4. Jekyll을 사용해 **블로그를 실행해 본다.**
> 5. 블로그 제목, 소개, 카테고리 등 **기본 설정을 수정**한다.
> 6. **변경 사항을 GitHub에 푸시(push)**하고, **GitHub Pages를 사용해 배포**한다.

<br>

이번 글에서는 이 중 **1번과 2번**,  
즉 테마를 선택하고, 포크 및 클론하는 과정까지 살펴보겠다. 

---

## 테마 고르기

먼저, 블로그의 뼈대가 될 테마를 골라야 한다.  
나는 Jekyll 기반의 Minimal Mistakes 테마를 커스터마이징한 버전을 사용했다.

<br>

### 📌 Jekyll과 Minimal Mistakes란?

- **Jekyll**은 마크다운 파일을 정적인(static) 웹사이트로 변환해 주는 정적 사이트 생성기(static site generator)다.  
  블로그나 포트폴리오처럼, 저장된 콘텐츠를 사용자에게 그대로 보여주는 사이트에 적합하다. GitHub Pages를 기본으로 지원하며, 자유롭게 웹사이트를 커스터마이징할 수 있다는 장점이 있다.
    
- **Minimal Mistakes**는 Jekyll을 기반으로 만들어진 웹사이트 테마 중 하나다. 깔끔한 2단 레이아웃이 특징이다.

<br>

### ✅ 원하는 테마 선택

Minimal Mistakes 외에도 다양한 Jekyll 테마가 있다. 아래 사이트를 참고해 마음에 드는 테마를 골라보자.

<br>

**Minimal Mistakes 테마**

- 웹사이트: [https://mmistakes.github.io/minimal-mistakes/](https://mmistakes.github.io/minimal-mistakes/)
- GitHub 저장소: [https://github.com/mmistakes/minimal-mistakes](https://github.com/mmistakes/minimal-mistakes) 

<br>

**기타 Jekyll 테마 모음 사이트**

- [http://jekyllthemes.org/](http://jekyllthemes.org/)
- [https://jekyllthemes.io/](https://jekyllthemes.io/)
- [https://jekyll-themes.com/](https://jekyll-themes.com/) 

<br>

나는 Minimal Mistakes 테마를 그대로 쓰지는 않고, choiiis님의 커스터마이징 버전을 사용했다. 미니멀한 디자인이 딱 마음에 들어서 감사한 마음으로 포크했다. 🍴

- [choiiis님의 Minimal Mistakes customized 테마 GitHub 저장소](https://github.com/choiiis/minimal-mistakes-choiiis-customized)

---

## 원하는 테마를 로컬로 가져오기

테마를 선택했다면, 이제 로컬에서 블로그 작업을 시작할 차례다.  
그 첫 단계는 해당 테마의 GitHub 저장소를 **내 계정으로 포크**하고, **로컬로 클론**하는 것이다. 
  
<br>

### 📌 포크와 클론이란?

- **포크**는 다른 사용자의 저장소를 **내 GitHub 계정으로 복사**하는 작업이다. 원본 저장소를 직접 수정할 수는 없기 때문에 내 계정으로 포크해야 한다.
- **클론**은 GitHub에 있는 내 저장소를 **로컬로 복사**하는 작업이다. 이렇게 해야 로컬에서 저장소의 파일을 수정하고 실행할 수 있다. 

<br>

### ✅ 포크

1. 선택한 테마의 GitHub 저장소에 접속한다.

2. 오른쪽 상단의 **[Fork]** 버튼을 누른다.

    <img src="/assets/images/posts_img/2025-04-05-github-blog-start-1/01-click-fork-button.png" alt="GitHub에서 Fork 버튼을 누르는 화면" width="600">

3. 이때, 저장소 이름은 `<사용자 이름>.github.io` 형식으로 설정해야 한다.

    <img src="/assets/images/posts_img/2025-04-05-github-blog-start-1/02-set-repository-name.png" alt="저장소 이름을 설정하는 화면" width="600">

포크가 완료되면, 내 GitHub 계정에 저장소가 복사된 것을 확인할 수 있다.

  <img src="/assets/images/posts_img/2025-04-05-github-blog-start-1/03-forked-repository.png" alt="Fork가 완료되어 내 GitHub 계정으로 복사된 저장소 화면" width="600">

<br>

### ✅ 클론

1. 포크한 저장소 페이지로 이동한다.
2. **[Code]** 버튼을 눌러 **HTTPS 주소**를 복사한다.

    <img src="/assets/images/posts_img/2025-04-05-github-blog-start-1/04-copy-https.png" alt="Code 버튼을 눌러 HTTPS 주소를 복사하는 화면" width="600">

3. 터미널을 열고, 블로그 프로젝트를 작업할 디렉토리로 이동한다. 

    ```bash
    # 형식
    cd <작업할 디렉토리 경로>

    # 예시: 작업 디렉토리가 ~/workspace/blog인 경우
    cd ~/workspace/blog
    ```
 
    > 💡 내가 어디 있는지 헷갈린다? `pwd` 명령어를 입력하면 현재 디렉토리를 확인할 수 있다.
    > 
4. 복사한 HTTPS 주소를 사용해 저장소를 클론한다.
  
    ```bash
    # 형식
    git clone <HTTPS 주소>

    # 예시
    git clone https://github.com/seulahn/seulahn.github.io.git
    ```

클론이 완료되면, 해당 디렉토리에 저장소 파일이 복사된 것을 확인할 수 있다.

---

이제 저장소 파일을 무사히 로컬로 가져왔다.  
하지만 로컬에서 블로그를 실행하기 위해서는 이를 위한 환경을 먼저 구축해 줘야 한다.

다음 글에서는 **Jekyll 환경을 구축하고, Jekyll로 블로그를 실행하는 과정**을 살펴보겠다.