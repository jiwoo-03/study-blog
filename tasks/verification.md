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
