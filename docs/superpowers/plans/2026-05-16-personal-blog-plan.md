# Personal Blog Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a warm, lifestyle-oriented personal blog with Astro static site generator and Tailwind CSS.

**Architecture:** Astro static site with content collections for Markdown blog posts. Pages: home (article cards + photo preview), posts archive, post detail, about, gallery. Pure static HTML/CSS — no client-side framework. Minimal vanilla JS only for gallery lightbox.

**Tech Stack:** Astro 5, Tailwind CSS, Markdown (content collections), Google Fonts (Noto Serif SC + Noto Sans SC)

---

### Task 1: Scaffold Astro project and install dependencies

**Files:**
- Create: `package.json`, `astro.config.mjs`, `tailwind.config.mjs`, `.gitignore`
- Modify: (none)

- [ ] **Step 1: Create Astro project**

Run: `npm create astro@latest . -- --skip-houston --template basics --install`
Expected: Project scaffolded with `package.json`, `astro.config.mjs`, `src/` directory

- [ ] **Step 2: Install Tailwind CSS integration**

Run: `npx astro add tailwind --yes`
Expected: `@astrojs/tailwind` and `tailwindcss` added to `package.json`, `astro.config.mjs` updated with tailwind integration

- [ ] **Step 3: Configure Tailwind with Cream Latte theme**

Write `tailwind.config.mjs`:

```js
/** @type {import('tailwindcss').Config} */
export default {
  content: ['./src/**/*.{astro,html,js,jsx,md,mdx,svelte,ts,tsx,vue}'],
  theme: {
    extend: {
      colors: {
        cream: '#FAF7F2',
        'deep-brown': '#3D2C2A',
        'warm-gray': '#7B6B66',
        'warm-orange': '#E07B54',
        apricot: '#F5E6D3',
      },
      fontFamily: {
        serif: ['Noto Serif SC', 'serif'],
        sans: ['Noto Sans SC', 'sans-serif'],
      },
    },
  },
  plugins: [],
};
```

- [ ] **Step 4: Verify config**

Run: `npx astro dev`
Expected: Dev server starts without errors (Ctrl+C to stop)

- [ ] **Step 5: Commit**

```bash
git add -A
git commit -m "chore: scaffold Astro project with Tailwind CSS"
```

---

### Task 2: Write global styles

**Files:**
- Create: `src/styles/global.css`
- Modify: (none)

- [ ] **Step 1: Write global.css**

Write `src/styles/global.css`:

```css
@tailwind base;
@tailwind components;
@tailwind utilities;

@layer base {
  html {
    scroll-behavior: smooth;
  }

  body {
    @apply bg-cream text-deep-brown font-sans;
  }

  h1, h2, h3, h4, h5, h6 {
    @apply font-serif;
  }
}

@layer components {
  .card {
    @apply bg-white rounded-xl shadow-md hover:shadow-lg transition-all duration-300 hover:-translate-y-1;
  }

  .prose {
    @apply max-w-[680px] mx-auto text-base leading-[1.8];
  }

  .prose h1, .prose h2, .prose h3 {
    @apply mt-8 mb-4;
  }

  .prose p {
    @apply mb-4;
  }

  .prose img {
    @apply rounded-xl my-6 max-w-full;
  }

  .prose ul, .prose ol {
    @apply mb-4 pl-6;
  }

  .prose li {
    @apply mb-1;
  }

  .prose a {
    @apply text-warm-orange underline;
  }

  .prose blockquote {
    @apply border-l-4 border-warm-orange pl-4 italic text-warm-gray my-6;
  }

  .prose code {
    @apply bg-apricot px-1.5 py-0.5 rounded text-sm;
  }

  .prose pre {
    @apply bg-deep-brown text-cream p-4 rounded-xl overflow-x-auto my-6;
  }

  .prose pre code {
    @apply bg-transparent p-0 text-cream;
  }
}
```

- [ ] **Step 2: Verify CSS is loaded**

Run: `npx astro dev`
Expected: Dev server starts, check that page body has `bg-cream` background color in browser devtools

- [ ] **Step 3: Commit**

