---
title: "git worktree로 clone 없이 브랜치마다 작업 폴더 만들기"
categories:
  - "개발하며 든 생각"
tags:
  - "git"
  - "worktree"
---

## 요약

- 여러 브랜치를 동시에 다룰 때, 저장소를 여러 번 clone하는 대신 git worktree로 작업 폴더만 추가할 수 있음
- clone 3개는 별개 저장소 3개라 fetch를 3번 해야 하지만, worktree 3개는 저장소 1개를 공유해서 fetch 한 번이면 됨
- 작업 공간을 늘릴 때는 저장소를 통째로 복사하기 전에, 함께 써도 되는 것(커밋과 remote)과 따로 둬야 하는 것(작업 폴더)을 먼저 나눠 보면 됨

<!--more-->

## 상황: 여러 브랜치를 동시에 다뤄야 할 때

- 기능 브랜치 작업 중에 다른 브랜치의 이슈를 확인하거나, PR 리뷰와 개발을 같이 하거나, base 브랜치의 최신 변경을 반영해야 할 때가 있음
- 폴더 하나만 쓰면 다른 브랜치로 넘어갈 때마다 현재 작업을 `stash`로 치웠다가 돌아와서 다시 꺼내야 함

```bash
git stash
git switch feature/other
# 확인
git switch feature/current
git stash pop
```

- 그래서 많은 개발자가 저장소를 여러 번 clone해서 `some-api`, `some-api2`, `some-api3`처럼 폴더를 나눠 씀

## 불편: 여러 clone을 쓰면 생기는 일

- 여러 clone을 쓰면 브랜치 전환 문제는 줄지만, clone끼리는 서로 별개 저장소라 관리할 것이 늘어남
- clone마다 `git fetch`를 따로 해야 하고, 한쪽에서 만든 브랜치도 다른 clone에는 자동으로 보이지 않음
- 저장소를 통째로 복사하므로 clone을 하나 늘릴 때마다 디스크를 그만큼 더 씀

## 소개: worktree는 작업 폴더만 늘림

- git worktree는 저장소를 하나 더 복사하는 clone과 달리, 저장소 하나에 브랜치마다 작업 폴더만 따로 추가하는 기능임
- clone처럼 브랜치마다 폴더가 따로 있으니, 브랜치를 바꿀 때 `stash` 없이 폴더만 옮기면 됨
- 저장소 객체와 히스토리를 공유해서 clone보다 빨리 만들어지고 디스크도 적게 씀
- 한 worktree에서 `git fetch origin`을 실행하면 다른 worktree에서도 그 remote 정보를 바로 쓸 수 있음
- 같은 저장소를 여러 창으로 열어 둔 것에 가까워서, 여러 PR을 동시에 다루기 좋음

## 사용법: 만들고, 쓰고, 지우기

- worktree 경로는 저장소 바깥에 두는 것을 권장하고, 안에 두면 `.gitignore`에 추가해야 함

```bash
# 새 브랜치를 만들면서 worktree 생성
git worktree add -b feature/auth ../_worktrees/some-api/feature-auth

# 이미 있는 브랜치에 worktree 연결
git worktree add ../_worktrees/some-api/feature-auth feature/auth

# 경로의 마지막 이름(hotfix)과 같은 브랜치를 연결 (없으면 새로 만듦)
git worktree add ../_worktrees/some-api/hotfix

# worktree 목록 보기
git worktree list
```

- `git branch` 목록에서 `+` 기호가 붙은 브랜치는 다른 worktree가 쓰고 있는 브랜치임
- worktree 폴더로 이동하면 평범한 git 저장소처럼 커밋, fetch, merge, push를 하면 됨
- worktree 안에서도 `git switch`는 되지만 다른 worktree가 쓰고 있는 브랜치로는 옮길 수 없으므로, 브랜치를 바꾸려면 폴더를 옮기면 됨

```bash
# worktree 삭제 (브랜치는 남음)
git worktree remove ../_worktrees/some-api/feature-auth

# 커밋하지 않은 변경이 있어도 강제로 삭제
git worktree remove --force ../_worktrees/some-api/feature-auth

# rm -rf로 직접 지웠다면 남은 메타데이터 정리
git worktree prune

# 브랜치까지 지우기
git branch -d feature/auth
```

## 참고

- [git-worktree Documentation (Git 공식 문서)](https://git-scm.com/docs/git-worktree)
- [git worktree 사용법 (Dale Seo, 2024-06-30)](https://daleseo.com/git-worktree/)
- [Git Worktree : 여러 개의 작업 트리 관리하기 (천재웅, 2025-01-09)](https://cheonjaeung.com/posts/git-worktree/)
- [Git에서 다수의 워킹 디렉토리를 관리하는 worktree (Outsider, 2022-04-06)](https://blog.outsider.ne.kr/1588)
