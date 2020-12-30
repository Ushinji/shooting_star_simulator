<template>
  <div class="container">
    <div class="header"><h1 class="title">Shooting Star Simulator</h1></div>
    <div class="body">
      <div v-show="active" ref="startPostionRef" class="start-position">
        <div class="start" />
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import {
  defineComponent,
  ref,
  onMounted,
  onUnmounted,
} from '@vue/composition-api';

const getRadomNumber = (min: number, max: number) => {
  return Math.floor(Math.random() * (max + 1 - min)) + min;
};

export default defineComponent({
  setup() {
    const startPostionRef = ref<HTMLDivElement>();
    const active = ref(false);
    let intervalId: NodeJS.Timeout | null = null;

    onMounted(() => {
      intervalId = setInterval(() => {
        const element = startPostionRef.value;
        if (!element) return;
        const degree = getRadomNumber(-45, 45);
        element.style.transform = `rotateZ(${degree}deg)`;
        active.value = true;
        setTimeout(() => {
          active.value = false;
        }, 1000);
      }, 2000);
    });

    onUnmounted(() => {
      if (intervalId) clearInterval(intervalId);
    });

    return { active, startPostionRef };
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
  height: calc(100vh - 40px);
}

.start-position {
  margin-top: 60px;
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
