<script setup lang="ts">
import { ref } from 'vue';


const emit = defineEmits<{
    addTask: [newTask: string];
}>();

const newTask = ref('');
const error = ref('');

function formSubmitted() {
    if (!newTask.value.trim()) {
        error.value = 'Task cannot be empty';
        return;
    };
    emit('addTask', newTask.value);
    newTask.value = '';
    error.value = '';
}

</script>

<template>
    <form @submit.prevent="formSubmitted">
        <label>New Task
            <input v-model="newTask" name="newTask" :aria-invalid="!!error || undefined"
                aria-describedby="invalid-helper" @input="error = ''" />
            <small v-if="error" id="invalid-helper">
                {{ error }}
            </small>
        </label>
        <div class="button-container">
            <button>Add Task</button>
        </div>
    </form>
</template>