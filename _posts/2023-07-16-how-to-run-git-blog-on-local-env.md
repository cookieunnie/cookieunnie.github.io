---
layout: post
categories : BLOG
tags : Blog GitHub GitPages Jekyll Ruby
title:  "[Github 블로그] 내 로컬 환경에서 블로그 포스팅 미리 보기"
---

한번에 완벽하게 블로그 수정사항을 한 커밋에 깨끗하게 올리면 참 좋겠지만, 

꼭 다 올리고 나면 하나씩 수정할게 더 보이기도 하고, 미리 볼 수가 없다보니 불편함이 있었다. 

그래서 로컬에서 블로그 포스팅을 미리보면서 글을 쓸 수 있는 방법을 포스팅하려 한다. 



Jekyll, Ruby를 이용해 내 로컬에 블로그를 호스팅하는 방법이다. 

Mac m1기준, Git과 Homebrew가 설치되어있다는 전제로 설명할 예정이다. 

Git 사용법에 대해서도 함께 포스팅해야할까 고민해보았는데 너무 길어지기도 하고,

주제와 벗어나는 것 같아 아래에 설치 링크만 남겨두도록 하겠다. 

[>> Homebrew 설치하기](https://brew.sh/index_ko)

[>> Homebrew로 Git 설치하기](https://git-scm.com/download/mac) 



Jekyll은 Ruby언어로 개발된 정적 사이트 생성기로, 블로그 게시 시스템이라고 생각하면 이해가 쉬울 것이다. 

로컬에서 블로그를 생성하기 위해서 Jekyll이 필요하고, Jekyll을 실행시키기 위해 Ruby가 필요한 것이다.



## 1. Ruby 설치하기

그렇다면 Ruby를 먼저 설치해보자. 

Homebrew를 이용해서 아래의 명령어를 통해 Ruby를 설치한다.

```
$ brew install rbenv ruby-build
```



아래의 명령어로 설치 가능한 Ruby버전을 확인할 수 있는데, 

가장 최신버전보다는 1~2버전 아래의 Ruby가 보통 더 안정적이므로 

한두단계 아래의 버전을 설치하는 것을 추천한다. 

```
$ rbenv install -l
```



나는 3.1.4 버전을 설치했지만, 다른 버전을 설치하고 싶은 경우 바꾸어 명령하면 된다. 

```
$ rbenv install 3.1.4
```



내가 설치한 버전을 확인하기 위해 아래의 명령어를 입력하면 

아마 system, 내가 설치한 버전 두가지가 보일 것이다. 

```
$ rbenv versions
```



*이 system 옆에 보인다면, 설정을 다시 해주어야한다. 

내가 설치한 버전을 글로벌 버전으로 설정해준다. 

```
$ rbenv global 3.1.4 

$ rbenv rehash
```



### 2. Bundler 설치하기

Ruby 설치가 끝났다면, Bundler를 설치해준다. 

```
$ gem install bundler
```



### 3. Jekyll 설치하기

jekyll을 아래의 명령어로 설치해준다. 

```
$ gem install jekyll
```



내 블로그 git 파일 위치에서 아래의 명령어로 다시 bundler를 설치해주고, 

```
$ bundler install
```



 아래의 명령어로 jekyll을 실행시켜주면 localhost:4000환경에서 내 블로그를 확인할 수 있다. 

```
$ bundle exec jekyll serve
```

![1](/img/in-post/post-how-to-run-git-blog-on-local-env/1.png)

![2](/img/in-post/post-how-to-run-git-blog-on-local-env/2.png)



이렇게 변경사항을 로컬에서 미리 확인하면서 작업할 수 있기 때문에

더 깔끔하게 커밋을 할 수 있는 것이 큰 장점인 것 같다. 

그리고 대부분의 이런 정보는 템플릿 마다 필수적으로 요하는 것들이 있을 수 있으므로 

각 템플릿의 Readme.md 파일에 자세하게 명시되어있는 것들을 참고하는 것이 좋다. 