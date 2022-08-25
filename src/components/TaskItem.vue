<template>
  <div id="task-item">
    <div class="container">
      <div class="task-container">
        <!-- <CardIcons :id = "task.id" @delete-task = "deleteTask" @edit-task = "editTaskFunc"/> -->
        <div class="task-text">
          <h1>{{task.title}}</h1>
          <p>{{task.description}}</p>
        </div>
        <div class="task-edits">
          <div @click="toggleTaskFunc">
          <i v-if="!task.is_complete" class="fa-solid fa-circle"></i>
          <i v-else class="fa-solid fa-circle-check"></i>
          </div>
          <i @click="toggleEdit" class="fa-solid fa-pen-to-square"></i>
          <i @click="deleteTask(id)" class="fa-solid fa-trash"></i>
        </div>
        <div class="inputs" v-if="editTaskInputs">
          <input type="text" v-model="newTitle">
          <input type="text" v-model="newDescription">
          <button @click.prevent="editTaskFunc" @click="toggleEdit">Edit Task</button>
        </div>
      </div>
    </div> 
  </div>
  
</template>

<script setup>
  import {useTaskStore} from '../stores/task'
  import {ref, computed} from 'vue'


const emit = defineEmits([
  "editTaskChild", "deleteTaskChild", "toggleTaskChild"
])

const props = defineProps(["task", "id"])


// Initialy hidden
const editTaskInputs = ref(false) 

const toggleEdit = () => {
  editTaskInputs.value = !editTaskInputs.value
  newTitle.value = props.task.title
  newDescription.value = props.task.description
}

const toggleTask = ref(false)

const newTitle = ref("")

const newDescription = ref("")

const errorMsg = ref("")

const toggleTaskFunc = (id) => {
  toggleTask.value = !toggleTask.value
  emit("toggleTaskChild", props.task)

}

const editTaskFunc = () => {
  if(newTitle.value === "" && newDescription.value === ""){
    errorMsg.value = "This looks empty..."
    setTimeout(() => {
      errorMsg.value = null
    }, 3000)
  }
  else
  {
    const editValues = {
      oldValue: props.task,
      newValueTitle: newTitle.value, 
      newValueDescription: newDescription.value
      
    }
    emit("editTaskChild", editValues)
    }
  }
  const deleteTask = () => {
    emit("deleteTaskChild", props.task)
}

</script>

<style scoped>

.container {
  display: flex;
  margin-bottom: 2rem;
}

.task-container {
  background-color: #427AA1;
  width: 25rem;
  min-height: 12rem;
  margin: 1rem;
  border-radius: 6px;
  box-shadow: 0.2rem 0.2rem 0.5rem gray;
}

.task-text {
  color: white;
  text-align: center;
}

.task-container h1 {
  padding: 1.5rem;
  font-size: 1.5rem;
}

.task-container p {
  height: 12rem;
}

.task-edits {
  color: white;
  display: flex;
  justify-content: space-around;
  margin-bottom: 1rem;
}


.task-edits i:hover {
  cursor: pointer;
}

.inputs {
  display: flex;
  flex-direction: column;
  align-items: center;
  align-content: center;
  gap: 1rem;
  margin: 1rem 0;
}

.inputs input {
  padding: 0.5rem;
  width: 70%;
}

.inputs button {
  padding: 0.5rem;
  width: 70%;
  color: #427AA1;
  font-size: 0.9rem;
}

.inputs button:hover {
  cursor: pointer;
}

</style>

<!-- 
// **Hints**
// 1. ref() or reactive() can be used here to store the following, think if you want to store them either individually or like an object, up to you.
// 2. Data properties need here are the following: a boolean to store a false**Important variable, a string to store an error, a string to store the value of the task that the user can edit, an initial false boolean to hide the inputFIeld used to edit the new task detail or details[this is in reference of the task title and the task description].
// 3. Store the custom emit events that will be used to call the functions of the homeView for editing, deleting and toggling the status[completed, not complted] of the taskItem.
// 4. Function to handle the error with the data properties used to control the error and the the boolean controlling the boolean empty variable.
// 5. Function to handle the edit dialogue where the inputField is displayed and the string used to store the value of the inputField will be used here to save the value as a prop on this function.
// 6. Function to emmit a custom event emit() that takes 2 parameters a name for the custom event and the value that will be send via the prop to the parent component. This function can control the toggle completion of the task on the homeview.
// 7. Function to edit the task information that you decided that the user can edit. This function's body takes in a conditional that first checks if the value of the task [either title and description or just title] is empty which if true it runs the function used to handle the error on hint4. Else, the conditional sets the first mentioned boolean data property on hint2 back to its inital boolean value to hide the error message and repeat the same for the data property mentioned 4th on hint2; a constant that stores an object that holds the oldValue from the prop item and the value of the task coming from the data property mentioned 3rd on hint2; a custom event emit() that takes 2 parameters a name for the custom event and the value from the object used on this part of the conditional and lastly this part of the conditional sets the value of input field to an empty string to clear it from the ui. 
// 8. Function to emmit a custom event emit() that takes 2 parameters a name for the custom event and the value that will be send via the prop to the parent component. This function can control the removal of  the task on the homeview.
// -->
