<template>
  <div class="container">
    <div v-if="errorMsg">
      <p>{{errorMsg}}</p>
    </div>
    <form class="form" @submit.prevent="signUp">
      <h1>Register to TaskApp</h1>
      <p>Start Organazing your Tasks Today</p>
      <div class="form-input">
        <label for="email">Email</label>
        <input placeholder="Enter your email" class="input" type="email" required id="email" v-model="email">
      </div>
      <div class="form-input">
        <label for="password">Password</label>
        <input placeholder="Enter your password" class="input" type="password" id="password" v-model="password">
      </div>
      <div class="form-input">
        <label for="confirmPassword">Confirm Password</label>
        <input placeholder="Confirm your password" class="input" type="password" id="confirmPassword" v-model="confirmPassword">
      </div>
      <button class="button" type="submit">Register</button>
      <PersonalRouter :route="route" :buttonText="buttonText" />
    </form>
  </div>
  
</template>

<script setup>
import PersonalRouter from "./PersonalRouter.vue";
import {ref, computed} from "vue";
import {supabase} from '../supabase'
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
  if(password.value === confirmPassword.value){
    try {
      const {error} = await supabase.auth.signUp({
        email: email.value,
        password: password.value
      })
      if (error) throw error;
      redirect.push('login')
    }
    
    catch(error){
      errorMsg.value = error.message
      setTimeout(() => {
        errorMsg.value = null
      }, 3000)
    }
    return
  }
  errorMsg.value = "Passwords do not match" ;

  setTimeout(() => {
    errorMsg.value = null
  }, 3000)  
};

</script>

<style scoped>

</style>
