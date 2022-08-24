<template>
  <header>
    <nav class="navbar">
      <div class="logo">
        <img src="https://i.ibb.co/L6BKq4t/recurso.png" alt="logo">
        <h1>Task App</h1>

      </div>
      <div class="user">
        <p>Welcome {{cleanEmail}}</p>
        <button @click="signOut">Sign Out</button>
      </div>
    </nav>
  </header>
</template>

<script setup>
import { useRouter } from "vue-router";
import { useUserStore } from "../stores/user";

//constant to save a variable that will hold the use router method
const redirect = useRouter();

// constant to save a variable that will get the user from store with a computed function imported from vue
const userStore = useUserStore();

// constant that calls user email from the useUSerStore
const email = userStore.user.email

// constant that saves the user email and cleans out the @client from the user
const removeEmail = email.split("@")
const cleanEmail = removeEmail[0]

// async function that calls the signOut method from the useUserStore and pushes the user back to the Auth view.
const signOut = async () => {
  await useUserStore().signOut;
  redirect.push({path:"/auth/login"});}

</script>

<style scoped>

.navbar {
  width: 100%;
  margin: 0;
  height: 4rem;
  display: flex;
  justify-content: space-around;
  background-color: #427AA1;
  align-items: center;
}

.logo {
  display: flex;
  gap: 0.7rem;
  align-items: center;
}


.logo img {
  width: 2rem;
  height: 2rem;
}

.logo h1 {
  font-size: 2.5rem;
  color: white;
}

.navbar h1 {
  font-size: 2rem;
}

.user {
  display: flex;
  justify-content: space-around;
  align-items: center;
  gap: 1rem;
  font-size: 1rem;
  color: white;
}

.user button {
  padding: 0.5rem;
  background-color: white;
  border: none;
  font-size: 1rem;
  color: #427AA1;
  border-radius: 3px;
}

.user button:hover {
  cursor: pointer;
}

</style>
