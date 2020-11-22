<template>
  <div class="container">
    <p>Осталось: {{this.counter}} сек.</p>
    <TraficLightCircle color='#ff0000' :active='false' />
    <TraficLightCircle color='#ffff00' :active='true' :flashing='this.flashing'/>
    <TraficLightCircle color='#008000' :active='false'/>
    <button class="btn btn-success mb" @click="HandlerButtonStop" v-if="this.Timer">Стоп</button>
    <button class="btn btn-success mb" @click="HandlerButtonStart" v-else>Старт</button>
  </div>
</template>

<script>
import TraficLightCircle from '@/components/TraficLightCircle.vue';

export default {
  name: 'YellowLight',
  props: ['Timer'],
  data: () => ({
    flashing: false,
    counter: 3,
    prevRoute: null,
  }),
  methods: {
    countDown() {
      if (this.counter) {
        return setTimeout(() => {
          if (this.Timer) {
            this.counter -= 1;
          }
          if (this.counter < 3) {
            this.flashing = true;
          }
          this.countDown();
        }, 1000);
      }
      if (this.prevRoute.path === '/green') {
        return this.$router.push({ path: '/red' });
      }
      return this.$router.push({ path: '/green' });
    },
    async HandlerButtonStop() {
      this.$emit('updateLocalStorage', { counter: this.counter, address: this.$route.path });
    },
    async HandlerButtonStart() {
      this.$emit('deleteLocalStorage');
    },
  },
  beforeRouteEnter(to, from, next) {
    next((vm) => {
      const tempVm = vm;
      const tempFrom = from;
      tempVm.prevRoute = tempFrom;
    });
  },
  async mounted() {
    const newCount = JSON.parse(window.localStorage.getItem('localeDate'));
    if (newCount) {
      this.counter = newCount.counter;
    }
    this.countDown();
  },
  components: {
    TraficLightCircle,
  },
};
</script>
