---
layout: post-detail
title: "만자로 리눅스에서 카카오톡 설치하기"
date: 2021-05-15 13:41:00 +0900
category: Blog
tags: linux arch-linux manjaro

---


# 만자로 리눅스 - 카카오톡 설치

## playonlinux 설치 
```
sudo pacman -Syu playonlinux
```

정상 설치 후 실행해준다. 


## 카카오톡 설치 


### playonlinux 실행 > 설치 > `install a non-listed program` 클릭
![스크린샷, 2021-05-15 00-10-46](https://user-images.githubusercontent.com/62458327/118348203-afd13a00-b583-11eb-83d7-fb0011ede71c.png)


### wine 6.0 Version으로 설정 (설치되어있지 않다면 설치 필요)
![스크린샷, 2021-05-15 13-47-03](https://user-images.githubusercontent.com/62458327/118348274-19514880-b584-11eb-8ef0-14a37c755883.png)


카카오톡 설치 시 자동으로 설치된다면 설치 후 해당 버전으로 바꾸어주어도 된다. 


현재 기준 6.0이 안정된 버전이라고 하여 해당 버전으로 변경하였다. 


![스크린샷, 2021-05-15 13-47-11](https://user-images.githubusercontent.com/62458327/118348275-1a827580-b584-11eb-9069-39567f142bd3.png)


### 카카오톡 설치
![스크린샷, 2021-05-15 13-21-59](https://user-images.githubusercontent.com/62458327/118348210-c8d9eb00-b583-11eb-9e64-98540d2135f4.png)


![스크린샷, 2021-05-15 13-22-38](https://user-images.githubusercontent.com/62458327/118348213-ca0b1800-b583-11eb-854e-aa908726c1ac.png)


![스크린샷, 2021-05-15 13-22-41](https://user-images.githubusercontent.com/62458327/118348215-cb3c4500-b583-11eb-88b5-1f87e117a3cc.png)


![스크린샷, 2021-05-15 13-23-00](https://user-images.githubusercontent.com/62458327/118348216-cbd4db80-b583-11eb-948a-da68f92b8d04.png)


![스크린샷, 2021-05-15 13-23-20](https://user-images.githubusercontent.com/62458327/118348221-d2fbe980-b583-11eb-89b5-d8c0c9f379c2.png)


![스크린샷, 2021-05-15 13-23-53](https://user-images.githubusercontent.com/62458327/118348225-d55e4380-b583-11eb-81fd-989dfe68c791.png)


![스크린샷, 2021-05-15 13-24-30](https://user-images.githubusercontent.com/62458327/118348226-d68f7080-b583-11eb-93e7-3394adee608a.png)


![스크린샷, 2021-05-15 13-24-33](https://user-images.githubusercontent.com/62458327/118348228-d7c09d80-b583-11eb-8e55-247d02651ddb.png)


![스크린샷, 2021-05-15 13-25-22](https://user-images.githubusercontent.com/62458327/118348230-d8f1ca80-b583-11eb-8b0f-4feb30d6c310.png)


카카오톡 공식 홈페이지에서 받은 설치파일로 설치를 시작한다. (캡쳐가 누락되었다.ㅠㅠ)


![스크린샷, 2021-05-15 13-25-35](https://user-images.githubusercontent.com/62458327/118348239-de4f1500-b583-11eb-9732-edff8dc1857b.png)


![스크린샷, 2021-05-15 13-25-55](https://user-images.githubusercontent.com/62458327/118348243-e14a0580-b583-11eb-97cf-ceb900bccfef.png)


카카오톡 실행은 미체크해야 바로가기를 등록할 수 있다. 마침 선택 후 바로가기까지 등록해준다. 


![스크린샷, 2021-05-15 13-27-34](https://user-images.githubusercontent.com/62458327/118348244-e27b3280-b583-11eb-8ed5-c031a869bb3e.png)


![스크린샷, 2021-05-15 13-27-48](https://user-images.githubusercontent.com/62458327/118348246-e313c900-b583-11eb-82d2-25cddd863b14.png)


## 로그인 시 에러 관련
### 50114 (방화벽 관련 에러)
```
$ sudo ufw enable
$ sudo ufw allow 22/tcp
$ sudo pacman -S openssh
```
위 설정 후에도 50114, 50151 등의 에러가 발생하였으나 지속적으로 재로그인하면 정상적으로 로그인이 가능하다.


아직까진 다소 불안정해보이지만, 카카오톡 정식 리눅스 버전이 나오기 전까지는 어쩔 수 없을 듯하다.


사용이 필요해 어쩔 수 없이 이렇게라도 설치하긴 했지만, 불편하다. 


심지어 다시 업데이트가 이루어지면 실행 안 되는 경우가 생길 수 있을 듯...^^;


# 자료 출처 
* playonlinux (https://www.playonlinux.com/en/download.html)
* wine (https://www.winehq.org/)
* 카카오톡 설치 (https://tolovefeels.tistory.com/65)
* 에러 관련
  * https://m.blog.naver.com/PostView.nhn?blogId=oj8mm&logNo=221746214224&proxyReferer=https:%2F%2Fwww.google.com%2F
  * https://hamonikr.org/Free_Board/86912
  * https://gist.github.com/BEMELON/70f173af95480389445b6f52c6ffada7
  * https://linuxhint.com/arch_linux_ssh_server/
  * http://no1linux.org/board_WEnl84/38956



