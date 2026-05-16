# Personal Blog Design Spec

## Overview

A warm, lifestyle-oriented personal blog built with Astro static site generator. Content managed via Markdown files. Deployed as static HTML to free hosting (GitHub Pages / Netlify / Vercel).

## Routes

| Route | Page |
|---|---|
| `/` | Home — article card list + featured photo preview |
| `/posts/` | Article archive |
| `/posts/[slug]/` | Article detail |
| `/about/` | About me |
| `/gallery/` | Photo gallery |

## Tech Stack

- **Astro** — static site generator
- **Markdown** — content format for articles and about page
- **Tailwind CSS** — utility-first styling
- **No JS framework** — pure static pages, zero client-side framework

## Visual Design — "Cream Latte" Theme

### Color Palette

| Role | Color |
|---|---|
| Page background | `#FAF7F2` (cream white) |
| Card background | `#FFFFFF` |
| Headings / body | `#3D2C2A` (deep brown) |
| Secondary text | `#7B6B66` (gray-brown) |
| Accent | `#E07B54` (warm orange) |
| Tags / decor | `#F5E6D3` (light apricot) |

### Typography

- Headings: Noto Serif SC (serif)
- Body: Noto Sans SC (sans-serif)
- Article body: 16px, line-height 1.8
- Max content width: 680px for reading comfort

### Card Style

- Border-radius: 12px
- Light box-shadow
- Hover: slight translate-y lift + shadow increase

## Page Layouts

### Home (`/`)
- Hero section: nickname + one-line intro, warm gradient or solid cream background
- Article card list below: each card shows date, title, excerpt, optional cover image
- Sidebar or bottom strip: latest 4 gallery photos preview

### Article Detail (`/posts/[slug]/`)
- Centered content area (max-width 680px)
- Title + date at top
- Comfortable reading typography
- Optional back link at bottom

### Gallery (`/gallery/`)
- Grid or masonry layout
- Click to enlarge (lightbox)
- Photos stored in `public/images/gallery/`

### About (`/about/`)
- Avatar left, bio right
- Optional: timeline or interest tags below

## Content Structure

```
src/
  content/
    posts/
      hello-world.md
    about.md (or about/ page component)
  pages/
    index.astro
    posts/
      [slug].astro
      index.astro
    about.astro
    gallery.astro
  components/
    Layout.astro
    Header.astro
    Footer.astro
    ArticleCard.astro
    PhotoGrid.astro
public/
  images/
    gallery/
```

### Post Frontmatter

```yaml
---
title: "Post Title"
date: 2026-05-16
description: "A short excerpt"
cover: /images/posts/cover.jpg
---
```

## Responsive Design

- Mobile-first approach
- Single-column on small screens
- Multi-column cards on tablet+
- Gallery grid adapts column count by viewport

## Non-Goals

- No comment system
- No RSS feed (can add later)
- No tag/category filter (can add later)
- No dark mode toggle (can add later)
- No CMS backend
