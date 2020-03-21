---
layout: post
title:  "Github 블로그 만들기 (feat. Jekyll)"
date:   2020-03-21 16:34:00 +0900
categories: jekyll github update
permalink: /post/github/
---



# GitHub Blog 생성하기 
> Github blog를 생성하기 위한 단계 기록 


# 서론
Tistory에서 블로그를 가입한지 오래, 
꾸준히 써보고자 했지만 유지되지가 않았다. 

때문에 마크다운 파일로 글들을 편하게 관리하기 위해 
GitHub Pages Blog를 시작해보게 되었다. 

블로그를 만들면서 필요한 절차만 기록했기 때문에, 
터미널과 git에 익숙하지 않은 사용자에게는 적합하지 않을 수 있겠다. 

bundle을 통해 jekyll를 실행하게 되면 더 안정적으로 구동할 수 있다고 나와있는데, 
실행하면서 다소 시행착오가 생겨 일단 되는대로 한 번 해보았다. 

순서는 아래와 같다. 

* GitHub 가입 및 Repository 생성 
* Jekyll 설치 
* GitHub 원격 저장소를 내 PC로 복제 
* Jekyll로 사이트 생성 
* GitHub에 push



# GitHub 가입 및 Repository 생성 
GitHub 가입 시에 지정했던 username으로 github repository를 생성한다.  
Repository name : `username.github.io`  
ex) sloth-bear.github.io

생성 후 한 번 방문해보자.  
repository에는 아무것도 없으니 아마 빈 페이지가 뜰 것이다.  
http://{username}.github.io  

이와 관련해서는 생활코딩에서 잘 정리된 가이드가 있으니,  
공부하고 싶다면 참고하면 좋을 것 같다.  
  
<a href="https://opentutorials.org/course/3084/18891" target="_blank">웹호스팅 (github pages)</a>



# jekyll 설치
GitHub Pages site를 만들기 위해 `Jekyll`를 사용했다.  
설치부터 해보자.  

```
$ sudo install jekyll 
```

설명 참조: 
https://help.github.com/en/github/working-with-github-pages/creating-a-github-pages-site-with-jekyll



# github에 만든 원격 repository를 내 pc로 복제 
```
$ git clone https://github.com/sloth-bear/sloth-bear.github.io.git
```



# Create a new Jekyll site 
```
$ jekyll new .

Your user account isn't allowed to install to the system RubyGems.
  You can cancel this installation and run:

      bundle install --path vendor/bundle

  to install the gems into ./vendor/bundle/, or you can enter your password
  and install the bundled gems to RubyGems using sudo.
```



# Test to serve
```
$ jekyll serve
```

http://localhost:4000 으로 접속해보니, 잘 접속되었다. 



# git push 
```
$ git add .
$ git commit -m "init github pages"
$ git push -u origin master
```



# github pages 접속 테스트 
https://sloth-bear.github.io/ 

정상적으로 만들어진 것을 확인할 수 있다. 



# 글쓰기 
마크다운을 사용해 글을 작성했다. 파일명은 `how-to-create-github-blog.md` 이다.
현재 보여지고 있는 이 게시물이다. 
이 파일을 repository 내의 `_post` 폴더에 넣어두었다. 



# Git Push 
```
$ git add .
$ git commit -m "Create a new post"
$ git push 
```


# 마무리  
GitHub를 통해 어떻게 블로그를 만드는지 간단하게 정리해보았다.  
공식문서 및 여러 블로그를 참조해서 생성했고, 첫 포스팅도 해보았으니 차차 블로그 스타일을 다듬고 편의성을 갖춰나가야겠다.



