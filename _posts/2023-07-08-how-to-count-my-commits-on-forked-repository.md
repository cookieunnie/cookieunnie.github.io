---
layout: post
categories : BLOG
tags : Blog GitHub GitPages Jekyll Ruby
title:  "[Github 블로그] 블로그 포스팅으로 잔디 심는 법🌱"
---


블로그를 시작하려할 때, 티스토리나 벨로그가 아닌 Github로 시작한 이유 중 

가장 큰 이유는 바로 내 Github 프로필에 잔디 심기 때문이였다. 



잔디 심기란 Github 내 프로필에서 확인 가능한 contributions 그래프를 채우는 것을 말한다. 

아래 이미지처럼 내 커밋이 존재하는 일자에는 초록색으로 표시되기 때문에 잔디 심는다는 표현을 쓰는 것 같다. 

![](https://docs.github.com/assets/cb-35216/mw-1440/images/help/profile/contributions_graph.webp)



그런데 이전 포스팅에서 블로그를 만든 방식처럼 Fork해온 Repository에 Commit을 하는 경우는 잔디로 카운트해주지 않는다

아래 GitHub Docs에 이러한 문제에 대한 해결방법이 제시되어있으니 참고하면 좋을 것 같다.

[>> Github Docs - Why are my contributions not showing up on my profile?](https://docs.github.com/en/account-and-profile/setting-up-and-managing-your-github-profile/managing-contribution-settings-on-your-profile/why-are-my-contributions-not-showing-up-on-my-profile)



Github Docs에 명시되어있는 Commits를 살펴보면 (아래 이미지 참고)

아래의 조건들을 만족하는 Commit들만 Contributions Graph에서 잔디로 심어진다. 

1) 커밋할 때 사용된 이메일 주소가 Github 계정의 이메일 주소와 연관되어있는 경우
2) Fork해온 Repository가 아닌, 직접 생성한 Repository에서 생성된 Commit의 경우
3) Repository의 Default Branch에서 생성되었거나, 
   Project Sites가 있는 Repository의 `gh-pages` 브랜치에서 생성된 Commit의 경우 

![1](/img/in-post/post-how-to-show-my-contributions-of-posting/1.png)

바로 3번의 Project Sites가 있는 Repository가 템플릿 테마에서 Fork해온 Repository에 해당되므로 

우리는 `gh-pages` 브랜치를 이용해서 잔디를 심을 수 있다...! 



## 1. 브랜치 생성하기

`master` 브랜치에서 `gh-pages`라는 브랜치를 생성해준다. 

아래 이미지처럼 브랜치를 클릭하면, 

![2](/img/in-post/post-how-to-show-my-contributions-of-posting/2.png)



현재 Repository의 브랜치들을 확인할 수 있다. 

예제로 Fork한 Repository에는 추가로 생성한 브랜치가 없었기 때문에 Default인 `master`브랜치만 보이고 있다.

![3](/img/in-post/post-how-to-show-my-contributions-of-posting/3.png)



 `master`브랜치에서 새로 `gh-pages` 브랜치를 아래와 같이 생성해주고, 

해당 브랜치에서 작업을 진행하면 된다. 

![4](/img/in-post/post-how-to-show-my-contributions-of-posting/4.png)



## 2. Commit하고, Pull Request 생성하기 

`gh-pages` 브랜치에서 아래와 같이 포스트를 작성하고 (혹은 포스트 작성이 아니여도 파일 변경사항이 생겼을 경우) 커밋을 완료해준다. 

![5](/img/in-post/post-how-to-show-my-contributions-of-posting/5.png)



사실 이 모든 과정은 로컬에서 git 명령어를 이용해 진행하면 더 효율적이겠지만, 

여기까지는 비개발자도 따라할 수 있도록 하는 것이 목적이라 Github 홈페이지에서 진행하는 방법으로 설명해볼 생각이다. 

Pull requests 탭에서 생성할 수 있는데, 반드시 commit이 먼저 이루어져야 함을 주의하자.

![6](/img/in-post/post-how-to-show-my-contributions-of-posting/6.png)



`gh-pages` 브랜치의 커밋을 `master`로 Pull request하겠다는 뜻이다. 

이때, 내가 작업한 브랜치와 merge하고픈 브랜치를 반드시 잘 확인해야한다. 

특히 Fork한 Repository의 경우, 템플릿 repository의 `master`브랜치로 요청하지 않도록 주의해야한다. 

반드시 내 Repository의 master브랜치로 요청해야한다. 

![7](/img/in-post/post-how-to-show-my-contributions-of-posting/7.png)



요청한 Pull Request에서 'Merge Pull Request' 해주면 master 브랜치에 병합시킬 수 있고, 바로 변경사항이 적용된 것을 확인할 수 있다. 