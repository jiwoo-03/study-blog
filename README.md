# 공부 블로그

개인 공부 자료를 정리해서 올리는 블로그. GitHub Pages + Jekyll로 운영한다.

## Workflow

### 1. 로컬 환경 준비
- Ruby, Bundler 설치 확인
- `gem install jekyll bundler`

### 2. Jekyll 사이트 생성
- `jekyll new .` 로 기본 뼈대 생성 (현재 폴더에)
- `Gemfile`에서 `github-pages` gem 사용하도록 설정 (GitHub Pages와 버전 호환 보장)
- `bundle install`

### 3. 로컬에서 확인
- `bundle exec jekyll serve` 로 로컬 서버 실행
- `http://localhost:4000` 에서 결과 확인

### 4. 기본 설정
- `_config.yml`에 블로그 제목, 설명, 작성자 정보 입력
- 테마는 기본 테마(Minima) 유지, 필요한 부분만 최소로 커스터마이징

### 5. GitHub 저장소 연결
- GitHub에 저장소 생성 (`<username>.github.io` 또는 일반 저장소명)
- `git init`, `git remote add origin <repo-url>`
- 첫 커밋 후 push (push 전 사용자 확인)

### 6. GitHub Pages 배포 설정
- 저장소 Settings > Pages에서 배포 브랜치 지정 (`main`)
- 배포 완료 후 제공되는 URL로 접속 확인

### 7. 글 작성 루틴
- `_posts/YYYY-MM-DD-제목.md` 형식으로 새 글 추가
- frontmatter에 `title`, `date`, `categories`/`tags` 작성
- 로컬에서 `jekyll serve`로 확인 후 커밋 & push

### 8. 반복
- 글 추가 → 로컬 확인 → push → GitHub Pages 자동 배포 확인

> 작업 규칙과 하지 말아야 할 것들은 `CLAUDE.md` 참고.
Blog Site : https://jiwoo-03.github.io/study-blog/