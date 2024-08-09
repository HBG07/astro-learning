# Astro Starter Kit: Basics

```sh
npm create astro@latest -- --template basics
```

[![Open in StackBlitz](https://developer.stackblitz.com/img/open_in_stackblitz.svg)](https://stackblitz.com/github/withastro/astro/tree/latest/examples/basics)
[![Open with CodeSandbox](https://assets.codesandbox.io/github/button-edit-lime.svg)](https://codesandbox.io/p/sandbox/github/withastro/astro/tree/latest/examples/basics)
[![Open in GitHub Codespaces](https://github.com/codespaces/badge.svg)](https://codespaces.new/withastro/astro?devcontainer_path=.devcontainer/basics/devcontainer.json)

> 🧑‍🚀 **Seasoned astronaut?** Delete this file. Have fun!

![just-the-basics](https://github.com/withastro/astro/assets/2244813/a0a5533c-a856-4198-8470-2d67b1d7c554)

## 🚀 Project Structure

Inside of your Astro project, you'll see the following folders and files:

```text
/
├── public/
│   └── favicon.svg
├── src/
│   ├── components/
│   │   └── Card.astro
│   ├── layouts/
│   │   └── Layout.astro
│   └── pages/
│       └── index.astro
└── package.json
```

Astro looks for `.astro` or `.md` files in the `src/pages/` directory. Each page is exposed as a route based on its file name.

There's nothing special about `src/components/`, but that's where we like to put any Astro/React/Vue/Svelte/Preact components.

Any static assets, like images, can be placed in the `public/` directory.

## 🧞 Commands

All commands are run from the root of the project, from a terminal:

| Command                   | Action                                           |
| :------------------------ | :----------------------------------------------- |
| `npm install`             | Installs dependencies                            |
| `npm run dev`             | Starts local dev server at `localhost:4321`      |
| `npm run build`           | Build your production site to `./dist/`          |
| `npm run preview`         | Preview your build locally, before deploying     |
| `npm run astro ...`       | Run CLI commands like `astro add`, `astro check` |
| `npm run astro -- --help` | Get help using the Astro CLI                     |

## 👀 Want to learn more?

Feel free to check [our documentation](https://docs.astro.build) or jump into our [Discord server](https://astro.build/chat).


# Tutorial

## Pages
Astro puede crear paginas dinámicas en su ruta pages:
Aquí puedes referirte a la carpeta actual con el archivo index.astro

path|url
:-----:|:-----:
`pages/hello/index.astro` | `http://localhost:4321/hello`

Puedes crear rutas dinámicas:
Agregando `[param].astro` donde param es el parámetro a ingresar, una vez dentro del código se puede recuperar con `Astro.param`

Estas rutas dinámicas se pueden utilizar de manera estática para mejorar el rendimiento de la web, es decir, son dinámicas en el código pero se hace estáticas una vez se compile el mismo. Esto se puede lograr con la función `getStaticPaths()`:

```typescript
getStaticPaths(){
  return [
    { params: { tag: "successes" } },
    { params: { tag: "hello" } },
    { params: { tag: "blogging" } },
  ];
}
```
El código anterior hace que la ruta dinámica `[tag].astro` solo soporte esos tres valores. Esto al compilarse del lado del cliente (static) es muy rápido.

También se pueden hacer rutas dinámicas que funcionen del lado del servidor, estas pasan a ser un poco mas lentas pero nos ahorramos algo de código. Solo debemos de modificar una opción en `astro.config.mjs`

```typescript
// we have hybrid, static and server option
export default defineConfig({
  output: 'server', // static by default
});
```
De esta manera realizamos el manejo de rutas del lado del servidor y no tenemos que utilizar la función `getStaticPaths()`. La opción híbrida es para realizar la compilación en el lado cliente pero en caso de que aparezcan nuevas paginas se compilan automáticamente en el servidor.

Puedes crear paginas con md, a las mismas se les puede poner en la sección principal propiedades. Estos archivos md pueden ser recuperadas con `Astro.glob('path')`
Una vez recuperados en forma de arreglo cada uno de sus elementos tienen la propiedad de `frontmatter` es decir `elemento.frontmatter` la cual es un objeto con todas las propiedades declaradas en el md.
Ejemplo aquí muestro un md file

```markdown
---
title: 'My First Blog Post'

---

# Hello World
```
Este código tiene la propiedad title la cual puede ser accedida de la siguiente manera

```typescript
const mdFiles = Astro.glob('post/*.md');
const mdTitles = mdFiles.map((md) => md.frontmatter.title);
console.log(mdTitles);
```

## Components
Se pueden crear componentes en la sección de components los cuales tienen como objetivo reutilizarse en el código de la forma `<ComponentName />`

`components/HelloWorld.astro`
```typescript
---
---

<main>Hello World</main>
```

`pages/index.astro`
```typescript
---
import HelloWorld from 'components/HelloWorld.astro'
---

<div>
  <HelloWorld />
</div>
```

## Layouts
Los Layouts son como componentes utilizados como contenedores los cuales se utilizan para agrupar muchos componentes o HTML de manera general con piezas llamadas slots los cuales permiten insertar elementos dentro de los mismos. Estos slots se pueden utilizar dentro de cualquier componente.