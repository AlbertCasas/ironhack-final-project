<template>
  <div id="new-task">
    
    <p v-if="errorMsg" class="err-msg">{{ errorMsg }}</p>
    
    
      <form class="form">
        <h1 class="title">Add a New Task</h1>
        <p>and start organizing your life!</p>
        <input type="text" placeholder="Enter task title" required v-model="taskTitle"/>
        <input type="text" placeholder="Enter task description" required v-model="taskDescription"/>
        <button @click.prevent="addNewTask" type="submit">Add Task</button>
      </form>
    
  </div>
</template>

<script setup>
import { ref } from "vue";
import {useTaskStore} from '../stores/task'


// constant to save a variable that define the custom event that will be emitted to the homeView
const emit = defineEmits(["addNewTask"])

// constant to save a variable that holds the value of the title input field of the new task
const taskTitle = ref("");

// constant to save a variable that holds the value of the description input field of the new task
const taskDescription = ref("");

// constant to save a variable that holds an initial false boolean value for the errorMessage container that is conditionally displayed depending if the input field is empty
const showError = false

// const constant to save a variable that holds the value of the error message
const errorMsg = ref("");

// arrow function to call the form holding the task title and task description that uses a conditional to first checks if the task title is empty, if true the error message is displayed through the errorMessage container and sets a timeOut method that hides the error after some time. Else, its emmits a custom event to the home view with the task title and task description; clears the task title and task description input fields.
// const addNewTask = async () => {
//   try {
//     emit("add-new-task", taskTitle.value, taskDescription.value)
//   } catch (error) {
//     errorMsg.value = error.message
//     setTimeout(() => {
//       errorMsg.value = null
//     }, 3000)
//   }
// };

const addNewTask = async () => {
  if(taskTitle.value === "" && taskDescription.value === ""){
    errorMsg.value = "This looks empty...";
    setTimeout(() => {
      errorMsg.value = null
    }, 3000)
  }else{
    const newTask = {
      title: taskTitle.value,
      description: taskDescription.value
    };
    emit("addNewTask", newTask)
    taskTitle.value = "",
    taskDescription.value = ""
  }
}

</script>

<style scoped>

#new-task {
  height: 33vh;
}

.err-msg {
  color: red;
  text-align: center;
}

.form {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1rem;
  margin: 2rem 0;
}

.form p {
  font-size: 1.2rem;
  color: #427AA1;
  font-weight: 600;
}
  

.title {
  font-size: 2.2rem;
  color: #427AA1;
}

.form input {
  padding: 0.5rem;
  width: 50%;
  border-radius: 3px;
  border-color: #427AA1;
}

.form input:focus {
  outline: none;
}

.form button {
  padding: 0.5rem;
  width: 50%;
  background-color: #427AA1;
  color: white;
  font-size: 1rem;
  border: none;
  border-radius: 3px;
}

.form button:hover {
  cursor: pointer;
}

</style>
