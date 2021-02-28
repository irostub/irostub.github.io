---
title: github desktop에서 gpg key로 커밋 서명 방법
category: information
tags:
- gpg
- git
- github
- gpgsign
---

## .gitconfig를 고치자

gpg key를 생성해서 등록을 마쳤다. 이제 터미널이나 git bash 창에서 커밋에 서명을 할 수 있다. 하지만 github desktop에서 서명이 동작하지 않는 문제가 있었으니 이를 해결해보자.
<br>
## .gitconfig 파일 수정하기
### windows OS & mac OS
두 운영체제 모두 다 동일한 접근을 할 수 있다. 터미널을 열고 (윈도우의 경우 cmd) 다음 명령을 입력한다.
```
git config --global --edit
```
그러면 다음과 같이 .gitconfig 의 글로벌 설정이 보인다. vi편집기로 열릴텐데 vi 편집기를 다룰 줄 안다면 편집기에서 바로 다음 설정을 추가해준다. vi편집기를 다룰 줄 모른다면 위의 명령어를 쳤을 때 파일 위치가 같이 나오는데, 해당 경로로 찾아가서 textedit 등으로 열어서 추가해주면 되겠다.

{...} 으로 표기된 부분은 '{}' 를 제외하고 값을 넣어야한다.
```
[user]
  name = {닉네임}
  email = {깃허브 이메일}
  signingkey = {자신의 서명 키ID}
[gpg]
  program = {GPG바이너리 경로}
[commit]
  gpgsign = true
```
1. {닉네임} : 사용하고 싶은 닉네임
2. {깃허브 이메일} : 자신의 깃허브 계정 이메일
3. {자신의 서명 키ID} : gpg \-\-list\-keys 명령을 입력했을 때 나오는 키 ID
4. {GPG바이너리 경로}  
	* 윈도우의 경우 기본값 경로 C:\\\\Program Files\\\\Git\\\\usr\\\\bin\\\\gpg.exe
	* 맥의 경우 기본값 경로 /usr/local/bin/gpg

## 이제 github desktop에서 서명할 수 있다! YES!

**참고**
*xavierfoucrier -* [gpg-signing.md](https://gist.github.com/xavierfoucrier/c156027fcc6ae23bcee1204199f177da)
