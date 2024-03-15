<script setup>
import ColumnView from "./ColumnView.vue";
import ColumnAdd from "./ColumnAdd.vue";
import TypeSelect from "./TypeSelect.vue";
import { useTodoStore } from "../stores/todo";

const todoStore = useTodoStore();

import { onMounted } from "vue";
onMounted(() => {
  if (localStorage.getItem("todo-lists") !== null) {
    todoStore.lists = JSON.parse(localStorage.getItem("todo-lists"));
  }
});

todoStore.$subscribe((mutation, state) => {
  todoStore.saveToLocalStorage();
});

function emptyToDoLists() {
  todoStore.$reset();
  // todoStore.saveToLocalStorage();
}
</script>
<template>
  <div class="ms-9 mt-3 font-serif font-bold text-xl">
  <h1>Todo list</h1>
</div>
  <div class="flex">
    <div class="px-8">
      <ColumnAdd></ColumnAdd>
      <TypeSelect
        @personal="personal"
        @work="work"
        @social="social"
        @important="important"
      ></TypeSelect>
    </div>
    <div class="h-screen border-2 border-gray-300"></div>
    <ColumnView v-for="list in todoStore.lists" :taskList="list"></ColumnView>
  </div>
  <footer>
    <button
      @click="emptyToDoLists"
      class="text-white bg-gradient-to-r from-red-500 via-red-600 to-red-700 hover:bg-gradient-to-br font-medium rounded-lg text-xl px-5 py-5 text-center mb-10 ms-5"
    >
      <i class="bi bi-trash3"></i>
      Reset
    </button>
  </footer>
</template>
./Select.vue