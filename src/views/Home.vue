<template>
  <div id="main-container">
  <Nav />
  <NewTask @addNewTask = "addNewTask"/>
  <!-- <TaskItem :tasks = "tasks" @delete-task = "deleteTask" @edit-task = "editTaskFunc"/> -->
  <div id="task-item">
    <TaskItem v-for="task in taskStore.tasks" :key="task.id" :task="task" @editTaskChild = "editTaskFunc" @deleteTaskChild = "deleteTask" @toggleTaskChild = "toggleTaskFunc" />
  </div>
  <div id="footer">
  <Footer />
  </div>
  </div>
  <router-view />
</template>

<script setup>
import Nav from '../components/Nav.vue'
import NewTask from '../components/NewTask.vue'
import TaskItem from '../components/TaskItem.vue'
import Footer from '../components/Footer.vue'
import {useTaskStore} from '../stores/task'
import {onMounted} from 'vue'


//function to fetch tasks
const taskStore = useTaskStore()
onMounted(() => {
  taskStore.fetchTasks()
})


const addNewTask = async (newTask) => {
  const response = await taskStore.addTask(newTask.title, newTask.description)
  taskStore.fetchTasks()
}

const deleteTask = (item) => {
  console.log(item)
    let proceed = confirm("Are you sure you want to delete this task?")
    if (proceed){
      useTaskStore().deleteTask(item.id)
      }
      setTimeout(() => {
        taskStore.fetchTasks()
      },200)
}

const editTaskFunc = async (item) => {
const newTitle = item.newValueTitle
console.log(item)
const newDescription = item.newValueDescription
const editId = item.oldValue.id
  await useTaskStore().editTask(newTitle, newDescription, editId)
  taskStore.fetchTasks()
}

const toggleTaskFunc = async (item) => {
  const response = await taskStore.toggleTask(item.is_complete, item.id)
  taskStore.fetchTasks()
  
}

</script>


<style scoped>

#main-container {
  display: flex;
  flex-direction: column;
}

#task-item {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  min-height: 44vh;
}

#footer {
  justify-self: end;
}
</style>
