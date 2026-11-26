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
