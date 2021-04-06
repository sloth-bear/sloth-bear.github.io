---
layout: post-detail
title: "Github 블로그 만들기 1 - 블로그 생성 (feat. Jekyll)"
date: 2020-03-21 16:34:00 +0900
categories: blog
tags: jekyll github

---




# GitHub Blog 생성하기 

> Github blog를 생성하기 위한 단계 기록 

* __2020-03-22 추가__  
  GitHub Pages Blogs를 만들기 위해 Jekyll를 비롯해 여러 패키지들을 설치했다.  
  그러나 theme를 적용해보고 나니, Pages를 처음부터 하나하나 만들어보고 싶은 게 아니라면,  
  내가 원하는 theme의 repository에서 모든 폴더를 가져와 만드는 것이 가장 간편하다.  
  repository에서 모든 폴더를 가져와서 만드는 과정은  
  [Github 블로그 만들기 2 - theme 적용하기 (feat. minimal-mistakes)](https://sloth-bear.github.io/blog/how-to-create-github-blog/) 에서 참조할 수 있다.   

---




# 순서

* GitHub 가입 및 Repository 생성 
* Jekyll 설치 
* GitHub 원격 저장소를 내 PC로 복제 
* Jekyll로 사이트 생성 
* GitHub에 push




# GitHub 가입 및 Repository 생성 

GitHub 가입 시에 지정했던 username으로 github repository를 생성한다. 

Repository name : `username.github.io` 

ex) sloth-bear.github.io

http://{username}.github.io  



## 참조  

- <a href="https://opentutorials.org/course/3084/18891" target="_blank">웹호스팅 (github pages)</a>




# jekyll 설치

```text
    $ sudo install jekyll 
```

설명 참조: 
https://help.github.com/en/github/working-with-github-pages/creating-a-github-pages-site-with-jekyll




# github에 만든 원격 repository를 내 pc로 복제 

```text
    $ git clone https://github.com/sloth-bear/sloth-bear.github.io.git
```



# Create a new Jekyll site 

```text
    $ jekyll new .
    
    Your user account isn't allowed to install to the system RubyGems.
      You can cancel this installation and run:
    
          bundle install --path vendor/bundle
    
      to install the gems into ./vendor/bundle/, or you can enter your password
      and install the bundled gems to RubyGems using sudo.
```



# Test to serve

```text
    $ jekyll serve
```

- http://localhost:4000 으로 접속
- cf) `Welcome to Jekyll!` 글을 확인해보면 포스팅에 관해 작성되어있음



# git push 

```text
    $ git add .
    $ git commit -m "init github pages"
    $ git push -u origin master
```



# github pages 접속 테스트 

https://sloth-bear.github.io/ 



# 글쓰기 

- `_post`에 마크다운을 사용해 작성 
- 파일명: `2020-03-21-how-to-create-github-blog.md` 



# Git Push 

```text
    $ git add .
    $ git commit -m "Create a new post"
    $ git push 
```



