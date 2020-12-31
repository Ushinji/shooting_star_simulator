<template>
  <div class="container">
    <div class="header"><h1 class="title">Shooting Star Simulator</h1></div>
    <div ref="bodyRef" class="body" />
    <div class="footer">
      <div class="select">
        <label for="intervalSec">流れ星の頻度</label>
        <select id="intervalSec" v-model="intervalSec">
          <option value="3">3秒に1個</option>
          <option value="1">1秒に1個</option>
          <option value="0.5">0.5秒に1個</option>
        </select>
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
  watch,
} from '@vue/composition-api';

const getRadomNumber = (min: number, max: number) => {
  return Math.floor(Math.random() * (max + 1 - min)) + min;
};

export default defineComponent({
  setup() {
    const bodyRef = ref<HTMLDivElement>();
    const intervalSec = ref(3);
    const topAdjustor = 300;
    let intervalId: NodeJS.Timeout | null = null;
    let starId = 0;

    const getStarId = () => {
      const result = starId;
      starId += 1;
      return result;
    };

    const addShootingStar = () => {
      const bodyElement = bodyRef.value;
      if (!bodyElement) return;

      const targetId = `star-${getStarId()}`;

      // 追加するElementを作成
      const element = document.createElement('div');
      element.id = targetId;
      element.className = 'star-position';
      element.innerHTML = '<div class="star" />';

      // 角度を計算
      const degree = getRadomNumber(-90, 90);
      element.style.transform = `rotateZ(${degree}deg)`;

      // 表示位置の計算。中心から放射状に流れるように、位置は角度を元に計算する
      const radius = getRadomNumber(100, document.body.clientWidth / 2);
      const radian = degree * (Math.PI / 180);
      const vertical = radius * Math.cos(radian);
      const horizonal = radius * Math.sin(radian);
      const center = document.body.clientWidth / 2;
      element.style.left = `${center - horizonal}px`;
      element.style.top = `${vertical + topAdjustor}px`;

      // Elementを追加
      bodyElement.appendChild(element);

      // １秒後に追加したElementを削除する
      setTimeout(() => {
        const target = document.getElementById(targetId);
        if (target) {
          target.remove();
        }
      }, 1000);
    };

    onMounted(() => {
      intervalId = setInterval(addShootingStar, intervalSec.value * 1000);
    });

    watch(
      () => intervalSec.value,
      () => {
        if (intervalId) clearInterval(intervalId);
        intervalId = setInterval(addShootingStar, intervalSec.value * 1000);
      }
    );

    onUnmounted(() => {
      if (intervalId) clearInterval(intervalId);
    });

    return { intervalSec, bodyRef };
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
  overflow: hidden;
}

.footer {
  position: fixed;
  bottom: 40px;
  left: 0;
  display: flex;
  justify-content: center;
  width: 100vw;
}

.select {
  padding: 16px;
  display: flex;
  flex-direction: column;

  label {
    color: white;
    font-size: 1rem;
    font-weight: bold;
  }

  select {
    color: white;
    font-size: 1rem;
    padding: 6px 24px 5px 8px;
    background-color: #383838;
    border: 1px solid #6b6b6b;
    border-radius: 4px;
    -webkit-appearance: none;
    margin-bottom: 2px;
    cursor: pointer;
    outline: 0;

    &:hover {
      background-color: darken(#383838, 5%);
    }

    &:focus {
      outline: none;
      border: solid 1px #2783b9;
      border-radius: 4px;
    }

    &:disabled {
      cursor: not-allowed;
    }
  }
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
