##hugo 블로그 설치
1. 블로그 설치 
   1. hugo new site be-formula
2. 테마 설치
   1. git submodule add https://github.com/fauzanmy/pehtheme-hugo.git themes/pehtheme
   2. hugo.toml 파일에 theme = 'pehtheme-hugo' 추가


Hugo + GitHub Pages로 무료 블로그 만드는 방법
개념 정리
Hugo
정적 사이트 생성기(Static Site Generator)
마크다운 등으로 글을 쓰면 HTML/CSS/JS로 된 정적 웹사이트를 만들어 줍니다.
특징: 빌드가 빠르고, 서버/DB 없이 파일만으로 동작합니다.
출력: public/ 폴더에 생성된 HTML, CSS, JS 파일들.
GitHub Pages
GitHub에서 제공하는 무료 정적 웹 호스팅.
https://<username>.github.io/<repo-name>/ 형태의 URL을 줍니다.
푸시한 브랜치(보통 main 또는 gh-pages)의 내용을 그대로 웹으로 서빙합니다.
함께 쓰는 방식
로컬에서 Hugo로 글 작성 → hugo로 빌드 → public/ 내용을 GitHub 저장소에 푸시 → GitHub Pages가 그 저장소를 호스팅 → 무료 도메인 블로그 완성.
작업 절차 (요약)
Hugo 설치
macOS: brew install hugo
공식 설치 가이드 참고.
GitHub 저장소 만들기
새 저장소 생성 (예: my-blog).
이 저장소가 블로그 소스 + (선택) 빌드 결과를 담는 곳이 됩니다.
Hugo 사이트 생성
터미널에서:
hugo new site my-blog && cd my-blog
테마 선택 후 적용 (예: git submodule add https://github.com/theNewDynamic/gohugo-theme-ananke.git themes/ananke, config.toml에 theme = "ananke" 추가).
글 작성
hugo new posts/hello-world.md
생성된 .md 파일을 편집해 내용 작성.
로컬에서 확인
hugo server -D
브라우저에서 http://localhost:1313 로 확인.
GitHub Pages에 배포
방법 A – 저장소 루트에 배포
public/ 내용을 해당 저장소의 main 브랜치 루트에 푸시.
GitHub 저장소 설정 → Pages → Source: main 브랜치, / (root) 선택.
방법 B – gh-pages 브랜치에 배포
public/ 내용만 gh-pages 브랜치에 푸시.
Pages 설정에서 Source: gh-pages 브랜치 선택.
도메인
기본: https://<username>.github.io/ (저장소 이름이 username.github.io이면)
또는 https://<username>.github.io/<repo-name>/
커스텀 도메인을 쓰려면 저장소에 CNAME 파일 추가 + DNS 설정.
디렉터리 구조 예시
my-blog/├── archetypes/├── content/          # 글(.md)들이 들어감│   └── posts/├── layouts/├── static/├── themes/├── config.toml       # 사이트 설정└── public/           # hugo 빌드 결과 (이걸 GitHub에 배포)
한 번에 정리한 체크리스트
단계	작업
1	Hugo 설치 (brew install hugo)
2	GitHub에 새 저장소 생성
3	hugo new site <폴더명> 후 해당 폴더로 이동
4	테마 추가 및 config.toml에 theme = "테마명" 설정
5	hugo new posts/첫글.md 후 글 작성
6	hugo로 빌드 → public/ 생성 확인
7	public/ 내용을 GitHub 저장소에 푸시
8	Repo → Settings → Pages에서 브랜치/폴더 설정
9	https://<username>.github.io/<repo>/ 로 접속 확인
원하면 지금 열려 있는 be-formula 프로젝트 안에 Hugo 사이트를 만들고, GitHub 저장소 연결까지 단계별로 같은 폴더 기준으로 구체적인 명령어까지 적어줄 수 있습니다.