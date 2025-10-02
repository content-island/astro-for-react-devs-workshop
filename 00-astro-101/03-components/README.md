# Components

Til now we have been coding everything in a single file, but as our app grows, we will want to break it down into smaller pieces.

Let's create a simple component, to display the dogs fact.

Create a new folder in `src` called `components`. And a new file in the `src/components` folder, called `DogFacts.astro`:

- This will receive the list of facts as a typed prop.
- It will loop through the facts and display them in a list.

_./src/components/DogFacts.astro_

```astro
---
interface Props {
  facts: string[];
}
const { facts } = Astro.props;
---

<ul>
  {facts.map((fact) => <li>{fact}</li>)}
</ul>
```

Let's use this component in our main page.

In `src/pages/index.astro`, import the component and use it:

```diff
---
+ import DogFacts from './components/DogFacts.astro';
const res = await fetch("https://dogapi.dog/api/v2/facts?limit=5");
const response = await res.json();
const data = response?.data ?? [];
const facts : string[] = data.map((item: any) => item.attributes.body);
---
```

```diff
  <body>
 	<h1>Dog Facts</h1>
-    <ul>
-      {facts.map((fact : string) => (
-        <li>{fact}</li>
-      ))}
-    </ul>
+    <DogFacts facts={facts} />
    <button id="cat-fact-button">Get Cat Fact</button>
    <h2 id="cat-fact"></h2>
  </body>
```

What’s different compared to React props? The key point is that this only runs once, and once we’re on the client side, there’s no way to update the props. For example, this approach wouldn’t work as-is for the cat fact button we built earlier.

And what if we need a component that changes? That’s where Client Islands come into play. We can create a Client Island with React, Svelte, Vue, or any other supported framework, and we can decide whether it runs only on the server or enables client-side execution. We’ll see this in more detail later.
