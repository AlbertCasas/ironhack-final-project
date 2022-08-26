<template>
  <div class="container">
    <form class="form" @submit.prevent="signIn">
      <img src="https://i.im.ge/2022/08/26/OwFsi4.unnamed.png" alt="logo">
      <h1>Log In to Note It!</h1>
      <p>Start Organizing your Notes Today!</p>
      <div class="form-input">
        <input placeholder="Enter your email" class="input" type="email" required id="email" v-model="email">
      </div>
      <div class="form-input password">
        <input placeholder="Enter your password" class="input" :type="passwordFieldType" id="password" v-model="password">
        <i v-if="hidePassword === true" @click="hidePassword = !hidePassword" class="fa-solid fa-eye icon"></i>
        <i v-else @click="hidePassword = !hidePassword" class="fa-solid fa-eye-slash icon"></i>
      </div>
      <p class="err-msg size" v-if="errorMsg">{{errorMsg}}</p>
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
    }, 50000);
  }
};
</script>

<style scoped>

.container {
  background-color: #427AA1;
}

.err-msg.size {
  color: white;
  text-align: center;
  margin: 0;
  font-size: 1rem;
}

.form {
  display: flex;
  flex-direction: column;
  margin: 1rem 0;
  padding: 2rem;
  align-items: center;
  
}

.form img {
  width: 3rem;
  text-align: center;
  margin-bottom: 1rem;
}

.form h1 {
  margin: 0;
  font-size: 2.3rem;
  text-align: center;
  color: white;
}

.form p {
  text-align: center;
  font-size: 1.5rem;
  margin-bottom: 4rem;
  color: white;
}

.form-input {
  display: flex;
  flex-direction: column;
  width: 80%;
  font-size: 1rem;
  margin-bottom: 1rem;
}

.input {
  padding: 1rem;
  border-radius: 3px;
}

.password {
  position: relative;
}

.icon {
  position: absolute;
  right: 1rem;
  top: 1rem;
  color: #427AA1;
  width: 1rem;
  height: 1rem;
}

.icon:hover {
  cursor: pointer;
}


.button {
  background-color: #d9da00;
  border: none;
  color: #427AA1;
  padding: 15px 15px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  font-weight: 600;
  width: 80%;
  border-radius: 3px;
  margin-bottom: 1rem;
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
