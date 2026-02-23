---
title: "Keeping Your Blog Maintainable: Habits That Scale"
description: "Simple habits to keep your blog easy to update: gitignore, archetypes, and a small set of conventions."
date: 2026-03-01T10:00:00+09:00
draft: false
slug: "keeping-blog-maintainable"
categories: ["productivity", "development"]
tags: ["maintenance", "gitignore", "archetypes", "workflow"]
image: "images/image-01.jpg"
---

A few conventions keep your blog from becoming messy as it grows.

## Use .gitignore Well

Ignore build output (`/public/`), local lock files (e.g. `.hugo_build.lock`), and OS junk (e.g. `.DS_Store`, `._*`). This keeps the repo clean and avoids accidental commits.

## Archetypes for New Posts

Create an archetype (e.g. `archetypes/posts.md`) with the front matter you always want: title, description, date, draft, categories, tags. Then `hugo new content/posts/my-post.md` gives you a consistent starting point.

## Naming and Structure

- Use clear, short file names or slugs.
- Keep categories and tags consistent so filtering and menus make sense.
- Document your choices in a README or internal guide so you remember later.

## Summary

Good .gitignore, a reusable archetype, and consistent naming reduce friction every time you add or change content. Small habits pay off over time.
