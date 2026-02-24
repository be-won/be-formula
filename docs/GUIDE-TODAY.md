# 오늘 작업 가이드 — 포스트 작성 + 헤더·푸터 한글 디자인

어제 작업에 이어서 **실제 포스트 작성·업로드**와 **한글 기반 레이아웃(헤더·푸터) 커스터마이징** 연습을 위한 단계별 가이드입니다.

---

## 현재 프로젝트 상태 (스캔 요약)

| 항목 | 상태 |
|------|------|
| **사이트** | Hugo + pehtheme-hugo, `hugo.toml`에 `languageCode: ko-kr`, `title: black formula` |
| **콘텐츠** | `content/posts/`에 샘플 10편 + `sample-01-static-sites-with-hugo.md` 등 |
| **Archetype** | `archetypes/posts.md` — SEO·이미지·카테고리 포함 템플릿 |
| **테마** | `themes/pehtheme-hugo` (TailwindCSS, submodule) |
| **레이아웃** | 프로젝트 루트에 `layouts/` 없음 → 전부 테마 파일 사용 중 |
| **헤더** | `themes/.../partials/header.html` — 사이트 제목(.Site.Title), 메뉴, 검색 |
| **푸터** | `themes/.../partials/footer.html` — 영문 문구, 소셜 링크, 저작권 |
| **메뉴** | 프로젝트 `hugo.toml`에 `[menu]` 없음 → 테마 기본 메뉴 또는 비어 있을 수 있음 |

**원칙**: 테마 파일은 수정하지 않고, **프로젝트 루트**에 같은 경로로 파일을 만들어 **덮어쓰기(override)** 합니다.

---

## Part 1. 실제 포스트 작성·업로드 연습

### 1.1 새 포스트 파일 만들기

```bash
cd /Volumes/WORKS/dev/be-formula
hugo new content/posts/오늘-첫-포스트.md
```

- `archetypes/posts.md` 템플릿이 적용되어 front matter가 자동 생성됩니다.
- 파일명에 한글·공백 사용 가능; URL은 `slug` 또는 파일명 기반으로 생성됩니다.

### 1.2 Front matter 수정 (필수)

생성된 파일 상단을 다음처럼 맞춥니다.

```yaml
---
title: "오늘의 첫 포스트 제목"
description: "한 줄 요약 (150자 내외, 검색 결과에 노출)"
date: 2026-02-24T10:00:00+09:00
slug: "first-post-today"
draft: false

# 선택: 대표 이미지 (assets/images/에 파일 두고 경로만)
# image: "images/og-image.jpg"
# imageCaption: "캡션 또는 출처"

categories: ["web"]
tags: ["hugo", "연습"]
---
```

- **공개**: 반드시 `draft: false`.
- **이미지**: 테마는 `resources.Get` 사용 → `assets/images/`에 넣고 `image: "images/파일명.jpg"` 형식.

### 1.2a Obsidian + Templater로 글 쓴 뒤 content/posts 에 넣기

Obsidian에서 Templater로 **상단 속성(프론트매터)**을 넣고, 아래에 마크다운 본문을 작성한 뒤, 그 `.md` 파일을 `content/posts/`에 두면 Hugo가 **그대로** 인식합니다. Obsidian 속성은 `---` YAML 형식이라 Hugo와 동일합니다.

**주의:**

- archetypes의 `{{ .Date }}`, `{{ replace .File.ContentBaseName ... }}` 는 **Hugo 전용** 문법이라, 이미 있는 콘텐츠 파일 안에서는 처리되지 않습니다. Obsidian 템플릿에서는 **Templater 문법**으로 바꿔야 합니다.
- **Templater용 템플릿** 예시(날짜·제목 자동 치환): `docs/obsidian-templater-post.md`  
  → 이 파일을 Obsidian 템플릿 폴더에 복사해 두고, 새 노트 생성 시 Templater로 불러오면 됩니다.
- 배포 전에 **`draft: false`** 로 수정하고, 필요하면 `slug`에 영문 경로(예: `slug: "my-first-post"`)를 넣어 URL을 정리하세요.

### 1.3 본문 작성 (마크다운)

`---` 아래부터 일반 마크다운으로 작성합니다.

```markdown
## 소제목

본문 내용. **굵게**, *기울임*, [링크](https://example.com).

- 목록
- 항목

본문 안 이미지는 `static/images/`에 넣고:
![설명](/images/파일명.jpg)
```

- **대표 이미지(썸네일)**: `assets/images/` + front matter `image`
- **본문 안 이미지**: `static/images/` + `![](/images/...)`

### 1.4 로컬 확인

```bash
hugo server -D
```

- 브라우저: http://localhost:1313  
- 홈·글 목록·해당 글 페이지에서 표시 확인.

### 1.5 배포 (GitHub Pages)

```bash
git add content/posts/오늘-첫-포스트.md
# 이미지 썼다면 assets/images/ 또는 static/images/ 도 추가
git commit -m "Add post: 오늘의 첫 포스트"
git push origin main
```

- GitHub Actions 빌드 후 `https://be-won.github.io/be-formula/` 에 반영됩니다.

---

## Part 2. 레이아웃·디자인 — 한글 사이트, 헤더·푸터 먼저

