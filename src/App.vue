<template>
  <div id="app">
    <header>
      <h1 id="title"><code>v-</code>Logbook</h1>
      <div id="controls">
        <button @click="log">Log</button>
        <button @click="log('neutral')">Neutral</button>
        <button @click="log('info')">Info</button>
        <button @click="log('success')">Success</button>
        <button @click="log('warning')">Warning</button>
        <button @click="log('danger')">Danger</button>
      </div>
    </header>
    <Logbook
      id="log"
      ref="log"
      standby-message="Use the controls to create log entries"
      :custom-style="customStyle"
    />
    <footer>
      <button @click="clear">Clear</button>
    </footer>
  </div>
</template>

<script>
import Logbook from './';

export default {
  name: 'App',
  components: {
    Logbook,
  },
  data() {
    return {
      $log: null,
      customStyle: {
        background: '#1c1c1c',
        color: '#aaa',
      },
    };
  },
  methods: {
    log(highlight) {
      if (typeof highlight === 'string')
        this.$log.add({
          message: `${highlight} log example`,
          highlight: highlight,
        });
      else this.$log.add('no-highlight log example');
    },
    clear() {
      this.$log.clear();
    },
  },
  mounted() {
    this.$log = this.$refs.log;
  },
};
</script>

<style>
#app {
  width: 800px;
  margin: 0 auto;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}

#log {
  margin: 20px 0;
}

ul.entries#log::-webkit-scrollbar {
  width: 0;
  background: transparent;
}
</style>
