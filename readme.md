# cmjeon.github.io

배운 것을 기록하는 블로그. Jekyll과 minimal-mistakes 테마로 만들고 GitHub Pages로 배포한다.

## 글쓰기 흐름

1. `_drafts/제목.md`에 날짜 없이 초안을 쓴다.
2. 로컬에서 미리 본다. 첫 빌드는 30초 안팎 걸린다.

   ```bash
   bundle exec jekyll serve --drafts --livereload
   ```

   브라우저에서 http://localhost:4000 을 연다.

3. 다 쓰면 `_posts/YYYY-MM-DD-제목.md`로 옮기고 커밋해서 `main`에 푸시한다.
4. GitHub Actions(`Deploy Jekyll site to Pages`)가 1~2분 뒤 배포한다.

## 처음 설정

- Ruby 3.3과 bundler가 필요하다.
- 저장소 루트에서 `bundle install`을 한 번 실행한다.
