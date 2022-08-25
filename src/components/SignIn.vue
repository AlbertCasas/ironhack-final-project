<template>
  <div class="container">
    <form class="form" @submit.prevent="signIn">
      <p class="err-msg" v-if="errorMsg">{{errorMsg}}</p>
      <img src="https://i.ibb.co/5j0R8qr/recurso-blue.png" alt="logo">
      <h1>Log In to TaskApp</h1>
      <p>Start Organizing your Tasks Today!</p>
      <div class="form-input">
        <input placeholder="Enter your email" class="input" type="email" required id="email" v-model="email">
      </div>
      <div class="form-input password">
        <input placeholder="Enter your password" class="input" :type="passwordFieldType" id="password" v-model="password">
        <i v-if="hidePassword === true" @click="hidePassword = !hidePassword" class="fa-solid fa-eye icon"></i>
        <i v-else @click="hidePassword = !hidePassword" class="fa-solid fa-eye-slash icon"></i>
      </div>
      <button class="button" type="submit">Log In</button>
      <PersonalRouter :route="route" :buttonText="buttonText" />
    </form>
    <div class="photo-container"></div>
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
    }, 4000);
  }
};
</script>

<style scoped>


.err-msg {
  color: red;
  text-align: center;
  margin: 2rem 0;
}

.form {
  display: flex;
  flex-direction: column;
  margin: 1rem 0;
  padding: 2rem;
  align-items: center;
  gap: 1rem;
}

.form img {
  width: 2rem;
  text-align: center;
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
  width: 80%;
}

.input {
  height: 2rem;
}

.password {
  position: relative;
}

.icon {
  position: absolute;
  right: 1rem;
  top: 0.55rem;
  color: #427AA1;
}

.icon:hover {
  cursor: pointer;
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
  width: 80%;
}

.button:hover {
  cursor: pointer;
}

.photo-container {
  width: 100%;
  height: 50vh;
  background: url(https://assets.entrepreneur.com/content/3x2/2000/20190517204006-GettyImages-924558574.jpeg);
  background-position: center;
  background-size: cover;
  background-color: red;
}

@media only screen and (min-width: 550px){
  .container {
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .form {
    width: 50%;
  }

  .photo-container {
    width: 50%;
    height: 100vh;
  }

  
  
}
</style>
