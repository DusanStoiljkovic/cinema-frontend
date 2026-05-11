<script setup lang="ts">

const year = new Date().getFullYear()

</script>

<template>
  <RouterLink to="/">Pocetna</RouterLink> | <RouterLink to="/about">O nama</RouterLink>
  <RouterView/>
  <footer>&copy; {{ year }} Univerzitet Singidunum</footer>
</template>

<style scoped></style>
