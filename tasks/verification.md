# Verification

작업 완료를 선언하기 전 확인한 사항을 기록한다. 검증되지 않은 작업은 완료로 표시하지 않는다.

## 2026-11-26 — Jekyll 설치 및 사이트 생성
- Ruby 3.3.8 설치 확인 (`ruby -v`)
- `gem install jekyll bundler` 성공 (사용자 gem 경로에 설치됨)
- `jekyll new`로 scaffold 생성 후 기존 CLAUDE.md/README.md/tasks/와 파일명 충돌 없음을 확인, 복사
- `bundle config set --local path 'vendor/bundle'` 설정 후 `bundle install` 성공 (38 gems)
- `bundle exec jekyll build` 성공, `_site` 정상 생성 (Sass deprecation 경고만 있음, 빌드 실패 아님)
- 재검증 필요 항목: `_config.yml` 값(title/email 등)을 실제 값으로 채운 뒤 재빌드 확인, GitHub Pages 배포 후 실제 URL 접속 확인

## 2026-11-26 — gem bin PATH 영구 등록
- `~/.bashrc`에 `export PATH="$HOME/.local/share/gem/ruby/3.3.0/bin:$PATH"` 추가
- `bash -ic 'bundle exec jekyll -v'`로 새 대화형 쉘에서 정상 동작 확인 (`jekyll 4.4.1`)
- 참고: `jekyll` 단독 실행(`bundle exec` 없이)은 전역 rake 버전 충돌로 에러 발생 — 정상이며, 항상 `bundle exec jekyll ...`로 실행해야 함 (README.md에 이미 반영됨)

## 2026-11-26 — GitHub 저장소 생성 및 Pages 배포
- `_config.yml` 값(title=study-blog, email 비움, description, baseurl=/study-blog, url, github_username) 반영 후 `bundle exec jekyll build` 재성공 확인
- `git init` → main 브랜치로 변경 → `.bundle`을 `.gitignore`에 추가 → 첫 커밋 생성
- `gh auth status`로 `jiwoo-03` 계정 로그인 확인
- `gh repo create study-blog --public --source=. --remote=origin --push` 실행 — 저장소는 생성됐으나 이메일 프라이버시 정책(GH007)으로 최초 push 거부됨
- 커밋 작성자를 GitHub noreply 이메일(`275266434+jiwoo-03@users.noreply.github.com`)로 수정(`--amend --reset-author`) 후 재push 성공 (`git push -u origin main`)
- `gh api -X POST repos/jiwoo-03/study-blog/pages`로 Pages 활성화 (main 브랜치, root), 응답에서 `html_url: https://jiwoo-03.github.io/study-blog/` 확인
- 배포 완료 확인: `https://jiwoo-03.github.io/study-blog/` curl 응답 200 OK

## 2026-11-26 — 관리용 파일 노출 차단
- 재빌드 전 `_site`에 `CLAUDE.md`, `README.md`, `tasks/*.md`가 그대로 복사되어 공개 노출되고 있음을 확인
- `_config.yml`에 `exclude: [CLAUDE.md, README.md, tasks/]` 추가
- `_site` 삭제 후 재빌드하여 해당 파일들이 결과물에서 빠진 것을 확인 (`ls _site`에 404.html/about/assets/feed.xml/index.html/jekyll만 남음)
- 배포본 확인 완료: `/CLAUDE.md`, `/README.md`, `/tasks/todo.md` 모두 404 (curl)

## 2026-11-26 — Sveltia CMS + Cloudflare Workers OAuth 세팅
- `~/sveltia-cms-auth` 클론, `npm install`, `npx wrangler login`(사용자), `npx wrangler whoami`로 로그인 확인
- Cloudflare 계정에 workers.dev 서브도메인이 없어 첫 `wrangler deploy` 실패 — 대시보드에서 더미 Worker(`study-blog`) 생성으로 서브도메인(`jiwoo7914.workers.dev`) 등록 후 재배포 성공
- GitHub App("Sveltia CMS Auth for study-blog") 생성 (Redirect URI: `https://sveltia-cms-auth.jiwoo7914.workers.dev/callback`, Contents/PR 권한 read-write), `study-blog` 저장소에 설치
- `wrangler secret put`으로 `GITHUB_CLIENT_ID`, `ALLOWED_DOMAINS=jiwoo-03.github.io` 설정 (제가 실행), `GITHUB_CLIENT_SECRET`은 사용자가 Cloudflare 대시보드에서 직접 설정
- `wrangler secret list`로 3개 secret 모두 존재 확인
- `admin/index.html`, `admin/config.yml` 추가 (backend: github, repo: jiwoo-03/study-blog, base_url: Worker URL), `jekyll build`로 `_site/admin/`에 정상 포함 확인 후 push
- 배포 확인: `https://jiwoo-03.github.io/study-blog/admin/` 200 OK
- 재검증 필요 항목: 사용자가 실제로 `/admin/`에서 GitHub 로그인 버튼을 눌러 로그인 및 글 작성·커밋까지 되는지 end-to-end 테스트 (GitHub App 설치/권한 문제는 API로 검증 불가능해 실제 로그인으로만 확인 가능)