```bash
git add src/styles/global.css
git commit -m "style: add global styles with Cream Latte theme and prose typography"
```

---

### Task 3: Create Layout, Header, Footer components

**Files:**
- Create: `src/components/Layout.astro`, `src/components/Header.astro`, `src/components/Footer.astro`

- [ ] **Step 1: Write Header component**

Write `src/components/Header.astro`:

```astro
<header class="py-6 px-4 max-w-4xl mx-auto flex flex-col sm:flex-row justify-between items-center gap-4">
  <a href="/" class="font-serif text-2xl font-bold text-deep-brown hover:text-warm-orange transition-colors no-underline">
    My Blog
  </a>
  <nav class="flex gap-6 text-sm font-medium">
    <a href="/" class="text-warm-gray hover:text-warm-orange transition-colors no-underline">Home</a>
    <a href="/posts/" class="text-warm-gray hover:text-warm-orange transition-colors no-underline">Posts</a>
    <a href="/gallery/" class="text-warm-gray hover:text-warm-orange transition-colors no-underline">Gallery</a>
    <a href="/about/" class="text-warm-gray hover:text-warm-orange transition-colors no-underline">About</a>
  </nav>
</header>
```

- [ ] **Step 2: Write Footer component**

Write `src/components/Footer.astro`:

```astro
<footer class="py-8 text-center text-warm-gray text-sm border-t border-apricot mt-16">
  <p>&copy; {new Date().getFullYear()} My Blog</p>
</footer>
```

- [ ] **Step 3: Write Layout component**

Write `src/components/Layout.astro`:

```astro
---
import Header from './Header.astro';
import Footer from './Footer.astro';
import '../styles/global.css';

export interface Props {
  title: string;
  description?: string;
}
const { title, description = 'A personal blog about life, travels, and thoughts' } = Astro.props;
---

<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <meta name="description" content={description} />
  <title>{title}</title>
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+SC:wght@400;500;700&family=Noto+Serif+SC:wght@700&display=swap" rel="stylesheet" />
  <link rel="icon" type="image/svg+xml" href="/favicon.svg" />
</head>
<body class="bg-cream font-sans text-deep-brown min-h-screen flex flex-col">
  <Header />
  <main class="flex-1">
    <slot />
  </main>
  <Footer />
</body>
</html>
```

- [ ] **Step 4: Create favicon**

Write `public/favicon.svg`:

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100">
  <rect width="100" height="100" rx="20" fill="#E07B54"/>
  <text x="50" y="68" text-anchor="middle" fill="white" font-size="50" font-family="serif">B</text>
</svg>
```

- [ ] **Step 5: Verify components render**

Run: `npx astro dev`
Expected: Navigate to `http://localhost:4321` and see header with navigation, footer with copyright

- [ ] **Step 6: Commit**

```bash
git add src/components/Layout.astro src/components/Header.astro src/components/Footer.astro public/favicon.svg
git commit -m "feat: add Layout, Header, Footer components with favicon"
```

---

### Task 4: Create Hero, ArticleCard, PhotoGrid components

**Files:**
- Create: `src/components/Hero.astro`, `src/components/ArticleCard.astro`, `src/components/PhotoGrid.astro`

- [ ] **Step 1: Write Hero component**

Write `src/components/Hero.astro`:

```astro
---
export interface Props {
  nickname: string;
  intro: string;
}
const { nickname, intro } = Astro.props;
---

<section class="py-20 px-4 text-center bg-gradient-to-b from-apricot/30 to-cream">
  <h1 class="font-serif text-4xl md:text-5xl text-deep-brown mb-4">{nickname}</h1>
  <p class="text-warm-gray text-lg max-w-md mx-auto">{intro}</p>
</section>
```

- [ ] **Step 2: Write ArticleCard component**

Write `src/components/ArticleCard.astro`:

