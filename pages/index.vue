<template>
  <div class="container">
    <div class="header"><h1 class="title">Shooting Star Simulator</h1></div>
    <div ref="bodyRef" class="body" />
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
    const bodyRef = ref<HTMLDivElement>();
    const active = ref(false);
    const topAdjustor = 200;
    let intervalId: NodeJS.Timeout | null = null;
    let starId = 0;

    const getStarId = () => {
      const result = starId;
      starId += 1;
      return result;
    };

    onMounted(() => {
      intervalId = setInterval(() => {
        const bodyElement = bodyRef.value;
        if (!bodyElement) return;

        const targetId = `star-${getStarId()}`;

        const element = document.createElement('div');
        element.id = targetId;
        element.className = 'star-position';
        element.innerHTML = '<div class="star" />';

        const degree = getRadomNumber(-90, 90);
        element.style.transform = `rotateZ(${degree}deg)`;

        const radius = getRadomNumber(150, 300);
        const radian = degree * (Math.PI / 180);
        const vertical = radius * Math.cos(radian);
        const horizonal = radius * Math.sin(radian);
        const center = window.innerWidth / 2;
        element.style.left = `${center - horizonal}px`;
        element.style.top = `${vertical + topAdjustor}px`;

        bodyElement.appendChild(element);
        setTimeout(() => {
          const target = document.getElementById(targetId);
          if (target) {
            target.remove();
          }
        }, 1000);
      }, 3000);
    });

    onUnmounted(() => {
      if (intervalId) clearInterval(intervalId);
    });

    return { active, bodyRef };
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
  left: 0;
  width: 100vw;
  height: 64px;
  padding: 16px 0;
}

.title {
  font-size: 1.6rem;
}

.body {
  height: 100vh;
  width: 100vw;
}

.star-position {
  position: absolute;
}

.star {
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
  }

  100% {
    color: transparent;
    height: 100px;
    opacity: 0%;
    filter: blur(1px);
  }
}
</style>
