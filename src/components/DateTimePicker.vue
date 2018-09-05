<template>
  <div class="datetime-picker datetime-picker--clearfix">
    <div class="datetime-picker__preview datetime-picker--clearfix" @click="onOpenModal">
      <span>{{ formatedValue }}</span>
    </div>
    <modal ref="modal">
      <date-picker :value="value" @input="$emit('input', $event)"></date-picker>
      <time-picker :value="value" @input="$emit('input', $event)"></time-picker>
    </modal>
  </div>
</template>

<script>
import Modal from "../atoms/Modal.vue";
import format from "date-fns/format";
import DatePicker from "./DatePicker.vue";
import TimePicker from "./TimePicker.vue";
import en_locale from "date-fns/locale/en";

export default {
  name: "DateTimePicker",
  components: { DatePicker, TimePicker, Modal },
  props: {
    locale: {
      type: Object,
      default() {
        return {
          days: ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"],
          ...en_locale
        };
      }
    },
    format: {
      type: String,
      default: "D MMMM YYYY h:mm:ss A"
    },
    value: {
      type: [String, Date],
      required: true
    }
  },
  computed: {
    formatedValue() {
      return format(this.value, this.format, { locale: this.locale });
    }
  },
  methods: {
    onOpenModal() {
      this.$refs.modal.openModal();
    }
  },
  provide() {
    return {
      format: this.format,
      locale: this.locale
    };
  }
};
</script>

<style lang="scss">
.datetime-picker {
  font-weight: 400;
  font-family: system-ui;

  * {
    font-weight: 400;
    font-family: system-ui;
  }

  &__picker {
    position: relative;
  }

  &__block {
    width: 50%;
    display: table-cell;
    vertical-align: middle;
  }

  &__preview {
    white-space: nowrap;
    line-height: 20px;
    border: 1px solid #ccc;
    border-radius: 2px;
    padding: 5px;
    cursor: pointer;

    img,
    span {
      float: left;
    }

    img {
      width: 20px;
      margin-left: 10px;
    }
  }

  &__button {
    border: none;
    outline: none;
    background: none;
    width: 20px;
    height: 30px;
    padding: 0;
    cursor: pointer;

    &--left {
      float: left;
    }

    &--right {
      float: right;
    }
  }
}

.datetime-picker--clearfix {
  &:after,
  &:before {
    display: block;
    content: "";
  }

  &:after {
    clear: both;
  }
}
</style>