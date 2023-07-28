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
              value="work"
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

        <button type="submit">Add todo</button>
      </form>
    </section>
  </main>
</template>

<style scoped></style>
