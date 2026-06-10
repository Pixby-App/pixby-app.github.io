# Pixby Public Site And Update Feed

이 저장소는 Pixby 공개 홈페이지와 macOS 원격 업데이트 피드를 함께 호스팅합니다.

- 공개 URL: `https://pixby-app.github.io/`
- 업데이트 피드: `https://pixby-app.github.io/updates/latest-mac.yml`
- 최신 앱 파일: 같은 저장소의 GitHub Release asset

## 배포 흐름

앱 저장소(`/Users/chungbok/Documents/GitHub/Pixby`)에서 새 macOS 패키지를 만든 뒤 이 저장소로 산출물을 복사합니다.

```bash
PATH="/usr/local/bin:$PATH" npm run package:mac
PATH="/usr/local/bin:$PATH" npm run deploy:pages
```

`npm run deploy:pages`는 기본적으로 이 저장소 경로
`/Users/chungbok/Documents/GitHub/pixby-app.github.io`를 사용합니다. 다른 위치에 체크아웃한 경우
`PIXBY_PAGES_REPO=/path/to/pixby-app.github.io npm run deploy:pages` 또는 인자로 경로를 전달합니다.

DMG/ZIP/blockmap 파일은 GitHub 일반 커밋 100MB 제한을 넘을 수 있으므로 GitHub Release asset으로 업로드합니다.

```bash
gh release create v0.1.7 \
  /Users/chungbok/Documents/GitHub/Pixby/release/Pixby-0.1.7-arm64.dmg \
  /Users/chungbok/Documents/GitHub/Pixby/release/Pixby-0.1.7-arm64.dmg.blockmap \
  /Users/chungbok/Documents/GitHub/Pixby/release/Pixby-0.1.7-arm64-mac.zip \
  /Users/chungbok/Documents/GitHub/Pixby/release/Pixby-0.1.7-arm64-mac.zip.blockmap \
  --repo Pixby-App/pixby-app.github.io \
  --title "Pixby v0.1.7"
```

## 운영 원칙

- `updates/latest-mac.yml`은 항상 최신 버전을 가리킵니다.
- 실제 앱 파일은 GitHub Release asset으로 보관합니다.
- Pages git history에는 100MB 초과 앱 파일을 직접 커밋하지 않습니다.
- 새 버전을 배포할 때 `index.html`, `downloads.html`, `patch-notes.html`, `sitemap.xml`의 버전/날짜도 함께 갱신합니다.
