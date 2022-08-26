<template>
  <div>
    <router-link class="router-link" :to="route">{{ buttonText }}</router-link>
  </div>
</template>

<script setup>
import { useRouter } from "vue-router";

defineProps({
  route: String,
  buttonText: String,
});
</script>

<style>
.router-link {
  color: white;
  text-decoration: none;
  font-weight: 600;
}
</style>
