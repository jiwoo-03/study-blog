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
- GitHub App 생성 시 Client Secret처럼 민감한 값은 사용자가 채팅에 붙여넣지 않고 Cloudflare 대시보드(Workers 설정 > Variables and Secrets)에 직접 입력하도록 안내한다 — 대화 로그에 비밀값이 남지 않게 하기 위함. Client ID는 공개值이라 공유해도 무방.
- Sveltia CMS 설정 시 흔한 실수: `<script>` 태그에 `type="module"` 속성을 붙이거나 존재하지 않는 CSS `<link>`를 추가하는 것(공식 문서가 AI 에이전트들이 자주 하는 실수로 지적함) — 둘 다 불필요하며 오히려 오동작을 유발할 수 있다. 스크립트 버전도 고정하지 않고 unpkg 비버전 URL을 쓰는 게 권장된다.
