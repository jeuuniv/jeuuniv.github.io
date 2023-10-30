---
title: syntax error near unexpected token `$'{\r'' 오류 해결하는 법
date: 2023-10-30 00:00:00 +0900
categories: [깃허브 블로그, 문제해결]
tags: [github, github blog, jekyll, chirpy,]     # TAG names should always be lowercase
---

# 문제상황

wsl2의 터미널 창 안에서 git clone하고 가져온 레포지토리에서 bash tools/init 명령어를 입력하면 
~~~
tools/init: line 4: $'\r': command not found
: invalid option 5: set: -
set: usage: set [-abefhkmnptuvxBCHP] [-o option-name] [--] [arg ...]
tools/init: line 6: $'\r': command not found
tools/init: line 9: $'\r': command not found
tools/init: line 11: $'\r': command not found
tools/init: line 14: $'\r': command not found
tools/init: line 16: $'\r': command not found
tools/init: line 17: syntax error near unexpected token `$'{\r''
'ools/init: line 17: `help() {
~~~
위와 같이 정상적으로 실행이 안되는 모습을 보여줍니다.

# 원인
윈도우와 리눅스 간의 개행 문자 차이로 인해 발생하는 에러입니다.

# 해결법
다음의 명령어를 입력하면 해결됩니다.
~~~
sed -i 's/\r//' tools/init
~~~