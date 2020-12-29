<template>
  <div class="container">
    <div>
      <h1 class="title">shooting_star_simulator</h1>
      <div class="start" />
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from '@vue/composition-api';

export default defineComponent({
  setup() {
    return {};
  },
});
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
  background: linear-gradient(rgb(133, 133, 133), snow);
  height: 4px;
  width: 4px;
  position: fixed;
  border-radius: 25%;
  top: 10%;
  animation: anim 0.2s linear forwards;
}

@keyframes anim {
  0% {
    opacity: 100%;
  }

  100% {
    color: transparent;
    top: 20%;
    height: 100px;
    opacity: 0;
  }
}
</style>
