# 로컬 실행 방법

**Git Bash**에서 아래 순서대로 실행합니다. 최초 설치 후에는 **4번**만 실행하면 됩니다.

## 1. Ruby 설치

```bash
winget.exe install --id RubyInstallerTeam.RubyWithDevKit.3.3 --exact --source winget --architecture x64 --interactive
```

설치 창에서:

- 기본 설치 경로 `C:\Ruby33-x64` 사용
- **Add Ruby executables to your PATH** 선택
- MSYS2/Devkit 포함 설치
- 마지막 `ridk install` 창이 열리면 `1 3` 입력 후 Enter

설치가 끝나면 **Git Bash를 완전히 닫았다가 다시 엽니다.** VS Code 내장 터미널이면 VS Code도 재시작합니다.

```bash
ruby -v
gem -v
```

버전이 출력되면 다음으로 진행합니다. 오류가 나면 아래의 **명령을 찾지 못할 때**를 확인합니다.

## 2. Devkit 설치 확인

설치 마지막에 MSYS2/Devkit 설치를 건너뛰었다면 Git Bash에서 실행합니다.

```bash
MSYS_NO_PATHCONV=1 cmd.exe /d /c "ridk install 1 3"
```

설치 확인:

```bash
MSYS_NO_PATHCONV=1 cmd.exe /d /c "ridk exec gcc --version"
MSYS_NO_PATHCONV=1 cmd.exe /d /c "ridk exec make --version"
```

두 명령 모두 버전이 출력되면 다음으로 진행합니다.

## 3. 프로젝트 패키지 설치

```bash
cd /c/project/minseok5408/minseok5408.github.io

gem install bundler -v 2.3.27 --no-document
bundle _2.3.27_ config set --local path vendor/bundle
bundle _2.3.27_ config set --local without test
bundle _2.3.27_ install
```

`Bundle complete!`가 나오면 설치 완료입니다. 오류가 나면 해결한 뒤 다음 단계로 진행합니다.

```bash
bundle _2.3.27_ check
bundle _2.3.27_ exec jekyll -v
```

## 4. 서버 실행

```bash
cd /c/project/minseok5408/minseok5408.github.io
bundle _2.3.27_ exec jekyll serve --host 127.0.0.1 --port 4000 --livereload
```

`Server running... press ctrl-c to stop.`이 출력되면 브라우저에서 접속합니다.

**<http://127.0.0.1:4000/>**

- 종료: 서버 터미널에서 **Ctrl+C**
- 다시 실행: 위 두 줄 재실행
- 글 수정: 저장하면 자동 빌드 및 새로고침
- `_config.yml` 수정: 서버 종료 후 재실행

명령으로 브라우저를 열려면 **새 Git Bash 창**에서 실행합니다.

```bash
start http://127.0.0.1:4000/
```

## 추가 실행 옵션

기존 서버를 Ctrl+C로 종료한 다음 사용합니다.

```bash
# 초안과 미래 날짜 글 포함
bundle _2.3.27_ exec jekyll serve --host 127.0.0.1 --port 4000 --livereload --drafts --future

# 4000 포트가 사용 중이면 4001로 실행
bundle _2.3.27_ exec jekyll serve --host 127.0.0.1 --port 4001 --livereload --livereload-port 35730

# 파일 변경을 감지하지 못할 때
bundle _2.3.27_ exec jekyll serve --host 127.0.0.1 --port 4000 --livereload --force_polling

# 서버 없이 빌드만 실행. 결과는 _site/에 생성
bundle _2.3.27_ exec jekyll build
```

4001 포트로 실행했다면 <http://127.0.0.1:4001/>로 접속합니다.

## Git에 올릴 파일

글(`_posts/`), 정보 페이지(`_tabs/`), 설정(`_config.yml`), 테마 소스와 이미지 등 직접 수정한 파일을 커밋합니다.
`Gemfile.lock`과 `package-lock.json`도 설치 버전을 맞추는 데 필요하므로 변경 시 함께 커밋합니다.

