# Oh Seong Jun | Learning & Creating

오성준의 한국어·영어 포트폴리오입니다. 개발 경험, 동아리 활동, 수상·자격증과 게임 프로젝트 Space Janitors, DDUCK을 소개합니다.

- [공개 사이트 (영어)](https://kotach-seong.github.io/myWebstudy/)
- [공개 사이트 (한국어)](https://kotach-seong.github.io/myWebstudy/ko/)

## 필요한 도구와 설치 (Windows x64)

저장소를 복제하고 배포용 HTML을 빌드하려면 **Git**, **Hugo Extended 0.167.0**, **Python 3.12**가 필요합니다. Git 2.55.0, Hugo Extended 0.167.0, Python 3.12.10으로 확인했습니다. 테마와 빌드된 Tailwind CSS는 저장소에 포함돼 있습니다. [스타일시트](site/themes/template/static/css/tailwind.css)

1. [Git for Windows 설치 프로그램](https://git-scm.com/downloads/win)을 실행하고 PowerShell을 새로 엽니다.
2. Python 3.12 Windows 64비트 설치 프로그램을 실행합니다. `Add python.exe to PATH`를 선택해 설치하고 PowerShell을 새로 엽니다. [Python 3.12.10 다운로드](https://www.python.org/downloads/release/python-31210/)
3. 아래 명령으로 Hugo의 Windows x64 ZIP을 내려받아 압축을 풀고 현재 PowerShell의 `PATH`에 추가합니다.

   ```powershell
   $hugoInstall = Join-Path $env:LOCALAPPDATA 'Programs\Hugo\0.167.0'
   $hugoZip = Join-Path $env:TEMP 'hugo_extended_0.167.0_windows-amd64.zip'
   Invoke-WebRequest -Uri 'https://github.com/gohugoio/hugo/releases/download/v0.167.0/hugo_extended_0.167.0_windows-amd64.zip' -OutFile $hugoZip
   Expand-Archive -LiteralPath $hugoZip -DestinationPath $hugoInstall -Force
   $env:Path = "$hugoInstall;$env:Path"
   git --version
   hugo version
   python --version
   ```

   `hugo version`에 `v0.167.0`과 `extended`가 표시되면 설치가 완료된 것입니다. 새 PowerShell에서는 `$hugoInstall`과 `$env:Path`를 설정하는 두 줄을 다시 실행합니다.

4. 저장소를 복제한 뒤 저장소 최상위 폴더로 이동합니다. 빌드 명령도 이 위치에서 실행합니다.

   ```powershell
   git clone https://github.com/kotach-seong/myWebstudy.git
   cd myWebstudy
   ```

## 배포용 HTML 빌드

저장소 최상위 폴더에서 GitHub Actions와 같은 빌드 스크립트를 실행합니다. 스크립트는 공개 주소를 기준으로 링크와 자산 경로를 맞춥니다. [빌드 스크립트](.github/pages_build.py)

```powershell
python .github/pages_build.py --build
```

빌드 결과는 저장소 최상위의 `public/`에 생성됩니다. 영어 페이지는 `public/index.html`, 한국어 페이지는 `public/ko/index.html`입니다. Actions는 Hugo Extended 0.167.0, Python 3.12, Node.js 24, Go 1.27.1, Dart Sass 1.105.0을 준비합니다. [배포 워크플로](.github/workflows/hugo-pages.yml)

## 수정할 파일

경로는 저장소 최상위 폴더 기준입니다.

| 수정할 내용 | 파일 위치 |
| --- | --- |
| 소개, 연락처, 경험, 동아리 활동, 수상·자격증, 프로젝트 정보 | [site/hugo.toml](site/hugo.toml)의 `[params]`는 기본 및 영어 설정이고, `[languages.ko.params]`와 `[[languages.ko.params.*]]`는 한국어 설정입니다. |
| 섹션 제목, 버튼 이름 등 한국어·영어 화면 문구 | [site/i18n/ko.toml](site/i18n/ko.toml), [site/i18n/en.toml](site/i18n/en.toml) |
| 메인 페이지 섹션 순서와 레이아웃 | [site/layouts/index.html](site/layouts/index.html) |
| 상단 메뉴, 언어·테마 전환, 하단 영역 | [site/layouts/partials/header.html](site/layouts/partials/header.html), [site/layouts/partials/footer.html](site/layouts/partials/footer.html) |
| 기술 스택 목록과 표시 방식 | [site/data/workwith.json](site/data/workwith.json), [site/layouts/partials/work-with.html](site/layouts/partials/work-with.html) |
| 프로필 사진과 프로젝트 스크린샷 | [site/assets/images/profile/avatar.jpg](site/assets/images/profile/avatar.jpg), [site/static/images/projects/](site/static/images/projects/) |

## 변경 커밋과 공개 배포 확인

1. 변경을 시작하기 전에 저장소 최상위 폴더에서 `git switch main`을 실행합니다. 파일을 저장한 뒤 위의 빌드 명령으로 영어와 한국어 HTML이 생성되는지 확인합니다.
2. `git status --short`와 `git diff`로 변경을 확인한 후 수정한 파일을 추가해 커밋하고 `main`에 푸시합니다. 아래는 `site/hugo.toml`을 수정한 경우의 예시입니다. 다른 파일을 수정했다면 `git add`에 그 경로를 적습니다.

   ```powershell
   git status --short
   git diff
   git add -- site/hugo.toml
   git commit -m "Update portfolio content"
   git rev-parse --short HEAD
   git push origin main
   ```

3. `main` 푸시 후 [GitHub Actions의 Deploy Hugo to GitHub Pages 실행 목록](https://github.com/kotach-seong/myWebstudy/actions/workflows/hugo-pages.yml)에서 방금 확인한 커밋 해시와 일치하는 실행을 엽니다. `build`와 `deploy`가 모두 성공했는지 확인합니다. 실패했다면 해당 작업의 로그를 엽니다.
4. 배포 성공 후 [공개 영어 화면](https://kotach-seong.github.io/myWebstudy/)과 [공개 한국어 화면](https://kotach-seong.github.io/myWebstudy/ko/)을 열어 수정한 섹션이 반영됐는지 확인합니다.

배포 설정은 저장소의 [Settings → Pages](https://github.com/kotach-seong/myWebstudy/settings/pages)에서 관리합니다. **Build and deployment → Source**는 **GitHub Actions**입니다.

## 테마 출처

Pranam Shetty의 [Hugo Noir](https://github.com/prxshetty/hugo-noir)를 바탕으로 제작했습니다. 테마 설명서는 [site/themes/template/README.md](site/themes/template/README.md), 라이선스는 [LICENSE](LICENSE)에 있습니다.
