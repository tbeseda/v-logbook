<template>
  <ul class="entries" ref="list" :style="customStyle">
    <li v-show="entries.length === 0">{{ standbyMessage }}</li>
    <li v-for="(entry, index) in entries" :key="entry.id || index">
      <span v-if="timestamps"
        >[{{ entry.timestamp.toLocaleTimeString() }}]</span
      >
      &nbsp;
      <span class="highlight" :class="entry.highlight">{{
        entry.message
      }}</span>
    </li>
  </ul>
</template>

<script>
export default {
  name: 'Logbook',

  props: {
    standbyMessage: {
      type: String,
      default: 'standby...',
    },
    timestamps: {
      type: Boolean,
      default: true,
    },
    preloadedLogs: {
      type: Array,
      default() {
        return [];
      },
    },
    customStyle: {
      type: String,
    },
  },

  data() {
    return {
      entries: [],
    };
  },

  methods: {
    log(entry) {
      if (typeof entry === 'string') entry = { message: entry };

      this.validateEntry(entry);

      this.entries.push({
        ...entry,
        timestamp: new Date(),
      });

      this.$nextTick(() => {
        this.$refs.list.scrollTop = this.$refs.list.scrollHeight;
      });

      return this.entries;
    },

    validateEntry(entry) {
      if (!entry.message || typeof entry.message !== 'string')
        throw new Error('Log entry message is not valid');
    },

    clear() {
      this.entries = [];
    },

    getEntries() {
      return this.entries;
    },
  },

  beforeMount() {
    this.preloadedLogs.forEach(entry => {
      this.log(entry);
    });
  },
};
</script>

<style scoped>
.highlight.neutral {
  color: #fff;
}
.highlight.good {
  color: #03dac5;
}
.highlight.error {
  color: #cf6679;
}

ul.entries {
  height: 500px;
  margin: 0;
  padding: 10px;
  overflow-y: scroll;
  background: #1c1c1c;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
  list-style: none;
}
ul.entries::-webkit-scrollbar {
  width: 0;
  background: transparent;
}
ul.entries li {
  margin: 0 0 10px;
  padding: 0;
  font-family: monospace;
  color: #aaa;
}
</style>
