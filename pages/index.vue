<template>
  <div class="container">
    <div>
      <h1 class="title">shooting_star_simulator</h1>
      <div class="start" />
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'

export default Vue.extend({})
</script>

<style lang="scss">
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
  background-color: black;
}

.start {
  background-color: snow;
  height: 16px;
  width: 16px;
  position: fixed;
  border-radius: 50%;
  top: 10%;
  animation: anim 1s linear infinite;
}

@keyframes anim {
  100% {
    color: transparent;
    top: 50%;
    height: 100px;
  }
}
</style>
