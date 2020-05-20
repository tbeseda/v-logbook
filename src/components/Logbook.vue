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
        :class="'highlight-' + entry.highlight"
        :style="'color: ' + highlightColor[entry.highlight]"
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
    customStyle: {
      type: Object,
    },
  },

  data() {
    return {
      entries: [],
      style: {
        padding: '10px',
        height: '250px',
        'overflow-y': 'scroll',
        'box-shadow': '0 0 10px rgba(0, 0, 0, 0.2)',
        'text-align': 'left',
        'font-family': 'monospace',
        'list-style': 'none',
      },
    };
  },

  computed: {
    highlightColor() {
      return {
        neutral: '#fff',
        info: '#209cee',
        success: '#23d160',
        warning: '#ffdd57',
        danger: '#ff3860',
      };
    },
  },

  methods: {
    add(entry) {
      if (typeof entry === 'string') entry = { message: entry };

      this.validateEntry(entry);

      this.entries.push({
        // entry.id,
        // entry.message,
        // entry.highlight,
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
  },
};
</script>