```astro
---
export interface Props {
  title: string;
  date: string;
  description: string;
  slug: string;
  cover?: string;
}
const { title, date, description, slug, cover } = Astro.props;
---

<article class="card overflow-hidden">
  {cover && (
    <img src={cover} alt={title} class="w-full h-48 object-cover" loading="lazy" />
  )}
  <div class="p-6">
    <time class="text-sm text-warm-gray">{date}</time>
    <h2 class="font-serif text-xl mt-2 mb-2">
      <a href={`/posts/${slug}/`} class="text-deep-brown hover:text-warm-orange transition-colors no-underline">
        {title}
      </a>
    </h2>
    <p class="text-warm-gray text-sm leading-relaxed">{description}</p>
  </div>
</article>
```

- [ ] **Step 3: Write PhotoGrid component**

Write `src/components/PhotoGrid.astro`:

```astro
---
export interface Props {
  photos: Array<{ src: string; alt: string }>;
}
const { photos } = Astro.props;
---

<div class="grid grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-4">
  {photos.map(photo => (
    <a href={photo.src} class="block rounded-xl overflow-hidden shadow-md hover:shadow-lg transition-all duration-300 hover:-translate-y-1">
      <img src={photo.src} alt={photo.alt} class="w-full h-48 object-cover" loading="lazy" />
    </a>
  ))}
</div>
```

- [ ] **Step 4: Verify build passes**

Run: `npx astro build`
Expected: Build succeeds with all components imported

- [ ] **Step 5: Commit**

```bash
git add src/components/Hero.astro src/components/ArticleCard.astro src/components/PhotoGrid.astro
git commit -m "feat: add Hero, ArticleCard, PhotoGrid components"
```

---

### Task 5: Set up content collections and sample post

**Files:**
- Create: `src/content/config.ts`, `src/content/posts/hello-world.md`

- [ ] **Step 1: Write content collection schema**

Write `src/content/config.ts`:

```typescript
import { defineCollection, z } from 'astro:content';

const posts = defineCollection({
  schema: z.object({
    title: z.string(),
    date: z.date(),
    description: z.string(),
    cover: z.string().optional(),
  }),
});

export const collections = { posts };
```

- [ ] **Step 2: Write sample blog post**

Write `src/content/posts/hello-world.md`:

```markdown
---
title: "Hello World"
date: 2026-05-16
description: "Welcome to my new blog. This is the beginning of something wonderful."
---

## Welcome

Hello and welcome! This is the first post on my new blog. I created this space to share stories from my life — travels, daily moments, and random thoughts.

## What to Expect

I'll be writing about things that inspire me: the places I visit, the books I read, and the small moments that make life meaningful.

## Why a Blog?

In a world of short-form content, I wanted a slower, more thoughtful space. A place where words can breathe and stories can unfold at their own pace.

Stay tuned for more. I'm excited to share this journey with you.
```

- [ ] **Step 3: Verify content collection builds**

Run: `npx astro build`
Expected: Build succeeds with content collection recognized (no errors about schema or collections)

- [ ] **Step 4: Commit**

```bash
git add src/content/config.ts src/content/posts/hello-world.md
git commit -m "feat: add content collections and sample blog post"
```

---

### Task 6: Create home page

**Files:**
- Modify: `src/pages/index.astro`

- [ ] **Step 1: Write home page**

Write `src/pages/index.astro`:

