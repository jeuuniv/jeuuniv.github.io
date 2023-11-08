---
title: 깃허브 블로그 - 검색엔진 노출 등록 (구글/네이버/다음/빙) (Jekyll Chirpy 기준)
date: 2023-11-07 10:52:00 +0900
categories: [깃허브 블로그]
tags: [github, blog, github blog, jekyll, chirpy, jekyll chirpy, tutorial, seo, seo optimization, 깃허브, 블로그, 깃허브 블로그, 튜토리얼, 검색]     # TAG names should always be lowercase
image:
    path: 'https://github.com/jeuuniv/jeuuniv.github.io/assets/149172579/9cc96fcd-b5db-4cf2-9df3-b2e84b007502'
    alt: SEO Optimization
---

> 이번 글에서는 본인이 만든 블로그가 구글이나 네이버에서 검색해서 유입될 수 있도록 노출시키는 방법에 대해서 알려드리겠습니다. 
이를 검색 엔진 최적화라 하는데 Jekyll Chirpy에서는 `sitemap.xml`, `robots.txt`가 자동으로 생성되므로 간단하게 적용 할 수 있습니다.

## 구글
### Google Search Console 접속
<https://search.google.com/search-console/about>{:target="_blank"}에 들어가서 시작하기 버튼을 누릅니다.

누르면 속성 유형 선택에서 URL 접두어를 선택해 주신 뒤, 본인의 블로그 주소를 입력합니다.

### 소유권 확인
소유권 확인 창이 뜨면 화면에 뜨는 html 다운버튼을 눌러 본인 레포지토리의 최상단에 저장하시면 됩니다.

파일 경로는 `(깃허브id).github.io/googlea0255b0a3540a68f.html` 이렇게 됩니다.

그 다음 `_config.yml`을 열어 `google_site_verification:`을 찾아주신 뒤 값으로 html 파일의 이름을 넣어주시면 됩니다.

최종적으로 `google_site_verification: googlea0255b0a3540a68f.html` 처럼 됩니다.

그 다음 `git push`하게 되고 배포가 완료되면 Google Search Console에 확인 버튼을 누르시면 소유권 확인이 됩니다.

### 사이트맵 추가
사이트맵이란 검색엔진이 사이트에 있는 페이지, 동영상 및 기타 파일과 그 관계에 관한 정보를 제공하는 파일입니다.

보통은 `sitemap.xml`을 만들어야 하지만, Jekyll Chirpy에서는 자동으로 생성되므로 생략하겠습니다.

그러므로 바로 Google Search Console에서 색인생성-Sitemaps에 들어가 주신 뒤, 새 사이트 맵 추가에 그냥 `sitemap.xml`을 입력해주시고 제출하시면 됩니다.

이렇게 하시고 몇 주 정도 지나게 되면 검색 엔진이 블로그 글들을 크롤링 하고 검색결과에 노출이 됩니다.

## 네이버

## 다음

## 빙

