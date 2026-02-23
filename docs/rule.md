# 포스트 관리 규칙: 폴더 체계 vs 파일명 로직

글이 많아졌을 때 폴더로 나눌지, 파일명 규칙으로 관리할지 정리한 문서입니다.

---

## 1. 폴더 체계로 관리

### 예시 구조

```
content/posts/
├── 2026/
│   └── 02/
│       ├── seo-for-blogs.md
│       └── hugo-tips.md
├── tech/
│   ├── git-workflow.md
│   └── hugo-setup.md
└── life/
    └── reading-notes.md
```

### 장점

- **섹션/카테고리별 목록**: `tech/`, `life/` 같은 하위 폴더가 그대로 섹션이 되어 `/tech/`, `/life/` 같은 전용 목록 페이지를 만들기 좋음.
- **연도/월별 보관**: `2026/02/` 처럼 두면 “언제 쓴 글인지” 폴더만 봐도 파악하기 쉬움.
- **Leaf bundle**: `content/posts/my-post/index.md` + 같은 폴더에 `image.jpg` 넣어서 “글 하나 = 폴더 하나”로 이미지·첨부를 묶기 좋음.

### 단점

- 폴더가 많아지면 구조를 설계하고 유지할 부담이 있음.
- Hugo는 **첫 번째 수준만** 자동으로 섹션으로 쓰는 경우가 많아서, `posts/2026/02/` 까지 쓰면 URL/섹션 설계를 한 번 정해야 함.

---

## 2. 파일명에 로직 (날짜·슬러그)

### 예시

```
content/posts/
├── 2026-02-23-seo-for-blogs.md
├── 2026-02-22-git-workflow.md
├── 2026-02-21-hugo-tips.md
└── sample-01-static-sites.md
```

- **날짜-슬러그**: 정렬·검색이 쉬움. `date`는 front matter에 따로 두고, 파일명은 “찾기 쉽게”만 써도 됨.
- **접두어**: `sample-01-`, `draft-` 같은 규칙으로 상태/유형을 구분할 수 있음.

### 장점

- 구조가 단순함. `content/posts/` 하나만 써도 됨.
- 파일 목록만 봐도 대략적인 날짜·주제를 파악하기 쉬움.
- 새 글은 “파일 하나 추가”만 하면 되고, 폴더 설계를 바꿀 필요가 없음.

### 단점

- 카테고리/섹션별 전용 URL·목록을 만들려면 taxonomy(카테고리·태그)나 메뉴 설정을 따로 해야 함.
- “이 글에 딸린 이미지만 모아두기”는 leaf bundle보다 불편함 (대신 `assets/images/`, `static/images/` 로 관리).

---

## 3. 어떤 게 효과적인지

| 상황 | 추천 |
|------|------|
| **글 수십~백 개, 카테고리만 잘 나누면 됨** | **파일명 로직(날짜+슬러그)** + **front matter의 categories/tags** 로 충분. 폴더는 `content/posts/` 한 단계만 써도 됨. |
| **카테고리별로 완전히 다른 “섹션”(메뉴·URL)** 이 필요함 | **폴더로 섹션 분리** (예: `posts/`, `notes/`, `reviews/`) 또는 `content/tech/`, `content/life/` 처럼 섹션을 나눔. |
| **글마다 이미지·자료가 많고, 글 단위로 묶고 싶음** | **Leaf bundle**: `content/posts/my-post/index.md` + 같은 폴더에 이미지. |

---

## 4. 이 블로그에서의 권장

- **폴더**: `content/posts/` 한 단계만 두고, 하위 폴더로 연도/카테고리까지 나누지 않는다.
- **파일명**: `YYYY-MM-DD-슬러그.md` 또는 `슬러그.md` 로 통일.  
  정렬·검색은 front matter의 `date`와 `categories`/`tags`로 처리.
- **분류**: 카테고리·태그는 front matter에서만 관리.  
  나중에 “기술/일상”처럼 섹션을 나누고 싶을 때만 `content/tech/`, `content/life/` 같은 폴더 체계를 도입한다.
