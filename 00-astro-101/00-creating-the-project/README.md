# Astro project creation and structure

## Prerequisites

Astro plugin VSCode installed.

## Creating a new Astro project

To create a new Astro project, you can use the following command in your terminal:

```bash
npm create astro@latest
```

It will prompt you to choose:

- Destination folder.
- A template (you can choose "Minimal / empty" for a simple starting point).
- Whether to install dependencies right away (choose "Yes" for convenience), YES
- Whether to add Git (choose "Yes" if you want version control).

So far so good :).

Let's run the project

```bash
npm run dev
```

## Analyzing the project structure

Let's analyze the project structure.

```
blank-project/
├── public/             # Static assets (copied directly into final dist build folder)
├── src/
│ └── pages/            # Application pages (routes)
│ └── index.astro.      # Main page
├── astro.config.mjs.   # Astro configuration
├── package.json
├── tsconfig.json
└── README.md
```

Let's build the project

```bash
npm run build
```

We get the dist assets and a HTML (one HTML per page, this is not SPA :)).

## Prettier

Before going further, let's install Prettier to format our code.

```bash
npm install --save-dev prettier
```

```bash
npm install prettier-plugin-astro --save-dev
```

Create a `.prettierrc` file in the root of the project with the following content:

```json
{
  "plugins": ["prettier-plugin-astro"],
  "overrides": [
    {
      "files": "*.astro",
      "options": {
        "parser": "astro"
      }
    }
  ]
}
```
