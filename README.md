# Oh Seong Jun | Learning & Creating

오성준의 한국어·영어 포트폴리오입니다. 개발 경험, 동아리 활동, 수상·자격증과 게임 프로젝트 Space Janitors, DDUCK을 소개합니다.

- [공개 사이트 (영어)](https://kotach-seong.github.io/myWebstudy/)
- [공개 사이트 (한국어)](https://kotach-seong.github.io/myWebstudy/ko/)

## 필요한 도구와 설치 (Windows x64)

로컬 미리보기와 빌드에는 **Git**과 **Hugo Extended 0.167.0**을 사용합니다. 아래 명령은 Git 2.55.0과 Hugo Extended 0.167.0으로 확인했습니다. 테마와 [빌드된 Tailwind CSS](site/themes/template/static/css/tailwind.css)는 저장소에 포함돼 있어 로컬 실행에는 이 두 도구만 필요합니다.

1. [Git for Windows 설치 프로그램](https://git-scm.com/downloads/win)을 실행하고 PowerShell을 새로 엽니다.
2. 아래 명령으로 Hugo의 Windows x64 ZIP을 내려받아 압축을 풀고 현재 PowerShell의 `PATH`에 추가합니다.

   ```powershell
   $hugoInstall = Join-Path $env:LOCALAPPDATA 'Programs\Hugo\0.167.0'
   $hugoZip = Join-Path $env:TEMP 'hugo_extended_0.167.0_windows-amd64.zip'
   Invoke-WebRequest -Uri 'https://github.com/gohugoio/hugo/releases/download/v0.167.0/hugo_extended_0.167.0_windows-amd64.zip' -OutFile $hugoZip
   Expand-Archive -LiteralPath $hugoZip -DestinationPath $hugoInstall -Force
   $env:Path = "$hugoInstall;$env:Path"
   git --version
   hugo version
   ```

   `hugo version`에 `v0.167.0`과 `extended`가 표시되면 설치가 완료된 것입니다. 새 PowerShell에서는 `$hugoInstall`과 `$env:Path`를 설정하는 두 줄을 다시 실행합니다.

3. 저장소를 복제한 뒤 저장소 최상위 폴더로 이동합니다. 아래의 실행·빌드 명령도 이 위치에서 실행합니다.

   ```powershell
   git clone https://github.com/kotach-seong/myWebstudy.git
   cd myWebstudy
   ```

## 로컬 미리보기와 빌드

```powershell
hugo server --source site --config hugo.toml,pages.json --baseURL http://127.0.0.1:1315/ --bind 127.0.0.1 --port 1315 --renderToMemory
```

브라우저에서 [영어 화면](http://127.0.0.1:1315/)과 [한국어 화면](http://127.0.0.1:1315/ko/)을 엽니다. 파일을 저장하면 개발 서버가 다시 빌드합니다. 서버는 `Ctrl+C`로 종료합니다. 포트가 사용 중이면 `--baseURL`과 `--port`에서 같은 번호로 바꿉니다.

정적 파일만 빌드하려면 저장소 최상위 폴더에서 실행합니다.

```powershell
hugo --source site --config hugo.toml,pages.json --baseURL http://127.0.0.1:1315/ --destination ../public
```

빌드 결과는 저장소 최상위의 `public/`에 생성됩니다. 영어 페이지는 `public/index.html`, 한국어 페이지는 `public/ko/index.html`입니다. 배포 빌드는 [GitHub Actions 워크플로](.github/workflows/hugo-pages.yml)가 담당합니다. 워크플로는 Hugo Extended 0.167.0, Python 3.12, Node.js 24, Go 1.27.1, Dart Sass 1.105.0을 준비합니다.

## 테마 출처

Pranam Shetty의 [Hugo Noir](https://github.com/prxshetty/hugo-noir)를 바탕으로 제작했습니다. 테마 설명서는 [site/themes/template/README.md](site/themes/template/README.md), 라이선스는 [LICENSE](LICENSE)에 있습니다.
