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

</script>
<template>


    <div class="boards-container">
        <nav>
            <ul>
                <li><a href="#" >Create Board</a> </li>
            </ul>
        </nav>

        <div class="boards">
            <div class="board"
                v-for="board in boards" :key="board.id"
            >
                {{ board.name }}
                <div class="input">
                    <InputNew @on-new-item="(text) => handleNewItem(text, board)" />
                </div>
                <div class="items">
                    <div class="item"
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


</style>
