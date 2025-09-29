# Layout

Til now we have been working on a single page. But most websites have multiple pages, and they share some common elements, like the header and footer.

So if we want to create a second page, we could just create a new file in the `src/pages` folder, called `about.astro`:

_./src/pages/about.astro_

```astro
---
import "../styles.css";
---

<html lang="en">
  <head>
    <meta charset="utf-8" />
    <link rel="icon" type="image/svg+xml" href="/favicon.svg" />
    <meta name="viewport" content="width=device-width" />
    <meta name="generator" content={Astro.generator} />
    <title>Astro</title>
  </head>
  <body>
    <h1>About page</h1>
    <a href="/">Go back home</a>
  </body>
</html>
```

And even add a navigation link on the homepage:

_./src/pages/index.astro_

```diff
  <body>
    <h1>Dog Facts</h1>
    <DogFacts facts={facts} />
    <button id="cat-fact-button">Get Cat Fact</button>
    <h2 id="cat-fact"></h2>
+   <a href="/about">About</a>
  </body>
</html>
```

We can give it a try:

```bash
npm run dev
```

But we are repeating a lot of code! The `<head>`, the `<html>`, and the `<body>` tags are all the same on both pages, and what if we have a common header or footer. This is not very DRY (Don't Repeat Yourself).

What can we do about it? We can create a layout component that wraps our pages and includes the common elements.

Let's create a new folder in `src` called `layouts`. And a new file in the `src/layouts` folder, called `BaseLayout.astro`.

> We will use slots to define where the page content will go, quite similar to React's `props.children`.
> _./src/layouts/BaseLayout.astro_

```astro
---
import "../styles.css";
---

<html lang="en">
  <head>
    <meta charset="utf-8" />
    <link rel="icon" type="image/svg+xml" href="/favicon.svg" />
    <meta name="viewport" content="width=device-width" />
    <meta name="generator" content={Astro.generator} />
    <title>Astro</title>
  </head>
  <body>
    <slot />
  </body>
</html>
```

And in the Home page just use:

_./src/pages/index.astro_

```diff
---
- import "../styles.css";
+ import BaseLayout from "../layouts/BaseLayout.astro";
import DogFacts from "../components/DogFact.astro";

const res = await fetch("https://dogapi.dog/api/v2/facts?limit=5");
const response = await res.json();
const data = response?.data ?? [];
const facts: string[] = data.map((item: any) => item.attributes.body);
---

- <html lang="en">
-  <head>
-    <meta charset="utf-8" />
-    <link rel="icon" type="image/svg+xml" href="/favicon.svg" />
-    <meta name="viewport" content="width=device-width" />
-    <meta name="generator" content={Astro.generator} />
-    <title>Astro</title>
-  </head>
-  <body>
+ <BaseLayout>
    <h1>Dog Facts</h1>
    <DogFacts facts={facts} />
    <button id="cat-fact-button">Get Cat Fact</button>
    <h2 id="cat-fact"></h2>
    <a href="/about">About</a>
+  </BaseLayout>
- </html>

<script>
  const button = document.getElementById("cat-fact-button");
  const factEl = document.getElementById("cat-fact");

  if (button && factEl) {
    button.addEventListener("click", async () => {
      const res = await fetch("https://catfact.ninja/fact");
      const data = await res.json();
      factEl.innerText = data.fact;
    });
  }
</script>
```

And in the about page

_./src/pages/about.astro_

````diff
---
- import "../styles.css";
+ import BaseLayout from "../layouts/BaseLayout.astro";
---
-
- <html lang="en">
-  <head>
-    <meta charset="utf-8" />
-    <link rel="icon" type="image/svg+xml" href="/favicon.svg" />
-    <meta name="viewport" content="width=device-width" />
-    <meta name="generator" content={Astro.generator} />
-    <title>Astro</title>
-  </head>
-  <body>
+  <BaseLayout>
    <h1>About page</h1>
    <a href="/">Go back home</a>
+  </BaseLayout>
-  </body>
- </html>

There's only one problem: the title of the page is always "Astro". We can fix that by passing a `title` prop to the layout component.

_./src/layouts/BaseLayout.astro_

```diff
---
import "../styles.css";
+
+ export interface Props {
+   title: string;
+ }
+ const { title } = Astro.props;
---

<html lang="en">
  <head>
    <meta charset="utf-8" />
    <link rel="icon" type="image/svg+xml" href="/favicon.svg" />
    <meta name="viewport" content="width=device-width" />
    <meta name="generator" content={Astro.generator} />
-    <title>Astro</title>
+    <title>{title}</title>
  </head>
````

Now we go back to each page:

_./src/pages/index.astro_

```diff
- <BaseLayout>
+ <BaseLayout title="Home">
    <h1>Dog Facts</h1>
```

\__./src/pages/about.astro_

```diff
-  <BaseLayout>
+  <BaseLayout title="About">
    <h1>About page</h1>
```

Now we can see that both titles are correct.

```bash
npm run dev
```