## 2026-11-26 — Sveltia CMS 되돌리기 및 외부 리소스 정리
- 사용자가 CMS 없이 md 파일을 직접 작성해 push하는 원래 방식이 더 낫다고 판단, 되돌리기로 결정
- `git rm -r admin` → 커밋·push 완료, 배포본에서 `/admin/`이 다시 빠짐 (더 이상 접근 불가 — 의도한 결과)
- `npx wrangler delete`로 `sveltia-cms-auth`, `study-blog` Worker 둘 다 삭제 ("Successfully deleted" 확인)
- 로컬에 클론해둔 `~/sveltia-cms-auth` 폴더 `rm -rf`로 삭제, `ls`로 삭제 확인
- GitHub 웹사이트에서 사용자가 "Sveltia CMS Auth for study-blog" GitHub App 삭제 완료 (API로는 앱 자체 삭제가 불가능해 사용자가 직접 수행)
- 최종 확인: `https://jiwoo-03.github.io/study-blog/admin/` curl 응답 404 (정상 — admin 제거가 배포에 반영됨)

## 2026-11-26 — 첫 글 작성
- `_posts/2026-11-26-hello.markdown` 작성 → 로컬 빌드에서 permalink가 `/소개/2026/11/26/hello.html`로 한글 카테고리 포함되는 문제 발견
- `_config.yml`에 `permalink: /:year/:month/:day/:title/` 추가 후 재빌드, `/2026/11/26/hello/` 형태로 정상 생성 확인
- 사용자 확인 후 Jekyll 기본 샘플 글(`2026-11-26-welcome-to-jekyll.markdown`) 삭제, 재빌드로 결과물에서 빠진 것 확인
- push 후 배포 확인: `https://jiwoo-03.github.io/study-blog/2026/11/26/hello/` curl 응답 200 OK

## 2026-11-26 — Transformer 글 커밋/push
- `_posts/Transformer.markdown`(날짜 없는 파일명, front matter 없음)로는 `jekyll build` 성공해도 `_site`에 전혀 생성되지 않음을 확인 (조용히 무시됨)
- `2026-11-26-transformer.markdown`으로 리네임 + front matter(layout: post, title, date) 추가 후 재빌드, `_site/2026/11/26/transformer` 생성 확인
- 배포 확인 완료: `https://jiwoo-03.github.io/study-blog/2026/11/26/transformer/` curl 응답 200 OK

