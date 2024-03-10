# dragdrop

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

### clean project

Borramos los elementos creados de ejemplo para partir de cero.

### Proyecto construido con el patrón componente API

Lo primero crear el componente **Todo.vue** y lo vinculamos al app principal.

Ahora vamos a crear una variable reactiva. con **reactive()**

```js
// import necesarios para la reactividad
import { onMounted, ref, reactive } from "vue";

// objeto reactivo
let boards = reactive([
  {
    id: crypto.randomUUID(),
    name: "board-1",
    items: [{ id: crypto.randomUUID(), title: "Hola a todos" }],
  },
  {
    id: crypto.randomUUID(),
    name: "board-2",
    items: [{ id: crypto.randomUUID(), title: "Hola a todos 2" }],
  },
]);
```

Antes de pasar a la vista vamos a crear un input personalizado. **Inputnew.vue**

```js
<script setup>
import { onMounted, ref, reactive, defineProps } from "vue";

const text = ref("");
const emits = defineEmits(["onNewItem"]);

function handleKeyDown(evt) {
  if (evt.key === "Enter") {
    emits("onNewItem", text);
    text.value = "";
  }
}
</script>

<template>
New todo: <input v-model="text" @keydown="handleKeyDown" /></template>

<style scoped></style>
```

Con **v-model** lo conectamos con la variable **const text** y con **@keydown** controlamos el evento del intro desde **handleKeyDown(evt)** por ultimo exportamos el valor del input con **emits("onNewItem",text)** esto lo recuperaremos allí donde usemos el **<inputNew />**

Y este seria el ejemplo de uso:

```js
<InputNew @on-new-item="(text) => handleNewItem(text, board)" />
```

```js
function handleNewItem(text, board) {
  board.items.push({ id: crypto.randomUUID(), title: text.value });
}
```

Así seria como utilizaremos el componente **<InputNew/>** recuperamos el valor del input que esta en la variable **text** que recuperamos con el **emit** y el objeto **board** correspondiente que veremos acontinuación como lo recuperamos.

Ahora definimos la estructura basica de la vista o template.

```html
<div class="boards-container">
  <nav>
    <ul>
      <li><a href="#">Create Board</a></li>
    </ul>
  </nav>

  <div class="boards">
    <div class="board" v-for="board in boards" :key="board.id">
      {{ board.name }}
      <div class="input">
        <InputNew @on-new-item="(text) => handleNewItem(text, board)" />
      </div>
      <div class="items">
        <div class="item" v-for="item in board.items" :key="item.id">
          {{ item.title }}
        </div>
      </div>
    </div>
  </div>
</div>
```

Se define de la siguiente manera, un contenedor principal **boards-container** que se divide en 2 un **nav** que contine un enlace para la creación de nuevos **boards** el cual ya veremos como lo hacemos y el contenedor de **boards**

Este contenedor se divide tambien en 2 nuevos contenedores:
El primero el **board** que con la directiva **v-for** creara la iteración de board.

El segundo sera igual pero para los **item** de cada board importantes **board.items** para que sean los item del board en el que estemos y la **:key** en cada buble.

Ahora vamos a dar algo de estilos a los board para lograr que se vean como queremos.

Con la clase **.boards** vamos añadir un **display: flex** para simular celdas con cada board y con **gap** la separación entre ellas.

En cada celda o **.board** le aplicamos un fondo gris visualizar dicho elemento y un padding para centrar su texto y mejorar la visualización

Con la misma teoria aplicamos un fondo blanco a los **.item** para resaltarlos sobre el board gris.

Tambien para lograr separar los item usaremos la clase **.items** y le aplicaremos un **display: flex** **flex-direction: colum** y el **gap** de 5 pixel.

```css
.boards {
  display: flex;
  gap: 10px;
}
.board {
  background: #ccc;
  padding: 10px;
}
.items {
  display: flex;
  flex-direction: column;
  gap: 5px;
}
.item {
  background: rgb(232, 227, 227);
}
```

Ahora implementamos la creación de nuevos tableros, para ello aplicamos al enlace del **na** **@click="createNewBoard"** y desde dicho metodo recuperaremos el nuevo nombre y lo añadimos al listado de objetos.


```html
        <nav>
            <ul>
                <li><a href="#"  @click="createNewBoard"
                     >Create Board</a> </li>
            </ul>
        </nav>
```

```js
function createNewBoard(){
    const name = prompt("Name of board");
  if (name) {
    const board = {
      id: crypto.randomUUID(),
      name: name,
      items: [],
    };

    boards.push(board);
  }   
}
```

