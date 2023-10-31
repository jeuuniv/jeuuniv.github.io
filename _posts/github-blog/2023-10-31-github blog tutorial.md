---
title: 윈도우에서 Jekyll Chirpy 테마로 깃허브 블로그 만들기 (윈도우 환경 + WSL 사용) (2023.10.31 기준)
date: 2023-10-31 15:35:00 +0900
categories: [깃허브 블로그]
tags: [git, github, jekyll, chirpy, jekyll chirpy, bundler, node.js, blog, tutorial, wsl, wsl2, windows,]     # TAG names should always be lowercase
---

## 소개

이 글은 제가 윈도우 환경에서 WSL을 이용해 어떻게 Jekyll Chirpy 테마로 깃허브 블로그를 만들었는지 그 과정을 잊어버리지 않게 기록하려는 목적으로 작성되었습니다.

제가 아직 초보라서 과정 중에 설명이 부족하거나 틀린 부분이 있을 수도 있으니 해당 부분에 대해서 지적해주시면 적극적으로 수용하도록 하겠습니다!

제가 생각하기에 Jekyll Chripy는 리눅스나 맥 환경에 친화적이지만 윈도우에선 그렇지 않은 것 같습니다.

하지만 저는 윈도우 환경에서 진행하기를 원해서 Windows Subsystem for Linux (줄여서 WSL)를 사용해 따로 cmd를 쓰지 않고 WSL에서 제공하는 셸로 진행해보겠습니다.

## 기본 환경설정
### WSL 설치
먼저 WSL에 대해 간단하게 설명하면 윈도우 컴퓨터에서 윈도우와 리눅스의 기능을 사용할 수 있게 해주는 기능을 제공합니다. 

굳이 가상머신이나 듀얼부팅을 통해 작업하지 않아도 되서 매우 간편합니다.

설치하는 방법은 매우 간단합니다. cmd나 powershell을 열어 다음 명령어를 입력하면 됩니다.

> 아래 명령으로 WSL을 설치하려면 Windows 10 버전 2004 이상(빌드 19041 이상) 또는 Windows 11 이어야 합니다.
{: .prompt-warning }

```
wsl --install
```

설치 한 후 재부팅이 필요한데, 재부팅을 하게 되면, 다시 WSL창이 뜨면서 다음과 같은 출력이 보일겁니다.

```
Installing, this may take a few minutes...
Please create a default UNIX user account. The username does not need to match your Windows username.
For more information visit: https://aka.ms/wslusers
Enter new UNIX username: 
```

여기서 유닉스 유저네임을 입력하지 않고 그냥 창을 닫고 다시 띄워줘도 됩니다.

### WSL 초기설정
WSL 내부는 기본적으로 우분투 환경이므로 우분투 초기설정을 위해 다음 명령어를 입력해서 apt 패키지 업데이트와 업그레이드를 진행해줍니다.

```bash
sudo apt update
sudo apt upgrade
```

### Ruby 및 기타 필수 구성 요소 설치
Jekyll을 설치하기 위해서는 Ruby와 다른 필수 요소들을 설치해야 합니다. 

다음 명령어를 입력해서 설치해줍니다.

```bash
sudo apt install ruby-full build-essential zlib1g-dev
```

### gem 설치 경로 지정
루트 사용자로 RubyGems 패키지(줄여서 gem)를 설치하지 말고, 대신에 사용자 계정에 대한 gem 설치 경로를 설정하라고 공식 문서에 나와있습니다. 

다음 명령을 입력해서 ~/bashrc 파일에 환경 변수를 추가하여 gem 설치 경로를 지정해줍니다.

```bash
echo '# Install Ruby Gems to ~/gems' >> ~/.bashrc
echo 'export GEM_HOME="$HOME/gems"' >> ~/.bashrc
echo 'export PATH="$HOME/gems/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

### Jekyll, Bundler 설치
이제 다음 명령어를 입력해서 Jekyll과 Bundler를 설치해 줍니다.

```bash
gem install jekyll bundler
```

### Node.js 설치
블로그를 만드는 과정에서 자바스크립트 파일을 빌드하는 과정이 포함되어있기 때문에 Node.js가 필요합니다.

다음 명령어를 통해서 최신 버전의 Node.js를 설치합니다.

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
source ~/.bashrc
nvm install node
```

### 깃허브 personal access token 발급
wsl 셸을 통해서 나중에 git push를 하기 위해서는 토큰이 필요합니다.

