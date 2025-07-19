<script setup>
import { ref, onMounted, computed, watch } from 'vue';
import TodoHome from './TodoHome.vue';
import TodoItem from '@/components/TodoItem.vue';

const todos = ref([]);
const name = ref('');



const todos_asc = computed(() => todos.value.sort((a, b) => {
    return a.createdAt - b.createdAt
}))

const addContent = (contentToAdd, categoryToAdd) => {
    if (contentToAdd.trim() === '' || categoryToAdd.trim() === ''
    ) {
        return;
    }
    todos.value.push({
        content: contentToAdd,
        category: categoryToAdd,
        done: false,
        createdAt: new Date().getTime(),
    })
}
const toggleComplete = (todoPos) => {

    todos_asc.value[todoPos].done = !todos_asc.value[todoPos].done;
    todos.value = todos_asc.value;
}
const updateTodo = (todoPos, todovale) => {
    todos.value[todoPos].todo = todovale;
}

const deleteTodo = (todocreatedAt) => {
    todos.value = todos.value.filter((todo) => todo.createdAt !== todocreatedAt);
}
watch(todos, newval => {
    localStorage.setItem('todos', JSON.stringify(newval))
}, { deep: true })
watch(name, (newVal) => {
    localStorage.setItem('name', newVal);
})
onMounted(() => {
    name.value = localStorage.getItem('name') || '';
    const savedTodos = localStorage.getItem('todos');
    if (savedTodos) {
        try {
            todos.value = JSON.parse(savedTodos);
        } catch (e) {
            console.error('Failed to parse todos from localStorage:', e);
            todos.value = [];
        }
    }
});
</script>

<template>
    <main class="app my-9">
        <section class="m-4 text-xl">
            <h2>
                What's up , <input type="text" v-model="name" class="text-2xl">
            </h2>
        </section>
        <TodoHome @add-todo="addContent" />
        <div class="flex flex-col mt-5 ml-5">
            <h3> TODO list</h3>

            <TodoItem v-for="(todo, index) in todos_asc" :todo="todo" :index="index" @toggle-complete="toggleComplete"
                @update-todo="updateTodo" @delete-todo="deleteTodo" />


        </div>
    </main>
</template>