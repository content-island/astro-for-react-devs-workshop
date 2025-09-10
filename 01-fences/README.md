# Fences

Vamos a bucear en los componentes de Astro, si te fijas se parecen un poco a Vue, tienes código, HTML y estilos en un mismo archivo.

Vamos a hacer un prueba, vamos a cambiar el _h1_ de la página principal por un texto que tengamos definido en una variable.

_./src/pages/index.astro_

```diff
---
+ const title = "Hello React Alicante";
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
-		<h1>Astro</h1>
+    <h1>{title}</h1>
```

> El binding es exactamente igual al de React, usamos llaves para indicar que es una variable.

Si ejecutamos podemos ver el nuevo título.

```bash
npm run dev
```

¿Y aquí te estás preguntando que son las _Fences_? Es código que se ejecuta en servidor y que si estamos en modo SSG sólo se ejecuta una vez, cuando se genera el sitio.

Vamos a verlo de forma más claro, vamos a leer un valor random de una API y lo vamos a mostrar en la página.

Por ejemplo hay una api pública que devuelve un hecho sobre perros, vamos a usarla.

_./src/pages/index.astro_

```diff
---
+ const res = await fetch("https://dog-api.kinduff.com/api/facts");
+ const data = await res.json();
+ const title = data.facts[0];
- const title = "Hello React Alicante";
---
```

Let's check the result in the browser, we should see a random dog fact.

If we make a build, and check the files generated in _./dist/index.html_, we will see that the dog fact is already there, because it was fetched at build time.

```bash
npm run build
```

> Si estamos en modo SSR, este código se ejecutará en cada petición en servidor, nunca se ejecutará desde el navegador.

¿Y puedo ejecutar código en el navegador? Claro que sí, incluso puedes usar React, Vue o Svelte para hacerlo.

Vamos con un ejemplo sencillo, añadimos un botón que muestre un fact de gatos, el botón se va a llamar "Get Cat Fact".

_./src/pages/index.astro_

```diff
<html lang="en">
	<head>
		<meta charset="utf-8" />
		<link rel="icon" type="image/svg+xml" href="/favicon.svg" />
		<meta name="viewport" content="width=device-width" />
		<meta name="generator" content={Astro.generator} />
		<title>Astro</title>
	</head>
	<body>
		<h1>{title}</h1>
		<button id="cat-fact-button">Get Cat Fact</button>
		<p id="cat-fact"></p>
	</body>
</html>

+ <script>
+  document.getElementById("cat-fact-button").addEventListener("click", async () => {
+    const res = await fetch("https://catfact.ninja/fact");
+    const data = await res.json();
+    document.getElementById("cat-fact").innerText = data.fact;
+  });
+ </script>
```

Si ejecutamos podemos ver que al pulsar el botón se muestra un hecho sobre gatos.

```bash
npm run dev
```

Y ahora te surgirá una duda ¿Cómo puedo depurar esto?

Vamos a ver como depurar código entre fences y código del navegador.

Para depurar código entre fences:

- Ponemos un breakpoint en el código entre fences.
- Abrimos un terminal en modo JavaScript Debug terminal y ejecutamos:

```bash
npm run dev
```

Si te fijas al ejecutar el servidor se para en el breakpoint y podemos depurar.

Importante, en modo desarrollo en local, cada vez que recargues la página se volverá a ejecutar el código entre fences, pero esto sólo pasa en modo desarrollo, en producción sólo se ejecuta una vez y en tiempo de build.

¿Y cómo depuramos el código del navegador? Pues aquí como siempre, con las DevTools del navegador.