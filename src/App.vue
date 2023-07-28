<script setup>
import { ref, onMounted, computed, watch } from "vue";

const todos = ref([]);
const name = ref("");
const inputContent = ref("");
const inputCategory = ref("");

const todos_asc = computed(() =>
  todos.value.sort((a, b) => b.createdAt - a.createdAt)
);
const todos_desc = computed(() =>
  todos_desc.value.sort((a, b) => a.createdAt - b.createdAt)
);

const addTodo = () => {
  if (inputContent.value.trim() === "" || inputCategory.value === "") {
    return;
  }

  todos.value.push({
    content: inputContent.value,
    category: inputCategory.value,
    done: false,
    createdAt: new Date().getTime(),
  });

  inputContent.value = "";
  inputCategory.value = "";
};

const removeTodo = (todo) => {
  todos.value = todos.value.filter((t) => t !== todo);
};

watch(
  todos,
  (newVal) => {
    localStorage.setItem("todos", JSON.stringify(newVal));
  },
  { deep: true }
);

watch(name, (newVal) => {
  localStorage.setItem("name", newVal);
});

onMounted(() => {
  name.value = localStorage.getItem("name") || "";
  todos.value = JSON.parse(localStorage.getItem("todos")) || [];
});
</script>

<template>
  <main class="app">
    <section class="greeting">
      <h2 class="title">
        Welcome, <input type="text" placeholder="Name here" v-model="name" />
      </h2>
    </section>

    <section class="create-todo">
      <h3>CREATE A TODO</h3>

      <form @submit.prevent="addTodo">
        <h4>Add your todo list</h4>
        <input
          type="text"
          v-model="inputContent"
          placeholder="e.g buy groceries"
        />

        <h4>Add a category</h4>

        <div class="options">
          <label>
            <input
              type="radio"
              name="category"
              value="business"
              v-model="inputCategory"
            />
            <span class="bubble business"></span>
            <div>Work</div>
          </label>

          <label>
            <input
              type="radio"
              name="category"
              value="personal"
              v-model="inputCategory"
            />
            <span class="bubble personal"></span>
            <div>Personal</div>
          </label>
        </div>

        <input type="submit" value="Add todo" />
      </form>
    </section>

    <div class="todo-list">
      <h3>YOUR TODO LIST</h3>
      <div class="list">
        <div
          v-for="todo in todos_asc"
          :class="`todo-item ${todo.done && 'done'}`"
        >
          <label>
            <input type="checkbox" v-model="todo.done" />
            <span :class="`bubble ${todo.category}`"></span>
          </label>
          <div class="todo-content">
            <input type="text" v-model="todo.content" />
          </div>

          <div class="actions">
            <button class="delete" @click="removeTodo(todo)">Delete</button>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped></style>
