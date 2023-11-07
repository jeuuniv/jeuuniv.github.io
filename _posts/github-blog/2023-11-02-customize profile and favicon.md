---
title: 깃허브 블로그 - 프로필 사진, 파비콘 커스터마이징 (Jekyll Chirpy 기준)
date: 2023-11-02 10:26:00 +0900
categories: [깃허브 블로그]
tags: [github, blog, github blog, jekyll, chirpy, jekyll chirpy, tutorial, 깃허브, 블로그, 깃허브 블로그, 튜토리얼, profile, favicon]     # TAG names should always be lowercase
---

> 블로그를 만들 때 초기화 과정을 거쳤다면(`bash tools/init`) 아마 왼쪽 상단에 샘플 프로필 사진도 없이 그냥 텅 빈 원만 있을 것입니다.\
이번 글에서는 블로그 왼쪽 상단에 있는 프로필 사진과 파비콘(주소창에 표시되는 이미지)를 바꾸는 법에 대해 알려드리겠습니다.

## 프로필 사진 추가하기
### 프로필 사진 이미지 준비
일단 프로필 사진으로 쓰고 싶은 이미지 하나를 준비합니다.
![avatar.jpg](https://github.com/jeuuniv/jeuuniv.github.io/assets/149172579/aef0576c-d7c2-434d-871c-565b1468ca7b){: .w-50}
_저는 이 이미지를 준비했습니다_

저는 프로필 사진 파일 이름을 `avatar.jpg`라 했고, 이제 이 파일을 레포지토리 `assets/img` 폴더에 넣어줬습니다.

### _config.yml 수정
`_config.yml` 파일을 열어서 조금 아래로 내려보면 다음과 같은 항목이 있을 겁니다.

```yml
# the avatar on sidebar, support local or CORS resources
avatar: 
```

여기서 `avatar: ` 뒤에 아까 넣은 이미지 파일의 경로(`assets/img/avatar.jpg`)를 넣어주시면 됩니다.

최종적으로 다음과 같이 수정되어 있어야 합니다.

```yml
# the avatar on sidebar, support local or CORS resources
avatar: assets/img/avatar.jpg
```

### 로컬서버로 테스트하기
프로필 사진이 잘 나오는지 배포하기 전에 먼저 로컬서버를 열어 확인해 봅니다.

```bash
bundle exec jekyll s
```

## 파비콘(favicon) 바꾸기

### 파비콘 이미지 준비
PNG, JPG, SVG 파일 확장자를 가진 512x512(혹은 그 이상)의 정사각형 이미지를 준비해주세요.

저는 위에 있던 프로필 사진을 여기서도 쓸 생각이어서 따로 준비는 안했습니다.

### 파비콘 생성
파비콘을 생성해주는 사이트인 <https://realfavicongenerator.net/>{:target="_blank"} 에 접속합니다.
`Select your Favicon image`를 클릭해서 아까 준비한 이미지를 업로드합니다.

그리고 나서 변환이 끝나면 맨 아래에 있는 `Generate your Favicons and HTML code`를 클릭합니다.

### 파비콘 다운로드
`1. Download your package: Favicon package`에서 `Favicon package`를 클릭해서 파비콘 이미지를 포함한 zip 파일을 다운받습니다.

그리고 압축을 푼 뒤 다음 파일을 삭제합니다.

* `browserconfig.xml`
* `site.webmanifest`

그 다음 나머지 파일들을 전부 복사해서 레포지토리에 `assets/img/favicons`에 덮어쓰기 해줍니다.

### 로컬서버로 테스트하기
프로필 사진때와 똑같이 변경이 잘 됐는지 배포 전에 다음 명령어로 로컬서버에서 확인해 봅니다.

```bash
bundle exec jekyll s
```

## 마치며
여기까지 문제없이 적용이 잘 됐다면 해당 변경사항을 `git commit` 후 `git push` 하게 되면 자동적으로 사이트에 변경된 프로필 사진과 파비콘이 적용됩니다!

다음글에서는 깃허브 블로그에 댓글창을 추가하는 방법에 대해 적도록 하겠습니다. 긴 글 읽어주셔서 감사합니다.