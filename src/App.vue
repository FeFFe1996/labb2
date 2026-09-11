<script setup>
import { RouterLink, RouterView } from 'vue-router'

</script>

<template>
  <html>
    <header> 
    <div >
      <h1>Min Book lista</h1>
      <h4 class="headSubTxt">Allt för läsandet</h4>
    </div>
  </header>
  <main>
    <RouterView />
  </main>
</html>
</template>