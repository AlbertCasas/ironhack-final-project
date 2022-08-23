<template>
  <div>
    <div class="container">
      <div class="task-container">
        <!-- <CardIcons :id = "task.id" @delete-task = "deleteTask" @edit-task = "editTaskFunc"/> -->
        <div class="task-text">
          <h1>{{task.title}}</h1>
          <p>{{task.description}}</p>
        </div>
        <div class="task-edits">
          <i class="fa-solid fa-circle"></i>
          <i class="fa-solid fa-circle-check"></i>
          <i @click="toggleEdit" class="fa-solid fa-pen-to-square"></i>
          <i @click="deleteTask(id)" class="fa-solid fa-trash"></i>
        </div>
        <div class="inputs" v-if="editTaskInputs">
          <input type="text" v-model="newTitle">
          <input type="text" v-model="newDescription">
          <button @click.prevent="editTaskFunc">Edit</button>
        </div>
      </div>
      
  </div>
  </div>
</template>

<script setup>
  import {useTaskStore} from '../stores/task'
  import {ref, computed} from 'vue'
  import CardIcons from '../components/CardIcons.vue'


const emit = defineEmits([
  "editTaskChild", "deleteTaskChild"
])

const props = defineProps(["task"])

// const deleteTask = (id) => {
//     emit("delete-task", id)
// }

// const editTaskFunc = (id) => {
//   emit("edit-task", id)
// }

// Initialy hidden
const editTaskInputs = ref(false) 

const toggleEdit = () => {
  editTaskInputs.value = !editTaskInputs.value
  newTitle.value = props.task.title
  newDescription.value = props.task.description
}

const newTitle = ref("")

const newDescription = ref("")

const errorMsg = ref("")

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





// const tasks = computed(() => useTaskStore().fetchTasks())

// const getTasks = async () => {
//   tasks.value = await useTaskStore().fetchTasks()

// }

// getTasks()

// const props = defineProps(["ENTER-PROP-HERE"]);
</script>

<style scoped>

.container {
  display: flex;
  flex-wrap: wrap;
}


.task-container {
  background-color: beige;
  width: 20rem;
  height: 10rem;
  margin: 1rem;
}

.task-container h1 {
  text-align: center;
}

.task-container p {
  text-align: center;
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
