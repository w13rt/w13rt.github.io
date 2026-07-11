# My Blog

A minimal Jekyll blog, built for GitHub Pages.

## Local development (Docker)

```sh
docker compose up
```

Then open <http://localhost:4000>. The first run installs gems into a
cached volume, so later starts are fast. Edits to files reload
automatically (livereload).

Without Docker, if you have Ruby installed:

```sh
bundle install
bundle exec jekyll serve
```

## Deploying to GitHub Pages

1. Push this repo to GitHub.
2. **For a user site** (`username.github.io`): keep `baseurl: ""` in
   `_config.yml` and set `url: "https://username.github.io"`.
3. **For a project site** (`username.github.io/repo`): set
   `baseurl: "/repo"` and `url: "https://username.github.io"`.
4. In the repo settings, enable **Pages → Deploy from a branch → main**.

## Writing

Add Markdown files to `_posts/` named `YYYY-MM-DD-title.md` with front
matter:

```yaml
---
layout: post
title: "My post"
subtitle: "Optional"
tags: [topic-one, topic-two]
---
```

Tags are optional. They appear on posts and can be browsed on the Topics page.

## Structure

- `_config.yml` — site title, description, author, URL
- `_layouts/` — page templates (default, post, page)
- `assets/css/main.scss` — all styling; design tokens at the top
- `index.html` — home page (latest 10 posts)
- `archive.md` — all posts grouped by year
- `about.md` — about page
