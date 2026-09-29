# HELIOS HTML 실행 / GitHub Pages 업로드

## 1. 지금 내 컴퓨터에서 열기

`github-pages/index.html`을 더블클릭하세요. Node.js, 터미널, 로컬 서버, 인터넷 연결 없이 연구실 페이지와 논문 탐색기를 사용할 수 있습니다. 논문 원문·이메일·외부 프로필 링크를 열 때만 네트워크/해당 앱이 필요합니다.

폴더 구조를 유지하세요:

```text
index.html              ← 첫 화면 (한국어)
assets/                 ← CSS, JavaScript, 아이콘
en/                     ← 영문 페이지별 index.html
ko/                     ← 국문 페이지별 index.html
.nojekyll
404.html
robots.txt
sitemap.xml
```

논문 필터는 `?year=2022&area=sensing`처럼 주소에 저장됩니다. 페이지는 해시 기반 가상 주소 대신 실제 HTML 파일로 이동하므로 직접 열기, 새로고침, 뒤로 가기가 동작합니다. 복사 권한이 없는 브라우저에서는 BibTeX 텍스트가 나타나 수동으로 복사할 수 있습니다.

## 2. 가장 간단한 GitHub 업로드 방법

1. `HELIOS-GitHub-Pages.zip`을 압축 해제합니다.
2. GitHub에 저장소를 만들거나 사용할 저장소를 엽니다.
3. **압축을 푼 폴더 안의 내용 전체**를 저장소 최상위에 올립니다. GitHub 파일 목록의 최상위에서 `index.html`과 `assets`, `en`, `ko`가 보여야 합니다. ZIP 파일 자체를 올리는 것이 아닙니다.
4. 저장소 **Settings → Pages → Build and deployment**에서 **Source: Deploy from a branch**, **Branch: main**, **Folder: /(root)**를 선택하고 Save 합니다.
5. 배포가 끝나면 같은 Pages 화면의 Visit site를 누릅니다.

`사용자명.github.io/저장소명/` 형태와 사용자 홈페이지의 최상위 도메인 모두 대응합니다. 저장소 이름을 코드에 하드코딩하지 않아 별도의 경로 수정이 필요 없습니다. 모든 자산·내부 링크는 상대경로입니다.

업로드할 파일은 배포 ZIP에 담긴 파일들입니다. 원본 CV나 작업 폴더 전체를 이 방법으로 업로드할 필요가 없습니다.

## 3. 콘텐츠 변경

원본 프로젝트의 `content/*.json`을 수정한 다음:

```powershell
Set-Location -LiteralPath 'C:\Users\user\Desktop\Lab\[2] IoT\helios'
npm ci
npm run build:static
```

갱신된 `github-pages` 내용 전체를 다시 업로드합니다. 결과 HTML을 직접 수정하면 다음 빌드에서 덮어써집니다. Next.js와 HTML 버전은 동일한 React 컴포넌트와 연구 데이터를 사용합니다.

## 4. 자동 배포를 원하는 경우 (소스 저장소 방식)

`helios` 프로젝트의 내용을 GitHub 저장소 최상위에 올립니다. `package.json`, `package-lock.json`, `src`, `content`, `scripts`, `public`, `.github` 등이 포함되어야 하며 `node_modules`는 제외합니다. 이 경우 Pages의 Source는 **GitHub Actions**로 선택하세요.

`.github/workflows/pages.yml`이 main push 시 의존성 설치 → 데이터 검증 → HTML 생성 → Pages 배포를 수행합니다. 저장소의 실제 Pages 주소를 빌드에 전달해 canonical URL과 sitemap도 생성합니다. 직접 업로드하는 2번 방식과 혼동하지 마세요.

공식 주소를 알고 로컬에서 빌드하려면 `SITE_URL`을 실제 공개 사이트 주소로 지정한 뒤 `npm run build:static`을 실행합니다. 주소를 지정하지 않아도 사이트는 정상 동작하며 canonical은 생략되고 sitemap은 빈 목록입니다. 정적 배포본은 검색 차단 설정을 넣지 않습니다.

## 검증

`npm run test:static`: 데스크톱·모바일의 로컬 file:// 실행 및 임의의 저장소 하위 주소 HTTP 실행을 검증합니다. 검색·복합 필터·새로고침·초기화·연구 지도 연결·언어·테마·복사 실패 대체 UI·접근성·가로 넘침을 확인합니다. 실제 GitHub 계정에 원격 업로드한 것은 아닙니다.

GitHub 공식 안내: https://docs.github.com/en/pages/getting-started-with-github-pages/creating-a-github-pages-site
자동 배포 안내: https://docs.github.com/en/pages/getting-started-with-github-pages/using-custom-workflows-with-github-pages
