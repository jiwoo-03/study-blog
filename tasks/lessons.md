# Lessons

작업 중 발견한 실수, 교훈, 주의할 점을 기록한다. 같은 실수를 반복하지 않기 위한 참고 자료다.

- 이 WSL 환경에서는 `sudo`가 비대화형 쉘에서 비밀번호를 받을 수 없어 실패한다. `sudo apt install` 같은 명령은 사용자가 `!` 프리픽스로 직접 실행해야 한다.
- `gem install`은 시스템 디렉터리(`/var/lib/gems`)에 쓰기 권한이 없어 사용자 gem 경로(`~/.local/share/gem/ruby/<ver>/bin`)에 설치된다. 이 경로가 PATH에 없으면 `jekyll`, `bundle` 명령이 "command not found"가 되므로 매 명령 앞에 PATH를 명시적으로 export 해야 한다.
- `bundle install`도 기본적으로 시스템 디렉터리에 쓰려다 `Bundler::PermissionError`가 난다. `bundle config set --local path 'vendor/bundle'`로 프로젝트 로컬 경로에 설치하도록 설정해야 한다. (이미 `.gitignore`에 `vendor`가 포함되어 있어 git에는 올라가지 않음)
- 첫 `jekyll build`는 Rouge 문법 강조 lexer를 전부 로드해서 몇 초~십수 초 걸릴 수 있다. `timeout` 값을 너무 짧게 주면 SIGTERM으로 빌드가 중간에 끊겨 에러처럼 보이니, 첫 빌드는 넉넉한 타임아웃을 준다.
- GitHub 계정에 "이메일 비공개(Keep my email address private)" 설정이 켜져 있으면, 실제 이메일로 커밋한 내용을 GitHub가 push 단계에서 `GH007` 에러로 거부한다. 커밋 작성자 이메일은 `<github-user-id>+<username>@users.noreply.github.com` 형식의 noreply 이메일로 설정해야 한다 (`gh api user --jq '.id, .login'`으로 id 조회).
- GitHub Pages를 `<username>.github.io`가 아닌 일반 저장소 이름(예: study-blog)으로 만들면 "프로젝트 페이지"가 되어, `_config.yml`의 `baseurl`을 `/저장소이름`으로 설정해야 사이트 내 링크가 올바르게 생성된다.
- Jekyll은 front matter가 없어도 저장소 루트의 마크다운 파일(README.md 등)을 기본적으로 `_site`에 그대로 복사해 배포한다. 블로그 콘텐츠가 아닌 관리용 파일(README.md, CLAUDE.md, tasks/ 등)은 `_config.yml`의 `exclude:`에 명시적으로 추가해야 공개 사이트에 노출되지 않는다.
- Decap CMS는 혼자 쓰는 개인 블로그엔 과한 경우가 많다. Decap과 설정 포맷이 거의 동일하면서 더 활발히 유지보수되는 **Sveltia CMS**가 있고, 1인 사용자는 OAuth 설정 없이 "Personal Access Token 로그인"만으로도 충분하다 — 다만 이번엔 사용자가 "한 번 설정해두고 이후 편하게" 쪽(OAuth)을 선택함.
- Cloudflare Workers를 그 계정에서 처음 쓸 때는 `*.workers.dev` 서브도메인을 먼저 등록해야 하며, 이는 CLI(`wrangler deploy`)만으로는 안 되고 대시보드에서 Worker를 하나(더미여도 무방) 생성하는 과정 중에 등록된다.
- GitHub App 생성 시 Client Secret처럼 민감한 값은 사용자가 채팅에 붙여넣지 않고 Cloudflare 대시보드(Workers 설정 > Variables and Secrets)에 직접 입력하도록 안내한다 — 대화 로그에 비밀값이 남지 않게 하기 위함. Client ID는 공개 값이라 공유해도 무방.
- Sveltia CMS 설정 시 흔한 실수: `<script>` 태그에 `type="module"` 속성을 붙이거나 존재하지 않는 CSS `<link>`를 추가하는 것(공식 문서가 AI 에이전트들이 자주 하는 실수로 지적함) — 둘 다 불필요하며 오히려 오동작을 유발할 수 있다. 스크립트 버전도 고정하지 않고 unpkg 비버전 URL을 쓰는 게 권장된다.
- CMS/OAuth 연동처럼 외부 계정(Cloudflare, GitHub App)에 여러 리소스를 만드는 작업은, 막상 다 세팅해놓고 보면 사용자가 "굳이 이렇게까지 안 해도 됐다"고 되돌릴 수 있다. 되돌릴 때는 ① 블로그 저장소 내 변경(admin/) 되돌리기, ② 외부에 만든 리소스(Worker, GitHub App) 정리까지 둘 다 챙겨야 한다 — 전자만 하고 후자를 빠뜨리면 안 쓰는 리소스가 계정에 남는다. GitHub App 자체의 삭제는 API로 불가능해 사용자가 웹사이트에서 직접 해야 한다(Advanced 탭 > Danger zone).
- Jekyll의 기본 permalink 스타일(`date`)은 `/:categories/:year/:month/:day/:title.html`이라 front matter에 한글 `categories`를 쓰면 URL 경로에 한글이 그대로 들어간다. `_config.yml`에 `permalink: /:year/:month/:day/:title/`처럼 `:categories`를 뺀 패턴을 지정해야 깔끔한 URL이 된다.
- `_posts/`에 파일을 넣어도 파일명이 `YYYY-MM-DD-제목.md` 형식이 아니면 Jekyll이 에러 없이 그냥 조용히 무시한다(빌드 성공, 글만 안 보임). 사용자가 직접 글 파일을 만들 때는 매번 파일명 날짜 형식과 front matter(layout: post, title, date) 존재 여부를 먼저 확인해야 한다.
- `_posts/` 안에 하위 폴더를 만들어도 permalink(URL)에는 전혀 반영되지 않는다 — 순수하게 소스 파일 정리용. 실제 테스트(폴더에 글 하나 넣고 build)로 확인함.
- minima 테마에서 디자인(색상 등)을 바꿀 때는 site 루트에 `assets/main.scss`를 새로 만들어 `@import "minima";` 뒤에 원하는 선택자(예: `.site-title`)를 재정의하면 된다 — gem이 제공하는 기본 main.scss를 site 쪽 파일이 자동으로 덮어쓰는 Jekyll의 theme-gem 동작을 이용. CSS 캐스케이드상 같은 specificity면 나중에 선언된 규칙이 이기므로, `@import` 뒤에 커스텀 규칙을 적어야 한다.
- "글씨에 색을 입혀달라"와 "글씨 주변 배경(경계선 안쪽)에 색을 입혀달라"는 다른 요청이다 — color vs background-color, 그리고 어떤 요소(.site-title vs .site-header)를 대상으로 하는지 헷갈리기 쉽다. 배경색을 바꿀 때는 글씨와의 대비(contrast)도 같이 맞춰줘야 한다 (흰 글씨 등) — 그 영역 안의 nav 링크, 모바일 메뉴 아이콘 색도 함께 체크해야 안 보이는 요소가 없다.
- 사용자가 업로드한 파일명을 말로 전달할 때 오타(Transformer→Transfomer)나 경로 혼동(assets/ vs assets/images/)이 흔하다. 파일을 찾을 수 없을 땐 바로 못 찾았다고 보고하고, 유사한 이름으로 추측해서 아무 파일이나 쓰지 않는다 — 이미지/콘텐츠 파일은 잘못 집으면 엉뚱한 내용이 올라간다.
- Markdown 이미지 경로에 파일명 공백이 있으면 URL 인코딩(`%20`)해야 하고, 사이트가 서브경로(baseurl)에 있으면 `{{ site.baseurl }}`을 반드시 붙여야 한다.
