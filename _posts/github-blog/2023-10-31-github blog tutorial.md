---
title: 깃허브 블로그 제작 - Jekyll Chirpy 테마 (윈도우 + WSL) (2023.10 기준)
date: 2023-10-31 15:35:00 +0900
categories: [깃허브 블로그]
tags: [github, blog, github blog, jekyll, chirpy, jekyll chirpy, tutorial, 깃허브, 블로그, 깃허브 블로그, 튜토리얼, windows, wsl]     # TAG names should always be lowercase
pin: true
image:
    path: 'https://github.com/jeuuniv/jeuuniv.github.io/assets/149172579/3c664a25-b692-405d-9f17-7fd3e2b41291'
---

이 글은 제가 윈도우 환경에서 Windows Subsystem for Linux (WSL)을 활용하여 어떻게 Jekyll Chirpy 테마를 사용하여 깃허브 블로그를 만들었는지의 과정을 기록하기 위해 작성되었습니다. 

제가 아직 초보자이기 때문에 설명이 부족하거나 잘못된 부분이 있을 수 있으니 의견과 지적을 주시면 적극적으로 수용하도록 하겠습니다!

여기서 간단히 설명하자면, Jekyll은 사이트 생성기, Chirpy는 블로그 테마라고 이해하시면 도움이 될 것 같습니다.

Jekyll Chirpy는 주로 리눅스와 맥 환경에 최적화되어 있지만, 이 글에서는 윈도우 환경에서도 사용하는 방법을 소개합니다. 이를 위해 WSL(Windows Subsystem for Linux)을 활용하여, cmd 대신 WSL에서 제공하는 셸을 사용해 진행하겠습니다.

## 기본 환경설정
### WSL 설치
먼저, WSL(Windows Subsystem for Linux)은 윈도우 컴퓨터에서 윈도우와 리눅스의 기능을 함께 사용할 수 있게 해주는 기능을 제공합니다. 이를 통해 가상머신을 사용하거나 듀얼 부팅 설정을 할 필요가 없어 매우 간단하게 리눅스 환경을 활용할 수 있습니다.

WSL을 설치하는 방법도 간단합니다. cmd나 PowerShell을 열고 다음 명령어를 입력하면 됩니다.

> WSL을 설치하려면 Windows 10 버전 2004 이상 (빌드 19041 이상) 또는 Windows 11 운영 체제가 필요합니다.
{: .prompt-warning }

```
wsl --install
```

설치 후 컴퓨터를 재부팅해야 합니다. 다시 부팅한 후, WSL 창이 열리며 다음과 같은 출력을 볼 수 있습니다.

```
Installing, this may take a few minutes...
Please create a default UNIX user account. The username does not need to match your Windows username.
For more information visit: https://aka.ms/wslusers
Enter new UNIX username: 
```

여기서 유닉스 사용자 이름을 입력하지 않고 그냥 창을 닫고 다시 열어도 됩니다.

### WSL 초기설정
WSL 내부는 기본적으로 우분투 환경입니다. 따라서 우분투 초기 설정을 위해 다음 명령어를 입력하여 apt 패키지를 업데이트하고 업그레이드해야 합니다.

```bash
sudo apt update
sudo apt upgrade
```

### Ruby 및 기타 필수 구성 요소 설치
Jekyll을 설치하려면 Ruby와 기타 필수 요소를 설치해야 합니다. 이를 위해 다음 명령어를 입력하여 설치를 진행합니다.

```bash
sudo apt install ruby-full build-essential zlib1g-dev
```

### gem 설치 경로 지정
공식 문서에 따르면, 루트 사용자로 RubyGems 패키지(gem)를 설치하는 대신 사용자 계정에 대한 gem 설치 경로를 설정해야 합니다. 이를 위해 다음 명령을 입력하여 환경 변수를 ~/bashrc 파일에 추가하여 gem 설치 경로를 지정합니다.

