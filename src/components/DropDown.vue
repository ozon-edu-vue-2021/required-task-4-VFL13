<template>
  <div v-click-outside="close">
    <div @click="toggle">
      <slot name="drop-dow-toggle" />
    </div>

    <div class="list-items" v-show="opened">
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
  props: {
    opened: {
      type: Boolean,
      default: false,
    },
  },
  methods: {
    toggle() {
      this.$emit("toggleActive", !this.opened);
    },
    close() {
      this.$emit("toggleActive", false);
    },
  },
};
</script>
