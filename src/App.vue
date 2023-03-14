<template>
  <div class="container mx-auto px-10 md:px-0">
    <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
      <div></div>
      <div>
        <div class="mt-10">
          <h1 class="text-5xl font-semibold text-gray-600 text-center">
            Todo List
          </h1>
          <div class="mt-6">
            <form @submit.prevent="addTodo()">
              <div class="grid grid-cols-1 gap-4">
                <div>
                  <input
                    class="border-2 rounded-lg outline-none py-2 px-2 shadow-md font-medium w-full border-blue-300 hover:border-blue-600 focus:border-blue-600 focus:shadow-blue-200"
                    v-model="newTodo"
                  />
                </div>
                <div>
                  <div class="grid grid-cols-2 gap-2">
                    <div>
                      <button
                        class="text-white py-2 px-4 shadow-md w-full rounded-lg bg-red-400 hover:bg-red-600 font-semibold"
                        v-if="todos.length !== 0"
                        @click="removeAllTodos"
                      >
                        Remover todas as Tarefas
                      </button>
                    </div>
                    <div>
                      <button
                        class="text-white py-2 px-4 shadow-md w-full rounded-lg bg-blue-400 hover:bg-blue-600 font-semibold"
                      >
                        Adicionar Tarefas
                      </button>
                    </div>
                  </div>
                </div>
              </div>
            </form>
          </div>
          <div class="mt-8 text-center">
            <div class="grid grid-cols-1 gap-3">
              <div>
                <div class="h-full" v-if="todos.length === 0">
                  <p class="text-gray-400">Cadastre suas tarefas.</p>
                </div>
              </div>
              <div
                class="rounded-lg shadow-md p-3 h-full w-full text-gray-600 text-lg font-semibold cursor-pointer"
                :class="
                  todo.complete == true
                    ? 'bg-orange-300 hover:bg-orange-400'
                    : 'bg-teal-500 line-through hover:bg-teal-600'
                "
                v-for="(todo, index) in todos"
                :key="index"
                @click="completedTodo(todo)"
              >
                <span
                  class="text-gray-600 text-lg font-semibold grid justify-items-center"
                  >{{ todo.text }}</span
                >
              </div>
            </div>
          </div>
        </div>
      </div>
      <div></div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
const newTodo = ref("");
let storedTodos;
localStorage.getItem("todos")
  ? (storedTodos = JSON.parse(localStorage.getItem("todos")))
  : (storedTodos = []);
const todos = ref(storedTodos);
function addTodo() {
  if (newTodo.value !== "") {
    todos.value.push({ complete: false, text: newTodo.value });
    newTodo.value = "";
  }
}
function removeAllTodos() {
  todos.value.splice(0, todos.value.length);
  updateStorage();
}
function completedTodo(todo) {
  todo.complete = !todo.complete;
  updateStorage();
}
function updateStorage() {
  localStorage.setItem("todos", JSON.stringify(todos.value));
}
</script>

<style>
.completed {
  text-decoration: line-through;
  @apply bg-teal-500;
}
</style>
