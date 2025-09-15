# Components

Til now we have been coding everything in a single file, but as our app grows, we will want to break it down into smaller pieces.

Let's create a simple component, to display the dogs fact.

Create a new file in the `src/components` folder, called `DogFacts.astro`:

- This will receive the list of facts as a typed prop.
- It will loop through the facts and display them in a list.

```astro
---
interface Props {
  facts: string[];
}
const { facts } = Astro.props;
---
<ul>
  {facts.map((fact) => (
    <li>{fact}</li>
  ))}
</ul>
```

Let's use this component in our main page.

In `src/pages/index.astro`, import the component and use it:

```diff
---
+ import DogFacts from '../components/DogFacts.astro';
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

