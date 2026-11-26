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
- 사용자 피드백: 글씨색이 아니라 header 영역(경계선 안쪽) 배경을 버건디로 원함 → `.site-header`에 배경색 적용, 글씨/nav/모바일 메뉴 아이콘은 흰색으로 대비 조정, 커밋·push
- 사용자가 브라우저 캐시 때문에 변경이 안 보인다고 함 → curl로 서버/CDN은 이미 최신임을 캐시 우회(쿼리스트링)로 확인, 강력 새로고침/시크릿 창 안내
- 사이트 제목 글씨를 굵게(font-weight: 700) 변경, 빌드 확인 후 커밋·push
- Transformer 글 추가 내용(Feed Forward, Add & Norm) push, 배포 확인
- 사용자가 "assets/Transformer Architecture img.png"를 올렸다고 했으나 처음엔 찾지 못함 — 결국 `assets/images/Transfomer Architecture img.png`(오타 포함 실제 파일명)로 확인, Transformer 글에 이미지 삽입(baseurl + URL 인코딩) 후 커밋·push, 배포 확인
- 이미지 크기 축소 요청 → markdown 이미지 문법을 HTML `<img>` 태그로 바꿔 `style="max-width: 500px"` 적용, 커밋·push, 배포 확인
- 추가로 더 축소(350px) 및 가운데 정렬(`display:block; margin:0 auto`) 요청 반영, 커밋·push, 배포 확인
- 이미지 주변 흰색 여백을 글 칸 너비에 맞게 늘려달라는 요청 → 이미지를 `<div style="background:#fff; padding:32px 0; text-align:center">`로 감싸는 방식으로 반영, 커밋·push, 배포 확인
- 사용자가 글에 "Tansformer Encoder img.png" 플레이스홀더(blockquote)를 직접 추가해둠 — 아직 이미지 미업로드, 다음에 업로드하면 삽입 필요
