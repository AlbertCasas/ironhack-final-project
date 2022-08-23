<template>
  <div>
    <div v-if="errorMsg">
      <p>{{ errorMsg }}</p>
    </div>
    <div>
      <form>
        <h1>Add a new task</h1>
        <input type="text" placeholder="Enter task title" required v-model="taskTitle"/>
        <input type="text" placeholder="Enter task description" required v-model="taskDescription"/>
        <button @click.prevent="addNewTask" type="submit">Add Task</button>
      </form>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import {useTaskStore} from '../stores/task'


// constant to save a variable that define the custom event that will be emitted to the homeView
const emit = defineEmits(["add-new-task"])

// constant to save a variable that holds the value of the title input field of the new task
const taskTitle = ref("");

// constant to save a variable that holds the value of the description input field of the new task
const taskDescription = ref("");

// constant to save a variable that holds an initial false boolean value for the errorMessage container that is conditionally displayed depending if the input field is empty
const showError = false

// const constant to save a variable that holds the value of the error message
const errorMsg = ref("");

// arrow function to call the form holding the task title and task description that uses a conditional to first checks if the task title is empty, if true the error message is displayed through the errorMessage container and sets a timeOut method that hides the error after some time. Else, its emmits a custom event to the home view with the task title and task description; clears the task title and task description input fields.
const addNewTask = async () => {
  try {
    emit("add-new-task", taskTitle.value, taskDescription.value)
  } catch (error) {
    errorMsg.value = error.message
    setTimeout(() => {
      errorMsg.value = null
    }, 3000)
  }
};

</script>

<style></style>
