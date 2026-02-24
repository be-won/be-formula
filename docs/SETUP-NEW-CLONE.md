# 새 환경에서 클론 후 작업 시작 가이드

새 PC/드라이브에서 저장소를 클론한 뒤 로컬 작업 환경을 맞추는 절차입니다.

---

## 1. 현재 저장소 스캔 요약

### 1.1 폴더 구조

```
be-formula/
├── .github/workflows/hugo-pages.yml   # GitHub Actions: push 시 빌드 → GitHub Pages 배포
├── .gitignore
├── .gitmodules                        # 테마 서브모듈 (themes/pehtheme-hugo)
├── archetypes/                        # 새 글 템플릿
├── config/dev/hugo.toml               # 로컬 개발 시 baseURL (localhost)
├── content/posts/                     # 블로그 글(.md)
├── hugo.toml                         # 사이트 설정, theme = 'pehtheme-hugo'
├── layouts/                          # 테마 오버라이드 (헤더, 푸터, 홈, 목록 등)
├── themes/pehtheme-hugo/             # 서브모듈 — 클론 직후엔 비어 있음
├── docs/                             # 가이드·점검 문서
└── README.md
```

### 1.2 .gitignore로 보는 “저장소에 없는 것”

| 항목 | 설명 | 새 환경에서 필요한 조치 |
|------|------|-------------------------|
| `/public/` | Hugo 빌드 결과물 | 없어도 됨. `hugo` 실행 시 생성됨 |
| `.hugo_build.lock` | 빌드 락 파일 | 자동 생성 |
| `node_modules/` | Node 의존성 | **테마를 Tailwind로 커스터마이징할 때만** exampleSite의 package.json 등 복사 후 `npm install` |
| `.DS_Store`, `.idea/`, `.vscode/` 등 | OS/에디터 | 무시해도 됨 |

**정리**: 이 프로젝트는 테마를 서브모듈로만 쓰고, Tailwind 커스터마이징용 Node 설정은 저장소에 없음. **Hugo만 있으면 빌드·로컬 서버 가능.**

---

## 2. 필수 작업 체크리스트 (새 환경에서 클론 후)

### 2.1 Git 서브모듈 초기화 (필수)

클론 시 `git clone <url>` 만 하면 서브모듈 디렉터리(`themes/pehtheme-hugo`)는 **비어 있습니다**.  
테마 파일을 받으려면 반드시 한 번 실행:

```bash
cd /Volumes/WORKS/dev/be-formula
git submodule update --init --recursive
```

- `themes/pehtheme-hugo` 안에 테마 소스가 채워져야 Hugo가 정상 빌드됩니다.
- 앞으로 클론할 때 서브모듈까지 받으려면:  
  `git clone --recurse-submodules https://github.com/be-won/be-formula.git`

### 2.2 Hugo 설치 (필수)

- **macOS**: `brew install hugo`
- 현재 환경에서는 이미 설치됨: `hugo v0.156.0+extended` (확인: `hugo version`)

### 2.3 로컬에서 사이트 확인

```bash
cd /Volumes/WORKS/dev/be-formula
hugo server -D --environment dev
```

- `--environment dev` → `config/dev/hugo.toml` 적용 (baseURL `http://localhost:1313/`)
- 브라우저: http://localhost:1313

### 2.4 (선택) 테마 Tailwind 커스터마이징

[pehtheme-hugo](https://github.com/fauzanmy/pehtheme-hugo) 문서에 따르면, 테마 스타일을 바꾸려면:

1. `exampleSite/`의 `package.json`, `postcss.config.js`, `tailwind.config.js`를 프로젝트 루트로 복사
2. `exampleSite/input.css` → 프로젝트 `assets/input.css`로 복사
3. `npm install` 후 `npm run dev` / `npm run build`

지금은 **테마 기본 그대로** 쓰면 되므로 생략 가능.

---

## 3. 배포 흐름 (복습)

1. 로컬에서 글 작성/수정 (`content/posts/*.md`, `layouts/` 등)
2. `hugo server -D --environment dev` 로 확인
3. `git add` → `git commit` → `git push origin main`
4. GitHub Actions(`.github/workflows/hugo-pages.yml`)가 자동으로:
   - `actions/checkout` with `submodules: recursive` 로 저장소+테마 체크아웃
   - Hugo 빌드 (`hugo --minify`)
   - GitHub Pages에 배포
5. 결과: https://be-won.github.io/be-formula/

---

## 4. 현재 상태 요약 (2026-02-24 기준)

| 항목 | 상태 |
|------|------|
| **서브모듈** | `.gitmodules` 등록됨. **클론 직후엔 `themes/pehtheme-hugo` 비어 있음** → `git submodule update --init --recursive` 필요 |
| **Hugo** | 설치됨 (v0.156.0+extended) |
| **GitHub Actions** | `hugo-pages.yml` 존재, submodules: recursive 사용 |
| **hugo.toml** | theme = 'pehtheme-hugo', 한글 메뉴·params 설정됨 |
| **config/dev** | 로컬 baseURL 설정됨 |

---

## 5. 지금 당장 할 일 (한 줄 요약)

1. **터미널에서 서브모듈 초기화**  
   `git submodule update --init --recursive`
2. **로컬 서버로 확인**  
   `hugo server -D --environment dev`  
   → http://localhost:1313 에서 블로그 확인
3. (이미 했다면) 글 쓰고 → push → GitHub Actions로 자동 배포 확인

이후 다른 컴퓨터에서 다시 클론할 때는  
`git clone --recurse-submodules <repo-url>` 로 clone 하면 서브모듈까지 한 번에 받을 수 있습니다.
