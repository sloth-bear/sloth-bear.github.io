---
layout: post
title:  "Github 블로그 만들기 2 - theme 적용하기 (feat. minimal-mistakes)"
date:   2020-03-21 20:00:00 +0900
categories: blog
tags: jekyll github
---


# Theme 선택 및 적용 
Theme 적용에는 두 가지 방법이 있다. 
* 지원되는 테마 사용   
![Github themes](https://pages.github.com/themes/)
* 그 외 GitHub에 호스트된 Jekyll theme 사용   
![jekyllthemes](http://jekyllthemes.org/)

참고: ![Adding a theme to your github pages site using jekyll](https://help.github.com/en/github/working-with-github-pages/adding-a-theme-to-your-github-pages-site-using-jekyll)


## 지원되는 테마 사용 
GitHub Pages 사이트에서 제공되는 theme는 `_config.yml` 파일을 수정해 사용가능하다. 
```
# Build settings 
theme: minima 
```

`theme` 부분을 변경하면 된다. 




## GitHub에 호스팅된 Jekyll theme 사용 
* ![jekyllthemes](http://jekyllthemes.org/)   

위 사이트에서 theme 목록을 확인할 수 있다.   

사이트에 접속해보면 각 theme별로 아래 기능이 존재한다.   
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


GitHub Pages와의 호환을 위해서는 Remote theme method 방식으로 적용해야 한다. 
나의 경우 vi를 사용해서 편집했는데, Github 페이지 내에서도 수정 가능하다.   

_git pull & push 작업이 계속해서 이루어지기 때문에,   
git에 대해 잘 모른다면 이 작업이 난해하게 느껴질 수도 있을 듯하다._   


1. `Gemfile` 수정 
	```
	$ vi Gemfile 

	#gem "jekyll", "~> 4.0.0"
	gem "jekyll", "~> 3.8.5"

	...

	gem "github-pages", group: :jekyll_plugins

	...

	group :jekyll_plugins do
	  gem "jekyll-feed"
	  gem "jekyll-include-cache"
	```


	3.8.5 버전으로 변경해주는 이유는 4.0.0에서 github-pages가 지원되지 않기 때문이다. 

	![GitHub Pages - Dependency versions](https://pages.github.com/versions/)


2. `_config.yml` 파일 수정 - `jekyll-include-cache` plugin 설정 
	```
	$ vi _config.yml 

	plugins: 
	  - jekyll-feed
	  - jekyll-include-cache
	```



3. Fetch and update bundled gems 
	```
	$ bundle 
	```


4. `_config.yml` 파일 수정 - theme 설정
	```
	# remote_theme
	remote_theme: "mmistakes/minimal-mistakes@4.19.1"
	```





