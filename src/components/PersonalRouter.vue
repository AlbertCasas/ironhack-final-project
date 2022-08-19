<template>
  <div>
    <span>Don't have an account? </span>
    <router-link class="router-link" :to="route">{{ buttonText }}</router-link>
  </div>
</template>

<script setup>
defineProps({
  route: String,
  buttonText: String,
});
</script>

<style>
.router-link {
  color: red;
}
</style>
