<template>
    <div class="icons">
        <i
        v-if="taskComplete === false"
        @click="taskComplete = !taskComplete"
        class="fa-solid fa-circle"
        ></i>
        <i
        v-if="taskComplete"
        @click="taskComplete = !taskComplete"
        class="fa-solid fa-circle-check"
        ></i>
        <i @click="editTask = !editTask" class="fa-solid fa-pen-to-square"></i>
        <i @click="deleteTask(id)" class="fa-solid fa-trash"></i>
        <div class="icon-inputs">
            <input v-if="editTask" type="text" placeholder="Edit your title">
            <input v-if="editTask" type="text" placeholder="Edit your description">
        </div>
    </div>
</template>

<script setup>
import { ref, computed } from "vue";
import { useTaskStore } from "../stores/task";

const emit = defineEmits([
    "edit-task", "delete-task"
])

const props = defineProps(["id"])

const toggleTask = computed(() => (taskComplete.value ? false : true));

const taskComplete = ref(false);

const toggleEditTask = computed(() => (editTask.value ? false : true));

const editTask = ref(false);

const deleteTask = (id) => {
    emit("delete-task", id)
}
</script>

<style>
.icons {
    display: flex;
    justify-content: flex-end;
    gap: 0.7rem;
    padding: 0.5rem;
    font-size: 1rem;
}

.icons i:hover {
    cursor: pointer;
}

.icon-inputs {
    display: flex;
    flex-direction: column;
}
</style>
