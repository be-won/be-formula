---
title: "Theme Customization in Hugo: Override, Don't Edit the Theme"
description: "How to customize a Hugo theme by overriding templates in your project. Keep theme updates and your changes separate."
date: 2026-02-24T10:00:00+09:00
draft: false
slug: "theme-customization-hugo"
categories: ["web", "design"]
tags: ["hugo", "themes", "customization", "layouts"]
image: "images/image-01.jpg"
---

Changing a theme by editing files inside `themes/` is risky: updates can overwrite your edits. Override instead.

## Override in Your Project

Copy the file you want to change from `themes/YourTheme/layouts/...` to `layouts/...` in your project root, keeping the same path. Hugo uses your file first. The theme stays untouched.

## What You Can Override

- **Layouts**: e.g. `layouts/partials/footer.html` to change the footer.
- **Static assets**: Put files in `static/` with the same path as in the theme.
- **CSS/JS**: Depends on the theme; often you add or override in `assets/` or `static/`.

## After Overriding

Edit only the copy in your project. When you update the theme (e.g. submodule pull), your overrides remain. Re-copy only if you want to adopt new theme changes and then re-apply your edits.

## Summary

Override theme files in your project root. You keep full control and can still update the theme when needed.
