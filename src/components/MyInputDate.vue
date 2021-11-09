<template>
  <div>
    <div :class="inputClasses">
      <input
        class="my-input"
        :type="inputType"
        :disabled="disabled"
        :autofocus="autofocus"
        :value="value"
        :max="max"
        :min="min"
        @focus="focusHandler"
        @blur="blurHandler"
        @input="inputHandler"
      />
      <div v-show="showPlaceholder" :class="placeholderClasses">
        <div class="placeholder-text">{{ $attrs.placeholder }}</div>
      </div>
    </div>
    <p class="error">{{ errorMessage }}</p>
  </div>
</template>

<script>
import MixinInput from "./mixins/mixin-input";

export default {
  name: "MyInputDate",
  mixins: [MixinInput],
  props: {
    min: {
      type: String,
      default: "",
    },
    max: {
      type: String,
      default: "",
    },
  },
  methods: {
    validate(value) {
      if (this.rule.test(value) && this.max >= value && this.min <= value) {
        this.$emit("valid");
        this.valid = true;
      } else {
        this.$emit("not-valid");
        this.valid = false;
      }
    },
  },
};
</script>
