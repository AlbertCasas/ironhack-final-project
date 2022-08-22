<template>
  <div class="container">
    <div v-if="errorMsg">
      <p>{{errorMsg}}</p>
    </div>
    <form class="form" @submit.prevent="signIn">
      <h1>Log In to TaskApp</h1>
      <p>Start Organazing your Tasks Today</p>
      <div class="form-input">
        <label for="email">Email</label>
        <input placeholder="Enter your email" class="input" type="email" required id="email" v-model="email">
      </div>
      <div class="form-input">
        <label for="password">Password</label>
        <input placeholder="Enter your password" class="input" :type="passwordFieldType" id="password" v-model="password">
        <i @click="hidePassword = !hidePassword" class="fa-solid fa-eye"></i>
      </div>
      <button class="button" type="submit">Log In</button>
      <PersonalRouter :route="route" :buttonText="buttonText" />
    </form>
    <img class="photo" src="https://assets.entrepreneur.com/content/3x2/2000/20190517204006-GettyImages-924558574.jpeg" alt="signin-img">
  </div>
  
  
</template>

<script setup>
import { ref, computed } from "vue";
import PersonalRouter from "./PersonalRouter.vue";
import { supabase } from "../supabase";
import { useRouter } from "vue-router";
import { useUserStore } from "../stores/user";
import { storeToRefs } from "pinia";

// Route Variables
const route = "/auth/sign-up";
const buttonText = "Sign Up";

// Input Fields
const email = ref("");
const password = ref("");

// Error Message
const errorMsg = ref("");

//Show hide password variables
const passwordFieldType = computed(() =>
  hidePassword.value ? "password" : "text",
  
);
const hidePassword = ref(true);

// Router to push user once SignedIn to the HomeView
const redirect = useRouter();

// Arrow function to Signin user to supaBase
const signIn = async () => {
  try {
    // calls the user store and send the users info to backend to logIn
    await useUserStore().signIn(email.value, password.value);
    // redirects user to the homeView
    redirect.push({ path: "/" });
  } catch (error) {
    // displays error message
    errorMsg.value = `Error: ${error.message}`;
    // hides error message
    setTimeout(() => {
      errorMsg.value = null;
    }, 6000);
  }
};
</script>

<style>

.form {
  display: flex;
  flex-direction: column;
  margin: 1rem 0;
  padding: 2rem;
}

.form h1 {
  margin: 0;
  font-size: 1.5rem;
  text-align: center;
  color: #427AA1;
}

.form p {
  text-align: center;
}

.form-input {
  display: flex;
  flex-direction: column;
  margin-bottom: 1rem;
}

.input {
  height: 2rem;
}


.button {
  background-color: #427AA1;
  border: none;
  color: white;
  padding: 10px 10px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  margin-top: 1rem;
}

.photo {
  width: 100%;
}
</style>
