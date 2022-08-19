<template>
  <div>
    <h1>Sing up</h1>
    <form action="" class="form">
      <div class="form-input">
        <label for="email">Email</label>
        <input type="email">
      </div>
      <div class="form-input">
        <label for="password">Password</label>
        <input type="password">
      </div>
      <div class="form-input">
        <label for="password">Confirm password</label>
        <input type="password">
      </div>
    </form>
  </div>
  <PersonalRouter :route="route" :buttonText="buttonText" />
  
</template>

<script setup>
import PersonalRouter from "./PersonalRouter.vue";
import {ref, computed} from "vue";

import {useRouter} from "vue-router";
import {useUserStore} from "../stores/user"
import {storeToRefs} from "pinia";

// Route Variables
const route = "/auth/login";
const buttonText = "Sign In";

// Input Fields
const email = ref("");
const password = ref("");
const confirmPassword = ref("");

// Error Message
const errorMsg = ref("");

// Show hide password variable
const passwordFieldType = computed(() =>
  hidePassword.value ? "password" : "text"
);

// Show hide confirmmPassword variable
const hidePassword = ref(true);

// Router to push user once SignedUp to Log In
const redirect = useRouter();

// Arrow function to SignUp user to supaBase with a timeOut() method for showing the error
const signUp = async () => {
  try {
    // calls the user store and send the users info to backend to logIn
    await useUserStore().signUp(email.value, password.value, confirmPassword.value);
    // redirects user to the homeView
    redirect.push({ path: "/" });
  } catch (error) {
    // displays error message
    errorMsg.value = `Error: ${error.message}`;
    // hides error message
    setTimeout(() => {
      errorMsg.value = null;
    }, 3000);
  }
};

</script>

<style></style>
