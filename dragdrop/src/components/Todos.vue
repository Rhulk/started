<script setup>
import { onMounted, ref, reactive } from "vue";
import InputNew from "./InputNew.vue";

//const count = ref(0);
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
function handleNewItem(text, board){
    board.items.push(
        { id: crypto.randomUUID(),title: text.value }
    )
}
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

function startDrag(evt, boardId, itemId) {
  console.log(boardId, itemId);
  evt.dataTransfer.dropEffect = "move";
  evt.dataTransfer.effectAllowed = "move";
  evt.dataTransfer.setData("item", JSON.stringify({ boardId, itemId }));
}
function onDrop(evt, boarDest) {
    // Recuperados los id's del board e item del evt.dataTransfer
  const { boardId, itemId } = JSON.parse(evt.dataTransfer.getData("item"));
  console.log({ boardId, itemId });
  //Se recuperan los objetos de los id's
  const board = boards.find((x) => x.id === boardId);
  const item = board.items.find((x) => x.id === itemId);
  console.log(board,item);
  // con filter quitamos el item de su board actual
  board.items = board.items.filter((x) => x.id !== item.id);
  // añadimos el item al nuevo board
  boarDest.items.push({ ...item });
}

</script>
<template>


    <div class="boards-container">
        <nav>
            <ul>
                <li><a href="#"  @click="createNewBoard"
                     >Create Board</a> </li>
            </ul>
        </nav>

        <div class="boards">
            <div class="board"
                @drop="onDrop($event, board)"
                @dragover.prevent
                @dragenter.prevent
                v-for="board in boards" :key="board.id"
            >
                {{ board.name }}
                <div class="input">
                    <InputNew @on-new-item="(text) => handleNewItem(text, board)" />
                </div>
                <div class="items">
                    <div class="item"
                        draggable="true"
                        @dragstart="startDrag($event, board.id, item.id)"
                        v-for="item in board.items" :key="item.id"
                >
                {{ item.title }}
                </div>
                </div>
            </div>
        </div>
    </div>
    

    
    
</template>

<style scoped>
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
nav {
    background: black;
    
}

nav ul {
    list-style: none;
    padding: 0;
    margin: 0;
    display: flex;
}
nav ul li a {
    display: block;
    padding: 10px;
    color: white;
    text-decoration: none;
}


</style>
