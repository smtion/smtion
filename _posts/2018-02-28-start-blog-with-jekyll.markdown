---
layout: single
title:  Jekyll로 블로그를 시작하며
date:   2018-02-28 20:58:00 +0900
tags: jekyll
---

공부하고 알아야할 것은 많아지고 기억력엔 한계가 있고 지식들을 정리하고 기록해야할 필요성이 절실하여 귀차니즘을 뒤로한 채 블로그를 개설하였다.
그리고 기술 블로그들을 읽어보며 요약 정리 및 레퍼런스를 보관도 해야하고, 그러면서 글쓰는 연습은 겸사겸사.

#### _기록은 기억을 지배한다._
{: .quote.text-center}

나이가 들 수록 저 말은 정말 최고의 명언으로 느껴진다.

## 블로그 플랫폼 선택

블로그는 가입형, 설치형, 무료 호스팅형 등 여러 유형이 있는데 ~~티스토리도 생각해 봤지만~~ 일단 가입형은 좋은 대안이 아닌 것 같다. (특히 네이버)

### 요구사항

- 서비스에 의존하는 것은 지양 (서비스의 영속성이 보장되지 않는다.)
- 최대한 심플
  - 포스팅의 시간을 최소화
- 콘텐츠 소유의 주체 = 나(내 로컬)
- 콘텐츠 관리의 용이
- 사이트 관리의 자유성과 편의성
  - 커스터마이징
  - 무료 호스팅
- Markdown 지원
- Highlight 및 Bootstrap 스타일 컴포넌트 등 지원

위의 요구사항들을 대부분 충족하여 Github을 연동하여 손쉽게 블로그를 운영할 수 있는 Github Pages가 제일 좋은 방법인 것 같다.
또한 요즘 개발자들이라면 Markdown에 익숙하여 Jykyll과 같은 프레임워크를 사용하여 간단하게 포스팅하기 좋다.

**개발자라면 Github이지!**

### Jekyll을 선택한 이유

Github과 연동하여 사용할 정적 사이트 생성기 중 **Jekyll** 과 **Hexo** 가 후보군.

Github 스타 기준으로 Jekyll > Hugo > Hexo 순이다. [순위보기](https://www.staticgen.com/)
{: .notice--info}

둘 다 사용해보다 Jekyll을 선택하였다. 그 이유는 아래와 같다.

- Github은 Jekyll을 기본으로 제공하고 있어 별도의 빌드가 필요 없다.
  - 깃헙 리포지토리에 소스를 푸시하기만 하면 자동 적용
  - Hexo는 빌드와 배포를 cli로 통해 한 번에 할 수 있지만 매 번 빌드에 걸리는 시간을 기다리기 싫다.
  - Github가 아닌 별도의 호스팅을 한다면 별 상관 없다.
- 맘에 드는 테마
  - 다양한 컴포넌트 및 정리가 잘 되어있는 문서
- 상대적으로 쉬운 커스터마이징
  - SASS 빌드 자동화가 기본 적용되어 있는 내장 서버
  - Hexo보다 front-end 도구를 덜 사용해도 됨 (사실 사용할 것이 없어서 편하다.)
- 깃헙 스타 1위
- 디렉토리와 파일로 콘텐츠를 관리 (제너레이션 프레임워크의 장점)
  - Atom과 같은 에디터로 콘텐츠 작성 가능

Hexo의 버전관리 이슈는 소스를 별도의 브랜치로 관리하면 되기 때문에 선택에 전혀 상관없다.
Hexo는 cli로 배포 시 자동으로 빌드된 정적 리소스만 master 브랜치에 푸시한다.
{: .notice--info}

### 단점

- 새 포스트 생성 시 불편함
  - 파일을 수동으로 생성
  - Hexo는 cli를 사용하여 포스트를 생성할 수 있다.
    - 파일명, Front-matter 등 자동 생성

### 설치

_Mac에서 진행_
{% highlight console %}
sudo gem install jekyll bunlder
jekyll new SITE_NAME
cd SITE_NAME
bundle install
jekyll serve
{% endhighlight %}

[공식 문서](http://jekyllrb-ko.github.io/docs/installation/)

### 제약사항

- 포스트의 파일명 포맷은 반드시 YYYY-MM-DD-POST-NAME-WITH-HYPENS
  - URL의 포맷은 변경 가능

Hexo는 포스트 파일명의 포맷을 변경할 수 있다.
{: .notice--info}

### 테마

Jekyll 테마 중 깃헙 스타 1위인 minimal-mistakes-jekyll을 사용하였다.

[테마 깃헙](https://github.com/mmistakes/minimal-mistakes)

소스코드 다운로드 후 내 사이트 폴더에 복사 후 사용

#### Gemfile 수정

{% highlight ruby %}
source "https://rubygems.org"

gem "github-pages", group: :jekyll_plugins
{% endhighlight %}

#### Jekyll 서버 실행

{% highlight console %}
bundle exec jekyll serve
{% endhighlight %}

### 커스터마이징

현재 수정한 사항
- 전체적인 폰트 시스템 변경
- TOC 레이아웃 조정 및 affix 적용
  - 나중에 spyscroll도 적용...
- 커스텀 스타일 추가
  - e.g. `.quote` 등

커스터마이징하면 느낀 점
- Jekyll 내장 서버의 자동 빌드는 매우 편하다!
  - 별도의 개발 도구 및 설정으로부터 자유롭다.
  - 템플릿과 스타일에만 집중
- 커스터마이징에 대한 시간 소비?
  - 조금씩 고치다보니 점점 많은 것을 고치고 있는 나를 발견

TOC 사용 시 링크의 위치(헤더)가 한글인 경우 브라우저 콘솔에 에러가 출력된다. 헤더의 텍스트가 id로 자동 설정되는데 한글로 인한 unrecognized expression 발생. (사용에는 지장 없음)
{: .notice--danger}
