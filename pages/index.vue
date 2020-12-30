<template>
  <div class="container">
    <div class="header"><h1 class="title">Shooting Star Simulator</h1></div>
    <div class="body">
      <div v-show="active" ref="startPostionRef" class="start-position">
        <div class="start" />
      </div>
    </div>
    <div class="footer">
      <button @click="onClick">Start</button>
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

.header {
  position: fixed;
  top: 0;
  height: 40px;
  padding: 8px 0;
}

.title {
  font-size: 1.6rem;
}

.body {
  height: calc(100vh - 80px);
}

.footer {
  position: fixed;
  bottom: 40px;
  height: 40px;
}

.start-position {
  transform: rotateZ(45deg);
}

.start {
  background: linear-gradient(transparent, rgb(255, 255, 255));
  height: 2px;
  width: 2px;
  position: fixed;
  border-radius: 25%;
  animation: anim 0.8s linear forwards;
}

@keyframes anim {
  0% {
    opacity: 50%;
    height: 2px;
  }

  50% {
    height: 90px;
    opacity: 100%;
    filter: blur(1px);
  }

  75% {
    height: 95px;
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
