# AGENTS.md

Jekyll 블로그(minimal-mistakes 원격 테마). `main`에 푸시하면 GitHub Actions가 GitHub Pages로 배포한다.

- 초안은 `_drafts/`에 두고 `bundle exec jekyll serve --drafts`로 미리 본다.
- 검색 스크립트(lunr)는 검색창을 열 때만 불러온다. 글 전체 본문 색인을 모든 페이지가 받으면 전송량과 색인 생성 부담이 크기 때문이다.
- `_layouts`·`_includes`는 minimal-mistakes 테마 사본을 덮어쓴 것이므로 레이아웃 수정은 이 로컬 파일에서 한다.
- `_pages/resume.md` 등 공개 페이지에는 회사 내부 시스템 이름과 내부 수치를 쓰지 않는다.
