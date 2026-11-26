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
- 사용자 판단으로 CMS 도입을 되돌리기로 결정 — admin/ 제거 후 push, Cloudflare Worker 2개 삭제, 로컬 클론 삭제, GitHub App 삭제(사용자 직접)까지 전부 정리 완료. md 파일을 직접 작성해 push하는 원래 방식으로 복귀
- 첫 글("블로그를 시작합니다") 작성 및 push, permalink 형식을 `/:year/:month/:day/:title/`로 정리(한글 카테고리가 URL에 들어가는 문제 해결), Jekyll 기본 샘플 글 삭제
- 사용자가 직접 만든 `_posts/Transformer.markdown`이 날짜 없는 파일명 때문에 Jekyll에 무시되는 문제 발견 → `2026-11-26-transformer.markdown`으로 리네임, front matter 추가 후 커밋·push
- `assets/images/` 폴더 생성(`.gitkeep` 포함) 및 push, 이미지 삽입 방법(`{{ site.baseurl }}` 경로) 안내
- 사용자가 Transformer 글에 실제 내용(Encoder/Decoder, Attention, Self Attention 등) 작성 → 빌드 확인 후 커밋·push
- `_posts/` 하위 폴더가 URL에 영향 없음을 실험으로 확인 후 안내
- Transformer 글 추가 수정사항 커밋·push
- 사이트 제목에 버건디 색상(`#7b1e3a`) 적용 — `assets/main.scss` 생성, 배포 확인
