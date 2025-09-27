# Styles

Let's get started with Styling, if we go for standard CSS:

- We can have a global CSS file.
- We can have component-level CSS files (scoped to the component).

Astro also supports Tailwind and there's a plugin for it.

## Global CSS

Let's add a global CSS file.

_./src/styles.css_

```css
/* =========================
   Basic global styles
   ========================= */

/* Light reset */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

/* Global font */
body {
  font-family: Arial, Helvetica, sans-serif;
  font-size: 16px;
  line-height: 1.5;
  color: #333;
  background-color: #f9f9f9;
}

/* Headings */
h1,
h2,
h3,
h4,
h5,
h6 {
  font-weight: bold;
  color: #222;
  margin-bottom: 0.5em;
}

/* Paragraphs */
p {
  margin-bottom: 1em;
}

/* Links */
a {
  color: #007bff;
  text-decoration: none;
}
a:hover {
  text-decoration: underline;
}

/* Buttons */
button {
  cursor: pointer;
  padding: 0.5em 1em;
  border: none;
  border-radius: 6px;
  background-color: #007bff;
  color: white;
  font-size: 1em;
  transition: background-color 0.3s;
}
button:hover {
  background-color: #0056b3;
}

/* General container */
.container {
  width: 90%;
  max-width: 1200px;
  margin: 0 auto;
}
```

And use it in the main page:

_./src/pages/index.astro_

```diff
---
+ import "../styles.css";
import DogFacts from "../components/DogFact.astro";

const res = await fetch("https://dogapi.dog/api/v2/facts?limit=5");
const response = await res.json();
const data = response?.data ?? [];
const facts: string[] = data.map((item: any) => item.attributes.body);
---
```

Now let's style the dogs facts components.

Let's add this styles at the bottom of the dog fact astro component

_./src/components/DogFacts.astro_

```astro
<style>
  ul {
    list-style: none;
    padding: 1rem;
    border-radius: 8px;
    background: #f0f4ff;
    box-shadow: 0 2px 6px rgba(0, 0, 0, 0.1);
  }

  li {
    padding: 0.5rem 0;
    border-bottom: 1px solid #ddd;
    color: #333;
    font-size: 1rem;
  }

  li:last-child {
    border-bottom: none;
  }
</style>
```

If we open the devtools you will see that the styles are scoped to the component.
