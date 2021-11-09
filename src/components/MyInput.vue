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
  name: "MyInput",
  mixins: [MixinInput],
};
</script>
