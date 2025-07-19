<script setup>
import { ref, onMounted, computed, watch } from 'vue';
import TodoView from './views/TodoView.vue';

const todos = ref([]);
const name = ref('');

const input_content = ref([]);
const input_category = ref(null)

const todos_asc = computed(() => todos.value.sort((a, b) => {
  return a.createdAt - b.createdAt
}))


watch(name, (newVal) => {
  localStorage.setItem('name', newVal);
})
onMounted(() => {
  name.value = localStorage.getItem('name') || '';
})
</script>

<template>
  <main class="app my-9">
    <section class="m-4 text-xl">
      <h2>
        What's up , <input type="text" v-model="name" class="text-2xl">
      </h2>
    </section>
    <TodoView />
  </main>
</template>
