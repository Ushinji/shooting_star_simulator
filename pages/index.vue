<template>
  <div class="container">
    <div>
      <h1 class="title">Shooting Star Simulator</h1>
      <button @click="onClick">Start</button>
      <div v-if="active" class="start" />
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from '@vue/composition-api';

export default defineComponent({
  setup() {
    const active = ref(false);
    const onClick = () => {
      active.value = !active.value;
    };
    return { active, onClick };
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
  color: white;
}

.start {
  background: linear-gradient(transparent, rgb(255, 255, 255));
  height: 2px;
  width: 2px;
  position: fixed;
  border-radius: 25%;
  top: 10%;
  animation: anim 0.8s linear forwards;
}

@keyframes anim {
  0% {
    opacity: 50%;
    height: 2px;
  }

  50% {
    height: 80px;
    opacity: 100%;
    filter: blur(1px);
  }

  75% {
    height: 90px;
    opacity: 50%;
    filter: blur(1px);
  }

  100% {
    color: transparent;
    height: 100px;
    opacity: 0%;
    filter: blur(1px);
  }
}
</style>
