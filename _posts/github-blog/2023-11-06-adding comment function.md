---
title: 깃허브 블로그에 댓글 기능 추가 - giscus (Jekyll Chirpy 기준)
date: 2023-11-06 14:44:00 +0900
categories: [깃허브 블로그]
tags: [github, blog, github blog, jekyll, chirpy, jekyll chirpy, tutorial, 깃허브, 블로그, 깃허브 블로그, 튜토리얼, comment]     # TAG names should always be lowercase
---

> 깃허브 블로그에는 기본적으로 댓글 기능이 없습니다. 그렇기 때문에 이번 글에서는 giscus를 이용해서 댓글 기능을 추가하는 법에 대해서 알려드리겠습니다.

## giscus란?
### 소개
giscus는 깃허브의 토론(GitHub Discussions) 기능으로 작동하는 댓글 시스템입니다. 이 방법을 통해 블로그에 댓글을 달게 되면 자동으로 레포지토리의 토론에 기록됩니다.

### 특징
다음은 giscus의 특징을 나열해 보았습니다.

* 오픈 소스
* 광고 X, 무료
* 커스텀 테마, 여러 언어, 여러 설정이 가능함
* 자동으로 깃허브에서 새로운 댓글과 수정사항을 반영함



## giscus 설치
### 레포지토리 지정
먼저 giscus에 연결할 레포지토리를 지정합니다, 여기서 주의할 점은 **Public** 레포지토리여야 합니다.

저는 제 깃허브 블로그 레포지토리(`jeuuniv/jeuuniv.github.io`)를 사용하겠습니다.

