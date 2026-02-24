---
title: "GitHub Actions: Deploy Your Hugo Site Automatically"
description: "Set up GitHub Actions to build and deploy your Hugo site to GitHub Pages on every push. No manual build step."
date: 2026-02-25T10:00:00+09:00
draft: false
slug: "github-actions-deploy"
categories: ["development", "devops"]
tags: ["github-actions", "ci-cd", "hugo", "github-pages", "ed-pick", "popular"]
image: "images/image-01.jpg"
---

Automate your blog deployment so that every push to main triggers a build and publish.

## Why Automate

Manual builds are easy to forget. With GitHub Actions, you push content or code and the site updates after the workflow runs. No need to run `hugo` or upload files yourself.

## Basic Workflow

A typical workflow checks out the repo (with submodules for the theme), installs Hugo, runs `hugo --minify`, and uploads the `public` folder to GitHub Pages. The workflow file lives in `.github/workflows/`.

## Keys to Remember

- Use `submodules: recursive` when your theme is a submodule.
- Set the Pages source to "GitHub Actions" in the repo Settings.
- The first run may take a minute; later runs are usually faster.

## Summary

Once the workflow is set up, publishing is just a push. Focus on writing; let Actions handle the build and deploy.