`_site/`는 실행할 때마다 빌드 시각과 캐시 번호 등이 바뀌는 자동 생성 결과입니다. GitHub Actions가 배포 시 다시 생성하므로 커밋하지 않습니다.
`node_modules/`, `vendor/`, Jekyll 캐시와 `.DS_Store`도 `.gitignore`로 제외합니다.
이미 등록됐던 파일의 추적을 해제하면 한 번은 Git 변경 목록에 삭제로 표시됩니다. 이 변경을 커밋하면 이후부터 제외되며, 로컬 파일은 그대로 유지됩니다.

`assets/js/dist/`와 `_sass/vendors/`의 테마 자산은 현재 배포 과정에서 그대로 사용하므로 계속 커밋합니다.

## 테마 JS / Bootstrap 자산을 수정할 때만

**글 작성과 기존 블로그 실행에는 생략합니다.** 저장소에 빌드된 자산이 포함되어 있습니다.

Node.js가 없다면 Git Bash에서 설치합니다.

```bash
winget.exe install --id OpenJS.NodeJS.LTS --exact --source winget
```

터미널과 터미널을 띄운 앱을 재시작한 뒤 실행합니다.

```bash
cd /c/project/minseok5408/minseok5408.github.io
node -v
npm -v
npm ci
npm run build
```

Node.js는 LTS를 사용합니다. 현재 잠금 파일의 개발 도구에는 최소 20.8.1이 필요합니다.
`npm ci`는 기존 `node_modules`를 다시 구성하고, `npm run build`는 JS/Bootstrap 자산을 재생성합니다.
빌드가 끝나면 **4번**으로 서버를 실행합니다.

## 명령을 찾지 못할 때

Ruby를 기본 경로에 설치했다면 Git Bash에서 실행합니다. 다른 경로에 설치했으면 해당 경로로 바꿉니다.

```bash
ls /c/Ruby33-x64/bin/ruby.exe
export PATH="/c/Ruby33-x64/bin:$PATH"
hash -r
ruby -v
gem -v
```

`ruby.exe`가 없으면 **1번** 설치부터 완료합니다.
위 PATH 설정은 현재 창에만 적용됩니다. 계속 사용하려면 Windows **환경 변수 → 사용자 변수 → Path**에 `C:\Ruby33-x64\bin`을 추가하고 터미널을 다시 엽니다.

`bundle`만 없다면 다음을 실행합니다.

```bash
gem install bundler -v 2.3.27 --no-document
bundle _2.3.27_ --version
```

여전히 `bundle`만 찾지 못하면 `gem environment`의 `EXECUTABLE DIRECTORY` 경로도 PATH에 추가합니다.

## Git Bash에서 설치 명령이 안 될 때만: PowerShell

PowerShell을 열고 **실패했던 단계만** 실행합니다.

```powershell
# Ruby 설치
winget install --id RubyInstallerTeam.RubyWithDevKit.3.3 --exact --source winget --architecture x64 --interactive
```

Ruby 설치 후에는 PowerShell도 닫았다가 다시 열고 실행합니다.

```powershell
# Devkit 설치 및 확인
ridk install 1 3
ridk exec gcc --version
ridk exec make --version
```

Node.js 설치가 필요한 경우:

```powershell
winget install --id OpenJS.NodeJS.LTS --exact --source winget
```

완료 후 **Git Bash를 새로 열고 원래 단계부터** 이어갑니다.
PowerShell에서도 `winget`이 없으면 [RubyInstaller](https://rubyinstaller.org/downloads/)에서 **Ruby+Devkit 3.3.x (x64)**를 설치합니다. Node.js가 필요한 경우 [Node.js](https://nodejs.org/)에서 LTS를 설치합니다.

설치 참고: [Jekyll Windows 설치 안내](https://jekyllrb.com/docs/installation/windows/).
