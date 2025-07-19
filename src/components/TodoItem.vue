<script setup>
import TodoButton from './TodoButton.vue';

const props = defineProps({
    todo: {
        type: Object,
        required: true,
    },
    index: {
        type: {
            type: Number,
            required: true,
        }
    }
});
defineEmits(["toggle-complete", "update-todo", "delete-todo"])
</script>


<template>
    <div class="m-1 px-3 rounded-md flex justify-between"
        :class="{ 'bg-thiredcolor ': todo.category == 'personal', ' bg-forthcolor': todo.category == 'Business' }">
        <label class="flex gap-3 my-1">
            <input :checked="todo.done" @input="$emit('toggle-complete', index)"
                :class="{ 'accent-maincolor': todo.category == 'Business', ' accent-secondcolor': todo.category == 'personal' }"
                type="checkbox" />

            <form @submit.prevent="$emit('update-todo', todo.content)">
                <input :class="{ 'line-through': todo.done }" class=" focus:border-none px-0.5" name="todo.content"
                    type="text" v-model="todo.content">
            </form>

        </label>
        <button class="self-end py-0.5 px-2 bg-red-500 rounded-xl cursor-pointer "
            @click="$emit('delete-todo', todo.createdAt)">
            delete
        </button>
    </div>

</template>
