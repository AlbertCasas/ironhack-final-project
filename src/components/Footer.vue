<template>
  <div class="footer">
    <div class="logo">
      <img src="https://i.ibb.co/L6BKq4t/recurso.png" alt="logo">
      <h1>TaskApp</h1>
    </div>
    <p>© 2022 TaskApp. All rights reserved. Built by Albert Casas</p>
  </div>
</template>

<script setup>

</script>

<style>

.footer {
  background-color: #427AA1;
  display: flex;
  flex-direction: column;
  height: 9rem;
  justify-content: space-evenly;
  align-items: center;
}

.footer p {
  color: white;
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

</style>
