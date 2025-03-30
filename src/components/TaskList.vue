<script lang="ts" setup>
import type { Task } from '../types';

const props = defineProps<{
    tasks: Task[];
}>();

const emits = defineEmits<{
    toggleDone: [id: string];
    deleteTask: [id: string];
}>();
</script>

<template>
    <TransitionGroup name="list" tag="div" class="task-list">
        <article v-for="task in props.tasks" class="task" :key="task.id">
            <label>
                <input @input="emits('toggleDone', task.id)" type="checkbox" :checked="task.done" />
                <span :class="{ done: task.done }">{{ task.title }}</span>
            </label>
            <button @click="emits('deleteTask', task.id)" class="btn outline">Delete</button>
        </article>
    </TransitionGroup>
</template>

<style>
.task-list {
    margin-top: 1rem;
}

.task {
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.btn {
    color: var(--pico-color-red-400);
    border-color: var(--pico-color-red-400);
}

.done {
    text-decoration: line-through;
}

/* Transition Group*/
.list-enter-active,
.list-leave-active {
    transition: all 0.5s ease;
}

.list-enter-from,
.list-leave-to {
    opacity: 0;
    transform: translateX(30px);
}
</style>