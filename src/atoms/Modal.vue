<template>
  <transition name="datetime-picker--modal-animation">
    <div class="datetime-picker__modal" v-if="open" @click.self="closeModal">
      <div class="datetime-picker__modal-container datetime-picker--clearfix">
        <slot></slot>
      </div>
    </div>
  </transition>
</template>

<script>
export default {
  name: "Modal",
  data() {
    return {
      open: false
    };
  },
  methods: {
    openModal() {
      this.open = true;
    },
    closeModal() {
      this.open = false;
    },
    toggleModal() {
      this.open = !this.open;
    }
  }
};
</script>

<style lang="scss">
.datetime-picker {
  &__modal {
    position: fixed;
    z-index: 9998;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.5);
    transition: all 0.3s ease-in-out;
  }

  &__modal-container {
    max-width: 252px * 2;
    width: 100%;
    background-color: #fff;
    border-radius: 2px;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.33);
    position: relative;
    transform: translate(-50%, -50%);
    top: 50%;
    left: 50%;
    display: table;
  }
}

.datetime-picker--modal-animation-leave-active,
.datetime-picker--modal-animation-enter {
  opacity: 0;
}

.datetime-picker--modal-animation-enter,
.datetime-picker--modal-animation-leave-active {
  transform: scale(1.1);
}
</style>
