# be-formula 블로그 가이드

## 0. SEO 템플릿과 샘플 글

- **Archetype**: `archetypes/posts.md` — 새 글 생성 시 사용되는 SEO 친화적 front matter 템플릿 (Google 인덱싱·클릭률 고려).
- **샘플 글**: `content/posts/sample-01-*.md` ~ `sample-10-*.md` — 동일한 속성 구조로 작성된 예시 10편. 참고용으로 활용하거나 수정·삭제해도 됨.

---

## 1. 포스트 상태 관리 (Front Matter)

각 글의 **상단 속성 영역**(front matter)에서 상태와 메타데이터를 관리합니다.

```yaml
---
title: "글 제목"
date: 2026-02-23T20:34:55+09:00
draft: false    # true = 초안(비공개), false = 공개
# 테마에서 지원하면 추가 가능:
# description: "요약 문구"
# tags: [태그1, 태그2]
# categories: [카테고리]
---
```

- **draft**: `true`면 빌드 시 제외(비공개), `false`면 사이트에 노출.
- **date**: 정렬·표시용. 배포 후에는 수정해도 됨.

---

## 2. 실제 포스트 올리는 방법 (연습)

### 2.1 새 글 파일 만들기

```bash
# 프로젝트 루트(be-formula)에서
hugo new content/posts/첫-포스트.md
```

또는 직접 `content/posts/내글.md` 파일을 만들어도 됩니다.

### 2.2 Front matter 수정

생성된 파일 상단을 다음처럼 맞춥니다.

```yaml
---
title: "첫 포스트 제목"
date: 2026-02-23T21:00:00+09:00
draft: false
---

본문은 여기부터 마크다운으로 작성합니다.
```

- **공개하려면 반드시** `draft: false` 로 설정.
- 제목·날짜는 원하는 대로 수정.

### 2.3 본문 작성 (마크다운)

```markdown
## 소제목

일반 문단. **굵게**, *기울임*, [링크](https://example.com).

- 목록
- 항목

```코드```
```

### 2.4 로컬에서 확인

```bash
# 초안(draft) 포함해서 보기
hugo server -D

# 브라우저: http://localhost:1313
```

### 2.5 배포 (GitHub Pages)

```bash
git add content/posts/첫-포스트.md
git commit -m "Add post: 첫 포스트"
git push origin main
```

푸시 후 GitHub Actions가 빌드하고, 몇 분 뒤 https://be-won.github.io/be-formula/ 에 반영됩니다.

---

## 2.6 이미지 업로드 및 문서에 넣는 방법 (Pehtheme)

