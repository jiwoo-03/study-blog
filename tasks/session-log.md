# Session Log

세션 단위 작업 기록. 날짜와 수행한 작업을 간단히 남긴다.

## 2026-11-26
- CLAUDE.md 작성 (하지 말아야 할 규칙 정의)
- README.md에 Jekyll + GitHub Pages 블로그 제작 workflow 작성
- tasks/ 디렉터리 및 todo.md, lessons.md, session-log.md, verification.md 추가
- CLAUDE.md에 "작업 전 사용자 허락 구하기" 규칙 추가
- Ruby/Jekyll/Bundler 설치 (sudo apt install은 사용자가 직접 실행), `jekyll new`로 사이트 생성, `bundle install` 및 `jekyll build` 검증 완료
- `~/.bashrc`에 gem bin PATH 영구 등록
- `_config.yml` 실제 값 채움, git 초기화 및 첫 커밋, `gh`로 GitHub 저장소(jiwoo-03/study-blog) public 생성 및 push, GitHub Pages 활성화
- 배포 완료 확인 (https://jiwoo-03.github.io/study-blog/ → 200 OK)
- CLAUDE.md/README.md/tasks/가 _site에 노출되던 문제 발견, `_config.yml` exclude로 차단 후 재빌드 검증
- Sveltia CMS + sveltia-cms-auth(Cloudflare Workers, GitHub App OAuth) 세팅: Worker 배포, GitHub App 생성·설치, secret 설정, admin/ 추가 및 배포 완료 (https://jiwoo-03.github.io/study-blog/admin/ → 200 OK)
