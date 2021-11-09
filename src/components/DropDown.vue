<template>
  <div v-click-outside="close">
    <div @click="toggle">
      <slot name="drop-dow-toggle" />
    </div>

    <div v-show="opened">
      <slot name="drop-dow-content" />
    </div>
  </div>
</template>

<script>
import ClickOutside from "vue-click-outside";

export default {
  name: "DropDown",
  directives: {
    ClickOutside,
  },
  data: () => ({
    opened: false,
  }),
  methods: {
    toggle() {
      this.opened = !this.opened;
    },
    close() {
      this.opened = false;
    },
  },
};
</script>

<style scoped></style>
