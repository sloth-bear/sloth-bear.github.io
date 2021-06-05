---
layout: post-detail
title: "Github 블로그 만들기 2 - theme 적용하기 (feat. minimal-mistakes)"
date: 2020-03-21 20:00:00 +0900
categories: blog
tags: jekyll github
thumbnail: '/assets/thumbnail/2020-03-21-how-to-change-theme-github-blog.png'
---


# Theme 선택 및 적용 
Theme 적용에는 두 가지 방법이 있다. 
* 지원되는 테마 사용   
[Github themes](https://pages.github.com/themes/)
* 그 외 GitHub에 호스트된 Jekyll theme 사용   
[jekyllthemes](http://jekyllthemes.org/)

<p markdown="1" class="info">참고: [Adding a theme to your github pages site using jekyll](https://help.github.com/en/github/working-with-github-pages/adding-a-theme-to-your-github-pages-site-using-jekyll)</p>



## 지원되는 테마 사용 
GitHub Pages 사이트에서 제공되는 theme는 `_config.yml` 파일을 수정해 사용가능하다.
<div markdown="1" class="file-wrapper">
<p class="filename-badge">_config.yml</p> 
```yaml
    # Build settings 
    theme: minima 
```
</div>

`theme` 부분을 변경하면 된다. 




## GitHub에 호스팅된 Jekyll theme 사용 
* [jekyllthemes](http://jekyllthemes.org/)   

위 사이트에서 theme 목록을 확인할 수 있다.   

사이트에 접속해보면 각 theme별로 아래 기능이 존재한다.   
* `Homepage`: 해당 theme의 Github 
* `Demo`: 상세 화면을 확인
* `Download`: download


적용을 위해서는 `_config.yml`을 수정한다.
<div markdown="1" class="file-wrapper">
<p class="filename-badge">_config.yml</p>   
```yaml
    # Build settings 
    remote_theme: benbalter/retlab
```
</div>

`Homepage`로 접속해보면 각 theme별로 설명을 해주고 있으니,   
먼저 설명을 참고하고 적용해보는 것이 좋을 것 같다.   



# minimal-mistakes theme 적용하기 
`minimal-mistakes`는 사람들이 많이 적용하는 테마라는데,   
Demo를 확인해보니 깔끔하고 괜찮은 것 같아 이 테마로 적용해보고자 한다.   

이 테마는 여러 방식으로 설치할 수 있다.  
* Gem-based method   
* Remote theme method (GitHub Pages 호환)
* `minimal-mistakes-jekyll` repository download


_Remote theme method 방식으로 설치했을 때,  
내 글 목록에 sytle이 적용되지 않는 문제가 있었다.  
때문에 그냥 해당 Repository를 싹 다 가져와서 대체하는 방법으로 바꾸기로 했다.  
git에 대해 익숙하지 않다면 다소 난해하고, 추가적인 검색이 필요할 수 있겠다._  


아직 이 github 블로그에 대해 잘 모르기도 하고, setting할 것이 없기 때문에  
모든 repository를 복사해오기로 결정했다.  


1. `mmistakes/mm-github-pages-starter` download
	* Repository 내용 다운받기 ([mmistakes/mm-github-pages-starter](https://github.com/mmistakes/mm-github-pages-starter)) - `Donwload Zip` 혹은 `git clone`
	```text
	    $ git clone https://github.com/mmistakes/mm-github-pages-starter
	```
	가장 최소한으로 구성된 mmistakes repository를 가져왔다. 


2. Repository 복사하기 
	* 기존 내 Repository의 `_config.yml`, `posts` 백업
	* download 받은 파일 목록을 내 repository로 덮어씌우기 


3. 필요없는 파일 제거 
	* `_posts` 내의 모든 파일
	* README.md


4. `_posts`에 글 목록 추가
	작성된 글목록이 있어 posts 폴더를 백업해두었다면, `_posts` 폴더에 추가해준다. 


5. `_config.yml` 수정 
<div markdown="1" class="file-wrapper pl-2">
<p class="filename-badge">_config.yml</p>
```yaml
    # Site Settings 
    title                 : 배운 것을 남기는 공간 # {{ site.title }}
    email                 : slothbear.hj@gmail.com
    description           : 배운 것을 남기기 위한 공간입니다. 개발과 관련된 내용이 주된 내용입니다. 
    github_username       : sloth-bear
    minimal_mistakes_skin : default
    search                : true
    
    ...
    
    # Site Author
    author:
      name     : "느리지만 나아가는 개발자"
      avatar   : "/assets/images/bio-photo.jpg"
      bio      : "느리지만 나아가는 개발자입니다."
      location : "Seoul, Korea"
      links:
        - label: "Email"
          icon: "fas fa-fw fa-envelope-square"
          url: slothbear.hj@gmail.com
        - label: "GitHub"
          icon: "fab fa-fw fa-github"
          url: "https://github.com/sloth-bear"

    # Site Footer
    footer:
      links:
        - label: "GitHub"
          icon: "fab fa-fw fa-github"
          url: "https://github.com/sloth-bear"
```
    
모르는 설정들은 과감히 패스하고, 필요해보이는 것들만 수정해두었다. 
</div>

6. `git push`
	```text
        $ git add .
        $ git commit -m "Init github pages"
        $ git push
	```
	

테마가 잘 설치되었음을 확인할 수 있었다. 

