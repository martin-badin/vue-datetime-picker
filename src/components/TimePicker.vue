<template>
  <div class="datetime-picker__block">
    <div class="datetime-picker__picker datetime-picker--clearfix">
      <counter :value="selected.hours" @input="onHourChange" v-if="enableHours"></counter>
      <span class="datetime-picker__counter__dots" v-if="enableMinutes">:</span>
      <counter :value="selected.minutes" @input="onMinuteChange" v-if="enableMinutes"></counter>
      <span class="datetime-picker__counter__dots" v-if="enableSeconds">:</span>
      <counter :value="selected.seconds" @input="onSecondChange" v-if="enableSeconds"></counter>
    </div>
  </div>
</template>

<script>
import Counter from "../atoms/Counter.vue";
import get_hours from "date-fns/get_hours";
import get_minutes from "date-fns/get_minutes";
import get_seconds from "date-fns/get_seconds";
import set_hours from "date-fns/set_hours";
import set_minutes from "date-fns/set_minutes";
import set_seconds from "date-fns/set_seconds";

export default {
  name: "TimePicker",
  props: {
    value: {
      type: [String, Date],
      required: true
    }
  },
  components: { Counter },
  inject: ["format", "locale"],
  computed: {
    selected() {
      return {
        seconds: get_seconds(this.value),
        minutes: get_minutes(this.value),
        hours: get_hours(this.value)
      };
    },
    enableSeconds() {
      return this.format.indexOf("s") !== -1;
    },
    enableMinutes() {
      return this.format.indexOf("m") !== -1;
    },
    enableHours() {
      return this.format.indexOf("h") !== -1;
    }
  },
  methods: {
    onHourChange(hours) {
      this.$emit("input", set_hours(this.value, hours));
    },
    onMinuteChange(minutes) {
      this.$emit("input", set_minutes(this.value, minutes));
    },
    onSecondChange(seconds) {
      this.$emit("input", set_seconds(this.value, seconds));
    }
  }
};
</script>

<style>
</style>
