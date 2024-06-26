---
layout: single
title: git 에 잘못 업로드된 파일 삭제하기
categories: 
  - trouble shooting
tags: 
  - git
  - gitguadian
  - github
---

## 문제사례

gitguadian 의 메일을 받았음

내부 서버정보, 로그인정보 등의 내용이 github 에 올라오게 되는 경우 알림메일이 옴

이 경우 github에 올라간 파일을 삭제해야하는데 이미 github 에 올라간지가 오래되어서 삭제하는 방법이 쉽지않았음

검색하다가 gitguadian과 atomic0x90 님 블로그에서 발견하게 되어 기록함

## 해결방안

history 에서 파일을 삭제

```bash
$ git filter-branch --force --index-filter "git rm --cached --ignore-unmatch 'file_path/file_name'" --prune-empty --tag-name-filter cat -- --all
```

file_path/file_name 에 지우고자 하는 파일을 입력하면 됨

그리고 github에 강제로 적용해야함

```bash
$ git push origin main --force
```

git 에서 관리하지 않고자 하는 파일은 .gitignore 에 저장

```bash
$ git add .gitignore
$ git commit -m "Update .gitignore (file_name)"
$ git push orign main
```



## 참고

- [https://blog.gitguardian.com/rewriting-git-history-cheatsheet/](https://blog.gitguardian.com/rewriting-git-history-cheatsheet/)
- [https://atomic0x90.github.io/github/2020/03/16/github-history-remove.html](https://atomic0x90.github.io/github/2020/03/16/github-history-remove.html)
