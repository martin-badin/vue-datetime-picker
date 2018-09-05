<template>
  <div id="app">
    <date-time-picker v-model="datetime"></date-time-picker>
  </div>
</template>

<script>
import { DateTimePicker } from "./components";

export default {
  name: "app",
  components: {
    DateTimePicker
  },
  data() {
    return {
      datetime: new Date()
    };
  },
  watch: {
    datetime(value) {
      console.log(value);
    }
  }
};
</script>

<style lang="scss">
</style>