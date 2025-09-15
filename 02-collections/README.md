# Collections

What if we want to display a collection of items? That's a React piece of cake. We can use the JavaScript `map` function to transform an array of data into an array of React elements.

Vamos a cambiar la petición a la API para que nos devuelva varios datos. En este caso, vamos a pedir 5 datos de perros.

```diff
---
- const res = await fetch("https://dogapi.dog/api/v2/facts");
+ const res = await fetch("https://dogapi.dog/api/v2/facts?limit=5");
const response = await res.json();
-const title = response?.data[0]?.attributes?.body ?? "Ooops api not working?";
+ const facts = response?.data ?? [];
---
```

Y en el markup:

```diff
  <body>
-    <h1>{title}</h1>
+ 	<h1>Dog Facts</h1>
+    <ul>
+      {facts.map((fact) => (
+        <li>{fact.attributes.body}</li>
+      ))}
+    </ul>
    <button id="cat-fact-button">Get Cat Fact</button>
    <h2 id="cat-fact"></h2>
  </body>

```

