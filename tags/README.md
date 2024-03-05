# Input tag componet

This template should help get you started developing with Vue 3 in Vite.

## Recommended IDE Setup

[VSCode](https://code.visualstudio.com/) + [Volar](https://marketplace.visualstudio.com/items?itemName=Vue.volar) (and disable Vetur) + [TypeScript Vue Plugin (Volar)](https://marketplace.visualstudio.com/items?itemName=Vue.vscode-typescript-vue-plugin).

## Customize configuration

See [Vite Configuration Reference](https://vitejs.dev/config/).

## Project Setup

```sh
npm install
```

### Compile and Hot-Reload for Development

```sh
npm run dev
```

### Compile and Minify for Production

```sh
npm run build
```

## Paso a paso.

## 1.- Estructura del componente basada en objetos
Primero definimos la **estructura** base sobre la que vamos a construir el input.
```html
<template>
  
</template>

<script>
export default {

}
</script>

<style>

</style>
```

A continuación pasamos a **crear** la vista sobre la etiqueta <**template**>.

```html
      <div class="inputTag">
        <div class="tags">
            <div class="tag" v-for="(tag, index) in tags" :key="index">
                {{ tag }} <button >X</button>
            </div>
        </div>
        <form >
          <input class="input" type="text" >
        </form>
```

La idea funcional es la siguiente, desde el **input** iremos guardando los tags en un **array** y los mostraremos con la directiva **v-for**

Ahora desde el scritp del propio componente definimos el modelo de datos, el valor del input y el array.

```js
<script>
export default {

    data() {
        return {
            currentValue: "",
            tags: [],
        };
  },


};
</script>

```