```astro
---
import Layout from '../components/Layout.astro';
import Hero from '../components/Hero.astro';
import ArticleCard from '../components/ArticleCard.astro';
import PhotoGrid from '../components/PhotoGrid.astro';
import { getCollection } from 'astro:content';

const posts = await getCollection('posts');
const sortedPosts = posts.sort((a, b) => b.data.date.getTime() - a.data.date.getTime());
const recentPosts = sortedPosts.slice(0, 6);
const recentPhotos = [
  { src: '/images/gallery/photo1.svg', alt: 'Gallery photo 1' },
  { src: '/images/gallery/photo2.svg', alt: 'Gallery photo 2' },
  { src: '/images/gallery/photo3.svg', alt: 'Gallery photo 3' },
  { src: '/images/gallery/photo4.svg', alt: 'Gallery photo 4' },
];
---

<Layout title="My Blog">
  <Hero nickname="Your Name" intro="Welcome to my corner of the internet — a place for stories, photos, and quiet moments." />

  <section class="max-w-4xl mx-auto px-4 mt-12">
    <h2 class="font-serif text-2xl mb-6">Recent Posts</h2>
    <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
      {recentPosts.length > 0 ? (
        recentPosts.map(post => (
          <ArticleCard
            title={post.data.title}
            date={post.data.date.toLocaleDateString('zh-CN')}
            description={post.data.description}
            slug={post.slug}
            cover={post.data.cover}
          />
        ))
      ) : (
        <p class="text-warm-gray col-span-2 text-center py-12">No posts yet. Check back soon!</p>
      )}
    </div>
  </section>

  <section class="max-w-4xl mx-auto px-4 mt-16 mb-12">
    <h2 class="font-serif text-2xl mb-6">
      <a href="/gallery/" class="text-deep-brown hover:text-warm-orange transition-colors no-underline">Recent Photos →</a>
    </h2>
    <PhotoGrid photos={recentPhotos} />
  </section>
</Layout>
```

- [ ] **Step 2: Create placeholder gallery images**

Run: `mkdir -p public/images/gallery`

Create placeholder SVGs for the 4 gallery photos. Write `public/images/gallery/photo1.jpg`:

Actually, create simple SVG placeholders since we can't download images:

Write `public/images/gallery/photo1.svg`:

```svg
<svg xmlns="http://www.w3.org/2000/svg" width="400" height="300" viewBox="0 0 400 300">
  <rect width="400" height="300" fill="#F5E6D3"/>
  <text x="200" y="160" text-anchor="middle" fill="#E07B54" font-size="48" font-family="sans-serif">📷</text>
</svg>
```

Write `public/images/gallery/photo2.svg`:

```svg
<svg xmlns="http://www.w3.org/2000/svg" width="400" height="300" viewBox="0 0 400 300">
  <rect width="400" height="300" fill="#F5E6D3"/>
  <text x="200" y="160" text-anchor="middle" fill="#E07B54" font-size="48" font-family="sans-serif">🌿</text>
</svg>
```

Write `public/images/gallery/photo3.svg`:

```svg
<svg xmlns="http://www.w3.org/2000/svg" width="400" height="300" viewBox="0 0 400 300">
  <rect width="400" height="300" fill="#F5E6D3"/>
  <text x="200" y="160" text-anchor="middle" fill="#E07B54" font-size="48" font-family="sans-serif">🏔️</text>
</svg>
```

Write `public/images/gallery/photo4.svg`:

```svg
<svg xmlns="http://www.w3.org/2000/svg" width="400" height="300" viewBox="0 0 400 300">
  <rect width="400" height="300" fill="#F5E6D3"/>
  <text x="200" y="160" text-anchor="middle" fill="#E07B54" font-size="48" font-family="sans-serif">🌅</text>
</svg>
```

- [ ] **Step 3: Verify home page renders**

Run: `npx astro dev`
Expected: Navigate to `http://localhost:4321`, see hero section, recent posts card with "Hello World", and photo grid at bottom

- [ ] **Step 4: Commit**

```bash
git add src/pages/index.astro public/images/gallery/
git commit -m "feat: add home page with hero, recent posts, and photo preview"
```

---

### Task 7: Create posts pages (archive + detail)

**Files:**
- Create: `src/pages/posts/index.astro`, `src/pages/posts/[slug].astro`

- [ ] **Step 1: Write posts archive page**

Write `src/pages/posts/index.astro`:

```astro
---
import Layout from '../../components/Layout.astro';
import ArticleCard from '../../components/ArticleCard.astro';
import { getCollection } from 'astro:content';

const posts = await getCollection('posts');
const sortedPosts = posts.sort((a, b) => b.data.date.getTime() - a.data.date.getTime());
---

<Layout title="Posts - My Blog" description="All blog posts">
  <div class="max-w-4xl mx-auto px-4 py-12">
    <h1 class="font-serif text-3xl text-center mb-10">All Posts</h1>
    {sortedPosts.length > 0 ? (
      <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
        {sortedPosts.map(post => (
          <ArticleCard
            title={post.data.title}
            date={post.data.date.toLocaleDateString('zh-CN')}
            description={post.data.description}
            slug={post.slug}
            cover={post.data.cover}
          />
        ))}
      </div>
    ) : (
      <p class="text-warm-gray text-center py-20">No posts yet. Coming soon!</p>
    )}
  </div>
</Layout>
```