## 2026-11-26 — 사이트 디자인(버건디) 변경
- minima gem 경로(`bundle show minima`)에서 `_sass/minima/_layout.scss`의 `.site-title`, `.site-header` 규칙과 기본값(`$grey-color-dark` 등) 직접 확인
- site 루트 `assets/main.scss` 생성 (`@import "minima"` 뒤 커스텀 규칙) → 1차로 `.site-title` 글자색을 버건디(#7b1e3a)로, 빌드 결과 CSS에서 커스텀 규칙이 minima 기본 규칙보다 나중에 선언돼 우선 적용됨을 확인 후 push, 배포 확인(curl로 main.css에 7b1e3a 포함 확인)
- 사용자 피드백으로 의도가 "글자색"이 아니라 "header 영역(경계선 안쪽) 배경색"임을 확인 → `.site-header`에 `background-color: #7b1e3a` 적용, 대비를 위해 `.site-title`/`.site-nav .page-link`/모바일 메뉴 아이콘을 흰색으로 변경
- 재빌드로 `_site/assets/main.css`에 `.site-header { background-color: #7b1e3a; }` 반영 확인 후 push
- 사용자가 "변경이 안 보인다"고 함 → `curl`로 캐시 우회 쿼리스트링(`?v=timestamp`) 붙여 재조회, 서버/CDN에는 이미 최신 CSS가 반영되어 있음을 확인 (브라우저 캐시 문제로 결론)
- `.site-title`에 `font-weight: 700` 추가, 로컬 빌드로 반영 확인 후 push, 배포본 캐시 우회 조회로 `font-weight:700` 포함 확인

## 2026-11-26 — Transformer 글 추가 내용 push
- 로컬 빌드가 평소보다 오래 걸림(24초, OneDrive 동기화 폴더 I/O 지연 추정) — 백그라운드로 넘어갔지만 정상 완료(exit 0), `_site` 생성 확인
- 커밋·push 후 curl로 배포본에 새 내용("Residual" 키워드) 포함 확인

## 2026-11-26 — 테마 전환 (Minima → minimal-mistakes)
- `bundle install` 성공 (jekyll-remote-theme, jekyll-include-cache 포함 40 gems) — libssl-dev 설치 후 재시도로 해결
- `bundle exec jekyll build` 성공 (에러 없음, Sass 쪽 deprecation 경고만 있음 — 테마 자체의 구버전 문법 때문이며 빌드 실패 아님)
- 빌드 결과물(`_site`) 구조 확인: `index.html`, `about/`, `404.html`, `2026/11/26/transformer/`, `assets/css/main.css`, `feed.xml`만 생성됨 — 의도치 않은 카테고리 페이지 등 추가 생성 없음
- `_site/assets/css/main.css`에 커스텀 배색(`#7b1e3a`)이 정상 포함됨을 grep으로 확인
- `bundle exec jekyll serve`로 로컬 서버 구동 후 Playwright(`npx playwright screenshot`)로 스크린샷 촬영·육안 확인:
  - 데스크톱(1280px) 홈: 버건디 헤더, 흰 글씨 "study-blog"/"Home"/"About", Recent Posts에 Transformer 글 정상 표시
  - 데스크톱(1280px) 포스트: 제목/본문/목차용 헤딩/리스트/이미지 3장/읽기 시간/업데이트 날짜 모두 정상 렌더링
  - 모바일(390px) 홈·포스트: 반응형으로 1단 레이아웃 전환, 헤더 배색 유지, 텍스트 깨짐이나 가로 스크롤 없음
  - (최초 스크린샷에서 전체 화면이 흐릿하게 찍힌 현상 발견 → 테마의 fade-in 애니메이션 타이밍 문제로 확인, `--wait-for-timeout=1500` 추가 후 정상 확인됨 — 실제 버그 아님)
- `git status`로 변경 범위 확인: `404.html`, `Gemfile`, `Gemfile.lock`, `_config.yml`, `_posts/2026-11-26-transformer.markdown`, `about.markdown` 수정, `assets/main.scss` → `assets/css/main.scss` rename, `_data/navigation.yml` 신규 — 의도하지 않은 변경 없음, `_site`/`.jekyll-cache` 등 빌드 산출물은 gitignore로 깔끔
- 미검증(다음에 필요): 실제 GitHub Pages 배포 후(push 이후) 같은 확인을 배포 URL에서 재확인 — 로컬 `remote_theme` 다운로드가 GitHub Pages 빌드 환경에서도 동일하게 성공하는지는 push 전까지 보장 안 됨
- push 후 재확인 완료: `gh api repos/jiwoo-03/study-blog/pages/builds/latest`로 새 커밋(3a59910) 기준 `status: built` 확인, `curl`로 홈페이지 200 및 `assets/css/main.css`에 `7b1e3a` 포함, nav 마크업(`masthead__menu-item`) 정상 포함 확인

## 2026-11-26 — 버건디/화이트/블랙 배색 재설계
- `bundle exec jekyll build` 성공, `_site/assets/css/main.css`에서 `$primary-color`→버건디, `$text-color`→#1a1a1a, 블록쿼트 테두리·버튼 테두리에도 버건디 반영됨을 grep으로 확인
- 1차 스크린샷에서 헤더 글자(로고/Home/About)가 안 보이는 회귀 발견 → 원인 특정(`$masthead-link-color`가 `$primary-color`를 따라가며 배경과 같은 색이 됨) 후 `$masthead-link-color`/`$masthead-link-color-hover`를 흰색 계열로 재정의, 재빌드하여 `.greedy-nav a { color: #ffffff; }`로 컴파일됨을 grep으로 확인
- `bundle exec jekyll serve`로 재구동, 실행 중인 서버가 파일 변경을 자동 반영했는지 `curl`로 컴파일된 CSS를 직접 조회해 확인(재시작 없이 반영됨)
- Playwright 스크린샷으로 최종 확인: 데스크톱(1280px)·모바일(390px) 홈/포스트/About 페이지 모두 헤더(버건디 배경, 흰 글씨), 본문(검정 텍스트, 버건디 링크·블록쿼트 테두리), Follow 버튼(버건디 테두리) 정상 렌더링, 가로 스크롤이나 대비 문제 없음
- 아직 미완료: 커밋·push (사용자 승인 대기 중)
