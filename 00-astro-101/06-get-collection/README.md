# Content Collections and Dynamic Routing

This guide shows how to use **Astro Content Collections** and create **dynamic routes** for a blog or any content-driven site.
This system allows you to organize content with type safety, automatic validation, and page generation.

## 1. Content Collections Setup

Content Collections in Astro provide a way to organize and validate your content.
Create a new folder at `./src/content/` and inside it, a `config.ts` file. This file defines the structure of our blog posts.

### Add Configuration File

_./src/content/config.ts_

```typescript
import { defineCollection, z } from "astro:content";

const postCollection = defineCollection({
  type: "content",
  schema: z.object({
    title: z.string(),
    description: z.string(),
    image: z.string(),
  }),
});

export const collections = {
  postCollection,
};
```

**What this does:**

- Defines a `postCollection` with type "content" for markdown files
- Uses Zod schema to validate frontmatter data
- Ensures type safety across our application

### Content Structure

Our blog posts are stored in `./src/content/postCollection/` with the following structure:

```
src/
└── content/
    ├── config.ts
    └── postCollection/
        ├── astro-image-component.md
        ├── astro-new-features.md
        └── modern-css-techniques.md
```

Each markdown file includes frontmatter that matches our schema:

```markdown
---
title: "Astro Image component - Complete Guide"
description: "Complete guide to Astro's Image component: automatic optimization, lazy loading, and best practices for web images."
image: "https://images.unsplash.com/photo-1611224923853-80b023f02d71?w=800&h=400&fit=crop"
---

Your markdown content goes here...
```

## 2. Blog Pages Implementation

Add a link to the blog in `index.astro`:

_./src/pages/index.astro_ (addition)

```diff
  <h1>Dog Facts</h1>
  <DogFacts facts={facts} />
  <button id="cat-fact-button">Get Cat Fact</button>
  <h2 id="cat-fact"></h2>
  <a href="/about">Go to about page</a>
+ <a href="/blog">Go to blog page</a>
```

### Create Blog Index Page

Create a new folder in pages called `blog` to hold our blog index and individual post pages.

Create `/src/pages/blog/index.astro` to list all posts.

_./src/pages/blog/index.astro_

```astro
---
import BaseLayout from "../../layouts/BaseLayout.astro";
import { getCollection } from "astro:content";

const posts = await getCollection("postCollection");
---

<BaseLayout title="Blog">
  <h1>Blog</h1>
  <ul>
    {
      posts.map((post) => (
        <li>
          <a href={`/blog/${post.slug}`}>{post.data.title}</a>
        </li>
      ))
    }
  </ul>
  <a href="/">Go back home</a>
</BaseLayout>
```

Key features:

- Uses `getCollection("postCollection")` to fetch all blog posts
- Automatically generates links using the file `slug` and `data.title` from frontmatter.

### Dynamic Post Pages

Individual blog post pages are generated using dynamic routing with `[slug].astro`.

_./src/pages/blog/[slug].astro_

```astro
---
import BaseLayout from "../../layouts/BaseLayout.astro";
import { getCollection } from "astro:content";

export async function getStaticPaths() {
  const posts = await getCollection("postCollection");
  return posts.map((post) => ({
    params: { slug: post.slug },
    props: { post },
  }));
}

const { post } = Astro.props;
const { Content } = await post.render();
---

<BaseLayout title={post.data.title}>
  <h1>{post.data.title}</h1>
  <img
    src={post.data.image}
    alt={post.data.title}
    style="max-width: 100%; height: auto;"
  />
  <p>{post.data.description}</p>
  <article>
    <Content />
  </article>
  <a href="/blog">Back to blog</a>
</BaseLayout>
```

**Explanation:**

- `getStaticPaths()` automatically generates all dynamic routes (`/blog/[slug]`).
- `entry.render()` safely processes the Markdown and returns the `Content` component.
- Metadata is rendered alongside the post content.