- [ ] **Step 2: Write post detail page**

Write `src/pages/posts/[slug].astro`:

```astro
---
import Layout from '../../components/Layout.astro';
import { getCollection } from 'astro:content';

export async function getStaticPaths() {
  const posts = await getCollection('posts');
  return posts.map(post => ({
    params: { slug: post.slug },
    props: { post },
  }));
}

const { post } = Astro.props;
const { Content } = await post.render();
---

<Layout title={`${post.data.title} - My Blog`} description={post.data.description}>
  <article class="max-w-[680px] mx-auto px-4 py-12">
    <header class="mb-10 text-center">
      <time class="text-sm text-warm-gray">{post.data.date.toLocaleDateString('zh-CN')}</time>
      <h1 class="font-serif text-3xl md:text-4xl mt-2">{post.data.title}</h1>
    </header>
    <div class="prose">
      <Content />
    </div>
    <footer class="mt-16 pt-6 border-t border-apricot">
      <a href="/posts/" class="text-warm-orange hover:underline font-medium">← Back to all posts</a>
    </footer>
  </article>
</Layout>
```

- [ ] **Step 3: Verify posts pages**

Run: `npx astro build`
Expected: Build succeeds. Static HTML generated at `dist/posts/index.html` and `dist/posts/hello-world/index.html`

Run: `npx astro dev` and navigate to:
- `http://localhost:4321/posts/` — see the posts archive with "Hello World" card
- `http://localhost:4321/posts/hello-world/` — see the full post content with prose styling

- [ ] **Step 4: Commit**

```bash
git add src/pages/posts/
git commit -m "feat: add posts archive and detail pages"
```

---

### Task 8: Create About and Gallery pages

**Files:**
- Create: `src/pages/about.astro`, `src/pages/gallery.astro`

- [ ] **Step 1: Write About page**

Write `src/pages/about.astro`:

```astro
---
import Layout from '../components/Layout.astro';
---

<Layout title="About - My Blog" description="Learn more about me and this blog">
  <div class="max-w-2xl mx-auto px-4 py-12">
    <h1 class="font-serif text-3xl text-center mb-10">About Me</h1>
    <div class="flex flex-col md:flex-row items-center gap-8 mb-12">
      <div class="w-32 h-32 rounded-full bg-apricot flex items-center justify-center shadow-md flex-shrink-0">
        <span class="text-4xl">🙂</span>
      </div>
      <div>
        <h2 class="font-serif text-2xl mb-2">Your Name</h2>
        <p class="text-warm-gray leading-relaxed">
          A short bio about yourself. Share your story, interests, and what this blog is about.
          Replace this text with your own introduction when you're ready.
        </p>
      </div>
    </div>
    <div>
      <h2 class="font-serif text-xl mb-4">Interests</h2>
      <div class="flex flex-wrap gap-2">
        <span class="bg-apricot text-deep-brown px-3 py-1 rounded-full text-sm">Photography</span>
        <span class="bg-apricot text-deep-brown px-3 py-1 rounded-full text-sm">Reading</span>
        <span class="bg-apricot text-deep-brown px-3 py-1 rounded-full text-sm">Travel</span>
        <span class="bg-apricot text-deep-brown px-3 py-1 rounded-full text-sm">Cooking</span>
      </div>
    </div>
  </div>
</Layout>
```

- [ ] **Step 2: Write Gallery page with lightbox**

Write `src/pages/gallery.astro`:

