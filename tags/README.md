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

Ahora desde el scritp del propio componente definimos el modelo de datos, el valor del input **currentValue** y el array **tags**.

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
Despues creamos el apartado **methods** donde se definen los diferentes metodos que podemos usar como **handleSubmit()** aqui añadiremos el valor del input al array.

```js
  methods:{

    handleSubmit(){
        if (this.currentValue != ""){
            const exist = this.tags.some( item => item == this.currentValue);
            if (!exist)
            this.tags.push(this.currentValue);
            this.currentValue="";
        }       
    }


  },

```
Ademas para que el evento funcione necesitamos asegurarnos que el form tiene el siguiente codigo que lanzara el evento.

```html
<form @submit.prevent="handleSubmit">
```
La clausula **.prevent** evita el comportamiento normal del submint y permite lanzar correctamente el evento anterior.

El siguiente paso sera implementar el borrado de tag.<br> 
Para ello crearemos el metodo **deleteTag(tag)** :

```js
    deleteTag(tag){
        this.tags = this.tags.filter(item => item != tag);
    },
```
Usaremos el propiedad **.filter** para dejar fuera del array el tag a eliminar. Y desde el **<button>** llamar al metodo anterior con el siguiente codigo:

```html
<button @click="deleteTag(tag)">X</button>
```

Ahora vamos a implementar el borrado con la tecla de retroceso. Para ello vamos tenemos que controlar que se pulsa sobre el input, mediante **@keydown="handlekeydown"**

```html
          <input class="input" type="text" v-model="currentValue"
           @keydown="handlekeydown">
```
Controlamos si se pulsa la tecla de retroceso y el valor del input esta vacio se borra el ultimo valor del array.
```js
    handlekeydown(e){

        if (e.key == "Backspace" && this.currentValue == ""){
            this.tags.pop();
        }
    },
```

Con los siguientes estilos simulamos que los tag estan dentro del input:
```css
<style>
.inputTag {
    display: inline-flex;
    border: solid 1px #000;
    border-radius: 3px;
    height: 43px;
}

.tags {
    display: flex;
    gap: 3px;
    padding: 5px;
}
.tags .tag{
    display: flex;
    padding: 5px;
    border: solid 1px #ccc;
    gap: 5px;
    align-content: center;
    border-radius: 3px;
}

.inputTag .input{
    border: none;
    outline: none;
    padding: 0 5px;
}
.inputTag form{
    display: inline-flex;
}

.tag button{
    background-color: transparent;
    border: none;
    border-radius: 3px;
    cursor: pointer;
}
.tag button:hover{
    background-color: #ccc;
}

</style>
```
Por ultimo falta implementar la  gestión del componente, para ello añadiremos las siguientes modificaciones:

**emits: ["onTagsChange"],**
```js
<script>
export default {
    emits: ["onTagsChange"],
    data() {
...
```
**this.$emit('onTagsChange',this.tags);**
```js
    deleteTag(tag){
        this.tags = this.tags.filter(item => item != tag);
        this.$emit('onTagsChange',this.tags);
    },
     
    handlekeydown(e){

        if (e.key == "Backspace" && this.currentValue == ""){
            this.tags.pop();
            this.$emit('onTagsChange',this.tags);
        }
    },
    handleSubmit(){
        if (this.currentValue != ""){
            const exist = this.tags.some( item => item == this.currentValue);
            if (!exist)
            this.tags.push(this.currentValue);
            this.currentValue="";
            this.$emit('onTagsChange',this.tags);
        }       
    }
```
Con estas modificaciones podremos recuperar los valores del imput allí donde usamos el componente

Ahora ya solo falta usar el componente desde App.vue 

En el script importaremos el componente:

```js
<script>
import InputTag from './components/inputTag.vue'

export default {
  components: { 
    InputTag,
   },

```
Ahora ya podemos usar el componente:

```html
<template>
  <inputTag /> Input component
</template>
```
Pero para poder recuperar los datos del input necesitamos hacer algo mas, **@onTagsChange="handleOnTagsChange"** este es el metodo que hemos definido en el componente para recuperar los tag

```html
<inputTag @onTagsChange="handleOnTagsChange"/> Input component
```

Y este es el metodo donde recuperaremos los tag:

```js
  methods:{
    handleOnTagsChange(tags){
      console.log(tags)
    },
  },
````










