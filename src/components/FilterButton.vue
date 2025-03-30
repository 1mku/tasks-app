<script setup lang="ts">
import type { TaskFilter } from '../types';

const props = defineProps<{
    filter: TaskFilter;
    currentFilter: TaskFilter;
}>();

const emits = defineEmits<{
    setFilter: [filter: TaskFilter];
}>();
</script>

<template>
    <button :class="{
        contrast: props.currentFilter === props.filter,
    }" class="button secondary" @click="emits('setFilter', props.filter)">{{ props.filter }}</button>
</template>

<style scoped>
.button {
    text-transform: capitalize;
}
</style>