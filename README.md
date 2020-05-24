# v-logbook

> A simple, visual component to display log entries in a Vue application.

![](https://github.com/tbeseda/v-logbook/blob/master/static/Logbook.png?raw=true)

## Installation
`npm i v-logbook` or `yarn add v-logbook`.

## Usage

```vue
<template>
  <Logbook
      ref="myLogBook"
      standby-message="Go for launch..."
      :custom-style="customStyle"
    />
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
    myMethod() {
      if (this.somethingScary)
        // add an entry with a red color
        this.$log.add({
          message: 'Houston, we have a problem.',
          highlight: 'danger',
        });
      // add a plain log entry
      else this.$log.add("This is Odyssey. It's good to see you again");
    },
    clear() {
      this.$log.clear();
    },
  },
  mounted() {
    // for ease
    this.$log = this.$refs.myLogbook;
  },
};
</script>
```

## Another example

![](https://github.com/tbeseda/v-logbook/blob/master/static/AlpacaMarket-Logbook.png?raw=true)

## API

### Props

<table class="table">
<thead><tr>
  <th>Name</th><th>Default</th><th>Type</th><th>Description</th>
</tr></thead>
<tbody>
  <tr>
    <td> timestamps </td>
    <td> true </td>
    <td> Boolean </td>
    <td>Whether or not each entry should be displayed with a timestamp</td>
  </tr>
  <tr>
    <td> standby-message </td>
    <td> "standby..." </td>
    <td> String </td>
    <td>The idle message shown when `entries.length === 0`</td>
  </tr>
  <tr>
    <td> custom-style </td>
    <td> { } </td>
    <td> Object (CSS rules) </td>
    <td>Custom styles applied after limited Logbook styling</td>
  </tr>
</tbody>
</table>

### Methods
Each Logbook [reference](https://vuejs.org/v2/api/#vm-refs) has a few simple methods for interacting with log entries. `[vm.$refs][v-Logbook Component]...`

### `#add(string | {})`

Add a log entry. Either directly as a string or an object with a highlight color and meta data.  
Returns list of entries in case you need it.  

Highlights: `"neutral" | "info" | "success" | "warning" | "danger"`

Example object:
```js
{
  message: "It's like trying to drive a toaster through a car wash.",
  highlight: "warning",
  id: "A13",
  myMetaData: { // any arbitrary data is saved with the entry
    crew: ["Jim", "Jack"],
    director: "Gene",
  },
}
```

### `#clear()`

Delete all log entries. `standby-message` will be displayed when entries is empty.

### `#entries`

Get the internal array of log entries.

## FAQs

#### Why internal methods and not a property?

It fit my use case a bit better, but it would be easy to add.

#### Do I need to add a `ref=` attribute to the Logbook component?

It's the easiest way to reference each instance in order to call methods for that Logbook.

#### Do I need to set a data attribute to reference the Logbook component?

No. I just did that in my usage example to make it slightly easier to reference.

#### Why is there an optional `id` for each entry?

It might be useful to your application later if you use log entries later. It's also used as a key on that entries `<li>` element.

#### Why the _Apollo 13_ references?

idk.