```astro
---
import Layout from '../components/Layout.astro';

const photos = [
  { src: '/images/gallery/photo1.svg', alt: 'Photo 1' },
  { src: '/images/gallery/photo2.svg', alt: 'Photo 2' },
  { src: '/images/gallery/photo3.svg', alt: 'Photo 3' },
  { src: '/images/gallery/photo4.svg', alt: 'Photo 4' },
  { src: '/images/gallery/photo1.svg', alt: 'Photo 5' },
  { src: '/images/gallery/photo2.svg', alt: 'Photo 6' },
];
---

<Layout title="Gallery - My Blog" description="A collection of photos and moments">
  <div class="max-w-5xl mx-auto px-4 py-12">
    <h1 class="font-serif text-3xl text-center mb-10">Gallery</h1>
    <div class="grid grid-cols-2 md:grid-cols-3 gap-4" id="gallery-grid">
      {photos.map((photo, i) => (
        <a
          href={photo.src}
          class="gallery-item block rounded-xl overflow-hidden shadow-md hover:shadow-lg transition-all duration-300 hover:-translate-y-1"
          data-src={photo.src}
        >
          <img src={photo.src} alt={photo.alt} class="w-full h-56 object-cover" loading="lazy" />
        </a>
      ))}
    </div>
  </div>

  <div id="lightbox" class="fixed inset-0 bg-black/80 hidden items-center justify-center z-50 cursor-pointer">
    <span id="lightbox-close" class="absolute top-4 right-8 text-white text-4xl">&times;</span>
    <img id="lightbox-img" src="" alt="" class="max-w-[90vw] max-h-[90vh] object-contain rounded-lg" />
  </div>
</Layout>

<script>
  function initLightbox() {
    const lightbox = document.getElementById('lightbox');
    const lightboxImg = document.getElementById('lightbox-img') as HTMLImageElement;
    const closeBtn = document.getElementById('lightbox-close');

    document.querySelectorAll('.gallery-item').forEach(item => {
      item.addEventListener('click', (e) => {
        e.preventDefault();
        const el = e.currentTarget as HTMLAnchorElement;
        lightboxImg.src = el.dataset.src || '';
        lightbox!.classList.remove('hidden');
        lightbox!.classList.add('flex');
      });
    });

    function closeLightbox() {
      lightbox!.classList.add('hidden');
      lightbox!.classList.remove('flex');
    }

    lightbox!.addEventListener('click', (e) => {
      if (e.target === lightbox) closeLightbox();
    });
    closeBtn!.addEventListener('click', closeLightbox);

    document.addEventListener('keydown', (e) => {
      if (e.key === 'Escape') closeLightbox();
    });
  }

  initLightbox();
</script>
```

- [ ] **Step 3: Verify new pages**

Run: `npx astro build`
Expected: Build succeeds. Static HTML generated at `dist/about/index.html` and `dist/gallery/index.html`

Run: `npx astro dev` and verify:
- `http://localhost:4321/about/` — avatar, bio, interest tags
- `http://localhost:4321/gallery/` — photo grid, click to open lightbox, Esc to close

- [ ] **Step 4: Commit**

```bash
git add src/pages/about.astro src/pages/gallery.astro
git commit -m "feat: add About and Gallery pages with lightbox"
```

---

### Task 9: Final verification and polish

**Files:**
- Modify: (cleanup if needed)

- [ ] **Step 1: Full production build**

Run: `npx astro build`
Expected: Zero errors. All pages built to `dist/`

- [ ] **Step 2: Verify all routes**

Run: `npx astro dev`

Visit each page and check:
| Route | Check |
|---|---|
| `/` | Hero, article cards with "Hello World", photo grid |
| `/posts/` | Posts archive with cards |
| `/posts/hello-world/` | Full post with prose styling, back link |
| `/about/` | Avatar, bio, interest tags |
| `/gallery/` | Photo grid, lightbox on click |

Also check:
- Resize browser to mobile width — layout adapts to single column
- Header nav links work on all pages
- Footer copyright shows current year

- [ ] **Step 3: Run build and check output**

Run: `ls -la dist/`
Expected: `dist/index.html`, `dist/posts/`, `dist/about/`, `dist/gallery/` directories with HTML files

- [ ] **Step 4: Commit**

```bash
git add -A
git commit -m "chore: final verification — all pages build successfully"
```
