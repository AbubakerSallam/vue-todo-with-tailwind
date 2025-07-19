<script setup>
import TodoButton from '@/components/TodoButton.vue';
import { defineEmits, reactive, ref } from 'vue';
const emit = defineEmits(["add-todo"]);

const todoState = reactive({
    todo: "",
    category: "",
    invalid: null,
    errMsg: "",
});
const createTodo = () => {

    todoState.invalid = null;
    if (todoState.todo !== "" && todoState.category != "") {
        emit("add-todo", todoState.todo, todoState.category)
        todoState.todo = "";
        todoState.errMsg = "";
        todoState.category = "";
        return;
    }
    todoState.invalid = true;
    todoState.errMsg = "todo or category Can't be empty";
}
</script>
<template>
    <div class=" ml-6">
        <h3 class="font-bold">CREATE A TODO</h3>
        <form>
            <h4 class="p-1 ml-5 text-m">What's in your todo list?</h4>
            <div class="block mx-auto text-center">
                <input class="px-2 py-1 border rounded-xl border-slate-300 " :class="{
                    ' border-red-500': todoState.invalid,
                    ' border-indigo-500': todoState.todo != ''
                }" type="text" placeholder="e.g. make an app" v-model="todoState.todo">
                <p class="my-2 text-red-400" v-show="todoState.invalid"> {{ todoState.errMsg }}</p>
            </div>
            <div class="flex gap-3 m-7">
                <label class="flex flex-1 justify-center items-center gap-4  "
                    :class="{ 'border border- rounded-md border-maincolor  bg-slate-100  ': todoState.category == 'Business' }">
                    <input class=" accent-maincolor" type="radio" name="category" value="Business"
                        v-model="todoState.category">
                    <span>Business</span>

                </label>
                <label class=" flex rounded-md py-5 flex-1 justify-center items-center gap-4"
                    :class="{ 'border border- rounded-md border-secondcolor  bg-slate-100': todoState.category == 'personal' }">
                    <input class=" accent-secondcolor" type="radio" name="category" value="personal"
                        v-model="todoState.category">
                    <span>Personal</span>
                </label>
            </div>
            <div class="flex ">
                <TodoButton class="flex-1" @click.prevent=" createTodo()">create a todo</TodoButton>
            </div>
        </form>

    </div>
</template>