```bash
echo '# Install Ruby Gems to ~/gems' >> ~/.bashrc
echo 'export GEM_HOME="$HOME/gems"' >> ~/.bashrc
echo 'export PATH="$HOME/gems/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

### Jekyll, Bundler 설치
이제 아래 명령어를 입력하여 Jekyll과 Bundler를 설치합니다.

```bash
gem install jekyll bundler
```

### Node.js 설치
블로그를 만드는 과정에는 자바스크립트 파일을 빌드하는 과정이 포함되므로 Node.js가 필요합니다. 다음 명령어를 사용하여 최신 버전의 Node.js를 설치합니다.

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
source ~/.bashrc
nvm install node
```

### 깃허브 personal access token 발급
나중에 WSL 셸을 통해 Git push를 수행하려면 GitHub 토큰이 필요합니다. [***링크***](https://github.com/settings/tokens/new){:target="_blank"}를 클릭하여 GitHub 토큰 생성 페이지를 엽니다.

![light mode only](https://github.com/jeuuniv/jeuuniv.github.io/assets/149172579/8b4495e3-9c6c-4f5e-a315-6528ef4d1b0b){: .light .w-90 .shadow .rounded-10 }
![dark mode only](https://github.com/jeuuniv/jeuuniv.github.io/assets/149172579/e51bff64-dc7f-4dd9-89a8-cda0523762df){: .dark .w-90 .shadow .rounded-10 }

토큰을 생성할 때, 만료 기간(Expiration)을 자유롭게 설정하고 Select scopes에서 'repo'와 'workflow'를 선택해주세요.

> workflow를 체크하면 자동으로 repo도 체크됩니다.
{: .prompt-tip }

맨 아래 'Generate token'을 클릭하면 토큰이 생성됩니다. 이제 토큰을 메모장에 복사하여 보관하시면 됩니다.

## 깃허브 포크 및 초기화
### 포크 만들기
![light mode only](https://github.com/jeuuniv/jeuuniv.github.io/assets/149172579/4600f90e-f9e0-4dd1-b3b3-86524884195d){: .light .w-90 .shadow .rounded-10 }
![dark mode only](https://github.com/jeuuniv/jeuuniv.github.io/assets/149172579/c1add358-050d-4450-91c0-1ff203854c47){: .dark .w-90 .shadow .rounded-10 }

[***링크***](https://github.com/cotes2020/jekyll-theme-chirpy/fork){:target="_blank"}를 클릭하여 Chirpy 리포지토리를 포크할 때, 레포지토리 이름을 `(깃허브id).github.io`로 설정하고 'Create fork'를 클릭하세요.


### git clone
1. 저 같은 경우는 로컬 디스크 (C:)에 `github_blog`라는 폴더를 만들었습니다.

2. 이 폴더로 이동한 후, 폴더 내에서 Shift + 오른쪽 클릭을 하면 `여기에서 Linux(L) 셸 열기` 옵션이 표시됩니다. 이 옵션을 선택하세요.

3. WSL 셸 창이 열리면 다음 명령어를 입력하여 Git 리포지토리를 복제하고, cd 명령어를 사용하여 복제한 폴더로 이동하세요.

```bash
git clone https://github.com/(깃허브id)/(깃허브id).github.io.git
cd (깃허브id).github.io/
```

### tools/init 실행
일반적으로 `bash tools/init` 명령만으로 실행되지만, WSL2 환경에서는 나중에 오류가 발생하지 않고 정상적으로 진행하기 위해 몇 가지 추가 명령을 입력해야 합니다.

다음의 명령어를 입력합니다.

```bash
git config --global core.autocrlf true input
sed -i 's/\r//' tools/init
git add tools/init
bash tools/init
```

정상적으로 진행됐으면 `[INFO] Initialization successful!` 메시지가 출력됩니다.

## 로컬서버 실행
### 의존성 패키지 설치
로컬에서 홈페이지 테스트를 위한 서버를 나중에 실행하려면 다음 명령어를 사용하여 필수 의존성 패키지를 설치해야 합니다.

```bash
bundle
```

### _config.yml 설정
`(깃허브id).github.io` 폴더에서 `_config.yml` 파일을 찾아 열어줍니다. (Notepad++나 Visual Studio Code같은 프로그램으로 열어주시면 됩니다.)

먼저, url 부분을 `url: "https://(깃허브id).github.io"`로 바꿔줍니다.
> 주의할 점은 주소부분이 "https://(깃허브id).github.io/" 처럼 '/'로 끝나면 안됩니다.
{: .prompt-warning }

그 다음에 lang, timezone, title, tagline을 저는 다음과 같이 바꿔줬습니다.

1. `lang: ko-KR`
2. `timezone: Asia/Seoul`
3. `title: 파덕의 블로그`
4. `tagline: 테스트 중 입니다.`

나머지 설정은 나중에 별도의 글을 게시할 때 추가로 다루겠으며, 현재는 이 정도로 설정을 마무리하도록 하겠습니다

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
![light mode only](https://github.com/jeuuniv/jeuuniv.github.io/assets/149172579/2724be03-3a0c-4bd8-8920-12f3384e79e0){: .light .w-90 .shadow .rounded-10 }
![dark mode only](https://github.com/jeuuniv/jeuuniv.github.io/assets/149172579/bf9be0aa-e1ca-48da-9c8a-215f83e8dad6){: .dark .w-90 .shadow .rounded-10 }

<http://127.0.0.1:4000/>{:target="_blank"} 로 접속하게 되면 홈페이지가 뜨게 됩니다!

## 깃허브 블로그 배포
### Github Actions 설정


![light mode only](https://github.com/jeuuniv/jeuuniv.github.io/assets/149172579/94b68029-e77e-4c7f-87be-8bc5099da603){: .light .w-90 .shadow .rounded-10 }
![dark mode only](https://github.com/jeuuniv/jeuuniv.github.io/assets/149172579/0ec5e5e5-fd33-49d9-91b1-7ce7ca75520d){: .dark .w-90 .shadow .rounded-10 }
먼저 깃허브 홈페이지에 들어가 본인이 포크한 레포지토리로 들어갑니다.
상단에 있는 Setting 클릭 후 Code and automation 탭의 Pages를 클릭합니다.
그 다음 Build and deployment에서 Source를 Deploy from branch에서 `GitHub Actions`로 바꾸면 끝입니다. (configure 눌러 설정할 필요 없습니다.)

### git 커밋 후 push
지금까지의 변경 사항을 커밋하고 `git push`를 실행하면 GitHub Actions가 자동으로 블로그를 빌드하고 배포합니다

그러나, 그냥 `git push`를 시도하면 `tools/init`에서 git checkout을 수행한 상태에서 커밋하여 브랜치가 분기된 상태이기 때문에 충돌이 발생하여 진행할 수 없습니다.

그래서 저는 이 문제를 해결하기 위해 `git push` 대신 `git push --force`를 사용하여 해결했습니다.

또한, 커밋할 때 git 사용자 이름과 이메일을 설정하지 않으면, 커밋한 사람이 root로만 표시되므로, 다음 명령어를 사용하여 커밋할 때 사용할 이름과 이메일을 설정했습니다.

```bash
git config --global user.name "Your Name"
git config --global user.email you@example.com
```

최종적으로 다음 명령어를 입력하면 됩니다.

> `git push`를 할때 본인 id와 token값을 물어보니 올바른 값을 입력하시면 됩니다.
{: .prompt-warning }

```bash
git add .
git commit -m "first commit"
git push --force
```

![light mode only](https://github.com/jeuuniv/jeuuniv.github.io/assets/149172579/c47396f3-ece9-4654-9a06-17c5b520c350){: .light .w-90 .shadow .rounded-10 }
![dark mode only](https://github.com/jeuuniv/jeuuniv.github.io/assets/149172579/01f1e64f-3a9d-4212-a70e-e01c34387afa){: .dark .w-90 .shadow .rounded-10 }

그 다음 깃허브 홈페이지 본인 레포지토리에서 Actions를 클릭해보면 빌드와 배포를 진행하는 것을 볼 수 있습니다. 

배포가 정상적으로 된 후 `https://(깃허브id).github.io/` 에 접속하면 홈페이지가 보이게 됩니다!

## 글을 마치면서
다음 글에서는 블로그에 글을 게시하는 방법에 대한 내용을 가지고 찾아뵙도록 하겠습니다.

처음 게시하는 글이라 미흡할 수 있지만 긴 글을 읽어주셔서 감사합니다!