<template>
  <div class="input-wrapper">
    <input
      class="input"
      type="radio"
      :checked="checked"
      @input="inputHandler"
    />
    <label>{{ label }}</label>
  </div>
</template>

<script>
export default {
  name: "MyRadioInput",
  props: {
    label: {
      type: String,
      default: "Label",
    },
    checked: {
      type: Boolean,
      default: false,
    },
  },
  methods: {
    inputHandler(event) {
      this.valid = true;
      this.$emit("input", event.target.value);
    },
  },
};
</script>

<style scoped>
.input-wrapper {
  position: relative;
  display: inline-flex;
  padding: 6px 16px 6px 6px;
  /*width: 100%;*/
  align-items: center;
}
.input {
  margin-right: 6px;
}
</style>
