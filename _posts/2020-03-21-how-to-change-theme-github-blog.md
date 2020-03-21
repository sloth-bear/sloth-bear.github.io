---
layout: post
title:  "Github 블로그 만들기 2 - theme 적용하기 (feat. minimal-mistakes)"
date:   2020-03-21 20:34:00 +0900
categories: jekyll theme github update 
permalink: /post/github/
---


# Theme 선택 및 적용 
Theme 적용에는 방법이 있다. 
* 지원되는 테마 사용 
![](https://pages.github.com/themes/)
* 그 외 GitHub에 호스트된 Jekyll theme 사용 
* ![](http://jekyllthemes.org/)



## 지원되는 테마 사용 
GitHub Pages 사이트에서 제공되는 theme는 `_config.yml` 파일을 수정해 사용가능하다. 
```
# Build settings 
theme: minima 
```

`theme` 부분을 변경하면 된다. 




## GitHub에 호스팅된 Jekyll theme 사용 
* ![](http://jekyllthemes.org/)
위 사이트에서 theme 목록을 확인할 수 있다.  

각 theme별로 아래 기능이 존재한다.  
* `Homepage`: 해당 theme의 Github 
* `Demo`: 상세 화면을 확인
* `Download`: download


적용을 위해서는 `_config.yml`을 수정한다. 
```
# Build settings 
remote_theme: benbalter/retlab
```

`Homepage`로 접속해보면 각 theme별로 설명을 해주고 있으니,  
먼저 설명을 참고하고 적용해보는 것이 좋을 것 같다. 



# minimal-mistakes theme 적용하기 
`minimal-mistakes`는 사람들이 많이 적용하는 테마라는데,  
Demo를 확인해보니 깔끔하고 괜찮은 것 같아 이 테마로 적용해보고자 한다.  

이 테마는 두 가지 방법으로 적용할 수 있다. 
* Gem-based method 
* Remote theme method 

Gem-based로 적용해보겠다.  
나의 경우 vi를 사용해서 편집했는데, Github 페이지 내에서도 수정 가능하다.  
_git pull & push 작업이 계속해서 이루어지기 때문에,  
git에 대해 잘 모른다면 이 작업이 난해하게 느껴질 수도 있을 듯하다._

1. `Gemfile` 수정 
```
$ vi Gemfile

# gem "minima", "~> 2.5"
gem "minimal-mistakes-jekyll"
```
기존 적용되어있던 gem "minima", "~> 2.5"는 주석처리하고,  
minimal-mistakes-jekyll을 추가해주었다. 


2. Bundler 명령어 실행 - fetch & update bundled gems
```
$ bundle
```

3. `_config.yml` 파일 수정 
```
$ vi _config.yml

theme: minimal-mistakes-jekyll
```

4. theme update 
```
$ bundle update
```

그리고 `git push` 해준다. 


