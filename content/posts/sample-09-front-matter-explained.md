---
title: "Hugo Front Matter Explained: Control How Your Post Appears"
description: "What is front matter in Hugo? Learn title, description, draft, date, and custom fields for better posts."
date: 2026-02-28T10:00:00+09:00
draft: false
slug: "front-matter-explained"
categories: ["web", "writing"]
tags: ["hugo", "front-matter", "yaml", "metadata"]
image: "images/image-01.jpg"
---

Front matter is the metadata at the top of each content file. It drives how Hugo and your theme display the page.

## Common Fields

- **title**: Shown as the page heading and often in the browser tab and meta tags.
- **description**: Used for meta description (SEO) and sometimes as a lead paragraph.
- **date**: Publication or sort date. ISO 8601 format is recommended.
- **draft**: `true` = not published in production; `false` = included in the build.
- **slug**: Custom URL path. Omit to use the file path.

## Categories and Tags

Many themes use `categories` and `tags` for navigation and archive pages. Use arrays in YAML.

## Custom Fields

You can add any field (e.g. `author`, `image`). If the theme template uses ` .Params.YourField`, it will show up. Check the theme docs for supported params.

## Summary

Front matter is the control panel for each page. Set title, description, and draft correctly for SEO and visibility; add categories and tags for structure.
