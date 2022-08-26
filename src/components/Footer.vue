<template>
  <div class="footer">
    <div class="logo">
      <img src="https://i.im.ge/2022/08/26/OwFsi4.unnamed.png" alt="logo">
      <h1>Note It!</h1>
    </div>
    <p>© 2022 Note It!. All rights reserved. Built by Albert Casas</p>
  </div>
</template>

<script setup>

</script>

<style>

.footer {
  margin-top: 5rem;
  background-color: #427AA1;
  display: flex;
  flex-direction: column;
  height: 10rem;
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
  width: 3rem;
  height: 3rem;
}

.logo h1 {
  font-size: 2.5rem;
  color: white;
}

</style>