[***링크***](https://github.com/settings/tokens/new)를 클릭해서 깃허브 토큰을 만드는 창을 띄워줍니다.

여기서 토큰 만료 기간(Expiration)을 자유롭게 설정하고 Select scopes에서 repo와 workflow를 체크해줍니다.

> workflow를 체크하면 자동으로 repo도 체크됩니다.
{: .prompt-tip }

그리고 맨 아래 Generate token을 누르게 되면 토큰이 발급됩니다. 

이제 토큰을 복사해서 다른곳에 메모해서 보관하시면 됩니다.

## 깃허브 포크 및 초기화
### 포크 만들기
[***링크***](https://github.com/cotes2020/jekyll-theme-chirpy/fork)를 클릭해서 Chripy 포크를 할때 여기서 레포지토리 이름을 `(깃허브id).github.io` 로 하고 Create fork를 클릭하면 됩니다.

### git clone
저같은 경우는 로컬 디스크 (C:)에 github_blog란 폴더를 만들었습니다. 

만든 폴더에 들어간 다음 쉬프트+오른쪽 클릭을 하게 되면 여기에서 Linux(L) 셸 열기가 보이는데 그걸 눌러줍니다.

WSL 셸 창에서 다음 명령어를 입력해서 git 레포지토리를 클론하고 cd 명령어를 통해 클론한 폴더로 경로를 변경합니다.

```bash
git clone https://github.com/(깃허브id)/(깃허브id).github.io.git
cd (깃허브id).github.io/
```

### tools/init 실행
원래라면 `bash tools/init` 만 하면 실행되지만, WSL2 환경에서는 앞에 추가적인 명령어들를 입력해야 나중에 오류가 안생기고 정상적으로 진행할 수 있습니다.

다음의 명령어를 입력합니다.

```bash
git config --global core.autocrlf true input
sed -i 's/\r//' tools/init
git add tools/init
bash tools/init
```

여담으로, 공식 문서를 참고해보면 `tools/init`은 다음 과정을 수행한다고 합니다.

1. 사이트의 안정성을 위해 가장 최신 release로 브랜치를 전환(git checkout)합니다.
2. 필수적이지 않은 샘플 파일을 제거하고 GitHub 관련 파일을 관리합니다.
3. JavaScript 파일을 빌드하고 assets/js/dist/로 내보낸 다음 Git에서 추적할 수 있도록 합니다.
4. 자동으로 새 커밋을 생성하여 위의 변경 사항을 저장합니다.

## 로컬서버 실행
### 의존성 패키지 설치
나중에 로컬에서 홈페이지 테스트를 하기 위한 서버를 실행하려면 다음 명령어를 입력해서 의존성 패키지들을 설치해야 합니다.

```bash
bundle
```

### _config.yml 설정
`(깃허브id).github.io` 폴더에서 _config.yml 파일을 찾아 열어줍니다. (Notepad++나 Visual Studio Code같은 프로그램으로 열어주시면 됩니다.)

먼저, url 부분을 `url: "https://(깃허브id).github.io"`로 바꿔줍니다.
> 주의할 점은 주소부분이 "https://(깃허브id).github.io/" 처럼 '/'로 끝나면 안됩니다.
{: .prompt-warning }

그 다음에 lang, timezone, title, tagline을 저는 다음과 같이 바꿔줬습니다.

1. `lang: ko-KR`
2. `timezone: Asia/Seoul`
3. `title: 파덕의 블로그`
4. `tagline: 테스트 중 입니다.`

그 이외의 설정들은 나중에 따로 글을 올리게 되면 추가로 링크를 달도록 하고 이정도만 설정하고 넘어가겠습니다.

### 로컬서버 실행
여기까지 오시면 다음 명령어를 입력해서 로컬서버에서 홈페이지를 띄울 수 있습니다.

```bash
bundle exec jekyll s
```

명령어를 입력하고 조금 기다리시면 다음과 같은 출력이 보이고 

```bash
Configuration file: /mnt/c/github_blog/jeuuniv.github.io/_config.yml
 Theme Config file: /mnt/c/github_blog/jeuuniv.github.io/_config.yml
            Source: /mnt/c/github_blog/jeuuniv.github.io
       Destination: /mnt/c/github_blog/jeuuniv.github.io/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
                    done in 15.525 seconds.
 Auto-regeneration: enabled for '/mnt/c/github_blog/jeuuniv.github.io'
    Server address: http://127.0.0.1:4000/
  Server running... press ctrl-c to stop.
```

<http://127.0.0.1:4000/> 로 접속하게 되면 홈페이지가 뜨게 됩니다!

## 깃허브 블로그 배포
### Github Actions 설정
먼저 깃허브 홈페이지에 들어가 본인이 포크한 레포지토리로 들어갑니다.
상단에 있는 Setting 클릭 후 Code and automation 탭의 Pages를 클릭합니다.
그 다음 Build and deployment에서 Source를 Deploy from branch에서 GitHub Action로 바꾸면 끝입니다. (configure 눌러 설정할 필요 없습니다.)

### git 커밋 후 push
지금까지 변경 내용을 이제 커밋하고 git push하면 GitHub Actions가 자동으로 블로그를 빌드하고 배포하게 됩니다.

하지만 이 상태에서 커밋하고 `git push`하려고 하면 아까 tools/init 때 git checkout을 한 상태에서 커밋해 브랜치가 나눠져서 충돌이 일어나 push가 안되고 오류가 발생합니다.

저는 이를 해결하기 위해 그냥 `git push` 대신 `git push --force`로 해결했습니다.

그리고 커밋할 때 git username과 email을 설정하지 않으면 커밋한 사람이 root로만 뜨기 때문에 다음 명령어를 입력해 커밋할 때 사용할 이름과 이메일을 입력했습니다.
```bash
git config --global user.name "Your Name"
git config --global user.email you@example.com
```
최종적으로 다음 명령어를 입력하면 됩니다.


> git push를 할때 올바른 본인 id와 token값을 입력해야 진행이 됩니다.
{: .prompt-warning }

```bash
git add .
git commit -m "first commit"
git push --force
```

그 다음 깃허브 홈페이지 본인 레포지토리에서 Actions를 클릭해보면 빌드와 배포를 진행하는 내용을 볼 수 있습니다. 

배포가 정상적으로 되면 `https://(깃허브id).github.io/` 에 접속하면 홈페이지가 보이게 됩니다!

## 글을 마치면서
다음 글에서는 블로그에 글을 게시하는 방법에 대한 내용을 가지고 찾아봽도록 하겠습니다.

긴글 읽어주시셔서 감사합니다.