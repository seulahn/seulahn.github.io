---
title: "[깃알못의 GitHub 블로그 시작하기] 2. Jekyll 환경 구축에서 로컬 서버 실행까지"
excerpt: "Ruby와 Jekyll을 설치하고, 로컬 서버에서 블로그를 확인해보기."

categories:
  - GitHub Blog
tags:
  - [GitHub, Blog]

permalink: /github-blog/github-blog-start-2-jekyll-setup

toc: true
toc_sticky: true

date: 2025-04-07
last_modified_at: 2025-04-07
---

[이전 글](https://seulahn.github.io/github-blog/github-blog-start-1-theme-setup)에서는 블로그 테마를 선택하고, 로컬로 클론하는 과정까지 살펴보았다.  

이번 글에서는 Jekyll 환경을 구축해 로컬에서 블로그를 실행해 보자. 

---

## 개발 환경 설정하기

Jekyll 블로그를 로컬에서 실행하려면 먼저 몇 가지 도구를 설치해야 한다. 내 Mac이 거의 백지상태라서 설치할 게 많았다. 

<br>

### 📌 설치 순서(macOS 기준)

**<mark>Homebrew → rbenv, ruby-build → Ruby → Bundler, Jekyll</mark>**

각 도구가 무엇인지, 왜 필요한지는 설치 단계에서 함께 설명하겠다.

<br>

### 📌 설치 전 알아둘 점

- 로컬 서버에서 블로그를 실행하려면 **Jekyll**이 필요하다.
- Jekyll은 **Ruby**라는 언어로 작성됐기 때문에 먼저 Ruby를 설치해야 한다.

<br>

macOS에는 시스템 Ruby가 기본으로 내장되어 있지만, 대개 오래된 버전이라 문제가 발생할 수 있다. 따라서 최신 버전의 Ruby를 별도로 설치해 줘야 한다. (시스템 Ruby를 사용하면 안 되는 더욱 자세한 이유는 [이 아티클](https://www.moncefbelyamani.com/why-you-shouldn-t-use-the-system-ruby-to-install-gems-on-a-mac/#apple-will-likely-stop-preinstalling-ruby-on-macos)에서 확인할 수 있다.)  

터미널에서 아래 명령어를 입력해 보자. 현재 설치된 Ruby 버전을 확인할 수 있다.

```bash
ruby -v
```

나는 Ruby를 따로 설치한 적이 없는데도 아래와 같이 `2.6.10`이 출력됐다. 바로 시스템 Ruby다.

```bash
ruby 2.6.10p210 (2022-04-12 revision 67958) [universal.arm64e-darwin22]
```

이 버전은 그대로 두고, 버전 관리 도구를 사용해 최신 Ruby를 설치할 예정이다. 
 
<br>

그럼 시작해 보자!

<br>

### ✅ Homebrew 설치(**이미 있다면 생략)**

먼저 Hombrew를 설치한다.

> **Hombrew**는 macOS에서 가장 널리 쓰이는 패키지 관리자다. rbenv 같은 도구를 간편하게 설치하도록 도와준다.

<br>

터미널에 아래 설치 명령어를 입력한다. 명령어는 [Homebrew 웹사이트](https://brew.sh/)에서 확인했다.

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)" 
```

설치가 완료되면, 아래 명령어를 입력해 정상 설치되었는지 확인한다.

```bash
brew -v
```

버전 정보(`Homebrew 4.4.27` 등)가 출력되면 성공이다.  

<br>

❗ 아래와 같은 오류가 나온다면, Homebrew 경로가 PATH에 등록되지 않았기 때문이다.  
`zsh: command not found: brew`

경로를 설정하기 위해 `.zshrc` 파일을 연다.

```bash
nano ~/.zshrc
```

열린 파일의 맨 아래에 다음 줄을 추가한다.

```bash
export PATH="/opt/homebrew/bin:$PATH"
```

`Control + X` → `Y` → `Enter`를 눌러 저장하고 종료한 뒤, 변경 사항을 적용한다.

```bash
source ~/.zshrc
```

다시 `brew -v`를 입력해 잘 작동하는지 확인하자.

> **💡 왜 경로를 설정해야 할까?**  
> 터미널에서 명령어를 실행할 때, 터미널은 환경 변수 PATH에 등록된 디렉토리에서 실행 파일을 찾는다. Homebrew로 설치한 프로그램, rbenv, Ruby 등의 실행 파일은 기본 PATH에 포함되지 않는다. 따라서 직접 PATH를 지정해 줘야 터미널이 명령어를 인식할 수 있다.
> 

<br>

### ✅ **rbenv와 ruby-build** 설치

이제 Hombrew를 사용해 rbenv와 ruby-build를 설치한다. 

> **rbenv**는 여러 버전의 Ruby를 쉽게 관리할 수 있도록 도와주는 버전 관리 도구다.  
**ruby-build**는 Ruby 설치 기능을 제공하는 rbenv의 플러그인이다. rbenv는 ruby-build가 있어야 Ruby를 설치할 수 있다.

<br>

다음 명령어로 `rbenv`와 `ruby-build`를 설치한다.

```bash
brew install rbenv ruby-build 
```

정상 설치되었는지 확인한다.

```bash
rbenv -v
```

버전 정보(`rbenv 1.2.0` 등)가 출력되면 성공이다.

<br>

❗ 아래와 같은 오류가 나온다면, rbenv 경로가 PATH에 등록되지 않았기 때문이다.  
`zsh: command not found: rbenv`

마찬가지로 `.zshrc` 파일을 연다.
```bash
nano ~/.zshrc
```

열린 파일의 맨 아래에 다음 두 줄을 추가한다.

```bash
export PATH="$HOME/.rbenv/bin:$PATH"
eval "$(rbenv init -)"
```

`Control + X` → `Y` → `Enter`를 눌러 저장하고 종료한 뒤, 변경 사항을 적용한다.

```bash
source ~/.zshrc
```

다시 `rbenv -v`를 입력해 잘 작동하는지 확인하자.

<br>

### ✅ Ruby 설치

이제 rbenv를 사용해 원하는 Ruby 버전을 설치한다. 

> **Ruby**는 Jekyll의 기반이 되는 프로그래밍 언어다. Jekyll을 사용하기 위해서는 Ruby가 필요하다.

<br>

다음 명령어를 입력해 설치할 수 있는 Ruby 버전 목록을 확인한다.

```bash
rbenv install -l
```

출력 예시(작성 시점 기준)

```bash
3.1.7
3.2.8
3.3.7
3.4.2
...
```

이 중 원하는 버전을 선택해서 설치하고, 기본값으로 설정한다. 나는 `3.1.7` 버전을 선택했다.  
설치가 꽤 오래 걸렸다. 30분 정도?

```bash
# Ruby 3.1.7 버전 설치
rbenv install 3.1.7

# 기본 Ruby 버전으로 설정
rbenv global 3.1.7
```

정상 설치되었는지 확인한다.

```bash
ruby -v
```

방금 설치한 버전 정보(이 경우 `ruby 3.1.7`)가 출력되면 성공이다.

<br>

### ✅ Bundler와 Jekyll 설치

Ruby에 내장된 RubyGems를 사용해 Bundler와 Jekyll을 설치한다.

> **RubyGems**는 Ruby에 내장된 패키지 관리 프레임워크다. Ruby로 작성된 다양한 패키지(gem)를 설치하고 관리하도록 돕는다.  
**Bundler**와 **Jekyll**은 모두 gem의 일종이다. Bundler는 프로젝트에 필요한 gem과 버전을 추적하고 설치해 주는 도구다(<em>대충 Jekyll을 시작할 때 Bundler를 사용하는 게 좋다고 이해했다</em>). Jekyll은 알다시피 정적 사이트 생성기다. 블로그 실행을 위한 우리의 최종 목표!

<br>

아래 명령어로 Bundler와 Jekyll을 설치한다:

```bash
gem install bundler jekyll
```

정상 설치되었는지 확인한다.

```bash
bundler -v
jekyll -v
```

버전 정보(`Bundler version 2.6.6`, `jekyll 4.4.1` 등)가 출력되면 성공이다.

<br>

❗ 다른 튜토리얼을 보면 문제가 없어야 하는 것 같은데, 나는 `jekyll -v`을 입력했을 때 계속해서 `zsh: command not found: jekyll` 오류가 발생했다. 기나긴 삽질 끝에... `.zshrc` 파일을 열어서 Ruby 경로를 설정하니 해결됐다.

```bash
# .zshrc 파일 열기
nano ~/.zshrc

# 열린 파일의 맨 아래에 다음 두 줄 추가
export PATH="$HOME/.rbenv/shims:$PATH"
export PATH="$HOME/.gem/ruby/3.1.7/bin:$PATH" # 3.1.7은 본인이 설치한 Ruby 버전으로 대체
```

`Control + X` → `Y` → `Enter`를 눌러 저장하고 종료한 뒤, 변경 사항을 적용한다.

```bash
source ~/.zshrc
```

<br>

## 로컬에서 블로그 실행하기

드디어 Jekyll 설치를 완료했다. 이제 블로그를 눈으로 확인해 볼 차례다.

<br>

### ✅ Jekyll 실행

먼저 블로그 작업 디렉토리로 이동한다.

```bash
cd <작업할 디렉토리 경로>
```

필요한 gem을 설치하고, 서버를 실행한다.

```bash
bundle install
bundle exec jekyll serve
```

그러면 코드가 주르륵 쏟아지면서 Server running이라는 감격의 메시지를 확인할 수 있다.

```
  Server address: http://127.0.0.1:4000
  Server running... press ctrl-c to stop.
```

아래 주소를 브라우저에 넣고 접속하면 블로그를 볼 수 있다.

`http://127.0.0.1:4000/`

<img src="/assets/images/posts_img/2025-04-07-github-blog-start-2/01-blog-main-local-preview.png" alt="로컬에서 실행된 블로그 메인 페이지 화면" width="600">

짠

---

거의 다 왔다! 이제 파일을 로컬에서 작성하거나 수정할 때, 변경 사항이 블로그에 어떻게 반영될지 로컬 서버에서 바로 확인할 수 있다.

이대로 GitHub에 올려서 블로그를 공개해도 되지만, 나는 먼저 몇 가지 기본 설정을 커스터마이징한 뒤에 배포했다.  

따라서 다음 글에서는 `_config.yml` 파일을 수정하고, 완성된 블로그를 GitHub에 올려서 배포하는 과정까지 정리해 보겠다.