[Pehtheme 데모](https://pehtheme-hugo.netlify.app/)처럼 글마다 대표 이미지를 쓰려면 아래를 따르면 됩니다.

### 이미지를 넣을 위치

| 위치 | 용도 | front matter 예시 |
|------|------|-------------------|
| **`assets/images/`** | 글 대표 이미지(썸네일·피처 이미지) | `image: "images/제목.jpg"` |
| **`static/images/`** | 그냥 URL로만 쓸 이미지 (테마가 `resources.Get`을 쓰면 **assets** 권장) | 본문 마크다운에서 `![](/images/파일.jpg)` |

Pehtheme 테마는 **`resources.Get`**으로 이미지를 불러오므로, **반드시 `assets/images/`** 에 넣어야 목록·글 상단에 제대로 나옵니다.

```
be-formula/
├── assets/
│   └── images/          ← 여기에 이미지 파일 저장
│       ├── post-01.jpg
│       └── post-02.png
├── content/
│   └── posts/
│       └── my-post.md
```

### 문서(front matter)에 넣는 방법

글 파일 **상단 front matter**에 `image`와(선택) `caption`을 넣습니다.

```yaml
---
title: "글 제목"
description: "요약 문구"
date: 2026-02-23T21:00:00+09:00
draft: false
# 대표 이미지 (assets/images/ 기준 경로)
image: "images/post-01.jpg"
# 캡션(출처 등, 선택)
caption: "Photo by Your Name on Unsplash"
categories: ["web"]
tags: ["hugo", "image"]
---
```

- **image**: `assets/` 기준 경로. 예: `images/파일명.jpg` → 실제 파일은 `assets/images/파일명.jpg`
- **caption**: 이미지 아래에 표시할 설명·출처 (테마가 지원하면 표시됨)
- **외부 URL**도 가능: `image: "https://example.com/photo.jpg"` 처럼 `http`로 시작하면 그대로 사용

### 본문 안에 이미지 넣기

이미지는 front matter 말고 **본문**에도 넣을 수 있습니다. 이때는 **`static/`** 에 두고 사이트 기준 URL로 참조합니다.

1. 이미지 저장: `static/images/본문용.jpg`
2. 마크다운에서:

   ```markdown
   ![설명 텍스트](/images/본문용.jpg)
   ```

정리: **글 대표 이미지(썸네일·피처)** → `assets/images/` + front matter `image` / **본문 안 그림** → `static/images/` + `![](/images/...)`

---

## 3. 테마 커스터마이징 방법

테마 파일을 **직접 수정하지 말고**, 프로젝트 루트에서 **덮어쓰기(override)** 하는 방식으로 커스터마이징합니다.  
이렇게 하면 테마 업데이트(submodule update) 시에도 변경 사항이 유지됩니다.

### 3.1 어떤 걸 덮어쓸 수 있는지

| 폴더 | 용도 |
|------|------|
| `layouts/` | HTML 템플릿 (헤더, 푸터, 글 레이아웃 등) |
| `static/` | 이미지, favicon, CSS/JS 파일 (경로 그대로 사용) |
| `assets/` | 번들/가공할 CSS, JS (Hugo 파이프라인 사용 시) |
| `data/` | 사이트 데이터(메뉴, 설정 등) |
| `i18n/` | 다국어 문자열 |

테마 안에 있는 파일과 **같은 상대 경로**를 프로젝트 루트에 만들면, 그 파일이 테마 파일을 대신합니다.

### 3.2 예: 푸터 문구 바꾸기 (연습)

1. **테마 쪽 푸터 파일 확인**

   테마 구조:
   ```
   themes/pehtheme-hugo/layouts/partials/footer.html
   ```

2. **프로젝트 루트에 같은 경로로 복사**

   ```bash
   mkdir -p layouts/partials
   cp themes/pehtheme-hugo/layouts/partials/footer.html layouts/partials/footer.html
   ```

3. **복사한 파일만 수정**

   `layouts/partials/footer.html` 을 열어 문구, 링크, 연도 등을 원하는 대로 수정합니다.

4. **로컬 확인 후 배포**

   ```bash
   hugo server -D
   git add layouts/partials/footer.html
   git commit -m "Customize footer"
   git push origin main
   ```

### 3.3 예: 사이트 제목·설정만 바꾸기 (hugo.toml)

테마가 `site.Params` 를 쓰면, 설정만으로 바꿀 수 있습니다.

```toml
# hugo.toml (프로젝트 루트)
title = "black formula"
baseURL = "https://be-won.github.io/be-formula/"

[params]
  # 테마 문서에서 지원하는 param 이름 확인 후 사용
  # author = "Your Name"
  # description = "블로그 설명"
```

테마별로 지원하는 `params` 는 테마의 `README`나 `exampleSite` 의 `hugo.toml` 에서 확인할 수 있습니다.

### 3.4 커스터마이징 시 주의

- **수정하는 곳**: 항상 **프로젝트 루트**의 `layouts/`, `static/`, `assets/` 등.
- **수정하지 않는 곳**: `themes/pehtheme-hugo/` 안의 파일은 submodule이라 업데이트 시 덮어쓰여질 수 있음.
- **경로 규칙**: 테마의 `themes/pehtheme-hugo/layouts/partials/header.html` → 프로젝트에는 `layouts/partials/header.html` 로 두면 됨.

---

## 4. 연습 순서 제안

1. **포스트 연습**  
   - `hugo new content/posts/연습글.md`  
   - `draft: false`, 제목·본문 수정  
   - `hugo server -D` 로 확인  
   - 커밋 후 푸시 → GitHub Pages에서 확인  

2. **테마 커스터마이징 연습**  
   - `layouts/partials/footer.html` 복사 후 문구 수정  
   - 로컬에서 확인 후 푸시  

이 순서로 하면 “포스트 올리기”와 “테마 덮어쓰기” 둘 다 연습할 수 있습니다.
