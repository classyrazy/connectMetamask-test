<template>
  <div class="wrapper">
    <div class="sh-bg-1"></div>
    <div class="main-con">
      <Welcome />
    </div>
  </div>
</template>

<script setup lang="ts">
import Welcome from '../components/Welcome.vue'
import { defineComponent } from 'vue'

console.log("main page")

</script>
<style>
  .wrapper{
    width: 100%;
    height: 100vh;
    background-color: var(--color-black);
    position: relative;
  }
  .main-con{
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  
  }
  .sh-bg-1{
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    /* min-width: 200px; */
    background: url("/sh_1.png") no-repeat;
    background-size: cover;
  }
</style>
