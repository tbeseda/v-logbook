<template>
  <ul class="entries" ref="list" :style="[style, customStyle]">
    <li v-show="entries.length === 0">{{ standbyMessage }}</li>
    <li
      v-for="(entry, index) in entries"
      :key="entry.id || index"
      style="margin: 0 0 10px; padding: 0px"
    >
      <span v-if="timestamps"
        >[{{ entry.timestamp.toLocaleTimeString() }}]</span
      >
      &nbsp;
      <span
        class="highlight"
        :class="entry.highlight"
        :style="highlightStyle(entry.highlight)"
        >{{ entry.message }}</span
      >
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
      style: {
        margin: '0',
        padding: '10px',
        height: '250px',
        background: '#1c1c1c',
        color: '#aaa',
        'overflow-y': 'scroll',
        'box-shadow': '0 0 10px rgba(0, 0, 0, 0.2)',
        'font-family': 'monospace',
        'list-style': 'none',
      },
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

    highlightStyle(type) {
      let color;

      switch (type) {
        case 'good':
          color = '#03dac5';
          break;
        case 'error':
          color = '#cf6679';
          break;
        case 'neutral':
          color = '#fff';
          break;
        default:
          color = 'inherit';
          break;
      }

      return `color: ${color}`;
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
ul.entries::-webkit-scrollbar {
  width: 0;
  background: transparent;
}
</style>
