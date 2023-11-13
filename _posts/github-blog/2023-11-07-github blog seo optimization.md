---
title: 깃허브 블로그 - SEO 검색 엔진 최적화 (구글/네이버/다음/빙) (Jekyll Chirpy 기준)
date: 2023-11-07 10:52:00 +0900
categories: [깃허브 블로그]
tags: [github blog, jekyll chirpy, tutorial, 깃허브 블로그, 튜토리얼, seo, 검색 엔진 최적화]     # TAG names should always be lowercase
image:
    path: 'https://github.com/jeuuniv/jeuuniv.github.io/assets/149172579/9cc96fcd-b5db-4cf2-9df3-b2e84b007502'
---

> 이번 글에서는 본인이 만든 블로그가 구글이나 네이버에서 검색해서 유입될 수 있도록 노출시키는 방법에 대해서 알려드리겠습니다. 
이를 검색 엔진 최적화라 하는데 Jekyll Chirpy에서는 `sitemap.xml`, `robots.txt`가 자동으로 생성되므로 간단하게 적용 할 수 있습니다.

---

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

---

## 네이버
### 소유권 확인
네이버는 `네이버 서치 어드바이저-웹마스터 도구`에서 사이트를 등록하게 되는데 과정이 `Google Search Console`때와 유사해 손쉽게 따라하실 수 있습니다.

먼저, <https://searchadvisor.naver.com/console/board>{:target="_blank"}에 들어가 주신 뒤 사이트를 등록해줍니다.

그 다음에 소유확인을 진행하기 위해 HTML 확인 파일을 다운해주시고 레포지토리 최 상단에 위치시켜주세요.

그리고 블로그 배포가 끝나고 나서 소유확인을 진행하시면 됩니다.

### 사이트맵 제출
그 다음 `사이트관리-요청-사이트맵 제출` 에서 사이트맵 URL 입력칸에 `https://(깃허브id).github.io/sitemap.xml`을 적어 확인 버튼을 누르시면 정상적으로 등록됩니다.

### RSS 제출
Jekyll Chirpy에서는 자동으로 `feed.xml`란 Atom 형식의 피드를 생성하지만, 네이버에서는 RSS2.0 형식만 지원하기 때문에 위의 xml 파일을 제출하게 되면 `올바른 RSS 가 아닙니다.` 라고 에러 메시지가 출력됩니다.

이를 해결하기 위해 <https://jekyllcodex.org/without-plugin/rss-feed/#> 링크를 들어가서 Installation 항목을 찾아 준 뒤, `feed.xml` 글자를 눌러 해당 파일을 레포지토리 최상단에 `rss.xml` 이름으로 저장해줍니다.

그리고 블로그 배포가 끝난 뒤 RSS 제출 URL에 `https://(깃허브id).github.io/rss.xml`을 적고 확인을 누르시면 오류없이 정상적으로 제출이 되는 것을 볼 수 있습니다.

---

## 다음
Daum 검색등록 <https://register.search.daum.net/index.daum>에 들어가 줍니다.

바로 보이는 화면에서 `블로그 등록`에 체크해 주신 뒤, 블로그 URL에 본인의 블로그 주소를 적고 확인을 누릅니다.

개인정보 수집 및 이용 동의에 동의해주시고, 이메일 주소를 기입해주시면 등록 신청이 됩니다.

---

## 빙
빙도 유사한 과정으로 소유권 확인을 html 파일로 인증하지만,

앞에 `Google Search Console`에 본인의 블로그를 등록했다면 과정이 매우 간단해 집니다.

먼저, 빙 웹마스터 도구 <https://www.bing.com/webmasters/home>에 들어가 줍니다.

사이트를 추가할 때 `Google Search Console에서 가져오기` 옵션이 있는데 선택 후 계정을 연동해주시면 됩니다.

사이트맵 등록은 연동 시 자동으로 등록되므로 추가적인 작업을 안하셔도 됩니다.


---

## 마치며
글 작성이 아직 서툴러서 이상한 표현이 있을 수 있습니다! 차후 수정을 통해 보완하도록 하겠습니다!

다음 글에서는 구글 애널리틱스 등록하는 방법으로 찾아뵙도록 하겠습니다.

긴 글 읽어주셔서 감사합니다!
