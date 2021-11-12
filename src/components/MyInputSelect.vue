<template>
  <div>
    <div :class="inputClasses">
      <input
        class="my-input"
        :type="inputType"
        :disabled="disabled"
        :autofocus="autofocus"
        :value="value"
        @focus="focusHandler"
        @blur="blurHandler"
        @input="inputHandler"
      />
      <ArrowSVG class="icon" />
      <div v-show="showPlaceholder" :class="placeholderClasses">
        <div class="placeholder-text">{{ $attrs.placeholder }}</div>
      </div>
    </div>
    <p class="error">{{ errorMessage }}</p>
  </div>
</template>

<script>
import MixinInput from "./mixins/mixin-input";
import ArrowSVG from "../assets/images/arrow.svg";

export default {
  name: "MyInputSelect",
  mixins: [MixinInput],
  components: {
    ArrowSVG,
  },
  props: {
    validateList: {
      type: Array,
    },
  },
  methods: {
    inputHandler(event) {
      this.valid = true;
      this.$emit("input", event.target.value);
    },
    validate(value) {
      if (this.validateList.length && this.validateList.includes(value)) {
        this.$emit("valid");
        this.valid = true;
      } else {
        this.$emit("not-valid");
        this.valid = false;
      }
    },
    blurHandler() {
      this.focused = false;
      this.$emit("setActive", false);
    },
    focusHandler() {
      this.focused = true;
      this.$emit("setActive", true);
    },
  },
  watch: {
    value: {
      handler: function (val) {
        this.debouncedValidation(val);
      },
    },
  },
};
</script>
