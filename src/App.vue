<script setup lang="ts">
import { ref } from 'vue';

const message = ref('Hello World!');
const newTask = ref('');
</script>

<template>
    <main>
        <h1>{{ message }}</h1>
        <form>
            <label>New Task
                <input v-model="newTask" name="newTask">
            </label>
        </form>
        <h3>{{ newTask }}</h3>
    </main>
</template>

<style scoped>
main {
    max-width: 800px;
    margin: 1rem auto;
}
</style>