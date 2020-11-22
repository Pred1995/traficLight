<template>
  <div>
    <div class="TraficLight" :class="{ flashingAnimation: flashing }" :style="{ background: color, opacity: active ? 1 : 0.2}"></div>
  </div>
</template>

<script>
export default {
  name: 'TraficLightCircle',
  props: ['color', 'active', 'flashing'],
};
</script>

<style scoped lang="scss">
  .TraficLight {
    width: 10em;
    height: 10em;
    border-radius: 50%;
    margin: 0 auto;
    margin-bottom: 5px;
    background: mistyrose;
  }
  .flashingAnimation {
    animation-name: flash;
    animation-duration: 1s;
    animation-timing-function: cubic-bezier(0, 0, 0.7, 0.94);
    animation-iteration-count: infinite;
    -webkit-animation-duration: 1s;
    -webkit-animation-timing-function: cubic-bezier(0, 0, 0.68, 0.99);
    -webkit-animation-iteration-count: infinite;
    -moz-animation-duration: 1s;
    -moz-animation-timing-function: linear;
    -moz-animation-iteration-count: infinite;
  }
  @keyframes flash {
    0% { opacity: 1.0; }
    50% { opacity: 0.5; }
    100% { opacity: 1.0; }
  }
</style>