### gicus 앱 설치
그 다음 gicus 앱을 활성화하기 위해 [***giscus 앱 설치***](https://github.com/apps/giscus){:target="_blank"}로 들어가서 오른 쪽 상단 초록색 버튼 `Install`을 클릭합니다.

그 다음 `for these repositories:`에서 `Only select repositories`를 선택한 뒤, 위에서 지정한 레포지토리로 바꿔줍니다. 

그리고 왼쪽 아래 `Install`을 클릭해주면 됩니다.

### Discussions 기능 활성화
giscus 앱을 설치한 레포지토리에 들어가서 Setting - General에서 Features에 있는 Discussions를 체크해줍니다.

### giscus 설정
먼저 <https://giscus.app/ko>{:target="_blank"}로 들어가서 `설정` 문단에 `저장소:` 에 giscus 앱을 설치한 레포지토리를 입력해줍니다.

입력한 뒤 옆에 초록색 체크표시와 아래에 `통과했습니다! 이 저장소는 모든 조건을 만족합니다.` 메시지가 나오는 것을 통해 정상적으로 진행됐다는 걸 알 수 있습니다.

아래에 `Discussion 카테고리` 문단에서 깃허브 블로그에서 댓글을 달았을 때 어떤 카테고리에 댓글을 모아둘 건지 원하는 걸 선택하시면 됩니다.

만약 본인이 따로 카테고리를 만들어서 사용하려면 giscus와 본인만 새 Discussions를 만들 수 있도록 `Discussion Format`을 `Announcements`로 해주셔야 합니다.

저는 여기서 General 카테고리로 설정하겠습니다.



그러고 나면 아래 `giscus 사용` 문단에 다음과 같은 코드가 보일겁니다.

```html
<script src="https://giscus.app/client.js"
        data-repo="jeuuniv/jeuuniv.github.io"
        data-repo-id="R_kgDOKmQFLQ"
        data-category="General"
        data-category-id="DIC_kwDOKmQFLc4Cah_w"
        data-mapping="pathname"
        data-strict="0"
        data-reactions-enabled="1"
        data-emit-metadata="0"
        data-input-position="bottom"
        data-theme="preferred_color_scheme"
        data-lang="ko"
        crossorigin="anonymous"
        async>
</script>
```

위 코드에서 다음은 항목은 `_config.yml` 설정에 사용되니 알고계시면 될 것 같습니다.

* data-repo="`jeuuniv/jeuuniv.github.io`"
* data-repo-id="`R_kgDOKmQFLQ`"
* data-category="`General`"
* data-category-id="`DIC_kwDOKmQFLc4Cah_w`"

### _config.yml 설정

앞서 얻은 코드를 활용해서 이제 `_config.yml` 설정을 해 보겠습니다.

먼저 `_config.yml`에서 `comment:` 부분을 찾으면 다음과 같이 있을 겁니다.

```yml
comments:
  active: # The global switch for posts comments, e.g., 'disqus'.  Keep it empty means disable
  # The active options are as follows:
  disqus:
    shortname: # fill with the Disqus shortname. › https://help.disqus.com/en/articles/1717111-what-s-a-shortname
  # utterances settings › https://utteranc.es/
  utterances:
    repo: # <gh-username>/<repo>
    issue_term: # < url | pathname | title | ...>
  # Giscus options › https://giscus.app
  giscus:
    repo: # <gh-username>/<repo>
    repo_id:
    category:
    category_id:
    mapping: # optional, default to 'pathname'
    input_position: # optional, default to 'bottom'
    lang: # optional, default to the value of `site.lang`
    reactions_enabled: # optional, default to the value of `1`
```
여기서 `active:`를 `active: giscus`로 바꿔줍니다.

그 다음 `giscus:`안에 있는 항목을 차례로 바꿔주겠습니다.

`repo:`에 위에서 얻은 `data-repo`값을 넣어줍니다, 저는 `repo: jeuuniv/jeuuniv.github.io`로 바꿔줬습니다.

위에 방법과 똑같이 
`repo_id:`는 `data-repo-id`값을, 
`category:`는 `data-category`값을, 
`category_id:`는 `data-category-id`값을 넣어주면 됩니다.


그리고 giscus는 기본값으로 댓글 입력 상자가 작성된 댓글 아래에 위치합니다.

저는 작성된 댓글 위에 댓글 상자를 배치하고 싶어서 `input_positions:`을 `input_position: top`으로 바꿔줬습니다.


일반적으로, giscus의 `lang:`은 설정한 홈페이지 언어로 자동 설정되기 때문에 따로 설정 할 필요가 없습니다.ㄷ

> 하지만, 본인 홈페이지의 lang값이 `ko-KR`과 같은 형식일 때 giscus가 작동하지 않으므로, 반드시 giscus 언어 설정에서 `ko`와 같은 형식을 강제로 지정해줘야 합니다.
{: .prompt-danger}

저는 `ko-KR`이라 설정되어 있으므로 `giscus:`내에 있는 `lang:`을 `lang: ko`로 바꿔줬습니다.


최종적으로 다음과 같이 설정된 모습을 볼 수 있습니다.

```yml
comments:
  active: giscus # The global switch for posts comments, e.g., 'disqus'.  Keep it empty means disable
  # The active options are as follows:
  disqus:
    shortname: # fill with the Disqus shortname. › https://help.disqus.com/en/articles/1717111-what-s-a-shortname
  # utterances settings › https://utteranc.es/
  utterances:
    repo: # <gh-username>/<repo>
    issue_term: # < url | pathname | title | ...>
  # Giscus options › https://giscus.app
  giscus:
    repo: jeuuniv/jeuuniv.github.io # <gh-username>/<repo>
    repo_id: R_kgDOKmQFLQ
    category: General
    category_id: DIC_kwDOKmQFLc4Cah_w
    mapping: # optional, default to 'pathname'
    input_position: top # optional, default to 'bottom'
    lang: ko # optional, default to the value of `site.lang`
    reactions_enabled: # optional, default to the value of `1`
```

## 로컬서버 테스트 후 git push
다음 명령어를 통해 로컬서버에서 정상적으로 댓글 시스템이 작동하는지 확인해봅니다.

```bash
bundle exec jekyll s
```

로컬에서 정상적으로 작동되는 것을 확인한 뒤 `git push`를 하면 본인의 깃허브 홈페이지에 댓글 기능이 생긴 것을 볼 수 있습니다!



## 마치며
제가 아직 글 작성하는 게 서툴러서 이상하게 서술된 부분도 있을 것 같습니다.
그런 부분이나 궁금한 점에 대해서 댓글로 남겨주시면 제가 빠르게 확인하고 답장 드리도록 하겠습니다!

오늘도 긴 글 읽어주셔서 감사합니다!