# be-formula 전체 점검 결과

점검 일자: 2026-02-24 기준

---

## 1. 오류·불일치

### 1.1 메뉴 설정 없음 (`hugo.toml`)
- **현상**: `[menu]` 설정이 없음. 헤더/푸터 메뉴가 테마 기본(영문 Home, Posts, Tags)이거나 비어 있을 수 있음.
- **조치**: 한글 메뉴(홈, 글 목록, 태그)를 쓰려면 `hugo.toml`에 추가.
  ```toml
  [menu]
    [[menu.main]]
      name = '홈'
      pageRef = '/'
      weight = 10
    [[menu.main]]
      name = '글 목록'
      pageRef = '/posts'
      weight = 20
    [[menu.main]]
      name = '태그'
      pageRef = '/tags'
      weight = 30
  ```

### 1.2 목록 페이지 개수 불일치
- **현상**: `layouts/_default/list.html` 13행에 **`.Paginator 6`** 하드코딩. `hugo.toml`의 `pagerSize = 9`와 불일치.
- **결과**: `/posts/`(View All)에서 **6개씩**만 표시됨.
- **조치**: `list.html`에서 `( .Paginator 6 )` → `( .Paginator )` 로 바꾸면 전역 `pagerSize = 9` 적용. 또는 `( .Paginator site.Params.pagerSize )` 등으로 통일.

### 1.3 홈 에디터 픽 기본값
- **현상**: `layouts/_default/home.html` 41행 `default "best"` → 설정에는 `homeTag = 'ed-pick'`이 있어 실제로는 `ed-pick` 사용 중. 주석/폴백만 예전 값.
- **조치**: 혼동 방지를 위해 `default "ed-pick"` 로 변경 권장.

### 1.4 헤더 검색
- **현상**: `layouts/partials/search.html`에 input **`name` 없음**, 검색 로직 없음. 제출 시 URL에 `?`만 붙고 검색되지 않음.
- **조치**: (이전에 적용했다가 undo된 상태라면) input에 `name="q"`, form `submit` 시 `preventDefault` + 클라이언트 검색 또는 외부 검색 연동 필요.

---

## 2. 불필요·정리 권장

### 2.1 `public/` 폴더
- **현상**: `.gitignore`에 `/public/` 있으나, 디스크에 `public/`이 있음(로컬 빌드 또는 예전 pull).
- **조치**: 저장소에서 추적 중이면 `git rm -r --cached public/` 후 커밋·푸시하면 이후 pull 시 내려오지 않음. 로컬에서는 `hugo` 시 다시 생성되므로 삭제해도 됨.

### 2.2 `docs/` 파일
- `GUIDE.md`, `GUIDE-TODAY.md`, `rule.md`: 참고용이면 유지. 사용하지 않으면 정리 가능.
- `chat-transcript-be-formula.md`: 대화 기록. 보관용이면 유지.

---

## 3. 누락

### 3.1 GitHub Actions 워크플로
- **현상**: `.github/workflows/` 디렉터리 없음.
- **결과**: push만으로는 자동 빌드·배포되지 않음. GitHub Pages에 최신이 반영되려면 워크플로 필요.
- **조치**: Hugo 빌드 후 `gh-pages` 또는 `main`의 특정 폴더에 배포하는 워크플로 YAML 추가. (예: `actions/checkout` + `peaceiris/actions-hugo` 등.)

---

## 4. 정상으로 보이는 항목

| 항목 | 상태 |
|------|------|
| `hugo.toml` baseURL, theme, params | 정상 |
| `config/dev/hugo.toml` (로컬 baseURL) | 정상 |
| `layouts/_default/home.html` (에디터 픽, 최신글, View All) | 정상 |
| `layouts/_default/terms.html` (에디터 픽/인기글 한글 제목) | 정상 |
| `layouts/_default/baseof.html` | 정상 (smooth 제거된 상태) |
| `layouts/partials/` (header, footer, menu, search, terms) | 오버라이드 존재, terms는 단일 글의 태그 목록용 |
| `themes/pehtheme-hugo` submodule | .gitmodules 정상 |
| `.gitignore` (/public/, .DS_Store 등) | 적절함 |
| 인기글 섹션 | 주석 처리로 비노출 상태 (의도된 것 같음) |

---

## 5. 요약 체크리스트

- [x] `hugo.toml`에 `[menu]` 추가 (한글 메뉴) — **적용함**
- [x] `layouts/_default/list.html`에서 Paginator 6 → 전역 설정 통일 — **적용함**
- [x] `layouts/_default/home.html`에서 default "best" → "ed-pick" 변경 — **적용함**
- [ ] 검색 수정 여부 결정 (사용자 요청으로 제외)
- [x] `public/` 저장소 추적 제거 — **이미 추적 중이 아님** (필요 시 수동 실행)
- [x] `.github/workflows/hugo-pages.yml` 추가 (GitHub Pages 자동 배포) — **적용함**
