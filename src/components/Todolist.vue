<template>
    <div>
        <input type="text" v-model="title" @keydown.enter="addTodo" />
        <button v-if="active < all" @click="clear">清理</button>
        <ul v-if="todos.length">
            <li v-for="todo in todos" :key="todo.title">
                <input type="checkbox" v-model="todo.done">
                <span :class="{done: todo.done }">{{todo.title}}</span>
            </li>
        </ul>
        <div v-else>暂无数据</div>
        <div>
            全选<input type="checkbox" v-model="allDone" />
            <span>{{active}} / {{all}}</span>
        </div>
    </div>
    <div>
        <span>{</span>
        {{x}} , {{y}}
        <span>}</span>
    </div>
</template>

<script setup>
import {computed, ref} from "vue"

import { useMouse } from '../utils/mouse';


let title = ref("");
let todos = ref([
    {
        title: "学习",
        done: false,
    }
]);

function addTodo() {
    if (title.value) {
        todos.value.push({
            title: title.value,
            done: false,
        });
        title.value = "";
    }
}

function clear() {
    todos.value = todos.value.filter(v=>!v.done);
}

let active = computed(() => {
    return todos.value.filter(v=>!v.done).length;
})

let all = computed(() => {
    return todos.value.length;
})

let allDone = computed({
    get: function () {
        return active.value === 0;
    },
    set: function (value) {
        todos.value.forEach(todo => {
            todo.done = value;
        })
    },
})

let {x, y} = useMouse()

</script>

<style>
  .done {
    color:gray;
    text-decoration: line-through;
  }

</style>