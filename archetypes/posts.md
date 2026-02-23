---
# === 필수·권장 (SEO) ===
# 검색 결과 제목 (50~60자, 핵심 키워드 포함)
title: "{{ replace .File.ContentBaseName "-" " " | title }}"
# 메타 설명 (150~160자, 클릭률)
description: ""
# 게시일 (ISO 8601)
date: {{ .Date }}
# URL 슬러그 (비워두면 파일명, 영문·하이픈 권장)
slug: ""
# 공개 여부
draft: true

# === 대표 이미지 (OG·트위터 카드) ===
# image: "images/og-image.jpg"
# imageCaption: "캡션 또는 출처"

# === 분류 ===
categories: []
tags: []

# === SEO·확장 (선택) ===
# lastmod: {{ .Date }}          # 수정일 (신선도)
# author: ""                    # 작성자 (E-A-T·스키마)
# canonicalURL: ""              # 원문 URL (중복 방지)
# keywords: []                  # 메타 키워드
# noindex: false                # true 시 색인 제외
# summary: ""                   # 목록용 요약
# series: []                    # 시리즈명
# weight: 0                     # 정렬 가중치
# aliases: []                   # 리다이렉트용 이전 URL
# type: "post"                  # 레이아웃 타입
# layout: ""                    # 사용할 레이아웃 오버라이드
# toc: true                     # 목차 표시
---

<!-- ![정적 사이트와 Hugo 로고](/images/image-01.jpg) -->
본문을 여기에 작성합니다.
