<template>
  <div id="app">
    <router-view  @updateLocalStorage="updateLocalStorage"
                  @deleteLocalStorage="deleteLocalStorage"
                  :Timer='Timer'
    />
  </div>
</template>

<script>
export default {
  data: () => ({
    Timer: true,
  }),
  async mounted() {
    const dateFromLocaleStorage = JSON.parse(window.localStorage.getItem('localeDate')) || undefined;
    if (this.$route.name === 'Home') {
      if (typeof dateFromLocaleStorage === 'undefined') {
        await this.$router.push({ path: '/red' });
      }
    }
    if (typeof dateFromLocaleStorage !== 'undefined') {
      await this.stopTimer();
      await this.$router.push({ path: dateFromLocaleStorage.address });
    }
  },
  methods: {
    async updateLocalStorage(value) {
      await this.stopTimer();
      window.localStorage.setItem('localeDate', JSON.stringify(value));
    },
    async deleteLocalStorage() {
      await window.localStorage.removeItem('localeDate');
      await this.startTimer();
    },
    stopTimer() {
      this.Timer = false;
    },
    startTimer() {
      this.Timer = true;
    },
  },
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  padding-top: 10px;
  color: #2c3e50;
}

.mb {
  margin-bottom: 5px;
}
</style>
