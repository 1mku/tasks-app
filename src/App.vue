<script setup lang="ts">
import { computed, ref } from 'vue';
import type { Task, TaskFilter } from './types';
import TaskForm from './components/TaskForm.vue';
import TaskList from './components/TaskList.vue';
import FilterButton from './components/FilterButton.vue';
import logo from './assets/vue.svg';

const tasks = ref<Task[]>([]);
const filter = ref<TaskFilter>('all');

const totalDone = computed(() => tasks.value.reduce((total, task) => total + (task.done ? 1 : 0), 0));
const filteredTasks = computed(() => {
    switch (filter.value) {
        case 'all':
            return tasks.value;
        case 'todo':
            return tasks.value.filter((task) => !task.done);
        case 'done':
            return tasks.value.filter((task) => task.done);
        default:
            return tasks.value;
    }
});


function addTask(newTask: string) {
    console.log(newTask);
    tasks.value.push({ id: crypto.randomUUID(), title: newTask, done: false });
}

function toggleDone(id: string) {
    const task = tasks.value.find((task) => task.id === id);
    if (!task) return;
    task.done = !task.done;
}

function deleteTask(id: string) {
    const index = tasks.value.findIndex((task) => task.id === id);
    if (index === -1) return;
    tasks.value.splice(index, 1);
}

function setFilter(value: TaskFilter) {
    filter.value = value;
}

</script>

<template>
    <main>
        <div class="header">
            <img :src="logo" alt="Vue logo" class="logo" />
            <h1>Tasks App</h1>
        </div>
        <TaskForm @add-task="addTask" />
        <h3 v-if="!tasks.length">Add a task to get started.</h3>
        <h3 v-else>{{ totalDone }} / {{ tasks.length }} tasks completed</h3>
        <div v-if="tasks.length" class="button-container">
            <FilterButton :current-filter="filter" :filter="'all'" @set-filter="setFilter" />
            <FilterButton :current-filter="filter" :filter="'todo'" @set-filter="setFilter" />
            <FilterButton :current-filter="filter" :filter="'done'" @set-filter="setFilter" />
        </div>

        <TaskList :tasks="filteredTasks" @toggle-done="toggleDone" @delete-task="deleteTask" />
    </main>
</template>

<style>
main {
    max-width: 800px;
    margin: 1rem auto;
}

.header {
    display: flex;
    align-items: center;
    gap: 1rem;
    margin-bottom: 1rem;

    h1 {
        margin: 0;
    }
}

.button-container {
    display: flex;
    justify-content: flex-end;
    gap: 0.5rem;
}
</style>