### 2.1 메뉴 한글화 (hugo.toml)

헤더 메뉴를 한글로 쓰려면 프로젝트 `hugo.toml`에 메뉴를 정의합니다.

**파일**: `hugo.toml` (프로젝트 루트)

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

- `name`을 한글로 넣으면 헤더·푸터 메뉴에 그대로 표시됩니다.

### 2.2 푸터 덮어쓰기 (한글 문구·디자인)

1. **경로 생성 및 복사**

```bash
mkdir -p /Volumes/WORKS/dev/be-formula/layouts/partials
cp /Volumes/WORKS/dev/be-formula/themes/pehtheme-hugo/layouts/partials/footer.html \
   /Volumes/WORKS/dev/be-formula/layouts/partials/footer.html
```

2. **수정할 내용** (`layouts/partials/footer.html`)

- "Write with love. Minimalism design." → 한글 문구로 변경 (예: "간결한 디자인으로 기록합니다.")
- "Lorem ipsum..." → 블로그 소개 한 줄로 변경 또는 삭제
- "Copyright © ... All rights reserved. Insertapps.com" → "© 2026 black formula" 등으로 변경
- 링크를 본인 블로그/소셜로 바꾸기 (선택)

푸터 안에 있는 **소셜 링크**는 `partial "content/social-media.html"` 로 불러옵니다.  
한글 사이트에 맞게 바꾸려면 이 partial도 덮어쓰기:

```bash
mkdir -p /Volumes/WORKS/dev/be-formula/layouts/partials/content
cp /Volumes/WORKS/dev/be-formula/themes/pehtheme-hugo/layouts/partials/content/social-media.html \
   /Volumes/WORKS/dev/be-formula/layouts/partials/content/social-media.html
```

그 다음 `layouts/partials/content/social-media.html` 에서 URL(`#` 부분)을 본인 링크로 수정하거나, 불필요하면 항목을 줄일 수 있습니다.

### 2.3 헤더 덮어쓰기 (선택 — 문구·스타일만)

사이트 제목은 이미 `{{ .Site.Title }}`을 쓰므로 `hugo.toml`의 `title = 'black formula'`만 바꿔도 됩니다.

- **로고/버튼 문구**, **검색 버튼** 등 텍스트를 한글로 바꾸고 싶다면 헤더를 덮어쓰기:

```bash
cp /Volumes/WORKS/dev/be-formula/themes/pehtheme-hugo/layouts/partials/header.html \
   /Volumes/WORKS/dev/be-formula/layouts/partials/header.html
```

- `layouts/partials/header.html` 에서 버튼 옆 라벨 등 필요한 부분만 한글로 수정합니다. (테마에 라벨이 없으면 생략 가능)

### 2.4 한글 폰트·타이포 (선택)

한글 가독성을 위해 본문/제목에 나눔고딕 등 적용하려면:

1. **프로젝트에 CSS 추가**  
   - `assets/css/custom.css` 또는 `static/css/custom.css` 생성  
   - 예: `font-family: 'Nanum Gothic', sans-serif;` 적용

2. **head에서 로드**  
   - 테마의 `layouts/partials/head.html` 또는 `head/css.html`를 프로젝트에서 덮어쓴 뒤, 위 custom CSS를 한 줄로 링크  
   - 또는 테마가 지원하는 `custom_css` 같은 param이 있다면 `hugo.toml`에서 지정

3. **폰트 로드**  
   - Google Fonts 등에서 한글 폰트 링크를 `head.html` 덮어쓴 partial에 추가

(우선 헤더·푸터 문구와 메뉴 한글화만 해도 “한글 기반 사이트” 느낌을 낼 수 있습니다.)

---

## 권장 작업 순서 (오늘)

| 순서 | 작업 | 확인 |
|------|------|------|
| 1 | `hugo new content/posts/오늘-첫-포스트.md` 후 front matter·본문 작성 | `hugo server -D` 로 로컬 확인 |
| 2 | `hugo.toml`에 `[menu]` 추가해 메뉴 한글화 (홈, 글 목록, 태그) | 로컬에서 헤더 메뉴 한글 표시 확인 |
| 3 | `layouts/partials/footer.html` 복사 후 한글 문구·저작권·링크 수정 | 로컬에서 푸터 확인 |
| 4 | (선택) `layouts/partials/content/social-media.html` 복사 후 소셜 링크 수정 | 푸터 소셜 영역 확인 |
| 5 | (선택) 헤더 덮어쓰기 후 버튼/라벨 한글화 | 로컬에서 헤더 확인 |
| 6 | `git add` → `commit` → `push` | GitHub Pages에서 반영 확인 |

이 순서대로 하면 **실제 포스트 업로드**와 **한글 헤더·푸터 디자인 변경** 연습을 한 번에 진행할 수 있습니다.  
이미지·한글 폰트는 여유 있을 때 추가해도 됩니다.

---

## 참고

- **포스트·이미지 상세**: `docs/GUIDE.md` 2장, 2.6절
- **테마 덮어쓰기 원칙**: `docs/GUIDE.md` 3장
- **Archetype 전체 필드**: `archetypes/posts.md`
- **샘플 글 구조**: `content/posts/sample-01-static-sites-with-hugo.md`
