# To-Do

할 일 목록. 작업 시작 전 항목을 추가하고, 완료되면 체크한다. 완료 항목도 지우지 않고 남겨서 기록으로 유지한다.

- [x] 로컬 환경에 Jekyll 설치 및 사이트 생성 (`jekyll new .`)
- [x] `_config.yml`의 title/email/description 등 실제 값으로 채우기
- [x] GitHub 저장소 생성 및 push, GitHub Pages 배포 설정
- [x] GitHub Pages 실제 배포 URL(https://jiwoo-03.github.io/study-blog/) 접속 확인
- [x] CLAUDE.md/README.md/tasks/ 관리 파일이 빌드 결과물(_site)에 노출되지 않도록 `_config.yml` exclude 처리
- [x] Sveltia CMS 세팅 (/admin 글쓰기 UI, GitHub App OAuth 연동 via Cloudflare Workers)
- [x] 사용자 판단으로 Sveltia CMS 되돌리기 결정 — admin/ 제거, md 파일 직접 작성·push하는 원래 방식으로 복귀
- [x] 더 이상 쓰지 않는 외부 리소스 정리 (Cloudflare Worker 2개, GitHub App)
- [x] 첫 글("블로그를 시작합니다") 작성, permalink를 /:year/:month/:day/:title/ 형식으로 정리, Jekyll 샘플 글 삭제
- [x] 사용자가 작성한 "Transformer.markdown" 파일명·front matter 수정 후 커밋·push
- [x] `assets/images/` 폴더 생성 (이미지 업로드용, `.gitkeep`으로 빈 폴더 유지)
- [x] 사용자가 Transformer 글에 내용 추가 → 커밋·push
- [x] `_posts/` 하위 폴더가 URL에 영향 주는지 실험 후 안내 (영향 없음, 소스 정리용일 뿐)
- [x] 사이트 제목(.site-title)에 버건디 색상 적용 (`assets/main.scss` 생성)
- [x] 사용자 피드백 반영 — 글씨색이 아니라 header 영역(경계선 안쪽) 배경을 버건디로 수정, 글씨는 흰색으로 대비 확보
- [x] 사이트 제목(.site-title) 글씨를 굵게(font-weight: 700) 변경
- [x] Transformer 글 추가 내용(Feed Forward, Add & Norm) 커밋·push, 배포 확인
- [x] 사용자가 올린 아키텍처 이미지(`Transfomer Architecture img.png`)를 Transformer 글에 삽입, 커밋·push, 배포 확인
- [x] 이미지 크기 축소(max-width: 500px), 커밋·push, 배포 확인
- [x] 이미지 추가 축소(max-width: 350px) 및 가운데 정렬, 커밋·push, 배포 확인
- [x] 이미지 주변 흰색 패딩을 글 칸(content column) 너비에 맞게 추가, 커밋·push, 배포 확인
- [x] Encoder/Decoder 섹션 플레이스홀더에 Figure 1(Encoder), Figure 2(Decoder) 이미지 삽입, 커밋·push, 배포 확인
- [x] 사용자가 Figure 이미지 크기(320px) 직접 조정 → 커밋·push
- [x] 첫 소개글("블로그를 시작합니다", hello.markdown) 삭제, 커밋·push, 배포 확인(404)
- [x] 테마를 Minima → minimal-mistakes(remote_theme, skin: default)로 전환, `_data/navigation.yml`(Home/About) 추가, 커스텀 버건디 배색을 새 테마 선택자로 이식
- [x] 테마 전환 변경사항 커밋·push 및 실제 배포본 확인
- [x] 사용자 요청으로 버건디/화이트/블랙 배색을 SCSS 변수(primary-color, text-color, link-color 등) 레벨에서 재설계 — 헤더 글자색 충돌(버건디 위 버건디 글씨) 버그 발견 후 수정
- [x] 배색 재설계 변경사항 커밋·push 및 실제 배포본 확인
- [x] 사용자 요청으로 배색을 재설계 이전(기존 버건디 헤더 + 테마 기본색)으로 복원, 겸사겸사 "1280px보다 넓은 화면에서 헤더 양옆이 흰색으로 남는" 구조적 버그(배경색이 `.greedy-nav`가 아니라 `.masthead`에 있어야 함) 발견·수정, 로고/메뉴 글자색이 흐린 회색으로 보이던 오래된 버그(`$masthead-link-color` 미지정)도 함께 수정
- [x] 위 수정사항 커밋·push
- [x] 사용자가 로컬 바탕화면의 `panthera-jekyll-master` 폴더(포트폴리오형 Jekyll 테마)를 보여주며 이 구조대로 블로그를 바꿔달라고 요청 — minimal-mistakes → panthera-jekyll(remote_theme)로 전면 테마 전환. Home=테마 원형 프로필 스타일, 글 목록은 별도 `/blog` 페이지, Skills & Offers/Contact Me/My Resume는 "추후 작성 예정" 스텁 페이지로 생성, 아바타 없음·색상은 테마 기본(블루) 유지로 진행(사용자 확인 완료)
- [ ] panthera-jekyll 전환 변경사항 커밋·push (사용자 승인 대기)
