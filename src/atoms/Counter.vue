<template>
  <div class="datetime-picker__counter">
    <button class="datetime-picker__counter__button" @click="increment"><img src="data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIyODUiIGhlaWdodD0iMjg1IiB2aWV3Qm94PSIwIDAgMjg0LjkgMjg0LjkiPjxwYXRoIGQ9Ik0yODIuMSAxOTUuM0wxNDkgNjIuMmMtMS45LTEuOS00LjEtMi45LTYuNi0yLjlzLTQuNyAxLTYuNiAyLjlMMi45IDE5NS4zQzEgMTk3LjIgMCAxOTkuNCAwIDIwMS45YzAgMi41IDEgNC43IDIuOSA2LjZsMTQuMyAxNC4zYzEuOSAxLjkgNC4xIDIuOSA2LjYgMi45IDIuNSAwIDQuNy0xIDYuNi0yLjlsMTEyLjItMTEyLjIgMTEyLjIgMTEyLjJjMS45IDEuOSA0LjEgMi44IDYuNiAyLjggMi41IDAgNC43LTEgNi42LTIuOGwxNC4zLTE0LjNjMS45LTEuOSAyLjgtNC4xIDIuOC02LjZDMjg0LjkgMTk5LjQgMjg0IDE5Ny4yIDI4Mi4xIDE5NS4zeiIvPjwvc3ZnPg=="></button>
    <input class="datetime-picker__counter__input" :value="value" @keypress.enter="enter">
    <button class="datetime-picker__counter__button" @click="decrement"><img src="data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIyODUiIGhlaWdodD0iMjg1IiB2aWV3Qm94PSIwIDAgMjg0LjkgMjg0LjkiPjxwYXRoIGQ9Ik0yODIuMSA3Ni41bC0xNC4zLTE0LjNjLTEuOS0xLjktNC4xLTIuOS02LjYtMi45IC0yLjUgMC00LjcgMS02LjYgMi45TDE0Mi41IDE3NC40IDMwLjMgNjIuMmMtMS45LTEuOS00LjEtMi45LTYuNi0yLjkgLTIuNSAwLTQuNyAxLTYuNiAyLjlMMi45IDc2LjVDMSA3OC40IDAgODAuNiAwIDgzLjFjMCAyLjUgMSA0LjcgMi45IDYuNmwxMzMgMTMzYzEuOSAxLjkgNC4xIDIuOSA2LjYgMi45czQuNy0xIDYuNi0yLjlMMjgyLjEgODkuNmMxLjktMS45IDIuOC00LjEgMi44LTYuNkMyODQuOSA4MC42IDI4NCA3OC40IDI4Mi4xIDc2LjV6Ii8+PC9zdmc+"></button>
  </div>
</template>

<script>
export default {
  name: "Counter",
  props: {
    value: {
      type: Number,
      required: true
    }
  },
  methods: {
    increment() {
      this.$emit("input", this.value + 1);
    },
    decrement() {
      this.$emit("input", this.value - 1);
    },
    enter(e) {
      this.$emit("input", e.target.value);
    }
  }
};
</script>

<style lang="scss">
.datetime-picker {
  &__counter {
    float: left;
    padding: 0 5px;

    &__button {
      display: block;
      outline: none;
      border: none;
      background: none;
      padding: 0;
      cursor: pointer;

      img {
        width: 30px;
      }
    }

    &__input {
      display: block;
      border: none;
      outline: none;
      background: none;
      font-size: 24px;
      width: 30px;
      padding: 0;
      text-align: center;
    }

    &__dots {
      float: left;
      font-size: 24px;
      line-height: 88px;
      user-select: none;
    }
  }
}
</style>
