---
title: 윈도우에서 Jekyll Chirpy 테마로 깃허브 블로그 만들기 (윈도우 환경 + WSL 사용)
date: 2023-10-29 00:00:00 +0900
categories: [깃허브 블로그, 만들기]
tags: [git, github, jekyll, chirpy, blog, tutorial, wsl, wsl2, windows, ]     # TAG names should always be lowercase
---

## 소개

이 글은 제가 윈도우 환경에서 어떻게 Jekyll Chirpy 테마로 깃허브 블로그를 만들었는지 그 과정을 까먹지 않게 기록하려는 목적으로 작성되었습니다.

제가 아직 초보자라서 과정 중에 틀린 부분이 있을 수도 있으니 이 부분에 대해서 지적해주시면 감사히 받아들이겠습니다!

제가 생각하기에 Jekyll Chripy는 리눅스나 맥 환경에 친화적이지만 윈도우에선 그렇지 않은 것 같습니다.

하지만 저는 윈도우 환경에서 진행하기를 원해서 Windows Subsystem for Linux (줄여서 WSL)를 사용해 따로 cmd를 쓰지 않고 WSL에서 제공하는 셸로 진행해보겠습니다.

## 기본 환경설정
### 1. WSL 설치

먼저 WSL에 대해 간단하게 설명하면 윈도우 컴퓨터에서 윈도우와 리눅스의 기능을 사용할 수 있게 해주는 기능을 제공합니다. 

굳이 가상머신이나 듀얼부팅을 통해 작업하지 않아도 되서 매우 간편합니다.

설치하는 방법은 매우 간단합니다.
cmd나 powershell을 열어 다음 명령어를 입력하면 됩니다.

> 아래 명령으로 WSL을 설치하려면 Windows 10 버전 2004 이상(빌드 19041 이상) 또는 Windows 11 이어야 합니다.
{: .prompt-danger }

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

### 2. WSL 초기설정
WSL 내부는 기본적으로 우분투 환경이므로 우분투 초기설정을 위해 다음 명령어를 입력해서 apt 패키지 업데이트와 업그레이드를 진행해줍니다.

```bash
sudo apt update
sudo apt upgrade
```

### 3. Ruby 및 기타 필수 구성 요소 설치
Jekyll을 설치하기 위해서는 Ruby와 다른 필수 요소들을 설치해야 합니다. 

다음 명령어를 입력해서 설치해줍니다.

```bash
sudo apt install ruby-full build-essential zlib1g-dev
```

### 4. gem 설치 경로 지정
루트 사용자로 RubyGems 패키지(줄여서 gem)를 설치하지 말고, 대신에 사용자 계정에 대한 gem 설치 경로를 설정하라고 공식 문서에 나와있습니다. 

다음 명령을 입력해서 ~/bashrc 파일에 환경 변수를 추가하여 gem 설치 경로를 지정해줍니다.

```bash
echo '# Install Ruby Gems to ~/gems' >> ~/.bashrc
echo 'export GEM_HOME="$HOME/gems"' >> ~/.bashrc
echo 'export PATH="$HOME/gems/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

### 5. Jekyll, Bundler 설치
이제 다음 명령어를 입력해서 Jekyll과 Bundler를 설치해 줍니다.

```bash
gem install jekyll bundler
```

### 6. Node.js 설치
블로그를 만드는 과정에서 자바스크립트 파일을 빌드하는 과정이 포함되어있기 때문에 Node.js가 필요합니다.

다음 명령어를 통해서 최신 버전의 Node.js를 설치합니다.

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
source ~/.bashrc
nvm install node
```

## 깃허브 포크
[***Chirpy 포크하기***](https://github.com/cotes2020/jekyll-theme-chirpy/fork) 링크를 클릭해서 