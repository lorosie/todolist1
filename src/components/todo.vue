<template>
    <main class="container">
        <article>
            <h3>事件清单</h3>
            <input type="text" v-model="newTodo" placeholder="请输入代办事项，回车确定" aria-invalid="false"
                @keyup.enter="add2todolist">
        </article>

        <details open>
            <summary>待办事项 ({{ pendingCount }})</summary>
            <!-- Checkboxes -->
            <fieldset>
                <label v-for="item in filterWaiting" :key="item.id">
                    <input type="checkbox" :id="'rb' + item.id" v-model="item.done" :name="'rb' + item.id">
                    {{ item.text }}
                    <a href="#" role="button" class="deletebtn" @click="removeItem(item.id)">x</a>
                </label>
            </fieldset>
        </details>

        <details open>
            <summary>已完成事项 ({{ doneCount }})</summary>
            <!-- Checkboxes -->
            <fieldset>
                <label v-for="item in filterDone" :key="item.id">
                    <input type="checkbox" :id="'rb' + item.id" v-model="item.done" :name="'rb' + item.id" disabled>
                    {{ item.text }}
                    <a href="#" role="button" class="deletebtn" @click="removeItem(item.id)">x</a>
                </label>
            </fieldset>
        </details>
    </main>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue';

let id = ref(0);
type TodoItem = {
    id: number;
    text: string;
    done: boolean;
};

const todos = ref<TodoItem[]>([]);
const newTodo = ref("");

function add2todolist() {
    if (newTodo.value.trim()) {
        todos.value.push({
            id: ++id.value,
            text: newTodo.value,
            done: false
        });
        newTodo.value = "";
    }
}

function removeItem(id: number) {
    todos.value = todos.value.filter(item => item.id !== id);
}

const filterWaiting = computed(() => todos.value.filter(item => !item.done));
const filterDone = computed(() => todos.value.filter(item => item.done));

const pendingCount = computed(() => filterWaiting.value.length);
const doneCount = computed(() => filterDone.value.length);
</script>

<style scoped>
.deletebtn {
    padding: 0px 5px;
    margin-left: 3em;
    font-size: 0.5em;
}
</style>