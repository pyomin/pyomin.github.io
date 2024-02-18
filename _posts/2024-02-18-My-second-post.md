---
layout: post
title: My second post
date: 2024-02-17 15:09:00
description: 로컬 오류를 해결해보자
tags: Testing Coding
categories: diary
---

이번 포스트는 언니 집에서 오류를 해결한 이슈에 대하여.  


맥북으로 처음 홈페이지를 만들때에는 루비?? 오오 이러면서 뚝딱뚝딱 만들었는데 (물론 오류는 많이 났었음)  
터미널을 닫았다가 다시 접속하려니까 로컬에서 실행이 잘 안되는 문제가 발생했다.  
저번에 연구실에서 페이지 접속하려다가 에러메세지 보고 나중에 해결해야겠네 하고 덮어놨었는데..   
학교도 정전이라 어차피 실험도 못돌리니,, 언니집에 노트북을 가지러 온 김에 흔들흔들 흔들의자에 앉아서 오류를 해결했다.  
아무래도 섹션2 쓰는것 보다는 흥미로웠던 것 같다.


```ruby
bundle exec jekyll serve

bundler: command not found: jekyll
Install missing gem executables with `bundle install`
```

갑자기 홈페이지를 실행하려니 이런 오류가 발생했다.   
분명히.. 며칠전에 다 설치했던 것인데..?   
페이지 만드는 과정에서 처음 시작할때 세팅했던 폴더를 옮긴 적이 있는데, 그래서 뭔가 환경 설정을 인식 못하는 건가..? 싶었다.  

첫번째로는 손상된 번들 파일이 있는지 확인해보았는데, 따로 파일이 없어서 루비 캐시 파일만 하나 삭제했다. 

그리고 
```
$ gem update --system
```
으로 업데이트 후에 손상된 번들을 업데이트 하려고 했는데 루비 버전 문제가 발생했다.

루비 버전은 rbenv로 관리를 해주는데, 분명히 저번에.. 버전을 설치하고 세팅을 해주었는데

```
zsh: command not found: rbenv
```

오류만 날 뿐이었다.  

확인해보니 설치된 적이 분명히 있는데..?? 설정에서 뭔가 반영이 안되었나..? 싶어서   
경로 잘 확인해서 zshrc 파일에 반영해 주었는데도 같은 오류가 발생했다.    

rbenv를 뭐로 설치했었는지를 확인해봤더니 homebrew로 설치했던 기록이 있어서 brew 명령어를 입력해봤다. 

```ruby
zsh: command not found: brew
```
command not found 였다. 그래서 여기서 부터 다시 세팅을 해주었다. 

```
$ eval $(/opt/homebrew/bin/brew shellenv)
```
를 입력해주면, 다시 brew를 입력했을때 정상적으로 인식되는 것을 확인 할 수 있다.


이제 rbenv도 정상적으로 명령어로 인식이 된다. 
다시

```
$ sudo gem update --system
```
해보면 아직 루비 버전 관련 에러가 발생한다.  

```
$ rbenv versions 
  system
  2.7.5
* 3.1.0 (set by /Users/pyomin/.rbenv/version)
```
인데, ruby --version 으로 확인해보면

```
ruby 2.6.8p205 (2021-07-07 revision 67951) [universal.arm64e-darwin21]
```
이다.

zshrc를 이렇게 수정해주었다.

```bash
[[ -d ~/.rbenv  ]] && \
  export PATH=${HOME}/.rbenv/bin:${PATH} && \
  eval "$(rbenv init -)"

```

명령어 적용은 
```
$ source ~/.zshrc
```

이제 루비 버전도 맞추어졌다. 

```
$ sudo gem update bundler
$ bundle install
```

정상적으로 로컬에서 홈페이지 실행이 가능하다 :blush:


+추가  


터미널을 열때마다 brew 명령어 인식이 안되는 것 같아서 매번 이거를 할 수는 없으니까
~/.zprofile을 만들어서 


```
$ echo 'eval $(/opt/homebrew/bin/brew shellenv)' >> /Users/pyomin/.zprofile
```

로 세팅해주었다.   


진짜 끝이예요.  :smile: